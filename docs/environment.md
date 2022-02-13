## Preparando o ambiente
Vamos detalhar algumas ferramentas que podem ou devem ser utilizadas para seguir nos estudos.

**Por hora precisamos:**

**Certificar-se** de ter instalado em sua máquina a ferramenta de gestão de containers, que nosso caso será o **Docker**.

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
