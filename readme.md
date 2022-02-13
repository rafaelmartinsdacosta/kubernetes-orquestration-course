# Geek University

## Orquestração de Containers com Kubernetes
[Curso sobre Kubernetes (Udemy)](https://www.udemy.com/course/orquestracao-de-containers-com-kubernetes/)


## Kubernetes
Kubernetes (K8s) é um produto Open Source utilizado para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos em contêiner.

Durante o curso vamos explorar os principais conceitos e comandos para gerenciar o seu cluster K8s.

Assim como o Kubernetes existem outras ferramentas semelhantes no mercado como:
- Docker swarm
- Mesos

**Guia para estudo**
- Leitura natural (cima p/ baixo)
- Sempre que encontrar um link:
  - Abra o link por uma nova aba
  - Estude-o pois será base para os próximos assuntos
  - Ao retornar, siga a leitura

## Dicionários de palavras
[Lista das principais palavras](./docs/dictionary.md) encontradas no curso.

## Pré-requisitos
[Aqui](./docs/environment.md) temos uma lista de itens que serão necessários para seguir nos estudos.

## Conceitos chaves
Separei alguns conceitos e práticas [nesta sessão](./docs/key-concepts.md) para auxiliar o estudo dos próximos tópicos.

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


