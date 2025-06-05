
![[Pasted image 20250605152752.png]]


Nos últimos dias, andei explorando o [Semaphore](https://ansible-semaphore.com/), uma interface web open-source para orquestrar *playbooks* Ansible. A ideia era criar uma POC (Prova de Conceito) simples, mas funcional, para testar sua integração com Docker e Ansible de forma local. O resultado está publicado no meu [repositório no GitHub](https://github.com/douglastos/Semaphore).

## O que é o Semaphore?

O Semaphore é uma ferramenta web para gerenciamento e execução de automações com Ansible. Com ele, podemos:
- Criar e agendar execuções de playbooks;
- Armazenar chaves SSH de forma segura;
- Organizar projetos e inventários;
- Usar variáveis, templates e versionamento com Git.

Tudo isso com uma interface intuitiva que facilita bastante o trabalho, principalmente para quem quer fugir um pouco do terminal.

---

## Pré-requisitos para rodar o projeto

Antes de começar, é necessário ter:
- **Docker** e **Docker Compose** instalados;
- **Git** para clonar o projeto;
- Acesso ao repositório: `https://github.com/douglastos/Semaphore`.

Com isso pronto, o processo é simples:

```bash
git clone https://github.com/douglastos/Semaphore.git
cd Semaphore
mkdir semaphore_data
docker compose up -d
```

Depois disso, é só acessar o [http://localhost:3000](http://localhost:3000) e logar com o usuário padrão:

```txt
Usuário: admin
Senha: admin
```

---

## Configuração passo a passo

Após o login, você passará pelas seguintes etapas no ambiente web:

1. Adicionar a chave privada no menu `Key Store`
2. Adicionar o repositório Git contendo seu playbook
3. Gerar um par de chaves SSH dentro do container do Semaphore
4. Criar variáveis e inventário
5. Criar o template de execução
6. Criar Template
7.  Rodar o playbook via interface

Todas essas etapas estão explicadas com imagens e exemplos no [README do projeto](https://github.com/douglastos/Semaphore/blob/main/README.md).

---

## Exemplo: Gerando chave SSH no container

```bash
docker container exec -it semaphore bash
ssh-keygen -t rsa -b 4096 -C "semaphore@seuprojeto" -f ~/.ssh/semaphore
ssh-copy-id -i ~/.ssh/semaphore.pub deploy@<ip-do-servidor>
```

Depois disso, copie a chave privada e cole no campo `Key Store` da interface.

---

## Conclusão

Essa foi uma pequena prova de conceito para mostrar como é simples e rápido integrar o Semaphore com Docker e Ansible. Ideal para testes locais ou até pequenos ambientes de automação.

Se quiser conferir o projeto completo e testar por conta própria, acesse:

🔗 [https://github.com/douglastos/Semaphore](https://github.com/douglastos/Semaphore)

Fique à vontade para dar uma estrela ⭐ no repositório se o conteúdo te ajudou ou te inspirou a automatizar mais com Ansible!

[[index| ]]

<script src="https://giscus.app/client.js" data-repo="douglastos/douglastos.github.io" data-repo-id="R_kgDOLvf9iw"
data-category="General" data-category-id="DIC_kwDOLixoLc4CeGqc" data-mapping="title"data-strict="1"data-reactions-enabled="1"data-emit-metadata="0"data-input-position="bottom"data-theme="dark"data-lang="pt"crossorigin="anonymous"async>
</script>