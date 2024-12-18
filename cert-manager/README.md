# Cert Manager

O **Cert Manager** é uma ferramenta poderosa para gerenciar certificados SSL/TLS em clusters Kubernetes. Ele automatiza a emissão e renovação de certificados usando diversas autoridades certificadoras (CA), como Let's Encrypt.

## 🚀 Passos de Instalação

### 1. Adicionar o repositório do Helm
Certifique-se de que o Helm está instalado. Depois, adicione o repositório do Cert Manager:

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

### 2. Instalar o Cert Manager
Instale o Cert Manager no namespace `cert-manager`:

```bash
kubectl create namespace cert-manager

helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --set installCRDs=true
```

> **Nota:** A opção `--set installCRDs=true` garante que os Custom Resource Definitions (CRDs) necessários sejam criados automaticamente.

### 3. Verificar a Instalação
Certifique-se de que os pods estão em execução no namespace `cert-manager`:

```bash
kubectl get pods -n cert-manager
```

Você deverá ver algo como:

```
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-xxxxx-yyyyy                   1/1     Running   0          1m
cert-manager-cainjector-xxxxx-yyyyy        1/1     Running   0          1m
cert-manager-webhook-xxxxx-yyyyy           1/1     Running   0          1m
```

---

## ⚙️ Configuração

### 1. Criar um Issuer ou ClusterIssuer
Um Issuer define como o Cert Manager obtém certificados. Para usar o Let's Encrypt, crie um arquivo YAML chamado `issuer.yaml`:

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: seu-email@example.com
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
    - http01:
        ingress:
          class: nginx
```

Aplique o manifesto:

```bash
kubectl apply -f issuer.yaml
```

### 2. Emitir um Certificado
Crie um manifesto para solicitar um certificado:

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: example-com
  namespace: default
spec:
  secretName: example-com-tls
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
  dnsNames:
  - example.com
  - www.example.com
```

Aplique o manifesto:

```bash
kubectl apply -f certificate.yaml
```

Verifique o status do certificado:

```bash
kubectl describe certificate example-com -n default
```

---

## 📂 Estrutura da Pasta

```
cert-manager/
├── README.md          # Este arquivo
├── issuer.yaml        # Exemplo de ClusterIssuer
└── certificate.yaml   # Exemplo de configuração de certificado

```

---

## 🌟 Recursos Úteis

- [Documentação Oficial](https://cert-manager.io/docs/)
- [Repositório GitHub](https://github.com/cert-manager/cert-manager)
- [Let's Encrypt](https://letsencrypt.org/)

