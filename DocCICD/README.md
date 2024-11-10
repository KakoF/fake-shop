# Documentação


## Template CICD

```yaml
name: CI-CD

on:
  push:
    branches: ["main"]
  
  # Se eu quiser disparar manualmente também
  workflow_dispatch:

jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - name: Autenticar no Docker Hub
        run: echo "Executando comando docker login"
      
      - name: Construção Imagem Docker
        run: echo "Executando comando docker build"
      
      - name: Envio Imagem Docker
        run: echo "Executando comando docker push"

  cd:
    runs-on: ubuntu-latest
    needs: [ci]
    steps:
      - name: Autenticar na AWS
        run: echo "Executando comando aws configure"
      
      - name: Configurar o Kubectl
        run: echo "Executando comando aws eks update-kubeconfig"
      
      - name: Deploy od Manifestos no Kubernetes
        run: echo "Executando comando kubectl apply"
```