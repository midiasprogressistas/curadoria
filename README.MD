# Curadoria de Mídias Progressistas

Site de página única que reúne iniciativas que vale a pena apoiar. Cada item aparece como um cartão com imagem, nome e descrição curta, e leva a um link externo.

**No ar em:** https://midiasprogressistas.github.io/curadoria/

---

## ✨ O que este projeto faz

- **Grade fluida e animada** — os cartões se adaptam a qualquer tela e entram com uma animação suave.
- **Ordem aleatória a cada visita** — cada carregamento embaralha os itens (algoritmo Fisher-Yates). A ideia é **justiça de exposição**: todos os itens têm a mesma chance de aparecer primeiro, e visitantes diferentes veem ordens diferentes.
- **Busca instantânea** — filtro por nome ou descrição, aparece quando há mais de 3 itens.
- **Compartilhamento nativo** — botão que usa a Web Share API do celular, com fallback para copiar o link no desktop.
- **Conteúdo 100% configurável** — tudo vem do arquivo `dados.json`. Nenhuma linha de HTML precisa ser tocada para adicionar, remover ou editar um item.
- **Sem servidor** — HTML, CSS e JavaScript puros. Roda em qualquer hospedagem estática, inclusive GitHub Pages.

---

## 📁 Estrutura de arquivos

```
curadoria/
├── index.html          # a página (HTML + CSS + JS, tudo num arquivo só)
├── dados.json          # conteúdo configurável — é aqui que você mexe
├── favicon.png         # ícone da aba do navegador
├── og-image.png        # imagem do preview ao compartilhar (1200×630, <300 KB)
├── README.md           # este arquivo
└── imagens/            # fotos dos itens
    ├── exemplo1.jpg
    └── exemplo2.jpg
```

---

## 🚀 Como rodar localmente

Como o site lê o `dados.json` com `fetch()`, **abrir o `index.html` com duplo clique não funciona** — o navegador bloqueia a leitura de arquivos locais por segurança. Você precisa de um servidor estático simples.

Escolha o que combinar com o que você já tem instalado:

**Python** (já vem no Linux e macOS):
```bash
python -m http.server 8000
```

**Node.js:**
```bash
npx serve
```

**VS Code:** instale a extensão *Live Server* e clique com o botão direito no `index.html` → *Open with Live Server*.

Depois abra `http://localhost:8000` no navegador.

---

## ✏️ Como editar o conteúdo

Tudo o que aparece no site vem de `dados.json`. Abra-o e edite os campos:

```json
{
  "marca": "Curadoria",
  "titulo": "Curadoria de Mídias Progressistas",
  "descricao": "Iniciativas que vale a pena apoiar",
  "pastaImagens": "imagens/",
  "itens": [
    {
      "nome": "Nome da iniciativa",
      "imagem": "foto.jpg",
      "link": "https://exemplo.com",
      "descricao": "Uma frase curta explicando por que apoiar."
    }
  ]
}
```

### O que cada campo faz

| Campo | Onde aparece | Observações |
|---|---|---|
| `marca` | etiqueta pequena no topo | aceita texto curto |
| `titulo` | título grande (H1) e aba do navegador | — |
| `descricao` | parágrafo de apresentação | — |
| `pastaImagens` | prefixo do caminho das imagens | mantenha a barra no final |
| `itens` | lista de cartões | ordem no arquivo é ignorada — o site embaralha |

Dentro de cada item:

| Campo | O que é |
|---|---|
| `nome` | título do cartão |
| `imagem` | só o **nome do arquivo** (a pasta já vem de `pastaImagens`) |
| `link` | URL completa, com `https://` |
| `descricao` | texto abaixo do título (corta em 3 linhas) |

### Regras rápidas para não quebrar o JSON

- **Aspas duplas** em tudo. Nunca aspas simples.
- **Sem vírgula** depois do último item de cada bloco.
- **Sem comentários** — JSON não aceita `//` nem `/* */`.
- **Sem vírgula solta** antes de `]` ou `}`.

Se errar, o site mostra uma mensagem de erro em vermelho. Aperte **F12 → Console** para ver o que quebrou e em qual linha.

---

## 🖼️ Como adicionar uma nova iniciativa

1. Coloque a foto em `imagens/` (ex.: `minha-foto.jpg`).
2. Abra o `dados.json`.
3. Adicione um novo bloco dentro de `"itens": [...]`, **com vírgula** antes se não for o primeiro:

```json
    {
      "nome": "Nova iniciativa",
      "imagem": "minha-foto.jpg",
      "link": "https://exemplo.com",
      "descricao": "Descrição curta do que é e por que apoiar."
    }
```

4. Salve, faça commit e push. Em cerca de 1 minuto, o site atualiza sozinho.

---

## 🌐 Como publicar / atualizar

O site está hospedado no **GitHub Pages**, configurado para servir da branch `main`, pasta raiz.

**Atualizações são automáticas.** Sempre que você fizer commit e push para `main`, o GitHub reconstrói e publica o site em 1–2 minutos. Não existe botão de "publicar" nem comando extra.

### Editando direto pelo navegador (sem instalar nada)

1. Abra o repositório no GitHub.
2. Clique no arquivo que quer editar (`dados.json`, por exemplo).
3. Clique no ícone de **lápis** (Edit).
4. Faça a alteração.
5. Clique em **Commit changes**.

O site atualiza em cerca de 1 minuto.

### Se a mudança não aparecer

O culpado mais comum é o **cache do navegador**. Tente:

- Recarga forçada: `Ctrl + Shift + R` (Windows/Linux) ou `Cmd + Shift + R` (Mac).
- Abrir numa janela anônima.
- Conferir a aba **Actions** do repositório — cada push gera um build; se estiver verde, foi publicado; se vermelho, algo quebrou.

---

## 🎨 Ajustes visuais

O CSS fica todo dentro do `<style>` no `index.html`. As variáveis principais estão no topo:

```css
:root{
  --bg:        #0a0c11;   /* fundo */
  --texto:     #e9ecf3;   /* cor do texto principal */
  --texto-2:   #98a1b4;   /* cor do texto secundário */
  --acento:    #6ea8fe;   /* azul de destaque */
  --acento-2:  #b06ef5;   /* roxo de destaque */
  --raio:      18px;      /* arredondamento dos cartões */
}
```

Mudar essas variáveis já altera a cara do site inteiro.

---

## 🔗 Compartilhamento em redes sociais

O preview que aparece no WhatsApp, Telegram, X e LinkedIn é controlado pelas **meta tags Open Graph** no `<head>` do `index.html`.

A URL compartilhada está fixada no JavaScript:

```js
const URL_PARA_COMPARTILHAR = "https://midiasprogressistas.github.io/curadoria/";
```

Para os previews funcionarem bem:

- `og:image` precisa ser uma **URL absoluta** (`https://...`), nunca relativa.
- A imagem deve ter **1200×630 px**, proporção 1.91:1, e **menos de 300 KB**.
- Depois de alterar, force a atualização do cache compartilhando com `?v=1` no final da URL.

Ferramenta útil para testar: [Social Preview Inspector](https://github.com/GPRizzi/social-preview-tool).

---

## 📬 Sugestões de novas iniciativas

Sugestões são bem-vindas por e-mail: **caca@caca.com**

---

## 🛠️ Detalhes técnicos

- **Sem dependências** — nenhuma biblioteca, nenhum framework, nenhum build step.
- **Sem servidor** — 100% estático, roda no navegador.
- **Acessível** — navegação por teclado, `aria-label` nos cartões, respeita `prefers-reduced-motion`.
- **Mobile-first** — área de toque adequada, ajustes específicos para telas pequenas, `-webkit-` prefixes para Safari.
- **Embaralhamento** — Fisher-Yates em `renderizar()`, aplicado sobre uma cópia do array original.
- **Animação de entrada** — `IntersectionObserver` com stagger de 65 ms por posição.

---

## 📄 Licença

Conteúdo editorial © Curadoria de Mídias Progressistas. Código disponível para reuso.
