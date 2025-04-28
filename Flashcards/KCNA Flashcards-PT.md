# Flashcards Nativos da Nuvem

Abaixo estão flashcards otimizados para Obsidian, cobrindo Kubernetes, arquitetura nativa da nuvem e conceitos relacionados. Cada flashcard usa um formato consistente Pergunta :: Resposta e inclui tags relevantes para filtragem.

---

Quais são as origens da Arquitetura Nativa da Nuvem? :: Evoluiu a partir dos desafios com aplicações monolíticas legadas. #flashcard #nativodanuvem

Qual é o foco da Arquitetura Nativa da Nuvem? :: Melhora a disponibilidade, gerenciamento de custos, eficiência e confiabilidade por meio de padrões de design. #flashcard #nativodanuvem

Quais são os benefícios da Arquitetura Nativa da Nuvem? :: Permite sistemas fracamente acoplados, resilientes, gerenciáveis e observáveis com automação robusta. #flashcard #nativodanuvem

No Kubernetes, qual framework sem servidor se destaca por velocidade e produtividade do desenvolvedor? :: Fission #flashcard #kubernetes #semservidor

No Kubernetes, qual componente substituiu os ReplicationControllers? :: Deployments #flashcard #kubernetes

Qual é a solução DNS padrão para descoberta de serviços no Kubernetes? :: CoreDNS #flashcard #kubernetes #rede

Qual mecanismo do Kubernetes seleciona informações com base em campos como nome ou status? :: Seletores de Campo #flashcard #kubernetes

Quais são os mecanismos de autorização suportados no Kubernetes? :: Todas as opções disponíveis estão corretas. #flashcard #kubernetes #segurança

Qual projeto da CNCF fornece um framework unificado para logs, métricas e rastreamentos? :: OpenTelemetry #flashcard #cncf #observabilidade

Qual componente do Kubernetes gerencia o estado dos nós de trabalho? :: kubelet #flashcard #kubernetes

Qual componente do Kubernetes agenda tarefas com base em intervalos de tempo? :: CronJobs #flashcard #kubernetes #tarefas

O que define os Grupos de Interesse Especial (SIGs) no Kubernetes? :: Equipes exclusivas focadas em áreas específicas do projeto. #flashcard #kubernetes #cncf

Quais são os dois recursos do Kubernetes para expor aplicações ao tráfego externo? :: Serviço (IP/DNS estável) e Ingress (roteamento HTTP/SSL). #flashcard #kubernetes #rede

Quais ferramentas nativas da nuvem sincronizam clusters Kubernetes com repositórios Git? :: Argo CD e Flux #flashcard #kubernetes #gitops

Qual produto é construído usando o GitOps Toolkit para entrega contínua? :: Flux #flashcard #kubernetes #gitops

Qual componente do Kubernetes gerencia principalmente um Nó? :: Kubelet #flashcard #kubernetes

Qual recurso do Kubernetes garante nomes DNS consistentes para pods de StatefulSet? :: Serviço Headless #flashcard #kubernetes #statefulsets

Qual ferramenta é um service mesh abrangente para microsserviços no Kubernetes? :: Istio #flashcard #kubernetes #servicemesh

Por quanto tempo a API do Kubernetes é garantida como retrocompatível? :: Pelo menos 1 ano após um lançamento. #flashcard #kubernetes #api

O que são CloudEvents na computação em nuvem? :: Padrões para descrever dados de eventos em um formato comum. #flashcard #nativodanuvem

Qual ferramenta do Kubernetes orquestra fluxos de trabalho paralelos complexos? :: Argo Workflows #flashcard #kubernetes #tarefas

O que é verdade sobre a comunicação Pod-para-Pod no mesmo nó no Kubernetes? :: Usa rede direta, sem NAT. #flashcard #kubernetes #rede

Qual é a implementação de referência da especificação de runtime OCI? :: runc #flashcard #contêineres #padrõesabertos

Quais são os três pilares da observabilidade em um sistema de software? :: Logs, Métricas, Rastreamentos #flashcard #observabilidade

Qual é a importância do limite de 1,5MB no etcd para o Kubernetes? :: Tamanho máximo recomendado para valores individuais para manter o desempenho. #flashcard #kubernetes #etcd

Na telemetria, o que registra o caminho de uma solicitação por meio de serviços em um sistema distribuído? :: Rastreamento #flashcard #observabilidade

O que significa OIDC em autenticação e autorização? :: OpenID Connect #flashcard #segurança

Qual é a função principal dos Perfis de Segurança do Kubernetes, como PSP e Kyverno? :: Aplicar configurações de segurança no nível do pod. #flashcard #kubernetes #segurança

Qual software é usado como proxy leve para gerenciamento de tráfego no Kubernetes? :: Envoy #flashcard #kubernetes #servicemesh

Qual é o fluxo de trabalho tradicional de um pipeline CI/CD? :: Construção, Testes, Lançamento, Implantação #flashcard #devops

Qual objeto do Kubernetes gerencia tarefas que são executadas várias vezes em um processo em lote? :: CronJob #flashcard #kubernetes #tarefas

Qual técnica reduz o tamanho das imagens de contêineres Docker em construções de múltiplos estágios? :: Descartar artefatos desnecessários usando vários estágios de construção. #flashcard #docker #contêineres

O que é um serviço do Kubernetes sem um ClusterIP chamado? :: Serviço Headless #flashcard #kubernetes #rede

Quais entidades do Kubernetes podem usar o atributo immutable:true? :: ConfigMaps e Secrets #flashcard #kubernetes #configmaps #secrets

Como os dados podem ser compartilhados entre cronjobs no Kubernetes? :: Reivindicação de Volume Persistente (PVC) #flashcard #kubernetes #armazenamento

Qual componente do Kubernetes faz parte da infraestrutura do nó, não do plano de controle? :: kube-proxy #flashcard #kubernetes

Qual é a função principal do kube-state-metrics no Kubernetes? :: Gera e expõe dados de estado do cluster no formato Prometheus. #flashcard #kubernetes #observabilidade

O que é verdade sobre o Ingress no Kubernetes para roteamento de tráfego? :: Roteia tráfego HTTP/HTTPS externo para serviços com base em regras. #flashcard #kubernetes #rede

Qual ferramenta avalia a segurança do cluster Kubernetes conforme as diretrizes da NSA/CISA? :: KubeScape #flashcard #kubernetes #segurança

Qual objeto do Kubernetes é melhor para aplicações sem estado? :: Deployment #flashcard #kubernetes #deployments

Qual papel constrói e mantém a infraestrutura da plataforma em uma organização nativa da nuvem? :: Engenheiros de Plataforma #flashcard #nativodanuvem #personas

Qual componente do Kubernetes roteia tráfego para serviços e gerencia regras de IP? :: Kube-Proxy #flashcard #kubernetes #rede

Qual ferramenta monitora a segurança em tempo de execução em contêineres e pods do Kubernetes? :: Falco #flashcard #kubernetes #segurança

Qual é a função principal dos Contextos de Segurança do Kubernetes? :: Definir configurações de segurança no nível do contêiner ou pod. #flashcard #kubernetes #segurança

Qual é a relação entre Deployments e ReplicaSets no Kubernetes? :: Deployments criam ReplicaSets para gerenciar réplicas de pods. #flashcard #kubernetes #deployments

Como os Contextos de Segurança diferem das Políticas de Segurança no Kubernetes? :: Contextos de Segurança aplicam-se a pods/contêineres individuais; Políticas de Segurança aplicam-se no nível de cluster/namespace. #flashcard #kubernetes #segurança

Qual recurso do Kubernetes gerencia custos categorizando recursos? :: Rótulos #flashcard #kubernetes #gerenciamentodecustos

Qual persona otimiza os custos da nuvem em ambientes nativos da nuvem? :: FinOps #flashcard #nativodanuvem #personas #gerenciamentodecustos

Qual runtime de contêiner é projetado para o Kubernetes e conforme o CRI? :: CRI-O #flashcard #kubernetes #contêineres

Qual runtime de contêiner é um runtime leve de baixo nível para contêineres Linux? :: LXC #flashcard #contêineres

Qual é a diferença entre LXC e CRI-O? :: LXC é para contêineres Linux leves; CRI-O é construído para o CRI do Kubernetes. #flashcard #contêineres

Qual abordagem gerencia ameaças de segurança e otimiza custos na computação em nuvem? :: Detecção de Anomalias na Nuvem #flashcard #nativodanuvem #segurança #gerenciamentodecustos

Qual Conta de Serviço padrão é usada para um Pod sem uma especificada no Kubernetes? :: Conta de serviço padrão #flashcard #kubernetes #segurança

O que significa SRE em operações de TI? :: Engenharia de Confiabilidade do Site #flashcard #nativodanuvem #personas

Como as especificações de contêineres são semelhantes em Deployments e StatefulSets do Kubernetes? :: Ambos usam especificações de modelo de pod idênticas. #flashcard #kubernetes #deployments #statefulsets

Qual é um problema chave com componentes fortemente acoplados em aplicativos monolíticos? :: Mudanças em uma área afetam todo o aplicativo; atualizações de bibliotecas podem quebrar a compatibilidade. #flashcard #monolítico

Como os aplicativos monolíticos eram tradicionalmente instalados? :: Diretamente no SO por meio de gerenciadores de pacotes como APT/YUM, compartilhando recursos. #flashcard #monolítico

Por que a arquitetura de aplicativos monolíticos é frágil? :: Acoplamento forte de servidor web, lógica de negócios e interfaces de dados complica mudanças. #flashcard #monolítico

O que é a Arquitetura de Microsserviços na Nuvem Nativa? :: Divide aplicativos em unidades independentes menores para facilitar gerenciamento e atualizações. #flashcard #nativodanuvem #microsserviços

Como o encapsulamento beneficia aplicativos Nativos da Nuvem? :: Isola dependências de microsserviços, reduzindo conflitos (por exemplo, /main vs /store). #flashcard #nativodanuvem #microsserviços

Como a Nuvem Nativa melhora a rede? :: Rede isolada para microsserviços evita conflitos de porta e simplifica a manutenção. #flashcard #nativodanuvem #rede

Que flexibilidade a camada de dados ganha na Nuvem Nativa? :: Microsserviços podem interagir independentemente com bancos de dados ou usar replicação/particionamento. #flashcard #nativodanuvem #microsserviços

O que envolve a Arquitetura Nativa da Nuvem? :: Microsserviços, conteinerização, CI/CD para escalabilidade e resiliência. #flashcard #nativodanuvem

Como a Nuvem Nativa garante alta disponibilidade? :: Usa redundância, autorreparação e escalabilidade para lidar com falhas. #flashcard #nativodanuvem

Quais benefícios práticos a Nuvem Nativa oferece? :: Manutenção, implantação, atualizações e mudanças frequentes mais fáceis por meio da modularidade. #flashcard #nativodanuvem

Como funciona a resiliência em aplicativos Nativos da Nuvem? :: Resiste a falhas por meio de redundância, failover e autorreparação (por exemplo, substituição de pods no Kubernetes). #flashcard #nativodanuvem

O que significa agilidade na Nuvem Nativa? :: Desenvolvimento, modificação e implantação rápidos usando microsserviços e CI/CD. #flashcard #nativodanuvem

O que é operabilidade em aplicativos Nativos da Nuvem? :: Implantação e gerenciamento fáceis com automação e IaC (por exemplo, Terraform). #flashcard #nativodanuvem

O que é observabilidade na Nuvem Nativa? :: Visão do estado do sistema por meio de logs, métricas e rastreamentos para diagnósticos. #flashcard #nativodanuvem #observabilidade

Qual é um mnemônico para as características da Nuvem Nativa? :: RAOO - "Guaxinins São Frequentemente Observadores" (Resiliência, Agilidade, Operabilidade, Observabilidade). #flashcard #nativodanuvem

Como a resiliência se manifesta na Nuvem Nativa? :: Aplicativos antecipam falhas com autorreparação (por exemplo, o Kubernetes reinicia pods com falha). #flashcard #nativodanuvem

Qual é o papel da automação na Nuvem Nativa? :: Reduz o esforço manual com ferramentas como Ansible/Terraform para implantações consistentes. #flashcard #nativodanuvem #devops

Qual é a diferença entre CI, CDelivery e CDeployment? :: CI: commits/testes frequentes; CDelivery: preparação de lançamento automatizada; CDeployment: lançamento automatizado em produção. #flashcard #devops

O que significa "Seguro por Padrão"? :: Sistemas projetados com segurança desde o início (por exemplo, Zero Trust, privilégio mínimo). #flashcard #nativodanuvem #segurança

Como a Nuvem Nativa otimiza velocidade, eficiência e custo? :: Usa autoescalonamento, sem servidor e design proativo para cargas dinâmicas. #flashcard #nativodanuvem #gerenciamentodecustos

O que é descoberta de serviços na Nuvem Nativa? :: Automatiza a detecção de serviços com ferramentas como o DNS do Kubernetes. #flashcard #nativodanuvem #rede

Quais são os quatro pilares da Arquitetura Nativa da Nuvem? :: Microsserviços, Conteinerização, DevOps, Entrega Contínua #flashcard #nativodanuvem

Como a Conteinerização beneficia a Nuvem Nativa? :: Encapsula aplicativos e dependências para consistência e isolamento. #flashcard #nativodanuvem #conteinerização

O que é DevOps na Nuvem Nativa? :: Combina Dev e Ops para automação, monitoramento e colaboração. #flashcard #nativodanuvem #devops

Qual é um mnemônico para os pilares da Nuvem Nativa? :: "Café da Manhã Entrega Cafeína Deliciosa" (Microsserviços, Conteinerização, DevOps, CD). #flashcard #nativodanuvem

O que é Autoescalonamento? :: Ajusta automaticamente os recursos com base na carga para eficiência de desempenho/custo. #flashcard #nativodanuvem #autoescalonamento

Quais são os três tipos de Autoescalonamento? :: Reativo (baseado em limiares), Agendado (picos previsíveis), Preditivo (baseado em IA). #flashcard #nativodanuvem #autoescalonamento

Qual é a diferença entre Escalonamento Vertical e Horizontal? :: Vertical: aumenta recursos do servidor; Horizontal: adiciona/remove instâncias. #flashcard #nativodanuvem #autoescalonamento

Quais são as ferramentas de autoescalonamento do Kubernetes? :: Cluster Autoscaler, HPA, VPA, KEDA (baseado em eventos). #flashcard #kubernetes #autoescalonamento

Qual é uma consideração chave para o autoescalonamento? :: Testes garantem desempenho; escalonamento horizontal aumenta a complexidade de compartilhamento de dados. #flashcard #nativodanuvem #autoescalonamento

O que é Computação Sem Servidor? :: Provedores gerenciam servidores; o código é executado em resposta a eventos, com autoescalonamento. #flashcard #nativodanuvem #semservidor

Quais são as características principais do Sem Servidor? :: Orientado a eventos, autoescalonamento (até zero), infraestrutura abstraída. #flashcard #nativodanuvem #semservidor

O que afeta os custos no Sem Servidor? :: Execução de código (execuções/duração) e autoescalonamento (recursos usados). #flashcard #nativodanuvem #semservidor #gerenciamentodecustos

O que é AWS Lambda? :: Plataforma FaaS que executa código em resposta a eventos com autoescalonamento. #flashcard #nativodanuvem #semservidor

Qual é um desafio no Sem Servidor? :: Dependência de fornecedor e latência de inicialização a frio após inatividade. #flashcard #nativodanuvem #semservidor

Quais são as opções de Sem Servidor de código aberto para o Kubernetes? :: Knative e OpenFaaS #flashcard #kubernetes #semservidor

O que é a CNCF? :: Organização neutra em relação a fornecedores sob a Linux Foundation, hospedando projetos nativos da nuvem. #flashcard #cncf

Quais são os níveis de maturidade dos projetos da CNCF? :: Sandbox, Incubação, Graduado #flashcard #cncf

O que faz o Comitê de Supervisão Técnica (TOC)? :: Avalia a maturidade do projeto com base na adoção e qualidade. #flashcard #cncf

Qual é o papel dos SIGs e TAGs na CNCF? :: SIGs focam em áreas do projeto; TAGs fornecem orientação de domínio. #flashcard #cncf

No que um Engenheiro DevOps se concentra? :: Faz a ponte entre Dev e Ops com automação e práticas nativas da nuvem. #flashcard #nativodanuvem #personas #devops

Qual é o papel de um Engenheiro de Confiabilidade do Site (SRE)? :: Garante confiabilidade em escala com SLAs/SLOs e gerenciamento de incidentes. #flashcard #nativodanuvem #personas

O que faz um Arquiteto de Nuvem? :: Projeta soluções em nuvem, otimizando custo e desempenho. #flashcard #nativodanuvem #personas

O que é único em um Engenheiro FinOps? :: Equilibra gastos na nuvem com velocidade e qualidade. #flashcard #nativodanuvem #personas #gerenciamentodecustos

O que são Padrões Abertos? :: Frameworks adotáveis livremente para colaboração e interoperabilidade. #flashcard #nativodanuvem #padrõesabertos

O que é a Iniciativa de Contêineres Abertos (OCI)? :: Define especificações de imagem e runtime de contêineres (por exemplo, runc). #flashcard #nativodanuvem #padrõesabertos

O que faz a Interface de Rede de Contêineres (CNI)? :: Simplifica a configuração de rede para o Kubernetes com plugins. #flashcard #kubernetes #rede #padrõesabertos

Qual é o propósito da Interface de Armazenamento de Contêineres (CSI)? :: Padroniza a integração de armazenamento para orquestração de contêineres. #flashcard #kubernetes #armazenamento #padrõesabertos

O que significa "Nativo da Nuvem"? :: Combina ambientes de nuvem e design nativo para escalabilidade e resiliência. #flashcard #nativodanuvem

Quais são as duas principais filosofias da CNCF? :: Arquitetura Nativa da Nuvem (melhores práticas) e Cultura (mudanças organizacionais). #flashcard #nativodanuvem #cncf

Quais são os principais benefícios das práticas Nativas da Nuvem? :: Resiliência, escalabilidade, automação, segurança e uso eficaz da nuvem. #flashcard #nativodanuvem

Como a Infraestrutura como Código (IaC) se relaciona com a Nuvem Nativa? :: Permite gerenciamento agnóstico de fornecedores com ferramentas como Terraform. #flashcard #nativodanuvem #devops

Executar um aplicativo na nuvem o torna Nativo da Nuvem? :: Não, requer melhores práticas, não apenas hospedagem na nuvem. #flashcard #nativodanuvem

Os contêineres são suficientes para tornar um aplicativo Nativo da Nuvem? :: Não, são um passo, mas são necessárias melhores práticas. #flashcard #nativodanuvem #conteinerização

Quais são as quatro práticas principais no desenvolvimento Nativo da Nuvem? :: Automação, Resiliência, Escalabilidade, Segurança por Padrão #flashcard #nativodanuvem

Quais desafios a Nuvem Nativa resolve? :: Erros de sistema único, limitações de recursos e vizinhos ruidosos. #flashcard #nativodanuvem

Qual é o papel da Linux Foundation na Nuvem Nativa? :: Padroniza o Linux, patrocina o Kubernetes, impulsiona o movimento de nuvem aberta. #flashcard #nativodanuvem #cncf

O que é a Cloud Native Computing Foundation (CNCF)? :: Promove tecnologia nativa da nuvem, supervisiona o Kubernetes, molda o ecossistema. #flashcard #nativodanuvem #cncf

Quando foi o primeiro grande marco do Kubernetes? :: Fevereiro de 2015, lançamento 1.0. #flashcard #kubernetes #cncf

O que é a certificação KCNA? :: Certificação de nível de entrada para ecossistemas Kubernetes e nativos da nuvem. #flashcard #kcna

Quais certificações avançadas seguem a KCNA? :: CKA (Administrador) e CKAD (Desenvolvedor de Aplicações). #flashcard #kcna

Quais tópicos a KCNA cobre? :: Kubernetes, segurança, telemetria/observabilidade (por exemplo, Prometheus). #flashcard #kcna

Por que a KCNA é uma base sólida? :: Prepara para certificações avançadas com conhecimento prático. #flashcard #kcna

Qual é o formato do exame KCNA? :: Múltipla escolha, nível de entrada, teórico. #flashcard #kcna

Como você pode fazer o exame KCNA? :: Presencial em centros de teste ou remotamente com supervisão. #flashcard #kcna

Por que a KCNA é uma boa escolha? :: Currículo diversificado, prepara para certificações avançadas, amigável para remoto. #flashcard #kcna

Onde você pode verificar atualizações do currículo KCNA? :: https://github.com/cncf/curriculum #flashcard #kcna

Como você pode se manter atualizado sobre notícias do exame KCNA? :: Siga a Linux Foundation e a CNCF nas redes sociais. #flashcard #kcna

Onde você agenda o exame KCNA? :: https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate #flashcard #kcna

Como você pode reduzir os custos do exame KCNA? :: Aguarde promoções, participe da KubeCon ou verifique códigos nas redes sociais. #flashcard #kcna

No que você deve focar para o exame KCNA? :: IaC, Velocidade, Eficiência e Custo em contextos Nativos da Nuvem. #flashcard #kcna

Qual é a fonte oficial para informações do exame KCNA? :: Repositório GitHub cncf/curriculum #flashcard #kcna

Como os contêineres transformaram a tecnologia? :: Revolucionaram infraestrutura, design de aplicativos e operações com ferramentas como o Kubernetes. #flashcard #contêineres

Qual é a base histórica da virtualização de mainframe (década de 1960)? :: IBM CP/CMS permitia múltiplas instâncias de SO virtualizadas em um mainframe. #flashcard #contêineres

O que é Chroot (1979) e suas limitações? :: Recurso Unix que isola processos; limitado por recursos compartilhados e sem processos de superusuário. #flashcard #contêineres

O que são FreeBSD Jails (2000)? :: Virtualização leve que isola recursos no FreeBSD. #flashcard #contêineres

O que são Solaris Zones e Partições Virtuais HP-UX (2000s)? :: Virtualização que isola recursos computacionais em Solaris e HP-UX. #flashcard #contêineres

Quais ingredientes modernos o Docker combinou? :: Namespaces Linux (isolamento) e cgroups (alocação de recursos). #flashcard #docker #contêineres

O que são Namespaces Linux (2002)? :: Isolam processos, redes, montagens, etc. (por exemplo, PID, NET, MNT). #flashcard #contêineres

O que são Grupos de Controle (cgroups, 2006)? :: Limitam, priorizam e controlam recursos; desenvolvidos pelo Google. #flashcard #contêineres

Como as Máquinas Virtuais se comparam aos contêineres? :: VMs usam hipervisores com sobrecarga de SO; contêineres compartilham o kernel do host. #flashcard #contêineres

O que tornou o Docker um divisor de águas em 2013? :: Combinou namespaces e cgroups em uma plataforma amigável. #flashcard #docker #contêineres

Por que os contêineres são preferidos às VMs? :: Leves, compartilham o kernel do SO host, reduzem a sobrecarga. #flashcard #contêineres

Qual é a arquitetura tradicional do Docker? :: Hardware → SO → Runtime Docker (containerd, runc). #flashcard #docker

O que o Docker Desktop fornece? :: Interface para Mac/Windows/Linux com CLI, Kubernetes e Extensões. #flashcard #docker

Qual é um benefício chave do Docker Desktop? :: Ambiente flexível e autocontido com redefinição fácil. #flashcard #docker

Como você executa um contêiner com o Docker Desktop? :: docker run -it ubuntu bash #flashcard #docker

Como você habilita o Kubernetes no Docker Desktop? :: Ative nas preferências para usar kubectl. #flashcard #docker #kubernetes

O que é uma imagem de contêiner? :: Pacote portátil, compatível com OCI, de software e dependências. #flashcard #contêineres

Qual é a diferença entre uma imagem de contêiner e um contêiner? :: Imagem é o blueprint; contêiner é uma instância em execução. #flashcard #contêineres

O que são tags em imagens de contêineres? :: Rótulos para versões/variantes (por exemplo, latest, v1.0). #flashcard #contêineres

Como as imagens de contêineres são estruturadas? :: Construídas como camadas; cada etapa cria uma camada reutilizável. #flashcard #contêineres

Quais são as desvantagens de muitas camadas de imagem? :: Aumento do tempo de construção, tamanho e complexidade. #flashcard #contêineres

O que é um Sistema de Arquivos Union no Docker? :: Mescla camadas de imagem em uma única visão com uma camada gravável. #flashcard #docker #contêineres

Qual é a diferença entre um digest e um ID de imagem? :: Digest identifica imagens de registro; ID de imagem é um hash de configuração local. #flashcard #contêineres

Como você salva uma imagem de contêiner? :: docker save exporta localmente com camadas e metadados. #flashcard #docker #contêineres

O que é um registro de contêineres? :: Serviço que hospeda/distribui imagens de contêineres com versionamento. #flashcard #contêineres

Como você valida uma instalação do Docker? :: docker version mostra detalhes do cliente/servidor. #flashcard #docker

Para que serve a imagem Funbox? :: Explore recursos como Nyan Cat com docker run -it --rm wernight/funbox. #flashcard #docker

O que fazem -i, -t e --rm no comando Docker run? :: -i: mantém a entrada aberta; -t: aloca terminal; --rm: remove automaticamente o contêiner. #flashcard #docker

Como você visualiza os estados dos contêineres? :: docker ps para em execução; docker ps -a para todos. #flashcard #docker

Qual é uma prática recomendada de segurança para executar contêineres? :: Executar como usuários não-root para reduzir a superfície de ataque. #flashcard #docker #segurança

Qual é a diferença entre -P e -p no comando Docker run? :: -P: publica todas as portas expostas aleatoriamente; -p: mapeia portas específicas. #flashcard #docker #rede

O que faz -d no comando Docker run? :: Desacopla o contêiner para rodar em segundo plano. #flashcard #docker

Como você explora um contêiner em execução? :: docker exec -it <container-id> bash #flashcard #docker

Como você faz alterações persistentes em um contêiner? :: Use volumes com -v /host/path:/container/path. #flashcard #docker #armazenamento

O que é orquestração de contêineres? :: Automatiza escalonamento, atualizações e disponibilidade para contêineres. #flashcard #orquestração

Quais são as características principais da orquestração de contêineres? :: Provisionamento, autorreparação, segurança, autoescalonamento. #flashcard #orquestração

Como a orquestração beneficia a implantação de aplicativos complexos? :: Padroniza a implantação, integra rede/armazenamento. #flashcard #orquestração

Quais são alguns orquestradores de contêineres? :: Nomad, OpenShift, Docker, Kubernetes #flashcard #orquestração

O que são CRDs no Kubernetes? :: Definições de Recursos Personalizados estendem a funcionalidade do Kubernetes. #flashcard #kubernetes #api

Quais são as duas áreas principais da arquitetura do Kubernetes? :: Plano de Controle (gerencia o cluster) e Nós (executam cargas de trabalho). #flashcard #kubernetes

Qual é o papel do runtime de contêiner? :: Baixo nível (runc) interage com namespaces; alto nível (containerd) gerencia o ciclo de vida. #flashcard #kubernetes #contêineres

O que faz o Kubelet? :: Executa nos nós, garante a saúde dos pods, lê especificações da API/YAML. #flashcard #kubernetes

O que é etcd? :: Armazenamento chave-valor distribuído para dados do cluster com consenso Raft. #flashcard #kubernetes #etcd

O que é o Servidor de API Kube? :: Ponto de acesso principal do cluster, expõe API RESTful, armazena dados no etcd. #flashcard #kubernetes #api

O que faz o Kube Scheduler? :: Atribui pods aos nós com base em recursos/restrições. #flashcard #kubernetes #agendamento

Qual é o papel do Kube Proxy? :: Gerencia a conectividade de rede (TCP/UDP/SCTP) nos nós. #flashcard #kubernetes #rede

O que é CoreDNS? :: Fornece resolução de DNS do cluster, executa como um Deployment. #flashcard #kubernetes #rede

O que o", "O que o Controller Manager lida? :: Executa loops de controle para impor o estado desejado. #flashcard #kubernetes

O que é o Cloud Controller Manager (CCM)? :: Faz a ponte entre o Kubernetes e provedores de nuvem para balanceadores de carga. #flashcard #kubernetes

Como funciona um cluster de Alta Disponibilidade (HA)? :: Múltiplos planos de controle, etcd em cluster, API Server balanceada por carga. #flashcard #kubernetes

O que é um Pod no Kubernetes? :: Unidade implantável menor com um ou mais contêineres. #flashcard #kubernetes #pods

Como os contêineres em um pod se comunicam? :: Via localhost; cada pod recebe um IP de cluster único. #flashcard #kubernetes #pods #rede

Como você cria um pod nginx? :: kubectl run nginx --image=nginx #flashcard #kubernetes #pods

Como você acessa um pod externamente? :: kubectl port-forward nginx 8080:80 #flashcard #kubernetes #pods #rede

Como você testa a comunicação entre pods? :: kubectl run curl --image=curlimages/curl --rm -it --restart=Never -- curl <nginx-pod-IP> #flashcard #kubernetes #pods #rede

Qual é a diferença entre kubectl create e apply? :: create falha se o recurso existe; apply atualiza recursos existentes. #flashcard #kubernetes #api

Como você gera YAML para um pod? :: kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx.yaml #flashcard #kubernetes #pods

Quais são as opções de restartPolicy? :: Always, Never, OnFailure #flashcard #kubernetes #pods

Como você combina múltiplos YAMLs em um arquivo? :: Use o separador --- #flashcard #kubernetes #api

O que é um contêiner sidecar? :: Contêiner secundário que melhora a funcionalidade do contêiner principal. #flashcard #kubernetes #pods

O que são namespaces do Kubernetes? :: Dividem os recursos do cluster para isolamento. #flashcard #kubernetes #namespaces

Por que usar namespaces? :: Multi-tenância, cotas, RBAC, organização. #flashcard #kubernetes #namespaces

Quais são os namespaces padrão comuns? :: kube-system, kube-public, kube-node-lease #flashcard #kubernetes #namespaces

Como você cria um namespace? :: kubectl create namespace my-namespace #flashcard #kubernetes #namespaces

Como você define um namespace padrão? :: kubectl config set-context --current --namespace=my-namespace #flashcard #kubernetes #namespaces

O que é um Deployment do Kubernetes? :: Gerencia atualizações de aplicativos com replicação e reversões. #flashcard #kubernetes #deployments

Como você escala um deployment? :: kubectl scale deployment/nginx --replicas=10 #flashcard #kubernetes #deployments

Como você atualiza um deployment? :: kubectl set image deployment/nginx nginx=nginx:stable #flashcard #kubernetes #deployments

Como você reverte um deployment? :: kubectl rollout undo deployment/nginx #flashcard #kubernetes #deployments

O que gerencia os ciclos de vida dos pods em um deployment? :: ReplicaSets #flashcard #kubernetes #deployments

Quais são os quatro principais tipos de serviço do Kubernetes? :: ClusterIP, NodePort, LoadBalancer, ExternalName #flashcard #kubernetes #serviços

O que é um serviço ClusterIP? :: IP apenas interno para comunicação no cluster. #flashcard #kubernetes #serviços #rede

O que é um serviço NodePort? :: Expõe uma porta estática em cada nó para acesso externo. #flashcard #kubernetes #serviços #rede

O que é um serviço LoadBalancer? :: Usa um balanceador de carga do provedor de nuvem para acesso externo. #flashcard #kubernetes #serviços #rede

O que é um Serviço Headless? :: Sem IP de cluster, resolve para IPs de pods. #flashcard #kubernetes #serviços #rede

O que é um Job do Kubernetes? :: Gerencia tarefas em lote, executa pods até a conclusão. #flashcard #kubernetes #tarefas

Qual é um exemplo de caso de uso de um Job? :: Calcular Pi com perl -Mbignum=bpi -wle "print bpi(2000)". #flashcard #kubernetes #tarefas

O que controlam completions e parallelism? :: completions: total de pods; parallelism: pods concorrentes. #flashcard #kubernetes #tarefas

O que é um CronJob do Kubernetes? :: Agenda Jobs em intervalos (por exemplo, * * * * *). #flashcard #kubernetes #cronjobs

O que acontece quando um CronJob é deletado? :: Para de agendar; Jobs/pods existentes persistem. #flashcard #kubernetes #cronjobs

Qual é a retenção padrão do histórico de jobs? :: 3 bem-sucedidos, 1 falhado #flashcard #kubernetes #cronjobs

O que são ConfigMaps? :: Armazenam dados de configuração não sensíveis para pods. #flashcard #kubernetes #configmaps

Como você cria um ConfigMap? :: kubectl create configmap color-configmap --from-literal=color=red #flashcard #kubernetes #configmaps

Como você usa um ConfigMap em um pod? :: envFrom: - configMapRef: name: color-configmap em YAML #flashcard #kubernetes #configmaps

O que é um ConfigMap imutável? :: Não pode ser modificado após a criação (immutable: true). #flashcard #kubernetes #configmaps

O que são Secrets do Kubernetes? :: Armazenam dados sensíveis (por exemplo, senhas) codificados em Base64. #flashcard #kubernetes #secrets

Como os Secrets diferem dos ConfigMaps? :: Secrets para dados sensíveis; ConfigMaps para não sensíveis. #flashcard #kubernetes #secrets #configmaps

Como você cria um Secret? :: kubectl create secret generic color-secret --from-literal=color=red #flashcard #kubernetes #secrets

Os Secrets são criptografados? :: Não, apenas codificados em Base64; armazenados no etcd. #flashcard #kubernetes #secrets #segurança

O que são Rótulos do Kubernetes? :: Pares chave-valor para marcar e organizar recursos. #flashcard #kubernetes #rótulos

Por que usar Rótulos? :: Organizar recursos, automatizar CI/CD, habilitar descoberta de serviços. #flashcard #kubernetes #rótulos

Como você cria um pod com rótulos? :: kubectl run nginx --image=nginx --labels="app=web,env=prod" #flashcard #kubernetes #rótulos #pods

Como você filtra por rótulos? :: kubectl get pods --selector app=web #flashcard #kubernetes #rótulos

O que interage com a API do Kubernetes? :: Usuários/ferramentas, monitoramento, componentes internos (scheduler, kubelet). #flashcard #kubernetes #api

Qual é o fluxo de processamento de uma solicitação de API? :: Solicitação → Roteamento → Autenticação → Autorização → Admissão → Validação → Manipulação → Resposta #flashcard #kubernetes #api

O que faz o Controlador de Admissão no fluxo da API? :: Impõe políticas e modifica recursos. #flashcard #kubernetes #api

O que são Definições de Recursos Personalizados (CRDs)? :: Estendem a API com novos tipos de recursos. #flashcard #kubernetes #api

Como o kubectl proxy simplifica o acesso à API? :: Permite acesso HTTP local sem autenticação manual. #flashcard #kubernetes #api

O que é a especificação OpenAPI no Kubernetes? :: Documenta endpoints e parâmetros da API. #flashcard #kubernetes #api

O que é RBAC no Kubernetes? :: Gerencia acesso usando papéis atribuídos a usuários/grupos. #flashcard #kubernetes #rbac #segurança

O que está em um arquivo kubeconfig? :: Detalhes do cluster e informações do usuário (por exemplo, certificados). #flashcard #kubernetes #rbac #segurança

Como o Kubernetes autentica usuários? :: Via certificados externos assinados por uma CA. #flashcard #kubernetes #rbac #segurança

O que é ClusterRole vs. ClusterRoleBinding? :: ClusterRole define permissões; ClusterRoleBinding as atribui. #flashcard #kubernetes #rbac #segurança

Como você verifica permissões com kubectl? :: kubectl auth can-i <verb> <resource> #flashcard #kubernetes #rbac #segurança

Como você cria um grupo de superusuários? :: Crie ClusterRole com todas as permissões e vincule ao grupo. #flashcard #kubernetes #rbac #segurança

O que faz o kube-scheduler? :: Atribui pods aos nós com base em recursos e restrições. #flashcard #kubernetes #agendamento

Quais são as três etapas no agendamento? :: Filtragem, Pontuação, Vinculação #flashcard #kubernetes #agendamento

Como você ignora o scheduler? :: Defina nodeName na especificação do pod. #flashcard #kubernetes #agendamento

O que é um scheduler personalizado? :: Usa lógica personalizada via schedulerName na especificação do pod. #flashcard #kubernetes #agendamento

Como funcionam os NodeSelectors? :: Usam rótulos para posicionamento de pods direcionado. #flashcard #kubernetes #agendamento

O que é armazenamento efêmero no Kubernetes? :: Armazenamento temporário que não persiste após reinicializações de pods. #flashcard #kubernetes #armazenamento

O que é um volume emptyDir? :: Criado vazio; persiste em falhas de contêineres, mas não em exclusões de pods. #flashcard #kubernetes #armazenamento

O que é armazenamento persistente no Kubernetes? :: Armazenamento que persiste além do ciclo de vida do pod (por exemplo, PVs, PVCs). #flashcard #kubernetes #armazenamento

O que são Classes de Armazenamento, PVs e PVCs? :: Classes de Armazenamento definem tipos; PVs são provisionados; PVCs solicitam armazenamento. #flashcard #kubernetes #armazenamento

O que é provisionamento dinâmico? :: PVC cria automaticamente PV contra uma Classe de Armazenamento. #flashcard #kubernetes #armazenamento

Para que são usados os StatefulSets? :: Gerenciam aplicativos stateful com identidades e armazenamento estáveis. #flashcard #kubernetes #statefulsets

Como os StatefulSets diferem dos Deployments? :: StatefulSets fornecem identidades de pod únicas e PVCs. #flashcard #kubernetes #statefulsets #deployments

O que é um serviço headless em StatefulSets? :: Sem IP de cluster, fornece nomes DNS estáveis. #flashcard #kubernetes #statefulsets #serviços

Como os pods de StatefulSet são nomeados? :: Sequencialmente com o prefixo do nome do StatefulSet. #flashcard #kubernetes #statefulsets

O que acontece com os PVCs quando um StatefulSet é deletado? :: PVCs e PVs persistem, reutilizáveis quando recriados. #flashcard #kubernetes #statefulsets #armazenamento

O que fazem as Políticas de Rede? :: Controlam a comunicação de pods permitindo/negando tráfego. #flashcard #kubernetes #rede

Qual é o papel do plugin CNI nas Políticas de Rede? :: Impõe regras de rede para controle de tráfego. #flashcard #kubernetes #rede #padrõesabertos

O que acontece sem Políticas de Rede? :: Pods podem enviar/receber tráfego de qualquer fonte. #flashcard #kubernetes #rede

Como várias Políticas de Rede se combinam? :: Efeitos são aditivos, criando regras cumulativas. #flashcard #kubernetes #rede

Qual é a diferença entre PDB e Replicas? :: Replicas garantem pods em execução; PDBs limitam interrupções. #flashcard #kubernetes #pdb

O que são interrupções voluntárias? :: Ações planejadas (por exemplo, manutenção) que podem encerrar pods. #flashcard #kubernetes #pdb

Como funciona um PDB durante o esvaziamento de um nó? :: Impede a expulsão abaixo do mínimo de pods disponíveis. #flashcard #kubernetes #pdb

O que são Contextos de Segurança no Kubernetes? :: Definem controle de acesso/privilégios para pods/contêineres. #flashcard #kubernetes #segurança

O que faz allowPrivilegeEscalation: false? :: Impede que contêineres obtenham privilégios elevados. #flashcard #kubernetes #segurança

O que substituiu as Políticas de Segurança de Pod no Kubernetes 1.25? :: Controladores de Admissão #flashcard #kubernetes #segurança

O que são Kyverno e OPA Gatekeeper? :: Ferramentas para aplicação de políticas personalizadas via controladores de admissão. #flashcard #kubernetes #segurança

Quais são os 4C’s da Segurança Nativa da Nuvem? :: Nuvem, Cluster, Contêiner, Código #flashcard #nativodanuvem #segurança

O que é Helm no Kubernetes? :: Simplifica o gerenciamento de aplicativos com Helm Charts. #flashcard #kubernetes #helm

Como você cria um Helm Chart? :: helm create flappy-app #flashcard #kubernetes #helm

Como você implanta um Helm Chart? :: helm install flappy-app <chart-file>.tgz #flashcard #kubernetes #helm

Qual é o benefício dos Helm Charts? :: Implantação, atualizações e escalonamento fáceis. #flashcard #kubernetes #helm

Qual é o papel de um service mesh? :: Gerencia a comunicação segura e confiável de microsserviços. #flashcard #nativodanuvem #servicemesh

Quais são as duas partes de um service mesh? :: Plano de Dados (proxies) e Plano de Controle (gerencia proxies). #flashcard #nativodanuvem #servicemesh

O que é um proxy sidecar vs. proxy de nó host? :: Sidecar se conecta a serviços; proxy de nó host executa no nível do nó. #flashcard #nativodanuvem #servicemesh

Quais são os benefícios principais dos service meshes? :: Segurança, controle de acesso, observabilidade, confiabilidade. #flashcard #nativodanuvem #servicemesh

O que é a Interface de Service Mesh (SMI)? :: API unificada para gerenciamento de tráfego e métricas. #flashcard #nativodanuvem #servicemesh #padrõesabertos

O que é observabilidade em sistemas nativos da nuvem? :: Determina o estado do sistema via telemetria (logs, métricas, rastreamentos). #flashcard #nativodanuvem #observabilidade

Quais são os três pilares da observabilidade? :: Logs, Métricas, Rastreamentos #flashcard #nativodanuvem #observabilidade

O que fazem os alertas na observabilidade? :: Fornecem avisos antecipados de anomalias ou falhas. #flashcard #nativodanuvem #observabilidade

Qual é o papel do OpenTracing e OpenTelemetry? :: Fornecem APIs/ferramentas para rastreamento e telemetria. #flashcard #nativodanuvem #observabilidade

Como os logs ajudam na observabilidade? :: Registram eventos para solução de problemas e análise. #flashcard #nativodanuvem #observabilidade

Quais são os tipos de métricas? :: Medidores, Contadores, Medidas, Histogramas #flashcard #nativodanuvem #observabilidade

O que os rastreamentos fornecem em um sistema distribuído? :: Visibilidade na latência de solicitações e gargalos. #flashcard #nativodanuvem #observabilidade

O que é Prometheus? :: Ferramenta CNCF de código aberto para monitoramento/alertas com PromQL. #flashcard #prometheus #observabilidade

Quais são as características principais do Prometheus? :: PromQL, nós autônomos, coleta de dados push-pull. #flashcard #prometheus #observabilidade

O que é Grafana? :: Plataforma para visualização de métricas do Prometheus e outras fontes. #flashcard #grafana #observabilidade

Como o Prometheus e o Grafana melhoram a observabilidade do Kubernetes? :: Oferecem monitoramento, visualização e alertas. #flashcard #kubernetes #prometheus #grafana #observabilidade

O que é o kube-prometheus-stack? :: Helm chart para configuração de Prometheus/Grafana no Kubernetes. #flashcard #kubernetes #prometheus #helm

Quais componentes são instalados com o kube-prometheus-stack? :: Prometheus Operator, Alertmanager, Node Exporter, Kube-State-Metrics, Grafana #flashcard #kubernetes #prometheus

Como você acessa a interface do Prometheus após a instalação? :: kubectl port-forward service/<prometheus-service> 9090:9090 #flashcard #kubernetes #prometheus

Qual é um exemplo de consulta PromQL para pods por namespace? :: sum by (namespace) (kube_pod_ips) #flashcard #prometheus #observabilidade

Como você importa painéis no Grafana? :: Use IDs (por exemplo, 15759) e selecione a fonte de dados Prometheus. #flashcard #grafana #observabilidade

Qual é um princípio chave do gerenciamento de custos nativo da nuvem? :: Flexibilidade para alocar recursos em várias nuvens. #flashcard #nativodanuvem #gerenciamentodecustos

Como você pode otimizar o uso de recursos? :: Revise o consumo, remova recursos não utilizados, projete para autoescalonamento. #flashcard #nativodanuvem #gerenciamentodecustos

O que são instâncias sob demanda? :: Recursos provisionados conforme necessário; flexíveis, mas caros. #flashcard #nativodanuvem #gerenciamentodecustos

O que são instâncias reservadas? :: Compromissos de longo prazo; econômicas, mas menos flexíveis. #flashcard #nativodanuvem #gerenciamentodecustos

O que são instâncias spot? :: Lance por recursos não utilizados; baratos, mas termináveis. #flashcard #nativodanuvem #gerenciamentodecustos

Qual é o risco da migração lift-and-shift para a nuvem? :: Provisionamento excessivo sem reavaliar as necessidades. #flashcard #nativodanuvem #gerenciamentodecustos

Como o autoescalonamento ajuda com os custos? :: Ajusta recursos à demanda, reduzindo custos ociosos. #flashcard #nativodanuvem #gerenciamentodecustos #autoescalonamento

O que faz o KubeCost? :: Monitora e gerencia custos específicos do Kubernetes. #flashcard #kubernetes #gerenciamentodecustos

Como a detecção de anomalias na nuvem ajuda no gerenciamento de custos? :: Identifica uso incomum para evitar excessos. #flashcard #nativodanuvem #gerenciamentodecustos

---

## Tags

#flashcard #nativodanuvem #monolítico #microsserviços #conteinerização #devops #entregacontínua #autoescalonamento #semservidor #cncf #kubernetes #observabilidade #prometheus #grafana #gerenciamentodecustos #docker #segurança #helm #servicemesh #kcna #rede #armazenamento #rbac #agendamento #pods #deployments #serviços #tarefas #cronjobs #configmaps #secrets #rótulos #api #orquestração #namespaces #pdb #statefulsets #padrõesabertos #personas