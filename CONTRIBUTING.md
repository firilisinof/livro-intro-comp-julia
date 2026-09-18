# Como contribuir

## Requisitos

- [micromamba](https://mamba.readthedocs.io/en/latest/)
- [Julia](https://julialang.org/) 1.11
- Jupyter Book, MyST e Typst (instalados por `environment.yml`)

## Ambiente de desenvolvimento

Na raiz do repositório, crie ou atualize o ambiente:

```bash
micromamba create --file environment.yml
micromamba run --name jubook juliaup add 1.11
```

Instale as dependências Julia e o kernel usado pelas células executáveis:

```bash
micromamba run --name jubook julia --project=. -e 'using Pkg; Pkg.instantiate()'
micromamba run --name jubook julia --project=. -e 'using IJulia; installkernel("Julia Livro", "--project=$(pwd)")'
```

## Estrutura do projeto

O livro é publicado em dois idiomas, cada um como um projeto MyST independente:

- `pt/`: edição em português.
  - `pt/book/`: fontes Markdown/MyST do livro em português.
  - `pt/myst.yml`: ordem do livro, tema do site e exportação do PDF (português).
- `en/`: English edition.
  - `en/book/`: the book's Markdown/MyST sources in English.
  - `en/myst.yml`: book order, site theme and PDF export (English).
- `web/index.html`: página inicial estática que direciona para `/pt/` ou `/en/`.
- `pt/_build/` e `en/_build/`: artefatos gerados; não versionar.
- `Project.toml` e `Manifest.toml`: ambiente Julia compartilhado, usado pelas células de ambos os idiomas.
- `.github/workflows/publish.yml`: builda os dois projetos e publica o site combinado (`/pt/`, `/en/`) e os dois PDFs no GitHub Pages.

Os arquivos de cada capítulo têm o mesmo número em ambos os idiomas (por exemplo, `pt/book/02-calculadora-repl.md` e `en/book/02-repl-calculator.md`), para facilitar a manutenção lado a lado, mas os nomes de arquivo em si são traduzidos.

## Adicionando conteúdo

Crie ou edite um arquivo Markdown em `pt/book/` (e o correspondente em `en/book/`, se for atualizar a tradução) e adicione-o na posição desejada das listas `project.toc` e `project.exports[].articles` no `myst.yml` daquele idioma. Para uma célula Julia executável, use:

````markdown
```{code-cell} julia
1 + 2
```
````

Use `{ref}` e rótulos MyST para referências cruzadas, por exemplo:

```markdown
(minha-secao)=
## Minha seção
```

Rótulos (`(minha-secao)=`) não precisam ser traduzidos entre os idiomas. Apenas mantenha o mesmo rótulo consistente dentro de cada arquivo/idioma.

## Validação local

Execute os mesmos passos do CI para cada idioma:

```bash
cd pt
micromamba run --name jubook jupyter book build --execute --html
micromamba run --name jubook jupyter book build --typst --execute
cd ../en
micromamba run --name jubook jupyter book build --execute --html
micromamba run --name jubook jupyter book build --typst --execute
```

O site de cada idioma será gerado em `pt/_build/html/` e `en/_build/html/`, e os PDFs em `pt/_build/exports/livro-intro-comp-julia.pdf` e `en/_build/exports/intro-computing-julia.pdf`.
