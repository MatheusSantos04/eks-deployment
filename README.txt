CRIAÇÃO DE USUÁRIO NO IAM

NAME: k8-admin
KEY ID - AKIAQRYS3**************
SECRET KEY - zCblw2Pfg8v97fLk****************
-----------------------------------------------------------

BAIXAR A AWS CLI V2

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

which aws

Atualizar a CLI:
sudo ./aws/install --bin-dir /usr/bin --install-dir /usr/bin/aws-cli --update

Configurando acesso a CLI
aws configure

-----------------------------------------------------------

INSTALANDO KUBECTL

curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl

Adiciona permissão para execurtar o binário: 
chmod +x ./kubectl

copia e move o binario:  
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin

verifica a versão:
kubectl version --short --client
-----------------------------------------------------------

INSTALANDO EKSCTL

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

move o binário:
sudo mv /tmp/eksctl /usr/bin

verifica a versão:
eksctl version

----------------------------------------------------------

PROVISIONANDO CLUSTER EKS

eksctl create cluster --name dev --version 1.21 --region us-east-1  --nodegroup-name standard-workers --node-type t3.micro --nodes 3 --nodes-min 1 --nodes-max 4 --managed

Retorna o nome do cluster e região:
eksctl get cluster

Configura o kubectl para que ele possa se conectar ao cluster do Amazon EKS.
aws eks update-kubeconfig --name dev --region us-east-1

-----------------------------------------------------------

INSTALANDO GIT NA INSTÂNCIA
sudo yum install -y git

Clonar repositorio git
https://github.com/ACloudGuru-Resources/Course_EKS-Basics.git

mudar para o direorio Course_EKS-Basics

-----------------------------------------------------------

IMPLANTAÇÃO NO CLUSTER EKS

Criar o service
kubectl apply -f ./nginx-svc.yaml

retorna as informações do service, que na verdade é um load balancer.
kubectl get service

Deploy dos pods e aplicação
kubectl apply -f ./nginx-deployment.yaml

retorna as informações do deploy
kubectl get deployment

retorna os pods
kubectl get pod

retorna a quantidade de pods em execução
kubectl get rs

retorna os nodes
kubectl get nodes

Acessa a aplicação pelo loadbalancer
curl "dns-do-load-balancer"

-----------------------------------------------------------
TESTANDO A ALTA DISPONILIDADE

Encerrar todas as instâncias EC2

Analisar o comportamento do EKS para provisionar os recursos novamente automaticamante.

Depois de alguns minutos acessa a aplicação pelo loadbalancer
curl "dns-do-load-balancer"