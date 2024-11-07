# fake-shop




# AWS

## Criar um usuario, com permissão Administrativa

### IAM - Indentity Access Manager
Acessar o IAM e criar um usuário com acesso Administrativo


## Criar Serviço Kubernetes

### EKS - Elastic Kubernetes Service
Criar um serviço Kubernetes (PaaS)

Etapas iniciais:

* Criar roles, por exemplo o control plane gerenciar os clusters (precisamos definir a role pra ele 
poder fazer isso)

1. Primeira role (nome: eks-cluster):
No IAM criamos a role (função) como AWS Service e definimos o Use Case do EKS do tipo EKS - Cluster (Allows the cluster Kubernetes control plane to manage AWS resources on your behalf)
2. Segunda role (nome: eks-worker):
No IAM criamos a role (função) como AWS Service e definimos o Use Case do EC2 o tipo EC2 (Allows EC2 instances to call AWS services on your behalf)
Nesse caso vamos ter que especificar as policies que vamos utilizar:
**AmazonEKS_CNI_Policy**, **AmazonEKSWorkerNodePolicy**, **AmazonEC2ContainerRegistryReadOnly**

* Montar estrutura de rede
Usaremos o **CloudFormation (Create and Manage Resources with Templates)**
1. Criaremos uma nova stack (pilha) do tipo ja existente, que esta na Amazon S3 (https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml) (nome: eks-stack-aula)

### Criar o Cluster Kubernetes
* Criar novo cluster (nome: eks-first-cluster)
* Selecionamos a subnet que criamos com o CloudFormation

### AWS CLI
Instalar o Aws cli, para interagirmos com o cluster (https://aws.amazon.com/pt/cli/)

Após instalação precisamos autenticar no mesmo, mas pra isso precisamos criar as credenciais para o usuário que foi criado.
Pra isso precisamos entrar no usuario e gerar crendencias de segurança, ir em Chaves de acesso e criar uma nova chave
```cmd
aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: us-east-1
Default output format [None]:
```
Configurar o cluster, ele vai adicionar o novo contexto ao kubectl
```cmd
aws eks update-kubeconfig --name eks-first-cluster
```
```cmd
kubectl get nodes
kubectl cluster-info
```

Ainda nãod definimos nosso Worker Nodes, pra isso temos que ir no nosso cluster na AWS e ir em Computação e definir os Grupos de Nós. Lembrando que vamos utilizar EC2 (maquinas virtuais) para esses Nodes

## Outros Serviços

### EC2
Maquinas Virtuais

### ECR
Container Registry, armazenamento de imagens

### KMS -> Key Management Service
Semelhante ao KeyVault


# Deletar o CLuster

```cmd
kubectl delete -f .\k8s\deployment.yaml
```

Ir na AWS e deletar o Grupo de Nos do Cluster, depois o Cluster e por ultimo no CloudFormation e deletar a stack de rede


