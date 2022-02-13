## Dicionário
Abaixo temos uma lista das principais palavras utilizadas no curso e uma breve descrição das mesmas.

### Node
Um node é uma maquina, física ou virtual, onde o Kubernetes está instalado.
No node criamos os containers (com docker) com as nossas aplicações.

### Cluster
Um cluster é um conjunto de nodes agrupados.
Quando provisionamos as nossas aplicação dentro de um cluster com 3 máquinas, por exemplo, o fato de ter uma falha em algum dos nodes faz com que o próprio Kubernetes redirecione as requisições para as outras máquinas.

### Master
O master é um node que é o verdadeiro responsável pela orquestração dos containers. Logo temos node master (principal) e node workers (onde roda nossas aplicações)

### API Server
A API Server funciona como um frontend para o Kubernetes, ou seja, é através dele que fazemos que gerenciamos usuários, dispositivos, interface de linha de comando e etc.

### etcd
É usado para armazenar dados, de forma distribuída, no formato chave/valor para configuração e gerenciamento dos clusters.
Podemos ter múltiplos clusters e cada cluster com seu master, e o etcd guardando dados de forma distribuída nos nodes.
Além disso os logs dos clusters são gravados aqui.

### Scheduler
O scheduler é responsável por distribuir o trabalho para os containers através dos múltiplos nodes.
Ele também busca novos containers criados e anexa eles aos nodes.

### Controller
É responsável pela orquestração de fato, ou seja, toma decisões quando um node falha ou apresenta problemas e pode ativar novos nodes para substituir os problemáticos.

### Container runtime
O container runtime faz interface com o software usado para criação de containers, no nosso caso o docker engine.

### kubelet
O agente responsável por checar se os containers estão sendo executados no node conforme o esperado. Temos o agente instalado em cada worker (node) do kubernetes.

### kubectl
Ao instalar o Kubernetes, teremos a disposição um programa de linha de comando chamado kubectl (Kube Control Tool) na qual iremos utilizar para executar comandos do kubernetes, como:
- Iniciar uma aplicação
- Exibir os dados da aplicação que está rodando
- Obter dados sobre os nós ativos
- Etc.

### Worker Master vs Worker Node

| Master | Worker |
| :---: | :---: |
| [kube-apiserver](#api-server) | [kubelet](#kubelet) |
| [etcd](#etcd) | - |
| [controller](#controller) | - |
| [scheduler](#scheduler) | - |
| - | [container runtime](#container-runtime) |