## Task

The goal of this exercise is to:

1. **Compile and Build**: Build the `cmatrix` source code into a container image.
2. **Optimize**: Apply techniques to reduce the container image size.
3. **Use a Base Image**:
    - Leverage the lightweight **Alpine Linux** image (~10 MB) as the base.
    - Integrate `nginx` as part of the solution if required.

## Solution

- Create a Dockerfile (https://github.com/opencontainers/image-spec)
  
  ```yaml
# Use the lightweight Alpine Linux as the base image
FROM alpine:latest

# Metadata for the image
LABEL org.opencontainers.image.authors="John Foo"
LABEL org.opencontainers.image.description="Container image for cmatrix"

# Set the working directory for all subsequent instructions
WORKDIR /cmatrix

# Install dependencies required for building cmatrix
RUN apk update && apk add --no-cache \
    git build-base autoconf automake ncurses-dev ncurses-static

# Clone the cmatrix repository
RUN git clone https://github.com/abishekvashok/cmatrix .

# Prepare the build system and make the configure script executable
RUN autoreconf -i
RUN chmod +x ./configure

# Configure, compile, and link cmatrix statically
RUN ./configure LDFLAGS="-static" && make

# Clean up build dependencies and cached files to reduce image size
RUN apk del --purge autoconf automake build-base && \
    rm -rf /var/cache/apk/* /cmatrix/.git

# Set the default command to run cmatrix
CMD ["./cmatrix"]
```

- The `CMD` instruction specifies the default command that will be executed when the container starts running, whereas the `RUN` instruction executes commands during the Docker image build process.
	- The primary difference between the `CMD` and `RUN` instructions in a Dockerfile lies in their purpose and timing of execution. The `CMD` instruction specifies the default command that will be executed when the container starts running, whereas the `RUN` instruction executes commands during the Docker image build process.
- `ENTRYPOINT` specifies the command that should be executed when the container is started. It sets the default command and parameters that will be used when running the image, unless overridden by the user.
- `LABEL`is used to specify metadata
	- `org.opencontainers.image.authors` is defined in the Open Container Initiative (OCI) specification as a way to provide information about the authors of the container image.
	- `org.opencontainers.image.maintainers` is used to specify who maintains or updates the image over time
- `WORKDIR` sets the working directory for subsequent instructions in the Dockerfile. This allows the Docker builder to change the current directory and execute commands within that context.
### Steps for Execution

#### 1. **Building the Container Image**:

- Use the following command to build the container image:
    ```bash
    docker build -t cmatrix .
    ```
    
- Test and debug the build by running the container interactively:

    ```bash
    docker run -it cmatrix
    ```

#### 2. **Installing Dependencies**:

- Alpine Linux uses `apk` for package management.
- Required dependencies:
    - **`git`**: To clone the `cmatrix` source repository.
    - **`autoconf` & `automake`**: For build system preparation.
    - **`build-base`**: A meta-package providing compilers and essential build tools.
    - **`ncurses-dev` & `ncurses-static`**: For terminal interface support and static linking.
    - **Cleaning Up**: Remove unneeded tools like `autoconf` and `automake` post-build to reduce image size.

#### 3. **Compiling the Source**:

- Steps to build the `cmatrix` binary:
    1. Initialize the build system:
        ```bash
        autoreconf -i
        ```
    2. Configure the build system for static linking:
        ```bash
        ./configure LDFLAGS="-static"
        ```
    3. Compile the source:
        ```bash
        make
        ```
    4. Verify the binary functionality by running it inside the container.

#### 4. **Setting a Work Directory**:

- Using `WORKDIR` ensures that subsequent `RUN`, `CMD`, and `ENTRYPOINT` instructions execute in the specified directory.
- Avoid using relative paths in `RUN` instructions, as `cd` commands do not persist across layers.

---

### Optimization Techniques

- **Reduce Layers**: Combine related `RUN` commands to minimize image layers:
    ```bash
    RUN apk update && apk add --no-cache \
     git build-base autoconf automake ncurses-dev ncurses-static && \ 
     git clone https://github.com/abishekvashok/cmatrix . && \
     autoreconf -i && \
     chmod +x ./configure && \
     ./configure LDFLAGS="-static" && make && \
     apk del --purge autoconf automake build-base && \
     rm -rf /var/cache/apk/* /cmatrix/.git
```
    
- **Static Linking**: Use `LDFLAGS="-static"` during configuration to ensure the binary has no runtime dependencies.
- **Small Base Image**: Use `alpine:latest` (~10 MB) to keep the base image lightweight.

### Multi-stage builds

```yaml
# Stage 1: Build Stage
FROM alpine:latest AS builder

# Install build dependencies
RUN apk update && apk add --no-cache \
    git \
    autoconf \
    automake \
    alpine-sdk \
    ncurses \
    ncurses-dev

# Clone the source code
WORKDIR /app
RUN git clone https://github.com/abishekvashok/cmatrix .

# Prepare for build
RUN autoreconf -i && \
    ./configure --enable-static && \
    make

# Stage 2: Final Runtime Image
FROM alpine:latest

# Install runtime dependencies
# add-user create a new user with reduced privileges.
# By creating a non-root user, we can avoid running our application with root privileges. Additionally, using a non-root user helps prevent accidental modifications to system files.
RUN apk add --no-cache \
    ncurses \
    ncurses-terminfo-base \
    adduser -g "Thomas Anderson" -s /usr/sbin/nologin -D -H thomas

# Copy the compiled binary from the builder stage
COPY --from=builder /app/cmatrix /usr/local/bin/cmatrix

# Create User directory
USER thomas

# Default command
# ENTRYPOINT instruction specifies the command that should be executed when the container is started. It sets the default command and parameters that will be used when running the image, unless overridden by the user.
ENTRYPOINT ["cmatrix"]
CMD ["-b"]
```

### Explanation of the Stages

1. **Build Stage (`builder`)**:
    - Uses the **Alpine Linux** image.
    - Installs necessary tools for building the software, such as compilers, `git`, `autoconf`, etc.
    - Clones the source code and compiles it into a static binary.
2. **Final Runtime Image**:
    - Uses another **Alpine Linux** image for runtime.
    - Installs only essential runtime dependencies, such as `ncurses` and `ncurses-terminfo-base`.
    - Copies the compiled binary from the build stage.
    - The runtime image is small since it excludes build tools and source code.
- Add user:
	- To prevent running cmatrix as root, it was added a user with reduced privileges.
	- -s
		- Specifies the shell for the user.
	    - `/usr/sbin/nologin` disables shell access for the user, making it a system or service account.
	    - It’s commonly used for users that do not need to log in (e.g., for security or service isolation).
	- -D
		- Creates the user with default settings.
		- No need to specify additional options like UID or GID unless required.
	- -H
		- Prevents the creation of a home directory for the user.
		- Useful for lightweight containers where home directories are unnecessary.
- The `CMD` option is to pass the parameters to the `cmatrix` program

### Steps to Build and Run the Image

1. **Build the Image**:
   ```bash
   docker build -t cmatrix:optimized .
```
1. Run the container:
   ```bash
   docker run --rm cmatrix:optimized
```


We can compare the size of the two images:

```bash
docker images
REPOSITORY   TAG         IMAGE ID       CREATED          SIZE
cmatrix      optimized   3c2bc9f0d18a   21 seconds ago   13.4MB
cmatrix      latest      f4d3b9cc09bc   19 minutes ago   449MB
```


## Push to Dockerhub

Let's create a multi-arch image, and push it to the dockerhub.
You must have a dockerhub account.

```bash
docker login -u <username>
docker buildx create --name buildx-multi-arch 
docker buildx use buildx-multi-arch
```

You can push the image to the registry
```bash
docker buildx build --no-cache --platform linux/amd64,linux/arm64/v8 . -t cmatrix --push
```

or load the image locally

```bash
docker buildx build --platform linux/amd64,linux/arm64/v8 . -t cmatrix-multi --load
```

- Buildx, you can build your image for **multiple target platforms simultaneously**, which simplifies the process of creating and maintaining multi-arch images.
You can test your image

```bash
docker run --rm -it cmatrix-multi -ab -u 2 -C magenta
```

**Remove stopped containers, unused networks, dangling images, and dangling build cache.**

```bash
docker system prune
```