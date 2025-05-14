### 1. **Conceitos Fundamentais do Kubernetes**

- **Pergunta**: Qual componente foi introduzido especificamente para substituir os ReplicationControllers?
    - **Resposta**: Deployments
- **Pergunta**: Qual componente é primariamente responsável por gerir um Nó?
    - **Resposta**: O Kubelet é um agente que roda em cada Nó em um cluster. Sua responsabilidade principal é gerir o Nó, garantindo que ele esteja executando o número esperado de containers e mantendo o estado desejado. O Kubelet se comunica com o API Server para receber instruções e reportar o status do Nó.
- **Pergunta**: Qual componente faz parte da infraestrutura do nó em vez do plano de controle?
    - **Resposta**: O kube-proxy roda em cada nó no cluster e é responsável por manter regras de rede para a comunicação de Pods, garantindo o balanceamento de carga adequado entre os pods. Ele opera no nível do nó e não interage diretamente com o plano de controle em termos de agendamento ou gestao de estado.
- **Pergunta**: Em um Pod, qual namespace Linux os containers geralmente compartilham?
    - **Resposta**: Os containers em um Pod geralmente compartilham o mesmo namespace de rede, o que permite que eles se comuniquem uns com os outros usando localhost ou o endereço IP do pod. Essa partilha do namespace de rede permite que os containers trabalhem juntos de forma integrada e fornece um modelo de rede simplificado.
- **Pergunta**: Qual é a relação entre Deployments e ReplicaSets?
    - **Resposta**: Quando se cria um Deployment, ele automaticamente cria um ReplicaSet para gerir o número desejado de réplicas de pods. O ReplicaSet garante que o número especifico de pods esteja em execução e disponível, enquanto o Deployment gerencia a implementação de novas versões da aplicação.
- **Pergunta**: Como as especificações de container em Deployments e StatefulSets são semelhantes?
    - **Resposta**: Tanto Deployments quanto StatefulSets usam a mesma especificação de container dentro de seus modelos de pod. A principal diferença entre esses dois recursos está em como eles gerem o ciclo de vida e a ordenação dos pods, mas a especificação de container dentro do modelo de pod permanece consistente para todas as réplicas em ambos os casos.

---

### 2. **Serviços e Rede**

- **Pergunta**: Qual componente serve como a solução padrão de DNS DNS para Service Discoverys e resolução de nomes dentro do cluster?
    - **Resposta**: CoreDNS
- **Pergunta**: Quais dois tipos de recursos são usados para expor aplicações ao tráfego externo, incluindo opções como ClusterIP, NodePort e LoadBalancer?
    - **Resposta**: Um Serviço fornece um endereço IP estável e um nome DNS para acessar uma aplicação, enquanto um recurso Ingress atua como um ponto de entrada para requisições HTTP, permitindo definir regras de roteamento e terminação SSL.
- **Pergunta**: Como é chamado um serviço quando ele é criado sem um ClusterIP?
    - **Resposta**: Um serviço headless é criado definindo o campo clusterIP como None. Esse tipo de serviço não possui um endereço IP de cluster e não realiza balanceamento de carga ou proxy. Em vez disso, permite que os pods sejam acessados diretamente usando seus IPs de pod.
- **Pergunta**: Qual modificação é feita em um serviço ClusterIP para criar um serviço headless?
    - **Resposta**: Para criar um serviço headless no Kubernetes, você precisa definir o campo clusterIP do objeto Service como 'None'. Isso instrui o Kubernetes a não alocar um endereço IP para o Service, tornando-o efetivamente headless. Um serviço headless permite que você acesse pods diretamente sem passar por um balanceador de carga ou proxy.
- **Pergunta**: O que é verdade sobre a comunicação Pod-para-Pod dentro do mesmo nó no Kubernetes?
    - **Resposta**: Os Pods usam rede direta, sem NAT, para comunicação no mesmo nó. 
- **Pergunta**: O que significa Service Discovery?
    - **Resposta**: O Service Discovery refere-se ao processo pelo qual os serviços podem localizar e se comunicar uns com os outros na rede. Isso é alcançado por meio do uso de Services, que fornecem um endereço IP estável e um nome DNS para acessar pods que estão rodando no cluster. Isso permite que os serviços sejam desacoplados de instâncias específicas de pods e possibilita a comunicação entre serviços.
- **Pergunta**: Qual componente do Kubernetes é responsável por expor aplicações rodando em um conjunto de Pods como um serviço de rede?
    - **Resposta**: Um Service é um recurso que fornece um endereço IP e um nome DNS para acessar uma aplicação rodando em um conjunto de Pods. Ele atua como um proxy de rede, roteando tráfego para o Pod apropriado, permitindo que as aplicações sejam expostas ao mundo exterior.
- **Pergunta**: Num cluster, qual componente é responsável por rotear tráfego para serviços e gerir regras de IP?
    - **Resposta**: O Kube-Proxy é um proxy de rede que roda em cada nó em um cluster e é responsável por rotear tráfego para services e gerir regras de IP. Ele garante que as requisições recebidas sejam direcionadas ao pod correto, mesmo que o pod esteja rodando em um nó diferente.
- **Pergunta**: Quais são os modos primários de Service Discovery dentro de um cluster Kubernetes?
    - **Resposta**: Envvars e DNS são os modos primários de Service Discovery dentro de um cluster Kubernetes. Quando um pod é criado, o Kubernetes configura automaticamente variáveis de ambiente para o container do pod que apontam para outros serviços rodando no mesmo namespace. Além disso, o Kubernetes fornece um serviço DNS embedded que permite que os pods descubram e se comuniquem uns com os outros usando nomes DNS.
- **Pergunta**: Qual afirmação é verdadeira sobre o Ingress no Kubernetes em relação ao roteamento de tráfego?
    - **Resposta**: O Ingress no Kubernetes fornece uma maneira de rotear tráfego HTTP e HTTPS externo para serviços dentro do cluster. Ele atua como um ponto de entrada para requisições recebidas e pode ser configurado para direcionar o tráfego com base em várias regras, incluindo caminhos de URL, hostnames e mais.

---

### 3. **Gestao de Recursos e Objetos**

- **Pergunta**: Qual mecanismo no Kubernetes permite especificar critérios para selecionar informações com base em campos como nome, namespace ou status de um objeto Kubernetes?
    - **Resposta**: Field Selectors
- **Pergunta**: Quais entidades podem utilizar o atributo immutable:true para garantir que seus dados não possam ser modificados após a criação?
    - **Resposta**: **ConfigMaps** e **Secrets** podem utilizar o atributo immutable:true para garantir que seus dados não possam ser modificados após a criação. Quando esse atributo é defin ido como true, qualquer tentativa de atualizar o ConfigMap ou Secret resultará em um erro.
- **Pergunta**: Qual objeto é mais adequado para implantar aplicações sem estado?
    - **Resposta**: Um **Deployment** é um objeto que gerencia um conjunto de réplicas de um Pod, tornando-o adequado para implantar aplicações sem estado. Os Deployments fornecem recursos como atualizações graduais e autocura, tornando-os ideais para gerir várias instâncias de uma aplicação sem estado.
- **Pergunta**: Para alcançar uma nomeação DNS consistente para pods geridos por um StatefulSet Qual recurso adicional do Kubernetes deve ser usado?
    - **Resposta**: Headless Service
- **Pergunta**: Qual feature é crucial para gerir aplicações stateful distribuídas para evitar conflitos como cenários de "cérebro dividido"?
    - **Resposta**: StatefulSets são um tipo de controlador no Kubernetes que fornece garantias sobre a implantação e gestao de aplicações stateful. Eles garantem que cada réplica tenha uma identidade única e que a aplicação possa manter seu estado mesmo em caso de falhas ou partições de rede, evitando assim cenários de "cérebro dividido".
- Pergunta: O que é um headless service?
	- Resposta: Um **headless service** é um tipo especial de serviço que **nao se atribui um ClusterIP**, em vez, expoe-se um **IP individual** directamente no Pod.
- **Pergunta**: Qual recurso é eficaz para gerir custos, permitindo a categorização e organização de recursos?
	    - **Resposta**: Labels são pares chave-valor anexados a objetos, como pods ou serviços, e fornecem uma maneira flexível de categorizar e organizar recursos. Ao usar Labels, você pode selecionar e gerir grupos de recursos com base em critérios específicos, facilitando o gestao de custos ao alocar recursos para equipes, projetos ou ambientes específicos.
- **Pergunta**: Quais são os namespaces padrão em uma instalação do Kubernetes?
    - **Resposta**: default, kube-system, kube-public, kube-node-lease. Em uma instalação do Kubernetes, há quatro namespaces padrão que são criados durante o processo de configuração do cluster. Esses namespaces são 'default', 'kube-system', 'kube-public' e 'kube-node-lease'. O namespace 'default' é onde as aplicações implantadas pelo usuário geralmente rodam. O namespace 'kube-system' contém objetos criados pelo sistema Kubernetes, como o API Server, o gerenciador de controladores e o agendador. O namespace 'kube-public' é usado para informações de todo o cluster que podem ser expostas publicamente com segurança sem autenticação. O namespace 'kube-node-lease' é usado para objetos de locação de nós.

---

### 4. **Jobs e Agendamento**

- **Pergunta**: Qual componente do Kubernetes permite que agendem jobs com base em intervalos de tempo definidos pelo usuário?
    - **Resposta**: CronJobs
- **Pergunta**: Qual objeto é recomendado para gerir jobs que precisam ser executados várias vezes de acordo com um processo em batch?
    - **Resposta**: Um CronJob é um objeto Kubernetes projetado especificamente para gerir jobs que precisam ser executados várias vezes de acordo com um processo em batch, como executar um job a cada hora ou diariamente. Ele permite especificar um agendamento usando uma expressão cron e garante que o job seja executado no horário especificado.
- **Pergunta**: Como você habilitaria o compartilhamento de dados entre diferentes cronjobs executados em vários horários?
    - **Resposta**: Um PVC Claim permite que vários pods acessem um volume de armazenamento compartilhado, possibilitando o compartilhamento de dados entre diferentes cronjobs executados em vários horários. Ao usar uma PVC, você pode montar um volume persistente em vários pods, permitindo que eles leiam e escrevam dados no mesmo local de armazenamento.

---

### 5. **Segurança**

- **Pergunta**: No contexto de autenticação e autorização, o que significa a sigla OIDC?
    - **Resposta**: OpenID Connect. Este é um protocolo de autenticação construído sobre o OAuth 2.0.
- **Pergunta**: Perfis de Segurança do Kubernetes, como PodSecurityPolicies (PSP) e Kyverno, são essenciais para impor padrões de segurança em um ambiente Kubernetes. Qual das seguintes opções descreve melhor a função primária desses Perfis de Segurança do Kubernetes?
    - **Resposta**: Perfis de Segurança do Kubernetes, como PodSecurityPolicies (PSP) e Kyverno, são usados principalmente para definir e impor configurações de segurança no nível do pod, garantindo que os pods cumpram padrões e requisitos de segurança específicos. Esses perfis permitem que os administradores controlem vários aspectos do comportamento do pod, incluindo políticas de rede, controles de acesso e alocação de recursos.
- **Pergunta**: Qual ferramenta focada em segurança é comumente usada para monitoramento de segurança em tempo de execução e detecção de atividades anômalas dentro de containers e pods?
    - **Resposta**: O Falco é uma ferramenta de segurança nativa da nuvem de código aberto que fornece monitoramento de segurança em tempo de execução e detecção de atividades anômalas dentro de containers e pods em ambientes Kubernetes. Ele usa chamadas de sistema para monitorar a atividade do container e pode detectar comportamentos incomuns, como escalonamento de privilégios ou acesso não autorizado.
- **Pergunta**: Qual função é primariamente associada aos SecurityContext do Kubernetes?
    - **Resposta**: Os SecurityContext estão primariamente associados à definição de configurações de segurança no nível do container ou pod. Um SecurityContext define as configurações de segurança para um container ou pod, como executar um container como um usuário específico, definir permissões de sistema de arquivos e configurar Labels SELinux. Essas configurações podem ser aplicadas a um único container ou a todos os containers em um pod.
- **Pergunta**: Na segurança do Kubernetes, o que diferencia os SecurityContext das SecurityPolicy (como PodSecurityPolicies ou Kyverno) em termos de scope e foco?
    - **Resposta**: **SecurityContext** são configurações definidas dentro de uma especificação de pod ou container. Scope: pod ou container individual. **SecurityPolicy** são ferramentas de imposição no nível do cluster ou namespace. scope: todos os pods em um cluster ou namespace.
- **Pergunta**: Qual ferramenta de código aberto é especificamente projetada para avaliar a postura de segurança de clusters Kubernetes de acordo com as diretrizes da NSA e CISA?
    - **Resposta**: O KubeScape é uma ferramenta de código aberto projetada especificamente para avaliar a postura de segurança de clusters Kubernetes com base nas diretrizes da NSA e da CISA. Ela fornece uma avaliação de risco abrangente e identifica possíveis vulnerabilidades na configuração do cluster.
- **Pergunta**: Quando um Pod é criado no Kubernetes sem especificar uma ServiceAccount, qual ServiceAccount padrão é usada?
    - **Resposta**: Default ServiceAccount

---

### 6. **Ferramentas e Integrações**

- **Pergunta**: No domínio do Kubernetes, qual framework serverless se destaca por sua velocidade excepcional, abordagem centrada no desenvolvedor e desempenho otimizado, visando aumentar a produtividade do desenvolvedor?
    - **Resposta**: Fission
- **Pergunta**: Quais ferramentas nativas da nuvem permitem que clusters Kubernetes sejam sincronizados automaticamente com repositórios Git?
    - **Resposta**: Argo CD e Flux são ferramentas nativas da nuvem que permitem que clusters Kubernetes sejam sincronizados automaticamente com repositórios Git. O Argo CD é uma ferramenta de entrega contínua declarativa para Kubernetes que automatiza a implantação de aplicações a partir de repositórios Git. O Flux é uma ferramenta de código aberto que sincroniza recursos Kubernetes com repositórios Git, permitindo a implantação e gestao automatizados de aplicações.
- **Pergunta**: Qual produto é construído usando o GitOps Toolkit, um conjunto de APIs componíveis e ferramentas especializadas para construir sistemas de entrega contínua baseados em GitOps?
    - **Resposta**: O Flux é realmente construído usando o GitOps Toolkit, que fornece um conjunto de APIs e ferramentas especializadas para construir sistemas de entrega contínua baseados em GitOps. O GitOps Toolkit permite que os desenvolvedores criem fluxos de trabalho personalizados e integrações com outras ferramentas, tornando-o uma escolha ideal para construir pipelines de entrega contínua complexos.
- **Pergunta**: Qual ferramenta é especificamente projetada como uma service mesh abrangente para controlar, proteger e observar as interações entre microsserviços, além de facilitar o gestao avançado de tráfego?
    - **Resposta**: O Istio é uma service mesh que fornece um conjunto abrangente de recursos para controlar, proteger e observar interações entre microsserviços no Kubernetes. Ele oferece capacidades avançadas de gestao de tráfego, incluindo disjuntores, tentativas e injeção de falhas, além de recursos de segurança como TLS mútuo e políticas de autenticação.
- **Pergunta**: No domínio do Kubernetes, qual ferramenta é especificamente projetada para orquestrar e gerir fluxos de trabalho paralelos complexos e jobs em batch?
    - **Resposta**: O Argo Workflows é uma ferramenta de código aberto especificamente projetada para orquestrar e gerir fluxos de trabalho paralelos complexos e jobs em batch no Kubernetes. Ele fornece uma maneira flexível de definir fluxos de trabalho usando arquivos YAML ou JSON e suporta recursos como lógica condicional, loops e tentativas.
- **Pergunta**: Em um ambiente Kubernetes, qual software é amplamente usado como um proxy leve para gerir o tráfego entre microsserviços?
    - **Resposta**: O Envoy é um proxy leve amplamente usado, projetado para gerir o tráfego entre microsserviços em um ambiente Kubernetes. Ele fornece recursos como Service Discoverys, balanceamento de carga e quebra de circuito, tornando-o uma escolha ideal para gerir a comunicação entre microsserviços.
- **Pergunta**: Qual ferramenta é comumente usada para instalar e gerir aplicações em um cluster Kubernetes?
    - **Resposta**: O Helm é um gerenciador de pacotes para Kubernetes que simplifica o processo de instalação e gestao de aplicações em um cluster Kubernetes. Ele fornece uma maneira fácil de instalar, atualizar e gerir aplicações usando pacotes pré-configurados chamados charts. Não é o kubectl. O Kubectl é uma ferramenta de linha de comando para interagir com um cluster Kubernetes, mas não é especificamente projetada para instalar e gerir aplicações. Embora possa ser usado para implantar aplicações usando arquivos YAML ou JSON, requer mais esforço manual e não fornece o mesmo nível de gestao de pacotes que o Helm.
- **Pergunta**: O que é OPA?
    - **Resposta**: O Agente de Políticas Abertas (OPA) é um motor de políticas que pode ser integrado ao Kubernetes para validar requisições e aplicar políticas em todo o cluster. O OPA fornece uma maneira unificada de definir e impor políticas em diferentes camadas da pilha, incluindo políticas de rede, controle de admissão e mais.

---

### 7. **Observabilidade e Telemetria**

- **Pergunta**: Qual projeto da CNCF foca em fornecer um framework unificado para geração, gestao e análise de logs, métricas e Tracing por meio de um conjunto abrangente de ferramentas, APIs e SDKs?
    - **Resposta**: OpenTelemetry
- **Pergunta**: Quais são os três pilares da observabilidade em um sistema de software?
    - **Resposta**: Os três pilares da observabilidade em um sistema de software são realmente **Logs, Métricas e Tracing**. A observabilidade refere-se à capacidade de entender o estado interno de um sistema analisando suas saídas. Esses três pilares fornecem diferentes tipos de insights sobre o comportamento do sistema: logs fornecem registros detalhados de eventos, métricas oferecem medições quantitativas de desempenho e saúde, e tracing permite a análise de interações complexas entre componentes.
- **Pergunta**: Em telemetria, qual entidade é primariamente usada para registrar e analisar o caminho que uma requisição percorre através de vários serviços em um sistema distribuído?
    - **Resposta**: Um Tracing registra toda a jornada de uma requisição enquanto ela percorre vários serviços, APIs ou componentes em um sistema distribuído.
- **Pergunta**: Qual é o propósito primário do kube-state-metrics em um cluster Kubernetes?
    - **Resposta**: O kube-state-metrics é um serviço que **gera e expõe dados de estado do cluster no formato Prometheus**. Ele faz isso coletando informações do API Server do Kubernetes sobre o estado atual do cluster, incluindo métricas sobre implantações, pods, nós e mais.

---

### 8. **Runtimes de container**

- **Pergunta**: Qual é a implementação de referência da especificação de runtime do Open Container Initiative (OCI)?
    - **Resposta**: **runc** é a implementação de referência da especificação de runtime do Open Container Initiative (OCI). É um ambiente de execução leve, portátil e de alto desempenho para containers que implementa a especificação de runtime OCI. O runc fornece uma maneira minimalista e flexível de executar containers, tornando-o uma escolha de referência. O OCI é um standard que diz como os containers, e as images dos containers devem ser build, store, and run.
- **Pergunta**: Qual dos seguintes é um runtime de container leve especificamente projetado para Kubernetes, conforme a Interface de Runtime de container (CRI)?
    - **Resposta**: O CRI-O é um runtime de container leve projetado especificamente para Kubernetes e conforme a Interface de Runtime de container (CRI). Ele fornece uma abordagem minimalista para executar containers em um ambiente Kubernetes, tornando-o uma escolha ideal para esse ecossistema. Não é o LXC, porque o LXC é uma interface de espaço de usuário para os recursos de contenção do kernel Linux. Embora o LXC forneça capacidades de containerização, não é especificamente projetado para Kubernetes ou conforme o CRI.
- **Pergunta**: Qual dos seguintes runtimes de container é um runtime leve de baixo nível projetado especificamente para executar containers no Linux?
    - **Resposta**: LXC
- **Pergunta**: Qual é a diferença entre LXC e CRI-O?
    - **Resposta**: O LXC é uma tecnologia de virtualização leve para executar múltiplos sistemas Linux isolados (containers) em um único host de controle. O CRI-O é um runtime de container projetado especificamente para Kubernetes.
- **Pergunta**: Quais dos seguintes runtimes de container são reconhecidos por fornecer recursos de segurança aprimorados, como isolamento mais forte por meio de virtualização?
    - **Resposta**: Kata Containers e gVisor são reconhecidos por fornecer recursos de segurança aprimorados por meio de virtualização. Eles usam uma combinação de containerização e virtualização para fornecer um isolamento mais forte entre containers. Kata Containers usa uma máquina virtual leve (VM) para executar cada container, enquanto o gVisor usa um kernel em espaço de usuário para fornecer um ambiente isolado para containers.
- **Pergunta**: Qual componente do Kubernetes interage com a Interface de Runtime de container (CRI)?
    - **Resposta**: O Kubelet interage com a Interface de Runtime de container (CRI) para gerir operações de runtime de container, como criar e excluir containers. O CRI fornece uma interface padronizada entre o agente de nó do Kubernetes (Kubelet) e o runtime de container.

---

### 9. **CI/CD e GitOps**

- **Pergunta**: Qual opção se alinha ao fluxo de trabalho "tradicional" de um pipeline CI/CD, onde "D" em CI/CD significa Deployment?
    - **Resposta**: Em um pipeline CI/CD tradicional, o fluxo de trabalho geralmente segue esta ordem:
        1. Construção: O código é compilado e empacotado em um artefato implantável.
        2. Teste: O artefato construído é então testado para garantir que atenda aos padrões exigidos.
        3. Lançamento: O artefato testado é preparado para implantação, o que pode envolver a criação de um candidato a lançamento ou o empacotamento do artefato para distribuição.
        4. Deployment: O artefato lançado é finalmente deployed em produção, tornando-o disponível para os usuários finais.

---

### 10. **Gestao de Imagens de container**

- **Pergunta**: Ao criar containers usando construções de múltiplos stages no Docker, qual técnica pode ser empregada para reduzir o tamanho da imagem final do container?
    - **Resposta**: Construções de múltiplos stages no Docker permitem o uso de várias instruções FROM em seu Dockerfile. Cada stage pode ser usado para realizar uma tarefa específica, como construir uma aplicação ou instalar dependências. Ao utilizar vários estágios de construção, você pode descartar artefatos e dependências de construção desnecessários em cada estágio, resultando em uma imagem de container final menor.

---

### 11. **Gestao de Clusters e Operações**

- **Pergunta**: No contexto do etcd usado, qual é o significado do limite de tamanho de 1,5MB para o etcd?
    - **Resposta**: No contexto do etcd usado no Kubernetes, o limite de tamanho de 1,5MB representa o tamanho máximo recomendado para um valor individual armazenado no etcd. Isso significa que qualquer valor armazenado no etcd deve ser menor ou igual a 1,5MB. Esse limite ajuda a evitar que valores grandes impactem o desempenho e a estabilidade do cluster etcd.
- **Pergunta**: Por quanto tempo a API do Kubernetes é garantida como retrocompatível após um lançamento?
    - **Resposta**: A API do Kubernetes é garantida como retrocompatível por pelo menos 1 ano após um lançamento. Isso significa que, se você escrever código contra uma versão específica da API, pode esperar que ele continue funcionando sem alterações por pelo menos 12 meses após o lançamento dessa versão.
- **Pergunta**: Qual é a função primária de um PDB?
    - **Resposta**: Manter um número mínimo de réplicas disponíveis durante interrupções voluntárias. Os PodDisruptionBudget (PDBs) foram introduzidos no Kubernetes para proteger contra interrupções voluntárias, como atualizações graduais ou dimensionamento. Sua função primária é manter um número mínimo de réplicas disponíveis durante essas interrupções, garantindo que um certo nível de disponibilidade de serviço seja sempre mantido.
- **Pergunta**: Qual é a estratégia de atualização padrão usada pelos Deployments do Kubernetes para implementar atualizações?
    - **Resposta**: RollingUpdate. RollingUpdate é a estratégia de atualização padrão usada pelos Deployments do Kubernetes para implementar atualizações. Ela permite uma implementação gradual de novas versões enquanto mantém a disponibilidade e minimiza o tempo de inatividade. Com RollingUpdate, novos pods são criados com a versão atualizada, e os pods antigos são encerrados assim que os novos estão disponíveis.
- **Pergunta**: Durante a fase em que as imagens de container estão sendo baixadas para um Pod no Kubernetes, em que estado o Pod está tipicamente?
    - **Resposta**: Pending State. Quando um Pod está no estado Pendente, significa que o Pod foi criado, mas pelo menos um de seus containers ainda está esperando que os recursos sejam alocados ou ainda está baixando sua imagem. Esse é tipicamente o caso quando as imagens de container estão sendo baixadas para um Pod.
- **Pergunta**: Qual kubectl comando é usado para listar todos os recursos da API, como Pods, Services e Deployments, disponíveis em um cluster Kubernetes?
    - **Resposta**: kubectl api-resources é usado para listar todos os recursos da API, como Pods, Services e Deployments, disponíveis em um cluster Kubernetes. Ele fornece uma lista abrangente de todos os recursos que podem ser geridos usando a API do Kubernetes.
- **Pergunta**: Quando o comando kubectl expose é usado, qual componente é criado?
    - **Resposta**: Service. Quando você executa kubectl expose, ele cria um Service, que é um recurso Kubernetes que fornece uma identidade de rede e balanceamento de carga para acessar um ou mais pods. O comando expose pega um pod ou controlador de replicação existente e cria um novo Serviço que expõe a porta especificada.
- **Pergunta**: Ao usar kubectl apply, qual é uma desvantagem potencial relacionada ao Tracing do estado dos recursos?
    - **Resposta**: Se você remover recursos do manifesto aplicado, o kubectl apply pode não rastreá-los e pode inadvertidamente excluir esses recursos. Se você remover recursos do manifesto aplicado e reaplicá-lo, o kubectl apply pode interpretar os recursos ausentes como não mais necessários e os excluirá do cluster. Esse comportamento é particularmente importante em situações em que um manifesto foi reduzido ou modificado sem a intenção de excluir recursos do cluster.

---

### 12. **Autoescalonamento**

- **Pergunta**: Qual solução de autoescalonamento do Kubernetes tem a capacidade de reduzir as cargas de trabalho para zero pods?
    - **Resposta**: O Kubernetes Event-Driven Autoscaling (KEDA) é uma solução de autoescalonamento que permite reduzir suas cargas de trabalho para zero pods com base em eventos, como comprimentos de filas de mensagens ou requisições HTTP. O KEDA usa uma abordagem baseada em pull, onde verifica periodicamente por novos eventos e escala de acordo.
- **Pergunta**: Como o KEDA habilita o autoescalonamento orientado a eventos no Kubernetes, e qual recurso personalizado ele utiliza para esse propósito?
    - **Resposta**: O KEDA habilita o autoescalonamento orientado a eventos no Kubernetes escalando aplicações com base em métricas externas, como comprimentos de filas de mensagens ou requisições HTTP. Ele utiliza um recurso personalizado chamado ScaledObjects para esse propósito. Os ScaledObjects definem como escalar uma aplicação com base nessas métricas externas.

---

### 13. **Eventos e Padrões**

- **Pergunta**: O que são CloudEvents no contexto da computação em nuvem?
    - **Resposta**: CloudEvents é realmente um conjunto de padrões para descrever dados de eventos de maneira comum. Ele fornece um formato padronizado e interoperável para expressar eventos, facilitando a compreensão e o processamento de dados de eventos por diferentes sistemas e aplicações.

---

### 14. **Papéis e Responsabilidades**

- **Pergunta**: Dentro do cenário colaborativo do Kubernetes, o que define os Special Interest Groups (SIGs)?
    - **Resposta**: Equipes exclusivas focadas em áreas ou domínios específicos do projeto.
- **Pergunta**: Em uma organização nativa da nuvem, qual papel é primariamente responsável por construir e manter a infraestrutura da plataforma subjacente, permitindo que os desenvolvedores de aplicações implantem e executem seus serviços de forma eficiente?
    - **Resposta**: Engenheiros de Plataforma
- **Pergunta**: No contexto de ambientes nativos da nuvem, qual persona foca primariamente na otimização de custos de nuvem e gestao financeiro?
    - **Resposta**: FinOps (Operações Financeiras) é uma persona relativamente nova que surgiu em ambientes nativos da nuvem. Eles são primariamente responsáveis por otimizar custos de nuvem, gerir recursos financeiros e garantir que a organização obtenha o melhor retorno possível sobre seu investimento em gastos com nuvem. As equipes FinOps trabalham em estreita colaboração com outras partes interessadas para identificar áreas de otimização de custos, implementar medidas de economia de custos e desenvolver estratégias para gestao financeiro.
- **Pergunta**: Em um ambiente nativo da nuvem, qual persona é tipicamente responsável pela criação e execução de um procedimento de gestao de incidentes?
    - **Resposta**: Um Site Reliability Engineer (SRE) é tipicamente responsável por garantir a confiabilidade e o tempo de atividade de um ambiente nativo da nuvem. Como parte desse papel, eles frequentemente estão envolvidos na criação e execução de procedimentos de gestao de incidentes para minimizar o tempo de inatividade e garantir uma recuperação rápida em caso de interrupção ou outros incidentes.
- **Pergunta**: No campo de operações de TI e desenvolvimento de software, SRE representa uma disciplina que enfatiza automação, melhoria contínua e uma abordagem equilibrada para confiabilidade do sistema e recursos. O que significa SRE?
    - **Resposta**: Site Reliability Engineer

---

### 15. **Tecnologias de Suporte**

- **Pergunta**: Qual recurso Linux é utilizado pelo Kubernetes para isolamento de containers, limitando o uso de recursos de um processo ou conjunto de processos?
    - **Resposta**: cgroups (grupos de controle) é um recurso Linux que fornece uma maneira de limitar, contabilizar e isolar o uso de recursos de um processo ou conjunto de processos rodando em um sistema. O Kubernetes utiliza esse recurso para isolamento de containers, garantindo que cada container tenha seu próprio ambiente isolado com recursos limitados.
- **Pergunta**: Quais são os componentes comuns encontrados em uma implementação de service mesh?
    - **Resposta**: Em uma implementação de service mesh, existem dois componentes primários: o plano de dados e o plano de controle. O plano de dados refere-se aos proxies que interceptam e gerem o tráfego entre serviços, enquanto o plano de controle gerencia a configuração e o comportamento do plano de dados. Exemplos de malhas de serviço incluem Istio, Linkerd e Envoy.