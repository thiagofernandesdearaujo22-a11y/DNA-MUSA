[README.md](https://github.com/user-attachments/files/29927518/README.md)
# MUSA+ — Protótipo

Site estático de página única (`index.html`). Sem build, sem dependências — a Vercel só precisa servir o arquivo.

## Deploy inicial (uma vez só)

```bash
# 1. Dentro desta pasta (musa-deploy), inicialize o git
git init
git add .
git commit -m "Primeira versão do protótipo MUSA+"

# 2. Crie um repositório vazio no GitHub (pelo site, sem README/gitignore)
#    Depois conecte e envie:
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/musa-plus-prototipo.git
git push -u origin main
```

## Conectar na Vercel (uma vez só)

1. vercel.com → **Add New → Project**
2. Importe o repositório `musa-plus-prototipo` que você acabou de criar
3. Framework preset: **Other** (é HTML puro, sem build necessário)
4. Deploy

A Vercel te dá uma URL fixa (tipo `musa-plus-prototipo.vercel.app`).

## Daqui pra frente, a cada atualização

Sempre que eu te mandar um `musa_plus_prototipo.html` novo:

```bash
cp caminho/do/novo/musa_plus_prototipo.html index.html
git add .
git commit -m "Atualização: [descreva o que mudou]"
git push
```

A Vercel detecta o push e publica sozinha em segundos — sem precisar tocar no painel da Vercel de novo.
