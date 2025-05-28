---
title: Migrar repo git com todos histórico para qualquer nuvem
date: 2024-04-23
tags:
  - git
  - comandos
  - linux
  - bash
---
---

![[cover3.webp]]

Hoje me deparei com um projeto para migrar todos os fontes que estao on premises em gitlab para azuredevops, e por acaso isso apareceu um grupo do telegram que me ajudou bastante resolvi deixar aki o registro para posteridade! vamos la!….

esse how to por incrível que parece é muito simples segue os comando abaixo:  
  
primeiro passo é baixar todo o repositório:

```bash
git clone --mirror <url_do_repo> repo/.git

cd repo/.git
```

o próximo passo é subir no repositório onde será o próximo versionador:

```bash
git push --mirror <url_do_repo_destino>
```

Feito isso demoro um pouco caso seja grande o repositorio, quando terminar ele ja tera migrado completamente tudo, entre branchs, commits, tags e etc…

referencias:  
[migra-repo-git](https://github.com/douglastos/migra-repo-git/blob/main/README.md)  
[konia](https://konia.com.br/como-migrar-um-repositorio-git-com-historico/)

[[index| ]]

<script src="https://giscus.app/client.js" data-repo="douglastos/douglastos.github.io" data-repo-id="R_kgDOLvf9iw"
data-category="General" data-category-id="DIC_kwDOLixoLc4CeGqc" data-mapping="title"data-strict="1"data-reactions-enabled="1"data-emit-metadata="0"data-input-position="bottom"data-theme="dark"data-lang="pt"crossorigin="anonymous"async>
</script>