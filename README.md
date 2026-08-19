# torbr

Bibliotecas locais para a ferramenta de QR Code de blog (uso no Blogger).

## Visão geral

Este repositório hospeda as bibliotecas JavaScript usadas pelo tema do Blogger. O tema em si (arquivo `theme-*.xml`) **não** fica no repositório por conter código privado do blog — ele permanece apenas na máquina local.

O Blogger consome as bibliotecas via CDN jsDelivr, que serve os arquivos diretamente da branch `main` do GitHub.

## Estrutura

- `Diversos/qrcode.min.js` — biblioteca de geração de QR Code (qrcode-generator 1.4.4).
- `Diversos/jsQR.min.js` — biblioteca de leitura de QR Code (jsQR 1.4.0).
- `.gitignore` — impede o envio acidental do tema `theme-*.xml`.

## URLs de CDN (jsDelivr, a partir do GitHub)

- `https://cdn.jsdelivr.net/gh/atm777/torbr@main/Diversos/qrcode.min.js`
- `https://cdn.jsdelivr.net/gh/atm777/torbr@main/Diversos/jsQR.min.js`

---

## Fluxo de trabalho (mudanças futuras)

### Cenário A — Alteração no tema do Blogger (HTML/CSS/JS do `theme-*.xml`)

1. Edite o arquivo `theme-8578514451183524045.xml` localmente.
2. **Não** faça commit deste arquivo (o `.gitignore` já bloqueia, mas confirme antes de `git add -A`).
3. Acesse o painel do Blogger: **Tema → Editar HTML**.
4. Cole o conteúdo atualizado do XML e **Salvar**.

> As URLs das bibliotecas no XML continuam as mesmas, apontando para o CDN via GitHub.

### Cenário B — Alteração/troca de uma biblioteca em `Diversos/`

1. Substitua ou edite o arquivo em `Diversos/` (ex.: `Diversos/qrcode.min.js`).
2. Confirme que o nome do arquivo permanece o mesmo (ou atualize a URL no tema do Blogger, caso renomeie).
3. Commit e push:
   ```bash
   git add Diversos/
   git commit -m "Atualiza biblioteca qrcode para vX.Y.Z"
   git push
   ```
4. O jsDelivr passa a servir a nova versão automaticamente na branch `main`.

### Cenário C — Adicionar um novo arquivo/recursos

1. Adicione o arquivo na pasta `Diversos/`.
2. Atualize o tema do Blogger (`theme-*.xml`) se for necessário referenciar o novo arquivo:
   - Insira a nova `<script>`/`<link>` apontando para `https://cdn.jsdelivr.net/gh/atm777/torbr@main/Diversos/<arquivo>`.
3. Commit e push no GitHub.
4. Atualize o Blogger colando o XML atualizado.

### Checklist final

- [ ] Arquivo novo/alterado está em `Diversos/`.
- [ ] Commit e push realizados no GitHub (branch `main`).
- [ ] Tema do Blogger atualizado com o XML (se o XML mudou).
- [ ] Se o nome do arquivo mudou, a URL no XML foi atualizada.

---

## Nota sobre cache do jsDelivr

O jsDelivr pode manter cache por alguns minutos (geralmente até 12h no limite, mas costuma atualizar em poucos minutos). Para forçar atualização imediata, use `https://purge.jsdelivr.net/gh/atm777/torbr@main/Diversos/<arquivo>` e acesse no navegador.

Para versões imutáveis (recomendado em produção), troque `@main` pelo hash do commit:
- `https://cdn.jsdelivr.net/gh/atm777/torbr@<commit-hash>/Diversos/qrcode.min.js`