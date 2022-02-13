# Geek University

## Orquestração de Containers com Kubernetes
[Curso sobre Kubernetes (Udemy)](https://www.udemy.com/course/orquestracao-de-containers-com-kubernetes/)


## Kubernetes
Kubernetes (K8s) é um produto Open Source utilizado para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos em contêiner.

Durante o curso vamos explorar os principais conceitos e comandos para gerenciar o seu cluster K8s.

Assim como o Kubernetes existem outras ferramentas semelhantes no mercado como:
- Docker swarm
- Mesos

## Dicionários de palavras
[Lista das principais palavras](./docs/dictionary.md) encontradas no curso.

## Pré-requisitos
Certifique-se de ter instalado em sua máquina a ferramenta de gestão de containers, que nosso caso será o **Docker**.

- [Instalando Docker no Windows](https://docs.docker.com/desktop/windows/install/)

## Preparando o ambiente
Como projeto oper-source o Kubernetes (Google) passou a ter algumas variações da sua implementação inicial, como:
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [MicroK8s](https://microk8s.io/)
- [Kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)
- [K3S](https://k3s.io/)

Uma boa recomendação para uso local é o [Minikube](https://minikube.sigs.k8s.io/docs/start/) (utilizada no curso).

### Minikube
O Minikube agrupa todos os componentes do Kubernetes em um simples node. Então nossa máquina se transforma em um node completo contendo:
- kubelet
- kube-apiserver
- etcd
- node-controller
- replica-controller
- Container Runtime (Docker)

Seguindo com a configuração da ferramenta vamos fazer download do minikuke por [este link](https://minikube.sigs.k8s.io/docs/start/)

Na sequência precisamos baixar o [kubectl](https://kubernetes.io/docs/tasks/tools/).

Após baixar os arquivos vamos:

- Criar as pastas: `C:/Users/${meu_usuario}/apps/kube/`
- Inserir esse caminho nas **Variáveis de Ambiente do Windows**
  1. Variáveis do sistema operacional
  2. Chave de nome `Path`
  3. Editar valor
  4. Inserir nova entrada
  5. Colar o caminho e salvar
- Copiar a ferramenta **kubectl** para dentro da pasta
`C:/Users/${meu_usuario}/apps/kube/`

Após isso instalar o **Minikube** via executável.

Ao témino você deve receberá uma mensagem parecida com essa:
> Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

---

## Conceitos importantes
Nesta seção vamos aboardar alguns conceitos importantes dentro do aprendizado de Kubernetes, como:
- POD
- Replication Controller
- ReplicaSets
- Deployments

<img src="https://d33wubrfki0l68.cloudfront.net/5cb72d407cbe2755e581b6de757e0d81760d5b86/a9df9/docs/tutorials/kubernetes-basics/public/images/module_03_nodes.svg" height="430px">

### Conhecendo POD
POD no Kubernetes é a menor unidade de computação publicável em um computador que podemos criar e gerenciar.
Desta forma, em Kubernetes, um pod é uma instância única de uma aplicação.

<img src="https://d33wubrfki0l68.cloudfront.net/aecab1f649bc640ebef1f05581bfcc91a48038c4/728d6/images/docs/pod.svg" alt="Pod creation diagram" height="270px">

Sempre que precisamos escalar a nossa aplicação o Kubernetes cria novos POD's com a estrutura exata do POD existe e passa a distribuir a carga dentro dessas instâncias.

<img alt="" class="oy rb t u v ke aj c" width="276" height="237" role="presentation" src="https://miro.medium.com/max/276/1*LCxxB5Ow73mkeqeoUwWDcg.png" height="270px">

Em Kubernetes, a relação POD/Container é de 1:1, ou seja, para que cada 1 container temos 1 POD.

- Para **escalar up** sua aplicação criamos um novo POD.
- Parar **escalar down** você deleta o POD.

[Referência](https://kubernetes.io/docs/concepts/workloads/pods/)

**PODs Multi-Container**

Em alguns casos temos a necessidade de utilizar mais de um container para o mesmo POD (ex.: cache + aplicação).

Neste caso os containers se comunicam via localhost e fazem parte da mesma rede e podem acessar dados de armazenamento em conjunto.

Fazemos uso do **kubectl** para criar novos `pods` dentro do cluster, ex.:
```sh
$ kubectl run nginx --image nginx
```
Esse comando irá criar um **POD** chamado **`nginx`** no cluster* do Kubernetes com um container gerado a partir da imagem informada.

Na sequência podemos acompanhar a criação do nosso POD pelo comando.
```sh
$ kubectl get pods
```

Que irá nos monstrar:
- nome do pod
- status de execução
- total de containers
- reinicializações
- tempo de vida

### Prática POD

Removendo nosso cluster
```sh
$ minikube delete
```

Iniciando nosso Kubernetes
```sh
$ minikube start
```

Criar um pod contendo um container **nginx**
```sh
$ kubectl run nginx --image nginx
```

Verificando o status do novo pod
```sh
$ kubectl get pods
$ kubectl get pods -o wide
```

Mais detalhes do prod criado
```sh
$ kubectl describe pod nginx
```

### Definição do Kubernetes com YAML
Um arquivo de definição do Kubernetes é escrito na linguagem YAML e tem 4 níveis bases.
- **apiVersion**
  - Versão do objeto que estamos criando na API do Kubernetes (ex.: v1)
- **kind**
  - Tipo de objeto (ex.: Pod)
  - Podemos ter os tipos:
   - Pod
   - Service
   - ReplicaSet
   - Deployment
- **metadata**
  - Dados pertencentes ao objeto que está sendo criado
  - Alguns tipos de meta-dados
    - **name** - Nome único para o objeto
    - **namespace** - Nome comum utilizado para agrupar outros objetos
    - **uid** - Identificador único
    - **labels** - Mapa de chave/valor (string)
    - [continua...](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md#objects)
- **spec**
  - Está relacionado ao tipo (kind) que está sendo criado para este recurso, descreve os detalhes para criação.

### Prática YAML
Nessa sessão vamos detalhar na prática a criação de um recurso utilizando o yaml.

[Aqui](./examples/nginx.yaml) temos um arquivo yaml de exemplo que podemos utilizar de modelo para criar nossos pods.


Criando pod a partir de um arquivo yaml.
```sh
$ kubectl create -f ./examples/pod.yaml
```
ou
```sh
$ kubectl apply -f ./examples/pod.yaml
```

**Obs.:** O `create` e `apply` tem o mesmo efeito.

Removendo um pod.
```sh
$ kubectl delete pod nginx
```

### Replication Controller
O [replication controller](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/) garante que o número especificado de pod(s) continue(m) rodando a qualquer momento. Ou seja, que a quantidade configurada de pod(s) esteja(m) sempre rodando e disponível para receber uma carga de trabalho. Não importa se temos um cluster com 1 ou 1000 pod(s).

**Exemplo:**
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: aplicacao-rc
  labels:
    app: aplicacao
    type: frontend
spec:
  template:
    metadata:
      name: aplicacao-pod
      labels: aplicacao
      type: frontend
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replica: 2
```
Perceba que o conteúdo do elemento `template:` é basicamente a definição de um arquivo yaml de um [pod](./examples/nginx.yaml)

Para o atributo `replica` definimos a quantidade de pods que gostaríamos de manter rodando dentro do cluster.

Abaixo o comando para subir os pods utilizando a definição do **replication controller** (mesmo para subir o pod).
```sh
$ kubectl create -f ./examples/nginx-rc.yaml
```

### ReplicaSets
Assim como o [replication controller](#replication-controller) o **replica set** tem o objetivo de garantir o número definido de pods, assim como a disponilibidade deles para uso. 

É uma versão mais nova do replication controller, basicamente.

Enquanto o replica controller utiliza o `appVersion: v1` o replica set utiliza o valor `apiVersion: apps/v1`

**Exemplo:**
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
spec:
  template:
  replicas:
  selector:
    matchLabels:
      type: frontend
```
Percebe que no modelo de **replica-set** temos os parâmetros extras abaixo:
```sh
  selector:
    matchLabels:
      type: frontend
```
Sendo o atributo `selector:` responsável por selecionar o pod dentro do nosso cluster que recebá a configuração em questão.

Lembrando que o cluster pode ter vários pods rodando e o `select` nos ajuda a manter configuração desejada por tipo de `label`, neste exemplo (podemos utilizar outros campos como seletores).

Para obter informações sobre o replicaset basta utilizar o comando abaixo:
```sh
$ kubectl get replicaset
$ kubectl get replicasets
```
**obs.:** os dois comando tem o mesmo efeito.

Também podemos utilizar:
```sh
$ kubectl get pods
```

Em alguns momentos vamos ter vários pods rodando dentro do nosso cluster e rodar apenas o comando `kubectl get pods` pode não ajudar muito na visualização dos pods em execução.

Neste momento é importante utilizar as `labels` junto ao comando `get pods` para filtrar os `pods` de interesse.
```sh
$ kubectl get pods -l env=production
```

**Escalando sua aplicação**

Uma das formas de alterar o número de replicas é via arquivo de definição do Replication Controller/ReplicaSet ajustando o atributo `replicas`, de 2 para 4, por exemplo.

**De:**
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
spec:
  template:
  replicas: 4
  selector:
    matchLabels:
      type: backend
```

Após a alteração basta atualizar o arquivo de definição dentro do cluster do Kubernetes pelo comando abaixo:
```sh
$ kubectl replace -f rs.yaml
```
Então o Kubernetes irá atualizar o total de pods dentro do cluster.

Outra forma de alterar o total de replica/pods seria:
```sh
$ kubectl scale --replicas=8 replicaset aplicacao-rs
```

### Prática rc & rs
Aqui vamos praticar os conceitos vistos anteriormente:
- **replication controller**
- **replica set**

Comandos de apoio:
```sh
$ kubectl create -f arquivo.yaml
$ kubectl get replicaset
$ kubectl delete replicaset nome-do-replicaset
$ kubectl replace -f arquivo.yaml
$ kubectl scale --replicas=8 -f arquivo.yaml
```

### Deployments

### Prática deployments


