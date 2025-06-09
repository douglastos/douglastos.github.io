---
title: " Descubra a Senha do Wi-Fi pelo CMD (Windows)"
date: 2025-06-09
tags:
  - Windows
  - comandos
  - cmd
  - wiifi
---

![[1749305084624.jpg]]


Descubra a Senha do Wi-Fi pelo CMD (Windows)  
  
Você já precisou lembrar a senha de uma rede Wi-Fi que seu PC está conectado, mas não tem como ver isso nas configurações gráficas?  
  
Aqui vai um truque rápido e 100% funcional pelo Prompt de Comando (CMD) sem precisar instalar nada!  
  
📌 Como fazer:  
  
1️⃣ Abra o CMD como administrador (Win + S → digite "cmd" → botão direito → "Executar como administrador")  
  
2️⃣ Liste as redes Wi-Fi já conectadas:  
→ netsh wlan show profiles  
  
3️⃣ Pegue o nome da rede desejada e use:  
→ netsh wlan show profile name="NOME_DA_REDE" key=clear  
  
4️⃣ Procure por:  
→ Conteúdo da chave : SUA_SENHA  
  
✅ Útil para:  
🔹 Recuperar senhas esquecidas  
🔹 Compartilhar com colegas de equipe  
🔹 Documentar redes já conectadas  
🔹 Diagnóstico e suporte técnico  
  
⚠️ Observação: Esse método funciona apenas para redes já conectadas anteriormente no PC.  
  
Se você curte dicas práticas de Windows, Linux e Infraestrutura, me segue aqui, trago conteúdos diretos ao ponto! 🚀


[[index| ]]

<script src="https://giscus.app/client.js" data-repo="douglastos/douglastos.github.io" data-repo-id="R_kgDOLvf9iw"
data-category="General" data-category-id="DIC_kwDOLixoLc4CeGqc" data-mapping="title"data-strict="1"data-reactions-enabled="1"data-emit-metadata="0"data-input-position="bottom"data-theme="dark"data-lang="pt"crossorigin="anonymous"async>
</script>