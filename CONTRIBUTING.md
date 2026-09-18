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

- `book/`: fontes Markdown/MyST do livro.
- `myst.yml`: ordem do livro, tema do site e exportação do PDF.
- `_build/`: artefatos gerados; não versionar.
- `Project.toml` e `Manifest.toml`: ambiente Julia usado pelas células.
- `.github/workflows/publish.yml`: publicação do site e do PDF no GitHub Pages.

## Adicionando conteúdo

Crie ou edite um arquivo Markdown em `book/` e adicione-o na posição desejada das listas `project.toc` e `project.exports[].articles` em `myst.yml`. Para uma célula Julia executável, use:

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

## Validação local

Execute os mesmos passos do CI:

```bash
micromamba run --name jubook jupyter book build --execute --html
micromamba run --name jubook jupyter book build --typst --execute
```

O site será gerado em `_build/html/` e o PDF em `_build/exports/livro-intro-comp-julia.pdf`.
