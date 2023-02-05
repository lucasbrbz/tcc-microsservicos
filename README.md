
# tcc-microsservicos

Passo a passo


## Instalação
Instalar o istio

```bash
  curl -L https://istio.io/downloadIstio | sh -
  cd istio-1.15.0
  export PATH=$PWD/bin:$PATH
  
  (Isso irá adicionar a pasta istio-1.15.0/bin em seu $PATH temporariamente
  Para fazer de forma permanente, você pode colocar o último comando (export ...)
  em algum arquivo de seu sistema que seja sempre carregado, como /etc/profile,
  ~/.bashrc, ~/.zshrc, entre outros)
```

Instalar o minikube

```bash
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  sudo install minikube-linux-amd64 /usr/local/bin/minikube

```

Build das imagens e deploy

```bash
  cd orders-and-payments-microservices
  minikube start
  eval $(minikube -p minikube docker-env) 
  docker build -t orders-and-payments/orders:latest services/orders/
  docker build -t orders-and-payments/payments:latest services/payments/
  minikube addons enable metrics-server
  minikube kubectl -- label namespace default istio-injection=enabled
  istioctl install --set profile=demo -y
  minikube kubectl -- apply -f k8s/
```

Para ver o status dos pods, services, deployments e replicasets:

```bash
  minikube kubectl -- get all
```

Para permitir o acesso externo ao cluster:

```bash
  (deixe rodando em um outro terminal)
  minikube tunnel --cleanup
```
