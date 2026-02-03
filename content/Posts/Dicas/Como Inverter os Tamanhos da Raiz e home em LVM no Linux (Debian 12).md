![Diagrama de LVM - root e home](https://muylinux.xyz/wp-content/uploads/2024/03/1710289508_Guia-para-principiantes-de-LVM-en-Linux-780x450.png)`

e você tem uma máquina com **LVM** configurado, por exemplo:

- `/` → 10 GB
    
- `/home` → 30 GB
    

e quer **inverter os tamanhos** para:

- `/` → 30 GB
    
- `/home` → 10 GB
    

existem algumas opções para fazer isso de forma segura.

---

## **Opção 1 – Redimensionamento direto dos LVs (Recomendado)**

> ⚠️ Requer **backup** antes! Qualquer erro pode causar perda de dados.

**Passos:**

1. **Diminuir `/home`:**
```bash
umount /home
e2fsck -f /dev/debian12-vg/home
resize2fs /dev/debian12-vg/home 10G   # reduzir para 10G
lvreduce -L 10G /dev/debian12-vg/home
mount /home
```

2. **Aumentar `/`:**
```bash
lvextend -l +100%FREE /dev/debian12-vg/root
resize2fs /dev/debian12-vg/root
```

3. **Verificar tamanhos:**
```bash
df -h
lvs
```

## **Opção 2 – Mover diretórios do / para /home (Menos invasivo)**

Se você não quiser mexer em LVs, pode **mover pastas pesadas** do `/` para `/home` e criar **links simbólicos**:

```bash
mv /usr/local /home/local
ln -s /home/local /usr/local
```

Isso libera espaço na raiz sem alterar partições. Bom para sistemas em produção onde não é possível mexer no LVM.

## **Opção 3 – Criar LVM do zero (Nova instalação ou disco novo)

Se você está instalando do zero ou criando uma nova máquina:

1. Criar PV e VG:
```bash
pvcreate /dev/sda2
vgcreate myvg /dev/sda2
```

2. Criar LVs com tamanhos desejados:
```bash
lvcreate -L 30G -n root myvg
lvcreate -L 10G -n home myvg
```

3. Criar sistemas de arquivos:
```bash
mkfs.ext4 /dev/myvg/root
mkfs.ext4 /dev/myvg/home
```
4. Montar volumes:
```bash
mount /dev/myvg/root /mnt
mkdir /mnt/home
mount /dev/myvg/home /mnt/home
```

✅ Essa opção é ideal para máquinas novas ou quando você quer definir os tamanhos exatos desde o início.


### **Conclusão**

- **Opção 1**: Melhor para sistemas já em LVM, dá controle total do espaço.
    
- **Opção 2**: Mais segura, não envolve risco de perda de dados, mas não resolve completamente se o `/` crescer muito.
    
- **Opção 3**: Perfeita para novas máquinas, planejamento de LVM do zero.
    

---

[[index| ]]

<script src="https://giscus.app/client.js" data-repo="douglastos/douglastos.github.io" data-repo-id="R_kgDOLvf9iw"
data-category="General" data-category-id="DIC_kwDOLixoLc4CeGqc" data-mapping="title"data-strict="1"data-reactions-enabled="1"data-emit-metadata="0"data-input-position="bottom"data-theme="dark"data-lang="pt"crossorigin="anonymous"async>
</script>





