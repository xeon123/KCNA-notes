---
A solicitação não está autorizada porque as credenciais estão ausentes ou são inválidas.# Evolução da Arquitetura Nativa da Nuvem
- **Quais são as origens da Arquitetura Nativa da Nuvem?**  
?
  Evoluiu a partir dos desafios com aplicações monolíticas legadas.  
  #flashcard

- **Qual é o foco da Arquitetura Nativa da Nuvem?**  
?
  Melhora a disponibilidade, gestão de custos, eficiência e confiabilidade por meio de padrões de design.  
  #flashcard

- **Quais são os benefícios da Arquitetura Nativa da Nuvem?**  
?
  Permite sistemas fracamente acoplados, resilientes, gerenciáveis e observáveis com automação robusta para mudanças de alto impacto.  
  #flashcard

---

# Monolithic Applications: Issues and Challenges
- **Qual é um problema chave com componentes fortemente acoplados em apps monolíticos?**
?
  Mudanças em uma área podem afetar todo o app, e atualizações de bibliotecas podem quebrar a compatibilidade.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Como as aplicações monolíticas eram tradicionalmente instaladas?**  
?
  Diretamente no SO usando gerenciadores de pacotes como APT ou YUM, compartilhando recursos e aumentando a complexidade.  
  #flashcard

- **Por que a arquitetura de apps monolíticos é frágil?**  
?
  O forte acoplamento entre servidor web, lógica de negócios e interfaces de dados dificulta mudanças frequentes.  
  #flashcard

---

# Cloud-Native Approach: Advantages

- **O que é a Arquitetura de Microsserviços na Nuvem Nativa?**
?
  Divide os apps em unidades menores e independentes para facilitar gerenciamento, implantação e atualizações independentes.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Como o encapsulamento beneficia os apps Nativos da Nuvem?**
?
  Cada microsserviço inclui suas dependências, isolando recursos e reduzindo conflitos (ex.: /main vs /store).
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Como a Nuvem Nativa melhora a rede?**
?
  Rede isolada para microsserviços evita conflitos de portas e simplifica a manutenção.
  #flashcard
<!--SR:!2025-03-28,1,230-->

- **Que flexibilidade a camada de dados ganha na Nuvem Nativa?**
?
  Microsserviços podem interagir independentemente com bancos de dados ou usar replicação, fragmentação ou armazenamentos distribuídos (ex.: etcd).
  #flashcard
<!--SR:!2025-03-30,3,250-->

---

# Key Benefits of Cloud-Native Design
- **O que envolve a Arquitetura Nativa da Nuvem?**  
?
  Projetar apps com microsserviços, conteinerização e CI/CD para escalabilidade, resiliência e flexibilidade em ambientes de nuvem.  
  #flashcard

- **Como a Nuvem Nativa garante alta disponibilidade?**  
?
  Constrói sistemas com redundância, autorreparação e escalabilidade para lidar com falhas de componentes.  
  #flashcard

- **Quais benefícios práticos a Nuvem Nativa oferece?**  
?
  Manutenção, implantação, atualizações mais fáceis e mudanças frequentes e previsíveis com modularidade.  
  #flashcard

---

# Characteristics of Cloud-Native Applications
- **Como funciona a resiliência em apps Nativos da Nuvem?**  
?
  Projetados para suportar falhas com redundância, failover e autorreparação (ex.: Kubernetes substitui pods com falha).  
  #flashcard

- **O que significa agilidade na Nuvem Nativa?**
?
  Desenvolvimento, modificação e implantação rápidos usando microsserviços, CI/CD e automação.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **O que é operabilidade em apps Nativos da Nuvem?**
?
  Foco em implantação e gerenciamento fáceis com automação e IaC (ex.: Terraform).
  #flashcard
<!--SR:!2025-03-31,4,270-->

- **O que é observabilidade na Nuvem Nativa?**  
?
  Insight sobre o estado do sistema por meio de logs, métricas e rastreamentos para diagnóstico de problemas e desempenho.  
  #flashcard

- **Qual é um mnemônico para as características da Nuvem Nativa?**  
?
  RAOO - "Ratos Atentos Observam Obstáculos" (Resiliência, Agilidade, Operabilidade, Observabilidade).  
  #flashcard

---

# Key Cloud-Native Practices
- **Como a resiliência se manifesta na Nuvem Nativa?**  
?
  Apps antecipam falhas com autorreparação (ex.: Kubernetes reinicia pods com falha em um conjunto de réplicas).  
  #flashcard

- **Qual é o papel da automação na Nuvem Nativa?**  
?
  Reduz esforço manual com ferramentas como Ansible e Terraform para implantações rápidas e consistentes.  
  #flashcard

- **Qual é a diferença entre CI, CDelivery e CDeployment?**  
?
  CI: commits frequentes com testes. CDelivery: preparação automatizada de lançamentos. CDeployment: lançamento totalmente automatizado para produção.  
  #flashcard

- **O que significa "Seguro por Padrão"?**  
?
  Sistemas são projetados com segurança desde o início (ex.: Zero Trust, acesso com privilégio mínimo).  
  #flashcard

- **Como a Nuvem Nativa otimiza velocidade, eficiência e custo?**
?
  Usa escalabilidade automática, arquiteturas serverless e design proativo para cargas dinâmicas.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **O que é descoberta de serviços na Nuvem Nativa?**  
?
  Automatiza a detecção de serviços com ferramentas como DNS do Kubernetes e variáveis de ambiente.  
  #flashcard

---

# Key Pillars of Cloud-Native Architecture
- **Quais são os quatro pilares da Arquitetura Nativa da Nuvem?**  
?
  Microsserviços, Conteinerização, DevOps, Entrega Contínua.  
  #flashcard

- **Como a Conteinerização beneficia a Nuvem Nativa?**
?
  Encapsula apps e dependências para consistência, isolamento e implantação fácil.
  #flashcard
<!--SR:!2025-03-28,1,230-->

- **O que é DevOps na Nuvem Nativa?**  
?
  Combina Dev e Ops para automação, monitoramento e colaboração.  
  #flashcard

- **Qual é um mnemônico para os pilares da Nuvem Nativa?**
?
  "Manhã com Café Dá Coragem Diária" (Microsserviços, Conteinerização, DevOps, CD).
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Autoscaling
- **O que é Escalabilidade Automática?**
?
  Ajusta recursos automaticamente com base na demanda de carga para desempenho e eficiência de custo.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Quais são os três tipos de Escalabilidade Automática?**  
?
  Reativa (baseada em limites), Agendada (picos previsíveis), Preditiva (IA baseada em dados históricos).  
  #flashcard

- **Qual é a diferença entre Escalabilidade Vertical e Horizontal?**
?
  Vertical: aumenta recursos em um servidor. Horizontal: adiciona/remove instâncias (preferida na Nuvem Nativa).
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Quais são as ferramentas de escalabilidade automática do Kubernetes?**
?
  Cluster Autoscaler (nós), HPA (réplicas de pods), VPA (recursos de pods), KEDA (baseado em eventos).
  #flashcard

- **Qual é uma consideração chave para escalabilidade automática?**
?
  Testes garantem desempenho sob carga; escalabilidade horizontal aumenta a complexidade de compartilhamento de dados.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Serverless Computing
- **O que é Computação Serverless?**
?
  Provedores gerenciam servidores; usuários implantam código que roda em resposta a eventos, escalando automaticamente.
  #flashcard

- **Quais são as características centrais do Serverless?**  
?
  Orientado a eventos, escalabilidade automática (até zero), gerenciamento de infraestrutura abstraído.  
  #flashcard

- **O que afeta os custos no Serverless?**  
?
  Execução de código (execuções e duração) e escalabilidade automática (recursos usados).  
  #flashcard

- **O que é AWS Lambda?**  
?
  Uma plataforma FaaS que executa código em resposta a eventos, escalando automaticamente com gerenciamento de concorrência.  
  #flashcard

- **Qual é um desafio no Serverless?**
?
  Dependência de fornecedor devido a APIs proprietárias; inicializações a frio causam latência após inatividade.
  #flashcard
<!--SR:!2025-03-28,1,230-->

- **Quais são as opções Serverless de código aberto para Kubernetes?**
?
  Knative e OpenFaaS fornecem FaaS com escalabilidade a partir de zero.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Community and Governance
- **O que é o CNCF?**
?
  Uma organização neutra sob a Linux Foundation, hospedando projetos nativos da nuvem como o Kubernetes.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Quais são os níveis de maturidade dos projetos do CNCF?**  
?
  Sandbox (estágio inicial), Incubating (adoção), Graduated (maduro, critérios rigorosos).  
  #flashcard

- **O que faz o Comitê de Supervisão Técnica (TOC)?**  
?
  Avalia a maturidade dos projetos com base em adoção, engajamento e qualidade.  
  #flashcard

- **Qual é o papel dos SIGs e TAGs no CNCF?**
?
  SIGs focam em áreas de projetos (ex.: segurança do Kubernetes); TAGs fornecem orientação de domínio (ex.: armazenamento).
  #flashcard
<!--SR:!2025-03-30,3,250-->

---

# Cloud-Native Personas
- **No que um Engenheiro DevOps se concentra?**
?
  Faz a ponte entre Dev e Ops com automação, scripts e práticas nativas da nuvem.
  #flashcard
<!--SR:!2025-03-30,3,250-->

- **Qual é o papel de um Engenheiro de Confiabilidade de Site (SRE)?**
?
  Garante confiabilidade em escala com SLAs, SLOs e gerenciamento de incidentes.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **O que faz um Arquiteto de Nuvem?**  
?
  Projeta soluções de nuvem, otimizando custo, desempenho e interoperabilidade.  
  #flashcard

- **O que é único em um Engenheiro FinOps?**  
?
  Equilibra gastos na nuvem com velocidade e qualidade, colaborando entre TI e finanças.  
  #flashcard

---

# Open Standards
- **O que são Padrões Abertos?**  
?
  Frameworks livremente adotáveis que promovem colaboração e interoperabilidade, evitando dependência de fornecedores.  
  #flashcard

- **O que é a Open Container Initiative (OCI)?**  
?
  Estabelecida em 2015, define especificações de imagens e runtime de contêineres (ex.: runc).  
  #flashcard

- **O que faz a Interface de Rede de Contêineres (CNI)?**
?
  Simplifica a configuração de redes para Kubernetes com compatibilidade de plugins.
  #flashcard
<!--SR:!2025-03-30,3,250-->

- **Qual é o propósito da Interface de Armazenamento de Contêineres (CSI)?**  
?
  Padroniza a integração de armazenamento para orquestração de contêineres (ex.: Rook).  
  #flashcard

---

# What is Cloud Native?
- **O que significa "Nativo da Nuvem"?**  
?
  Combina "nuvem" (ambientes públicos, privados, híbridos) e "nativo" (projetado para escalabilidade, resiliência, gerenciabilidade). É um conjunto de filosofias, não um estado binário.  
  #flashcard

- **Quais são as duas principais filosofias do CNCF?**  
?
  1. Arquitetura Nativa da Nuvem (melhores práticas para diversos ambientes de nuvem). 2. Cultura Nativa da Nuvem (mudanças organizacionais para design/implementação).  
  #flashcard

---

# Benefits of Cloud Native
- **Quais são os principais benefícios das práticas Nativas da Nuvem?**  
?
  Uso eficaz da infraestrutura de nuvem, resiliência, escalabilidade, automação, segurança e progressão além da hospedagem básica na nuvem.  
  #flashcard

- **Como a Infraestrutura como Código (IaC) se relaciona com o Nativo da Nuvem?**  
?
  Ferramentas como Terraform permitem gerenciamento agnóstico de fornecedor, alinhado aos princípios da Nuvem Nativa.  
  #flashcard

---

# Misconceptions About Cloud Native
- **Executar um app na nuvem o torna Nativo da Nuvem?**  
?
  Não, requer melhores práticas, não apenas hospedagem na nuvem.  
  #flashcard

- **Contêineres são suficientes para tornar um app Nativo da Nuvem?**
?
  Não, são um passo em direção a isso, mas as melhores práticas devem ser seguidas.
  #flashcard
<!--SR:!2025-03-28,1,230-->

---

# Key Practices in Cloud Native Development
- **Quais são as quatro práticas chave no desenvolvimento Nativo da Nuvem?**  
?
  1. Automação (configuração/entrega). 2. Resiliência (lidar com falhas). 3. Escalabilidade (adaptar à demanda). 4. Segurança por Padrão (medidas robustas).  
  #flashcard

---

# Challenges Addressed by Cloud Native
- **Quais desafios o Nativo da Nuvem resolve?**  
?
  Erros em sistemas únicos, limitações de recursos do host e "vizinhos barulhentos" em ambientes compartilhados.  
  #flashcard

---

# Ecosystem and Governance
- **Qual é o papel da Linux Foundation no Nativo da Nuvem?**
?
  Fundada em 2000, padroniza o Linux, patrocina projetos como Kubernetes e impulsiona o movimento de nuvem aberta.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **O que é a Cloud Native Computing Foundation (CNCF)?**  
?
  Fundada em 2015, promove tecnologia nativa da nuvem, supervisiona projetos como Kubernetes e molda o ecossistema.  
  #flashcard

- **Quando foi o primeiro marco importante do Kubernetes?**  
?
  Fevereiro de 2015, com o lançamento 1.0.  
  #flashcard

---

# KCNA Exam Overview
- **O que é a certificação KCNA?**  
?
  Uma certificação de nível básico para ecossistemas Kubernetes e nativos da nuvem, um portal para CKA e CKAD.  
  #flashcard

- **Quais certificações avançadas seguem o KCNA?**  
?
  CKA (Certified Kubernetes Administrator) e CKAD (Certified Kubernetes Application Developer).  
  #flashcard

---

# Highlights of KCNA
- **Quais tópicos o KCNA cobre?**  
?
  Kubernetes, segurança (leva ao CKS), telemetria/observabilidade (Prometheus) e mais.  
  #flashcard

- **Por que o KCNA é uma base sólida?**
?
  Prepara você para certificações avançadas com conhecimento prático alinhado à carreira.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# KCNA Exam Details
- **Qual é o formato do exame KCNA?**
?
  Múltipla escolha, nível básico, teórico.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Como você pode fazer o exame KCNA?**
?
  Presencialmente em centros de teste ou remotamente com monitoramento por proctor.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Why Choose KCNA?
- **Por que o KCNA é uma boa escolha?**
?
  Currículo diversificado, prepara para certificações avançadas, prático para papéis nativos da nuvem e acessível remotamente.
  #flashcard
<!--SR:!2025-03-31,4,270-->

- **Onde você pode verificar atualizações do currículo do KCNA?**
?
  https://github.com/cncf/curriculum (inclui GitOps, Prometheus).
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Pro Tips for KCNA
- **Como você pode se manter atualizado sobre notícias do exame KCNA?**  
?
  Siga a Linux Foundation e o CNCF nas redes sociais para promoções, eventos e atualizações.  
  #flashcard

- **Onde você agenda o exame KCNA?**  
?
  [Kubernetes Cloud Native Associate (KCNA)](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate). Use o código de desconto: DIVEINTO30.  
  #flashcard

- **Como você pode reduzir os custos do exame KCNA?**
?
  Espere por promoções (até 40-45% de desconto), participe do KubeCon/CloudNativeCon ou verifique códigos nas redes sociais.
  #flashcard
<!--SR:!2025-03-30,3,250-->

- **No que você deve focar para o exame KCNA?**  
?
  Infraestrutura como Código (IaC), Velocidade, Eficiência e Custo em contextos Nativos da Nuvem.  
  #flashcard

- **Qual é a fonte oficial para informações do exame KCNA?**  
?
  O repositório GitHub cncf/curriculum, atualizado pelo CNCF.  
  #flashcard

---

# Introduction to Containers
- **Como os contêineres transformaram a tecnologia?**
?
  Revolucionaram a infraestrutura, design de apps, desenvolvimento e operações, permitindo o gerenciamento eficiente de milhares de contêineres com ferramentas como Kubernetes.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Qual é a base histórica da virtualização de mainframe (década de 1960)?**  
?
  O IBM CP/CMS permitiu que múltiplos usuários executassem instâncias de SO virtualizadas em um único mainframe, marcando o início das arquiteturas de VM.  
  #flashcard

- **O que é Chroot (1979) e suas limitações?**  
?
  Recurso Unix que isola processos alterando o diretório raiz; limitado por recursos compartilhados (ex.: hostname, IP) e incapacidade de executar processos de superusuário.  
  #flashcard

- **O que são FreeBSD Jails (2000)?**
?
  Virtualização leve que isola recursos do sistema no FreeBSD, semelhante ao Docker, mas complexo e menos adotado.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **O que são Solaris Zones e HP-UX Virtual Partitions (década de 2000)?**  
?
  Tecnologias de virtualização que isolam recursos computacionais; Solaris Zones dividiam o SO, HP-UX segmentava sistemas logicamente.  
  #flashcard

- **Quais são os ingredientes modernos que o Docker combinou?**  
?
  Namespaces do Linux (isolamento de processos) e cgroups (alocação de recursos) para conteinerização leve.  
  #flashcard

- **O que são Linux Namespaces (2002)?**
?
  Isolam processos, redes, montagens, hostnames, etc., com tipos como PID, NET, MNT, UTS, IPC, USER.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **O que são Control Groups (cgroups, 2006)?**  
?
  Limitam, priorizam, contabilizam e controlam recursos; desenvolvidos pelo Google, integrados ao Linux em 2008.  
  #flashcard

- **Como as Máquinas Virtuais se comparam aos contêineres?**
?
  VMs (ex.: VMware ESXi) usam hipervisores para isolamento, mas têm sobrecarga de SO e segmentação de recursos; contêineres compartilham o kernel do host.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **O que fez do Docker um divisor de águas em 2013?**  
?
  Combinou namespaces e cgroups em uma plataforma amigável, simplificando a conteinerização escalável.  
  #flashcard

- **Por que os contêineres são preferidos às VMs?**
?
  Leves, compartilham o kernel do SO host, reduzindo sobrecarga e maximizando o uso de recursos.
  #flashcard
<!--SR:!2025-03-31,4,270-->

---

# Docker Installation
- **Qual é a arquitetura tradicional do Docker?**  
?
  Hardware → SO → Runtime do Docker (containerd e runc); requer kernel Linux 3.1+.  
  #flashcard

- **O que o Docker Desktop oferece?**  
?
  Interface para Mac, Windows, Linux; executa uma VM/subsistema oculto para o runtime do Docker, com CLI, Kubernetes e Extensões.  
  #flashcard

- **Qual é um benefício chave do Docker Desktop?**
?
  Ambiente flexível e autocontido com reinicialização fácil para desenvolvimento e testes.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Como você executa um contêiner com o Docker Desktop?**  
?
  `docker run -it ubuntu bash` abre um terminal interativo do Ubuntu.  
  #flashcard

- **Como você habilita o Kubernetes no Docker Desktop?**  
?
  Ative-o nas preferências para usar comandos `kubectl`.  
  #flashcard

---

# Container Images
- **O que é uma imagem de contêiner?**
?
  Um pacote portátil, compatível com OCI, de software e dependências para execução consistente entre ambientes.
  #flashcard

- **Qual é a diferença entre uma imagem de contêiner e um contêiner?**  
?
  Imagem é o blueprint; contêiner é uma instância em execução (ex.: múltiplos contêineres nginx de uma imagem).  
  #flashcard

- **O que são tags em imagens de contêineres?**
?
  Rótulos para versões/variantes (ex.: latest, v1.0); `latest` é padrão, nem sempre o mais recente.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Como as imagens de contêineres são estruturadas?**  
?
  Construídas em camadas; cada etapa do processo de build cria uma camada compartilhada e reutilizável, com uma camada gravável no topo.  
  #flashcard

- **Quais são as desvantagens de muitas camadas de imagem?**
?
  Tempo de build aumentado, tamanho maior, caching mais lento, complexidade e limites do sistema de arquivos.
  #flashcard
<!--SR:!2025-03-28,1,230-->

- **O que é um Union File System no Docker?**  
?
  Mescla camadas de imagem em uma única visão; mudanças são armazenadas em uma camada gravável, otimizando espaço.  
  #flashcard

- **Qual é a diferença entre um digest e um ID de imagem?**
?
  Digest (SHA-256) identifica uma imagem em um registro; ID de imagem é um hash local da configuração JSON.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Como você salva uma imagem de contêiner?**
?
  `docker save` exporta localmente, incluindo camadas e metadados JSON.
  #flashcard
<!--SR:!2025-03-28,1,230-->

---

# Container Registry
- **O que é um registro de contêineres?**  
?
  Um serviço que hospeda e distribui imagens de contêineres com armazenamento, versionamento e controle de acesso.  
  #flashcard

---

# Running Containers
- **Como você valida uma instalação do Docker?**
?
  `docker version` mostra detalhes do cliente/servidor; usa containerd e runc.
  #flashcard

- **Para que serve a imagem Funbox?**  
?
  `docker pull wernight/funbox` e `docker run -it --rm wernight/funbox` para explorar recursos como Nyan Cat.  
  #flashcard

- **O que fazem `-i`, `-t` e `--rm` no comando Docker run?**
?
  `-i` mantém a entrada aberta, `-t` aloca um terminal, `--rm` remove automaticamente o contêiner ao sair.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Como você vê os estados dos contêineres?**  
?
  `docker ps` para contêineres em execução; `docker ps -a` para todos, incluindo os encerrados.  
  #flashcard

- **Qual é uma prática recomendada de segurança para executar contêineres?**  
?
  Execute como usuários não-root para reduzir a superfície de ataque.  
  #flashcard

---

# Container Networking Service and Volumes
- **Qual é a diferença entre `-P` e `-p` no comando Docker run?**  
?
  `-P` publica todas as portas expostas para portas aleatórias do host; `-p` mapeia portas específicas do host para o contêiner (ex.: `-p 12345:80`).  
  #flashcard

- **O que faz `-d` no comando Docker run?**
?
  Desanexa o contêiner para rodar em segundo plano.
  #flashcard
<!--SR:!2025-03-28,1,230-->

- **Como você explora um contêiner em execução?**  
?
  `docker exec -it <container-id> bash` abre um terminal dentro dele.  
  #flashcard

- **Como você faz mudanças persistentes em um contêiner?**
?
  Use volumes com `-v /caminho/host:/caminho/contêiner` para montar um diretório do host (ex.: para conteúdo Nginx).
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Container Orchestration
- **O que é orquestração de contêineres?**  
?
  Automatiza tarefas como escalabilidade, alocação de recursos, atualizações, monitoramento e garantia de alta disponibilidade para contêineres.  
  #flashcard

- **Quais são os recursos chave da orquestração de contêineres?**
?
  Provisionamento, autorreparação, exposição de serviços, segurança, gerenciamento de armazenamento, escalabilidade automática e funcionalidades estendidas (ex.: CRDs).
  #flashcard
<!--SR:!2025-03-28,1,230-->

- **Como a orquestração beneficia a implantação de apps complexos?**  
?
  Padroniza a implantação, integra redes/armazenamento/segurança, permitindo que os desenvolvedores foquem no código.  
  #flashcard

- **Quais são alguns orquestradores de contêineres?**  
?
  Nomad, OpenShift, Docker, Kubernetes (padrão ouro devido a recursos e adoção).  
  #flashcard

- **O que são CRDs no Kubernetes?**  
?
  Definições de Recursos Personalizados estendem o Kubernetes além dos recursos principais (ex.: gerenciamento de MySQL).  
  #flashcard

---

# Kubernetes Architecture
- **Quais são as duas áreas principais da arquitetura do Kubernetes?**  
?
  Plano de Controle (gerencia o cluster) e Nós (executam cargas de trabalho).  
  #flashcard

- **Qual é o papel do runtime de contêineres?**  
?
  Nível baixo (ex.: runc) interage com namespaces/cgroups; nível alto (ex.: containerd) gerencia o ciclo de vida do contêiner.  
  #flashcard

- **O que faz o Kubelet?**  
?
  Executa em todos os nós, garante que os pods estejam saudáveis, lê especificações da API ou arquivos YAML estáticos.  
  #flashcard

- **O que é etcd?**  
?
  Armazenamento distribuído de chave-valor para dados do cluster, usa consenso Raft, requer backups.  
  #flashcard

- **O que é o Kube API Server?**
?
  Ponto principal de acesso ao cluster, expõe API RESTful, armazena dados no etcd, roda como pod estático.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **O que faz o Kube Scheduler?**  
?
  Atribui pods a nós com base em recursos e restrições, roda como pod estático.  
  #flashcard

- **Qual é o papel do Kube Proxy?**  
?
  Gerencia conectividade de rede (TCP/UDP/SCTP) em cada nó como um DaemonSet.  
  #flashcard

- **O que é CoreDNS?**
?
  Fornece resolução DNS no cluster, roda como um Deployment.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **O que o Controller Manager gerencia?**  
?
  Executa loops de controle (ex.: controladores de Nó e Deployment) para impor o estado desejado no cluster. Garante que o estado desejado é igual ao estado actual.
  #flashcard

- **O que é o Cloud Controller Manager (CCM)?**
?
  Faz a ponte entre Kubernetes e provedores de nuvem para balanceadores de carga e rotas.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Como funciona um cluster de Alta Disponibilidade (HA)?**  
?
  Múltiplos planos de controle, etcd em cluster com Raft, API Server balanceada por carga.  
  #flashcard

---

# Pod Basics
- **O que é um Pod no Kubernetes?**  
?
  Menor unidade implantável, contém um ou mais contêineres compartilhando rede/armazenamento.  
  #flashcard

- **Como os contêineres em um pod se comunicam?**
?
  Via localhost; cada pod recebe um IP único no cluster.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Como você cria um pod nginx?**  
?
  `kubectl run nginx --image=nginx`.  
  #flashcard

- **Como você acessa um pod externamente?**
?
  Use redirecionamento de portas: `kubectl port-forward nginx 8080:80`.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Como você testa a comunicação entre pods?**
?
  Execute um pod curl: `kubectl run curl --image=curlimages/curl --rm -it --restart=Never -- curl <nginx-pod-IP>`.
  #flashcard
<!--SR:!2025-03-27,1,230-->

---

# Using YAML to Create Pods
- **Qual é a diferença entre `kubectl create` e `apply`?**
?
  `create` falha se o recurso existir; `apply` atualiza recursos existentes.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Como você gera YAML para um pod?**  
?
  `kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx.yaml`.  
  #flashcard

- **Quais são as opções de restartPolicy?**
?
  Always (padrão), Never, OnFailure.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Como você combina múltiplos YAMLs em um arquivo?**  
?
  Use o separador `---`: `{ cat nginx.yaml; echo "---"; cat ubuntu.yaml; } > combined.yaml`.  
  #flashcard

- **O que é um contêiner sidecar?**
?
  Um contêiner secundário em um pod que aprimora a funcionalidade do contêiner principal.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Namespaces
- **O que são namespaces no Kubernetes?**  
?
  Dividem recursos do cluster para isolamento (ex.: usuários, apps, projetos).  
  #flashcard

- **Por que usar namespaces?**  
?
  Multi-tenancy, cotas de recursos, limites de alcance, RBAC, organização.  
  #flashcard

- **Quais são os namespaces padrão comuns?**  
?
  kube-system, kube-public, kube-node-lease.  
  #flashcard

- **Como você cria um namespace?**  
?
  `kubectl create namespace meu-namespace`.  
  #flashcard

- **Como você define um namespace padrão?**  
?
  `kubectl config set-context --current --namespace=meu-namespace`.  
  #flashcard

---

# Deployments and ReplicaSets
- **O que é um Deployment no Kubernetes?**  
?
  Gerencia atualizações de apps declarativamente com replicação, atualizações contínuas e reversões.  
  #flashcard

- **Como você escala um deployment?**  
?
  `kubectl scale deployment/nginx --replicas=10`.  
  #flashcard

- **Como você atualiza um deployment?**  
?
  `kubectl set image deployment/nginx nginx=nginx:stable`.  
  #flashcard

- **Como você reverte um deployment?**
?
  `kubectl rollout undo deployment/nginx`.
  #flashcard
<!--SR:!2025-03-31,4,270-->

- **O que gerencia o ciclo de vida dos pods em um deployment?**  
?
  ReplicaSets garantem que o número desejado de pods esteja em execução.  
  #flashcard

---

# Services
- **Quais são os quatro tipos principais de serviços no Kubernetes?**  
?
  ClusterIP (padrão), NodePort, LoadBalancer, ExternalName.  
  #flashcard

- **O que é um serviço ClusterIP?**  
?
  IP apenas interno para comunicação no cluster (ex.: nginx.default.svc.cluster.local).  
  #flashcard

- **O que é um serviço NodePort?**  
?
  Expõe uma porta estática em cada nó (30000-32767) para acesso externo.  
  #flashcard

- **O que é um serviço LoadBalancer?**  
?
  Usa um balanceador de carga do provedor de nuvem para acesso externo.  
  #flashcard

- **O que é um Serviço Headless?**  
?
  Sem IP de cluster, resolve diretamente para IPs de pods (clusterIP: None).  
  #flashcard

---

# Kubernetes Jobs
- **O que é um Job no Kubernetes?**  
?
  Gerencia tarefas em lote, executa pods até a conclusão, tenta novamente em caso de falha.  
  #flashcard

- **Qual é um exemplo de caso de uso de um Job?**  
?
  Calcular Pi: `perl -Mbignum=bpi -wle "print bpi(2000)"`.  
  #flashcard

- **O que controlam `completions` e `parallelism`?**
?
  `completions`: total de pods a executar; `parallelism`: pods simultâneos.
  #flashcard
<!--SR:!2025-03-27,1,230-->

---

# Kubernetes CronJobs
- **O que é um CronJob no Kubernetes?**  
?
  Agenda Jobs em intervalos (ex.: `* * * * *` para cada minuto).  
  #flashcard

- **O que acontece quando um CronJob é deletado?**  
?
  Para de agendar, mas Jobs/pods existentes persistem a menos que sejam manualmente deletados.  
  #flashcard

- **Qual é a retenção padrão do histórico de jobs?**  
?
  3 bem-sucedidos, 1 falhado (successfulJobsHistoryLimit, failedJobsHistoryLimit).  
  #flashcard

---

# ConfigMaps
- **O que são ConfigMaps?**  
?
  Armazenam dados de configuração não sensíveis (ex.: variáveis de ambiente) para pods.  
  #flashcard

- **Como você cria um ConfigMap?**  
?
  `kubectl create configmap color-configmap --from-literal=color=red`.  
  #flashcard

- **Como você usa um ConfigMap em um pod?**  
?
  Adicione `envFrom: - configMapRef: name: color-configmap` no YAML.  
  #flashcard

- **O que é um ConfigMap imutável?**
?
  Não pode ser modificado após a criação (immutable: true, Kubernetes 1.21+).
  #flashcard
<!--SR:!2025-03-30,3,250-->

---

# Secrets in Kubernetes
- **O que são Segredos no Kubernetes?**
?
  Armazenam dados sensíveis (ex.: senhas, chaves) codificados em Base64.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Como os Segredos diferem dos ConfigMaps?**  
?
  Segredos são para dados sensíveis; ConfigMaps são para configuração não sensível.  
  #flashcard

- **Como você cria um Segredo?**
?
  `kubectl create secret generic color-secret --from-literal=color=red`.
  #flashcard

- **Os Segredos são criptografados?**
?
  Não, apenas codificados em Base64; armazenados no etcd, exigindo acesso seguro.
  #flashcard

---

# Labels in Kubernetes
- **O que são Rótulos no Kubernetes?**  
?
  Pares chave-valor para marcar e organizar recursos para agrupamento/filtragem.  
  #flashcard

- **Por que usar Rótulos?**  
?
  Organizar recursos, automatizar CI/CD, habilitar descoberta de serviços, definir políticas de rede.  
  #flashcard

- **Como você cria um pod com rótulos?**  
?
  `kubectl run nginx --image=nginx --labels="app=web,env=prod"`.  
  #flashcard

- **Como você filtra por rótulos?**  
?
  `kubectl get pods --selector app=web` ou `-l env=prod`.  
  #flashcard

---

# Kubernetes API
- **O que interage com a API do Kubernetes?**
?
  Usuários/ferramentas (kubectl, Helm), ferramentas de monitoramento, componentes internos (scheduler, kubelet, controller manager).
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Qual é o fluxo de processamento de uma requisição da API?**  
?
  Chegada da Requisição → Correspondência de Rota → Autenticação → Autorização → Controlador de Admissão → Validação → Tratamento → Resposta.  
  #flashcard

- **O que faz o Controlador de Admissão no fluxo da API?**  
?
  Impõe políticas (ex.: cotas) e modifica recursos antes da aceitação.  
  #flashcard

- **O que são Definições de Recursos Personalizados (CRDs)?**  
?
  Estendem a API com novos tipos de recursos (ex.: gerenciamento de MySQL).  
  #flashcard

- **Como o `kubectl proxy` simplifica o acesso à API?**  
?
  Permite acesso HTTP local (localhost:8001) sem autenticação manual.  
  #flashcard

- **O que é a especificação OpenAPI no Kubernetes?**  
?
  Documenta endpoints da API, parâmetros e respostas (ex.: via localhost:8001/openapi/v2).  
  #flashcard

---

# Role-Based Access Control (RBAC)
- **O que é RBAC no Kubernetes?**
?
  Gerencia acesso a recursos usando papéis atribuídos a usuários/grupos para controle refinado.
  #flashcard

- **O que contém um arquivo kubeconfig?**  
?
  Detalhes do cluster (URL do servidor API, dados CA) e informações do usuário (nome de usuário, certificados).  
  #flashcard

- **Como o Kubernetes autentica usuários?**  
?
  Via certificados externos (CN=Usuário, O=Grupo) assinados por uma CA, não gerenciados internamente.  
  #flashcard

- **O que é um ClusterRole vs. ClusterRoleBinding?**  
?
  ClusterRole define permissões em todo o cluster; ClusterRoleBinding as atribui a usuários/grupos.  
  #flashcard

- **Como você verifica permissões com kubectl?**  
?
  `kubectl auth can-i <verbo> <recurso>` (ex.: `kubectl auth can-i '*' '*'`).  
  #flashcard

- **Como você cria um grupo superusuário?**  
?
  Crie um ClusterRole (`--verb='*' --resource='*'`) e vincule-o a um grupo com ClusterRoleBinding.  
  #flashcard

---

# Scheduling Process
- **O que faz o kube-scheduler?**  
?
  Atribui pods a nós com base em recursos, afinidade, manchas e tolerâncias.  
  #flashcard

- **Quais são as três etapas no agendamento?**  
?
  Filtragem (remove nós inadequados), Pontuação (classifica nós), Binding (atribui pod ao nó).  
  #flashcard

- **Como você ignora o scheduler?**  
?
  Defina `nodeName` na especificação do pod (ex.: `nodeName: worker-2`).  
  #flashcard

- **O que é um scheduler personalizado?**  
?
  Especificado via `schedulerName` na especificação do pod; pode usar lógica personalizada (ex.: seleção aleatória de nó).  
  #flashcard

- **Como funcionam os NodeSelectors?**  
?
  Usam rótulos (ex.: `kubernetes.io/hostname: worker-1`) para posicionamento direcionado de pods.  
  #flashcard

---

# Storage
- **O que é armazenamento efêmero no Kubernetes?**
?
  Armazenamento temporário (ex.: emptyDir) que não persiste após reinícios de pods.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **O que é um volume emptyDir?**  
?
Volume **efémero** que é criado quando o Pod começa e é apagado quando o Pod é removido do nó.
  #flashcard

- **O que é armazenamento persistente no Kubernetes?**  
?
  Armazenamento que persiste além do ciclo de vida do pod (ex.: PVs, PVCs).  
  #flashcard

- **O que são Classes de Armazenamento, PVs e PVCs?**  
?
  Classes de Armazenamento definem tipos de armazenamento; PVs são armazenamento provisionado; PVCs são solicitações de usuários por armazenamento.  
  #flashcard

- **O que é provisionamento dinâmico?**  
?
Permite que persistent storage é criado automaticamento quando necessário, em vez de se criar manualmente PVs.
  #flashcard

---

# StatefulSets
- **Para que servem os StatefulSets?**
?
  Gerenciam apps com estado (ex.: bancos de dados) com identidades estáveis e armazenamento persistente.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Como os StatefulSets diferem dos Deployments?**  
?
  StatefulSets fornecem identidades únicas de pods e PVCs individuais; Deployments são sem estado.  
  #flashcard

- **O que é um serviço headless em StatefulSets?**
?
  Sem IP de cluster, fornece nomes DNS estáveis (ex.: nginx-0.nginx.svc.cluster.local).
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Como os pods de um StatefulSet são nomeados?**
?
  Sequencialmente com o prefixo do nome do StatefulSet (ex.: nginx-0, nginx-1).
  #flashcard
<!--SR:!2025-03-28,1,230-->

- **O que acontece com os PVCs quando um StatefulSet é deletado?**  
?
  PVCs e PVs persistem, reutilizáveis quando recriados.  
  #flashcard

---

# Network Policies
- **O que fazem as Políticas de Rede?**  
?
  Controlam a comunicação de pods permitindo/negando tráfego com base em rótulos, portas, etc.  
  #flashcard

- **Qual é o papel do plugin CNI nas Políticas de Rede?**  
?
  Impõe políticas configurando regras de rede para controle de tráfego.  
  #flashcard

- **O que acontece sem Políticas de Rede?**  
?
  Pods podem enviar/receber tráfego de qualquer fonte por padrão.  
  # bothcard

- **Como múltiplas Políticas de Rede se combinam?**
?
  Efeitos são aditivos, criando regras de tráfego cumulativas.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Pod Disruption Budgets (PDB)
- **Qual é a diferença entre PDB e Replicas?**  
?
  Réplicas garantem pods em execução; PDBs limitam disrupções para manter disponibilidade.  
  #flashcard

- **O que são disrupções voluntárias?**  
?
  Ações planejadas (ex.: manutenção, atualizações) que podem terminar pods.  
  #flashcard

- **Como um PDB funciona durante o esvaziamento de um nó?**
?
  Impede despejo se isso reduzir abaixo do mínimo de pods disponíveis (ex.: 2).
  #flashcard
<!--SR:!2025-03-27,1,230-->

---

# Security
- **O que são Contextos de Segurança no Kubernetes?**  
?
  Definem controle de acesso/privilégios para pods/contêineres (ex.: runAsUser, runAsGroup).  
  #flashcard

- **O que faz `allowPrivilegeEscalation: false`?**
?
  Impede que contêineres ganhem privilégios elevados.
  #flashcard
<!--SR:!2025-03-28,1,230-->

- **O que substituiu as Políticas de Segurança de Pods no Kubernetes 1.25?**  
?
  Controladores de Admissão impõem segurança no nível do cluster.  
  #flashcard

- **O que são Kyverno e OPA Gatekeeper?**
?
  Ferramentas para imposição de políticas personalizadas via controladores de admissão.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Quais são os 4C’s da Segurança Nativa da Nuvem?**
?
  Nuvem, Cluster, Contêiner, Código — camadas de segurança do amplo ao específico.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Helm and Helm Charts
- **O que é Helm no Kubernetes?**  
?
  Simplifica o gerenciamento de apps com Helm Charts (pacotes de recursos pré-configurados).  
  #flashcard

- **Como você cria um Helm Chart?**
?
  `helm create flappy-app`, personalize Chart.yaml e values.yaml.
  #flashcard
<!--SR:!2025-03-31,4,270-->

- **Como você implanta um Helm Chart?**  
?
  `helm install flappy-app <arquivo-chart>.tgz`.  
  #flashcard

- **Qual é o benefício dos Helm Charts?**
?
  Implantação, atualizações e escalabilidade fáceis, como APT/YUM para Kubernetes.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Service Meshes
- **Qual é o papel de uma malha de serviço?**  
?
  Gerencia comunicação segura e confiável entre microsserviços em arquiteturas complexas.  
  #flashcard

- **Quais são as duas partes de uma malha de serviço?**  
?
  Plano de Dados (proxies para tráfego) e Plano de Controle (gerencia proxies).  
  #flashcard

- **O que é um proxy sidecar vs. proxy de nó host?**
?
  Sidecar anexa a cada serviço; proxy de nó host roda no nível do nó.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **Quais são os principais benefícios das malhas de serviço?**  
?
  Segurança (TLS mútuo), controle de acesso, observabilidade, confiabilidade (limitação de taxa).  
  #flashcard

- **O que é a Interface de Malha de Serviço (SMI)?**  
?
  API unificada padrão para gerenciar tráfego, acesso e métricas entre malhas.  
  #flashcard

---

# Observability
- **O que é observabilidade em sistemas nativos da nuvem?**
?
  Capacidade de determinar o estado do sistema via telemetria (logs, métricas, rastreamentos, alertas).
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Quais são os três pilares da observabilidade?**  
?
  Logs (registros de eventos), Métricas (medidas quantitativas), Rastreamentos (acompanhamento de requisições).  
  #flashcard

- **O que fazem os alertas na observabilidade?**  
?
  Fornecem avisos antecipados de anomalias ou falhas para resolução proativa.  
  #flashcard

- **Qual é o papel do OpenTracing e OpenTelemetry?**  
?
  Operam na camada de app, fornecendo APIs/ferramentas para rastreamento e coleta de telemetria.  
  #flashcard

- **Como os logs ajudam na observabilidade?**  
?
  Registram eventos (erros, informações de depuração) para solução de problemas e análise de comportamento.  
  #flashcard

- **Quais são os tipos de métricas?**  
?
  Gauges (instantâneos), Contadores (cumulativos), Medidores (taxas de eventos), Histogramas (distribuições).  
  #flashcard

- **O que os rastreamentos fornecem em um sistema distribuído?**
?
  Visibilidade sobre latência de requisições, travessia de componentes e gargalos.
  #flashcard
<!--SR:!2025-03-27,1,230-->

---
# Prometheus and Grafana
**Q: O que é Prometheus?**  
?
Uma ferramenta de código aberto do CNCF para monitoramento/alerta com um modelo de dados multidimensional.
  #flashcard

**Q: Quais são os recursos chave do Prometheus?**  
?
Consultas PromQL, nós autônomos, coleta de dados push-pull, alertas integrados.
  #flashcard

**Q: O que é Grafana?**  
?
Uma plataforma de código aberto para visualizar métricas de fontes como Prometheus.
  #flashcard

**Q: Como Prometheus e Grafana aprimoram a observabilidade no Kubernetes?**  
?
Oferecem monitoramento, visualização e alertas para metas de tempo de atividade/desempenho.
  #flashcard

**Q: O que é o kube-prometheus-stack?**  
?
Um chart Helm que simplifica a configuração de Prometheus/Grafana no Kubernetes.
  #flashcard

**Q: Quais componentes são instalados com o kube-prometheus-stack?**  
?
Prometheus Operator, Alertmanager, Node Exporter, Kube-State-Metrics, Grafana.
  #flashcard

**Q: Como você acessa a interface do Prometheus após a instalação?**
?
`kubectl port-forward service/<serviço-prometheus> 9090:9090`.
  #flashcard
<!--SR:!2025-03-29,3,250-->

**Q: Qual é um exemplo de consulta PromQL para pods por namespace?**
?
`sum by (namespace) (kube_pod_ips)`.
  #flashcard
<!--SR:!2025-03-29,3,250-->

**Q: Como você importa dashboards no Grafana?**
?
Use IDs (ex.: 15759 para Métricas de Nó) e selecione Prometheus como fonte de dados.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

# Cost Management in Cloud-Native Solutions
**Q: Qual é um princípio chave do gerenciamento de custos nativo da nuvem?**  
?
Flexibilidade para alocar recursos em nuvens públicas/híbridas/privadas.
  #flashcard/cloudnative

**Q: Como você pode otimizar o uso de recursos?**  
?
Revise o consumo, remova recursos não utilizados, projete para escalabilidade automática.
  #flashcard/cloudnative

**Q: O que são instâncias sob demanda?**  
?
Recursos provisionados conforme necessário; flexíveis, mas caros se não gerenciados.
  #flashcard/cloudnative

**Q: O que são instâncias reservadas?**  
?
Compromissos de longo prazo com recursos; custo-efetivo, mas menos flexível.
  #flashcard/cloudnative

**Q: O que são instâncias spot?**
?
Oferta por recursos não utilizados; baratos, mas podem ser terminados a qualquer momento.
  #flashcard/cloudnative
<!--SR:!2025-03-29,3,250-->

**Q: Qual é o risco do lift-and-shift na migração para a nuvem?**  
?
Superprovisionamento de recursos sem reavaliar necessidades.
  #flashcard/cloudnative

**Q: Como a escalabilidade automática ajuda com custos?**
?
Ajusta recursos dinamicamente à demanda, reduzindo custos ociosos.
  #flashcard/cloudnative
<!--SR:!2025-03-27,1,230-->

**Q: O que faz o KubeCost?**  
?
Monitora e gerencia custos específicos do Kubernetes (código aberto e comercial).
  #flashcard/cloudnative

**Q: Como a detecção de anomalias na nuvem ajuda no gerenciamento de custos?**  
?
Identifica padrões de uso incomuns para evitar excessos de custo.
  #flashcard/cloudnative

---

# Tags
 #flashcards #nuvemnativa #monolitico #microsservicos #conteinerizacao #devops #entregacontinua #escalabilidadeautomatica #serverless #cncf #kubernetes #observabilidade #prometheus #grafana #gerenciamentodecustos #docker #seguranca #helm #malhadeservico #kcna #rede #armazenamento #rbac #agendamento #pods #deployments #servicos #jobs #cronjobs #configmaps #segredos #rotulos #api #orquestracao #namespaces #pdb #statefulsets #padroesabertos #personas:w
 