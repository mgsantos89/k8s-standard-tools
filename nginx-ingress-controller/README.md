# Nginx Ingress Controller

O Nginx Ingress Controller é uma ferramenta essencial para gerenciar entradas HTTP e HTTPS no cluster Kubernetes. Ele permite configurar rotas para serviços internos do cluster, oferecendo alta disponibilidade e suporte a SSL.

## 🚀 Passos de Instalação

1. Adicione o repositório do Helm:
   ```bash
   helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
   ```

2. Instale o Ingress Controller:
   ```bash
   helm upgrade --install ingress-nginx ingress-nginx \ 
             --repo https://kubernetes.github.io/ingress-nginx \ 
             --namespace ingress-nginx \
             --create-namespac
   ```

3. Verifique a instalação:
   ```bash
   kubectl get pods -n ingress-nginx
   ```

## 📂 Estrutura da Pasta

```
nginx-ingress-controller/
├── README.md  # Este arquivo
```

## 🌟 Recursos Úteis

- [Documentação Oficial](https://kubernetes.github.io/ingress-nginx/)
- [Repositório GitHub](https://github.com/kubernetes/ingress-nginx)
