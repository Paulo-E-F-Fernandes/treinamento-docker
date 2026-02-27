# Treinamento Docker

Projeto criado para centralizar as informações do treinamendo em Docker.

Foi utilizado como base de conhecimento para esse projeto de treinamento, o curso `Aprenda DOCKER e contêineres de 
maneira simples e rápida` da [Udemy](https://www.udemy.com/). 

## Treinamento

> [O que é o DOCKER?](docs/training/01-what-is-docker.md)

> [Instalando o DOCKER](docs/training/02-installing-docker.md)

> [Download das primeiras imagens](docs/training/03-download-first-image.md)

## Estrutura de Diretórios e Arquivos

### Nomenclatura de Arquivos

A regra de ouro é a **legibilidade para máquinas e humanos**. Evite espaços e caracteres especiais para prevenir erros 
em servidores web ou sistemas operacionais distintos.

* **Tudo em minúsculas:** Use apenas letras minúsculas (ex: `guia-instalacao.md` em vez de `Guia-Instalacao.md`).
* **Hífens ou Underscores:** Use o traço/hífen (-) ou o sublinhado/underscore (_) para separar palavras. O hífen é 
  geralmente preferido para _**URLs**_ e _**SEO**_ (_Search Engine Optimization_ - Otimização para Motores de Busca).
* **Sem caracteres especiais:** Evite acentos, cedilhas ou símbolos como !, @, #, $.
* **Datas em ISO 8601:** Se precisar de datas no nome, use o formato _YYYY-MM-DD_ para garantir a ordenação cronológica
  correta na pasta, por exemplo:
  * ✅ `documentacao-projeto.md`
  * ✅ `2024-02-15-relatorio-mensal.md`
  * ❌ `Relatório de Projeto!.md`

### Estrutura de Diretórios

Para projetos maiores, como documentação de software, a organização lógica é essencial:

* **Ponto de Entrada:** Todo projeto deve ter um arquivo _**README.md**_ na raiz, que serve como a página inicial e introdução.
* **Subpastas Temáticas:** Agrupe arquivos relacionados em pastas (ex: _docs/_, _tutoriais/_, _assets/_ para imagens).
* **Ordenação Numérica:** Se os arquivos seguem uma sequência (como capítulos de um livro), prefixe-os com números:
  * `01-introducao.md`
  * `02-configuracao.md`
  * `03-uso-avancado.md`

```markdown
meu-projeto/
├── .github/                # Configurações de automação
├── assets/                 # Arquivos de mídia (não-texto)
│   ├── images/            # Prints de tela e diagramas
│   └── logos/              # Logotipos do projeto
├── docs/                   # Documentação detalhada do projeto
│   ├── 01-guia-usuario/    # Subpasta temática numerada
│   │   ├── instalacao.md
│   │   └── configuracao.md
│   ├── 02-tecnico/
│   │   ├── arquitetura.md
│   │   └── api-reference.md
│   └── faq.md
├── CHANGELOG.md            # Histórico de versões e mudanças
├── CONTRIBUTING.md         # Regras para novos colaboradores
├── LICENSE                 # Licença de uso
└── README.md               # Página inicial e visão geral
```