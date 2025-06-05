---
title: 📌  Configuração da Cadeia de Certificados SSL no Apache
date: 2025-01-29
tags:
  - linux
  - bash
  - ssl
  - apache
---
[[🐧Linux| ]][[index| ]] 
## 📜 Estrutura dos Certificados

Para configurar corretamente a cadeia de certificados no Apache, precisamos identificar:

1. **Certificado principal (Leaf Certificate)** → Assinado por uma CA intermediária.
2. **Certificado intermediário (Intermediate Certificate)** → Assinado pela Root CA.
3. **Certificado raiz (Root Certificate)** → Autoridade certificadora raiz.

Para identificar cada um:

```bash
openssl x509 -in certificado.crt -noout -subject -issuer
```

Exemplo de saída:

```
subject= /CN=*.example.com
issuer= /C=US/O=Example CA/CN=Example Intermediate CA
```

Isso mostra que `certificado.crt` foi emitido pela **CA intermediária**.

Agora, para o intermediário:

```bash
openssl x509 -in intermediario.crt -noout -subject -issuer
```

Saída:

```
subject= /C=US/O=Example CA/CN=Example Intermediate CA
issuer= /C=US/O=Example Root CA
```

Isso indica que `intermediario.crt` foi assinado pela **Root CA**.

---

## 🌍 **Baixando o Certificado Raiz com `wget`**

Caso o certificado raiz não esteja disponível, é possível baixá-lo diretamente do site da DigiCert:

```bash
wget -O /etc/httpd/ssl/DigiCertGlobalRootG2.crt http://cacerts.digicert.com/DigiCertGlobalRootG2.crt
```

## 🔗 Criando o `fullchain.crt`

O Apache requer um arquivo contendo toda a cadeia de certificados na ordem correta:

1. **Certificado principal** (`certificado.crt`)
2. **Certificado intermediário** (`intermediario.crt`)
3. **Certificado Raiz** (`DigiCertGlobalRootG2.crt`)

Para gerar:

```bash
cat certificado.crt intermediario.crt DigiCertGlobalRootG2.crt > /etc/httpd/ssl/fullchain.crt
```

---

## 🛠️ Configuração no Apache

Edite o arquivo de configuração SSL (`/etc/httpd/conf.d/ssl.conf` ou similar):

```apache
<VirtualHost *:443>
    ServerName example.com
    SSLEngine on
    SSLCertificateFile /etc/httpd/ssl/fullchain.crt
    SSLCertificateKeyFile /etc/httpd/ssl/private.key
    SSLCACertificateFile /etc/httpd/ssl/root_ca.crt
</VirtualHost>
```

**Explicação:**

- `SSLCertificateFile` → Contém o certificado do site + intermediário.
- `SSLCertificateKeyFile` → Contém a chave privada.
- `SSLCACertificateFile` → Contém o certificado raiz.

---

## ✅ Testando e Aplicando as Configurações

### 1️⃣ Verificar a configuração do Apache

```bash
apachectl configtest
```

Se retornar **Syntax OK**, reinicie o serviço:

```bash
systemctl restart httpd
```

### 2️⃣ Testar conexão local

```bash
curl -vk https://localhost
```

### 3️⃣ Verificar a cadeia de certificados

```bash
openssl s_client -connect example.com:443 -showcerts
```

Se tudo estiver correto, o **Azure Application Gateway** também aceitará a conexão sem erros de SSL.

---

🚀 **Agora sua configuração SSL está pronta e documentada!**


[[index| ]]

<script src="https://giscus.app/client.js" data-repo="douglastos/douglastos.github.io" data-repo-id="R_kgDOLvf9iw"
data-category="General" data-category-id="DIC_kwDOLixoLc4CeGqc" data-mapping="title"data-strict="1"data-reactions-enabled="1"data-emit-metadata="0"data-input-position="bottom"data-theme="dark"data-lang="pt"crossorigin="anonymous"async>
</script>