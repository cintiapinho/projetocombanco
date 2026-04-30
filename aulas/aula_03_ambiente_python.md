# Aula 3 — Preparando o ambiente

## O que vamos fazer nessa aula

1. Clonar o projeto do GitHub
2. Criar o ambiente virtual Python (venv)
3. Instalar o FastAPI e salvar as dependências

---

## Parte 1 — Clonando o projeto

### O que é clonar?

Clonar = copiar o repositório do GitHub para o seu computador.
Você vai ter todos os arquivos das aulas na sua máquina e poderá acompanhar o projeto do zero.

### Como clonar

1. Acesse o link do repositório que a professora vai compartilhar
2. Clique no botão verde **Code**
3. Copie o link que aparece (começa com `https://`)
4. Abra o terminal e execute:

```bash
cd Desktop
git clone https://github.com/usuario/estudio-tatuagem-api.git
cd estudio-tatuagem-api
```

> Substitua o link pelo que a professora compartilhou.

Agora você tem a pasta do projeto no Desktop com todas as aulas dentro.

---

## Parte 2 — Ambiente virtual (venv)

### Por que criar um ambiente virtual?

Imagine que você tem dois projetos no computador:
- Projeto A usa a versão 1.0 de uma biblioteca
- Projeto B usa a versão 2.0 da mesma biblioteca

Se você instalar tudo junto no Python da máquina, os projetos vão se misturar e quebrar.

O **venv** cria uma "caixinha" separada para cada projeto. As bibliotecas instaladas dentro dela não afetam nada fora.

**Regra:** sempre crie um venv antes de instalar qualquer biblioteca.

### Criando o venv

No terminal, dentro da pasta do projeto:

```bash
python -m venv venv
```

Isso cria uma pasta chamada `venv` com um Python isolado só para esse projeto.

### Ativando o venv

Você precisa ativar a caixinha antes de usar. **Faça isso toda vez que for trabalhar no projeto.**

**Windows:**
```bash
venv\Scripts\activate
```

**Mac/Linux:**
```bash
source venv/bin/activate
```

Quando ativado, o terminal vai mostrar `(venv)` no início da linha:

```
(venv) C:\Users\Aluno\Desktop\estudio-tatuagem-api>
```

### Por que o venv não vai pro GitHub?

Repare que já existe um arquivo `.gitignore` na pasta do projeto.
Ele diz ao Git para ignorar a pasta `venv/` — e está certo.

A pasta `venv` tem centenas de arquivos e pesa muito. Não faz sentido mandar pro GitHub porque qualquer pessoa pode recriar o venv com um único comando (você vai ver isso a seguir).

---

## Parte 3 — Instalando as bibliotecas

### O que vamos instalar

| Biblioteca               | Para que serve                            |
|--------------------------|-------------------------------------------|
| `fastapi`                | O framework para criar a API              |
| `uvicorn`                | O servidor que vai rodar a API            |
| `mysql-connector-python` | Conecta o Python ao banco MySQL           |
| `python-dotenv`          | Lê configurações do arquivo `.env`        |

### Instalando

Com o venv ativado, execute:

```bash
pip install fastapi uvicorn mysql-connector-python python-dotenv
```

### Salvando as dependências

O arquivo `requirements.txt` lista tudo que o projeto precisa.
Assim, qualquer pessoa pode instalar tudo com um único comando.

```bash
pip freeze > requirements.txt
```

> **Como usar depois:** quem clonar o projeto roda `pip install -r requirements.txt` e já tem tudo instalado — sem precisar lembrar o nome de cada biblioteca.

---

## Parte 4 — Salvando no GitHub

Agora vamos registrar a instalação no histórico do projeto.

```bash
git add requirements.txt
git commit -m "adiciona dependências do projeto"
git push
```

- `git add` → seleciona os arquivos que queremos salvar
- `git commit` → cria um "ponto de salvamento" com uma mensagem
- `git push` → envia para o GitHub

---

## Estrutura do projeto até agora

```
estudio-tatuagem-api/
├── aulas/                  ← material das aulas
├── venv/                   ← ambiente virtual (não vai pro GitHub)
├── .gitignore              ← diz ao Git o que ignorar
├── requirements.txt        ← bibliotecas instaladas
└── REGRAS.md               ← regras de código do projeto
```

---

## Próxima aula

Na **Aula 4** vamos criar o primeiro arquivo Python e fazer a API responder pela primeira vez.
