# Como contribuir

## Requisitos

- [Quarto](https://quarto.org/)
  - Testado na versão 1.6.40
- [Julia](https://julialang.org/)
  - Testado na versão 1.11.2
- Distribuição de LaTeX

## Instalação (Recomendada)

Atualmente, o Quarto suporta a execução de códigos Julia sem a necessidade de instalar o Jupyter (através da engine `julia`). Tudo deve funcionar depois de instalar o Quarto e o Julia, sem a necessidade de instalar os componentes adicionais.

## Instalação (Jupyter)

Após a instalação de Julia e Quarto, será preciso instalar alguns componentes para fazê-los funcionar juntos. Esses componentes são

- IJulia
- Revise.jl
- Jupyter Cache

As instruções podem ser encontradas na documentação do Quarto em [Using Julia](https://quarto.org/docs/computations/julia.html). Recomendo instalar o ecossistema Jupyter através do pacote IJulia (veja instruções no link anterior).

## Estrutura do repositório

Os diretórios são estruturado da seguinte forma:

- `_book/`: diretório onde o PDF e o HTML são gerados. Versionar apenas o PDF.
- `_freeze/`: diretório onde o cache das execuções do Quarto são armazenadas. Versionar.
- `.github/`: diretório com arquivos de configuração do GitHub. Contém o workflow que faz o deploy do livro no GitHub Pages.

No diretório raiz, temos os seguintes arquivos:

- `.gitignore`: arquivo de configuração do git para ignorar arquivos e diretórios.
- `CONTRIBUTING.md`: este arquivo.
- `README.md`: arquivo de apresentação do repositório.
- `_quarto.yml`: arquivo de configuração do Quarto.
- `Project.toml` e `Manifest.toml`: arquivos de configuração do Julia.
- `index.qmd`: arquivo obrigatório que contém a página inicial do livro.
- `agradecimentos.qmd`: arquivo que contém os agradecimentos do livro.
- `sobre-o-curso.qmd`: arquivo que contém informações sobre o curso.
- `XX-nome-do-capitulo.qmd`: arquivos de capítulos do livro.

## Adicionando um novo capítulo

Para adicionar um novo capítulo, crie um arquivo `.qmd` no diretório raíz. Além disso, adicione uma nova entrada no arquivo `_quarto.yml` na entrada `book.chapters`. O Quarto irá renderizar os capítulos na ordem em que eles aparecem no arquivo de configuração.

Caso o capítulo utilize algum pacote Julia, adicione a dependência da seguinte forma:

- Execute o comando `julia --project=@.` para abrir o REPL do Julia no contexto do projeto.
- Entre no modo de pacotes pressionando `]`.
- Adicione o pacote com o comando `add NomeDoPacote`.

## Subindo as alterações

Após adicionar um novo capítulo e antes de subir as alterações para o repositório, execute o comando `quarto render --to all` para gerar o PDF e o HTML do livro. Versione apenas o PDF no diretório `_book/` e o cache no diretório `_freeze/`.