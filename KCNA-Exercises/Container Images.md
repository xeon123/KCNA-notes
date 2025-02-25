
Challenge 3: Use the docker history command to inspect the layers that comprise of this image

```bash
$ docker history alpine:latest
IMAGE          CREATED      CREATED BY                                      SIZE      COMMENT
56fa17d2a7e7   9 days ago   CMD ["/bin/sh"]                                 0B        buildkit.dockerfile.v0
<missing>      9 days ago   ADD alpine-minirootfs-3.21.2-x86_64.tar.gz /…   8.5MB     buildkit.dockerfile.v0
```

Challenge 4: Compare this to the layers listing on Docker Hub

 ```bash
 $ crane manifest alpine:latest | sha256sum 
   56fa17d2a7e7f168a043a2712e63aed1f8543aeafdcee47c58dcffe38ed51099  -
 ```
The digest is the same as the one on docker hub, and the same as

```bash
$ docker inspect alpine
```

Challenge 5: Identify the image id and digest

  ```bash
  $ docker images --digest alpine
  REPOSITORY   TAG       DIGEST                                                                    IMAGE ID       CREATED      SIZE
alpine       latest    sha256:56fa17d2a7e7f168a043a2712e63aed1f8543aeafdcee47c58dcffe38ed51099   56fa17d2a7e7   9 days ago   12.1MB
  ```

Challenge 6: Remove the alpine image and confirm that it is removed

  ```bash
  $ docker rmi alpine
  ```

Challenge 7: Capture a specific version of ubuntu by using the tag 20.04 and the alternative command syntax in the format of docker noun verb

  ```bash
  
  $ docker pull ubuntu:20.04
  ```

Challenge 8: Check the digest, using the alternative command syntax

```bash
$ docker image list --digests ubuntu:20.04
REPOSITORY   TAG       DIGEST                                                                    IMAGE ID       CREATED        SIZE
ubuntu       20.04     sha256:8e5c4f0285ecbb4ead070431d29b576a530d3166df73ec44affc1cd27555141b   8e5c4f0285ec   3 months ago   109MB

```

Challenge 9: Inspect the layers of this image using docker history, via the alternative command syntax

```bash
  $ docker history ubuntu:20.04 
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
8e5c4f0285ec   3 months ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      3 months ago   /bin/sh -c #(nop) ADD file:7486147a645d8835a…   81.7MB    
<missing>      3 months ago   /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B        
<missing>      3 months ago   /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B        
<missing>      3 months ago   /bin/sh -c #(nop)  ARG LAUNCHPAD_BUILD_ARCH     0B        
<missing>      3 months ago   /bin/sh -c #(nop)  ARG RELEASE                  0B 
```

