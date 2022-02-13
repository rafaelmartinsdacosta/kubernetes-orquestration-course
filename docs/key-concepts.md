## Conceitos chaves

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