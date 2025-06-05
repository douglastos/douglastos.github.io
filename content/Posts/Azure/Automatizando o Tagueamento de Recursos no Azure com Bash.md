---
title: Automatizando o Tagueamento de Recursos no Azure com Bash
date: 2025-05-28
tags:
  - azure
  - tags
  - bash
  - shellscript
---
----

  <tr>

    <td><img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fcdn.icon-icons.com%2Ficons2%2F2699%2FPNG%2F512%2Flinux_logo_icon_171222.png&f=1&nofb=1&ipt=7b969f2234f747e1db21294a093082793cd402f722f9a867d58226223e3cfc1c" width="800" /></td>

  </tr>


# Automatizando o Tagueamento de Recursos no Azure com Bash

Você já precisou aplicar **tags em massa** nos recursos da sua assinatura Azure? E se fosse possível fazer isso com um simples script Bash? Foi exatamente isso que construí com o [Azure Resource Tagger](https://github.com/douglastos/azure-resource-tagger), uma solução simples, direta e eficiente para quem trabalha com ambientes cloud e precisa manter a governança em dia.

## 💡 O Que é o Azure Resource Tagger?

Um script Bash que aplica um conjunto fixo de tags a uma lista de recursos do Azure. Você define os recursos que deseja taguear em um arquivo `resources.txt`, e o script cuida do resto.

## 🧰 Pré-requisitos

Antes de rodar o script, certifique-se de ter:

- [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) instalado e autenticado (`az login`)
    
- Permissões para modificar tags na sua assinatura
    
- A variável de ambiente `AZ_SUB_ID` com seu Subscription ID:
    

```
export AZ_SUB_ID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

- Um arquivo `resources.txt` com os IDs completos dos recursos (um por linha):
    

```
/subscriptions/<id>/resourceGroups/rg-name/providers/Microsoft.Compute/virtualMachines/vm-name
```

## ⚙️ Como Usar

1. Exporte seu ID de assinatura:
    

```
export AZ_SUB_ID="xxxxx-xxx-xxx-xxxx-XXXXXX"
```

2. Certifique-se de que `resources.txt` está no mesmo diretório.
    
3. Execute o script:
    

```
bash tag.sh
```

## 🧾 O Que o Script Faz

- Lê os IDs dos recursos a partir do `resources.txt`
    
- Remove espaços e quebras de linha extras
    
- Aplica as seguintes tags:
    

```
status=emuso
env=prd
produto=produto
cliente=cliente
iac=nao
```

Tudo isso utilizando o comando:

```
az tag update --operation merge
```

Ou seja, as tags existentes são mantidas e apenas as novas são adicionadas ou atualizadas.

## ⚠️ Observações

- Recursos que não aceitam tags (ex: extensões de VMs desativadas) gerarão erro, mas o script continua rodando.
    
- Linhas vazias ou malformadas no `resources.txt` são ignoradas.
    

## 📁 Estrutura Esperada do Projeto

```
.
├── tag.sh           # Script principal
└── resources.txt    # Lista de IDs dos recursos
```

## ✅ Exemplo de Saída

```
Tagging: /subscriptions/xxx/resourceGroups/my-rg/providers/Microsoft.Compute/virtualMachines/vm01
{
  "tags": {
    "cliente": "client",
    "env": "prd",
    "iac": "nao",
    "produto": "produto",
    "status": "emuso"
  }
}
```

## 📥 Dica Extra: Gerando o resources.txt Automaticamente

Use o comando abaixo para gerar a lista de IDs diretamente da sua assinatura:

```
az resource list \
  --query "[].id" \
  -o tsv > resources.txt
```

Ou, se quiser mais controle com `jq` e `awk`:

```
az resource list -o json > resources.json

jq -r '.[] | .id' resources.json > resources.txt
```

---

🔗 Confira o projeto completo no GitHub:  
👉 [douglastos/azure-resource-tagger](https://github.com/douglastos/azure-resource-tagger)

[[index| ]]

<script src="https://giscus.app/client.js" data-repo="douglastos/douglastos.github.io" data-repo-id="R_kgDOLvf9iw"
data-category="General" data-category-id="DIC_kwDOLixoLc4CeGqc" data-mapping="title"data-strict="1"data-reactions-enabled="1"data-emit-metadata="0"data-input-position="bottom"data-theme="dark"data-lang="pt"crossorigin="anonymous"async>
</script>