# 🌟 DevOps Lab - Kubernetes + CI/CD + Terraform

Este repositório contém um **laboratório DevOps completo**, destinado a aprendizado e testes, simulando um ambiente de produção com Kubernetes, CI/CD, Terraform, observabilidade e exposição segura.

---

## 🚀 Stack do Laboratório

- **Kubernetes**: k3s (cluster leve local)  
- **CI/CD**: Jenkins  
- **Infraestrutura como Código**: Terraform (provider Kubernetes)  
- **Docker Registry**: Local (para armazenar imagens)  
- **Observabilidade**: Prometheus + Grafana + Loki (logs)  
- **Proxy / Load Balancer**: Traefik  
- **Exposição segura**: Cloudflare Tunnel  

---

## 📂 Estrutura do repositório

```bash
lab-devops/
├── manifests/   - YAMLs Kubernetes (Jenkins, apps, ingress)
├── terraform/   - Arquivos Terraform para deploy
├── apps/        - Exemplos de aplicações (Node.js, Spring, etc.)
├── Jenkinsfile  - Pipeline CI/CD
└── README.md    - Este arquivo
```

---

## ⚙️ Pré-requisitos

- Linux (testado em Arch/Ubuntu)  
- Docker  
- kubectl  
- k3s instalado  
- Helm (opcional)  
- cloudflared (para túnel Cloudflare)  

---

## 🛠️ Instalação e configuração

### 1️⃣ Instalar k3s
```bash
curl -sfL https://get.k3s.io | sh -
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

### 2️⃣ Configurar kubeconfig para usuário local
```bash
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(id -u):$(id -g) ~/.kube/config
export KUBECONFIG=~/.kube/config
kubectl get nodes
```

### 3️⃣ Rodar registry local
```bash
docker run -d --restart=always --name registry -p 5000:5000 registry:2
```

### 4️⃣ Deploy Jenkins
```bash
kubectl apply -f manifests/jenkins-deploy.yaml
```

### 5️⃣ Deploy app via Terraform
```bash
cd terraform
terraform init
terraform apply -auto-approve -var="image=localhost:5000/minha-app:0.1.0"
```

### 6️⃣ Expor via Cloudflare Tunnel
```bash
cloudflared tunnel --url http://localhost:8080    # Jenkins
cloudflared tunnel --url http://localhost:80      # App
```
---

## ⚡ Pipeline CI/CD (Jenkins)

- Jenkins clona o repo

- Build da imagem Docker da aplicação

- Push para o registry local

- Terraform aplica deploy no Kubernetes

- Observabilidade via Prometheus/Grafana
  
---

## 🔍 Observabilidade

- Prometheus: métricas do cluster e apps

- Grafana: dashboards

- Loki: logs centralizados

- Traefik: proxy reverso e load balancer

