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

```powershell
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

Crie a pasta backend/ na raiz do projeto

O venv fica dentro da pasta `backend/` porque é lá que está o código Python:
Abra o terminal e troque do PowerShell para o Command Prompt

Entre com cd backend

```powershell
python -m venv venv
```

### Ativando o venv

Você precisa ativar a caixinha antes de usar. **Faça isso toda vez que for trabalhar no projeto.**

```powershell
.\venv\Scripts\Activate.ps1
```

> Se aparecer erro de "política de execução", rode isso uma vez e confirme com `S`:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
> Depois tente ativar de novo.

Quando ativado, o terminal vai mostrar `(venv)` no início da linha:

```
(venv) PS C:\Users\Aluno\Desktop\estudio-tatuagem-api>
```

### Por que o venv não vai pro GitHub?

Repare que já existe um arquivo `.gitignore` na pasta do projeto.
Ele diz ao Git para ignorar a pasta `backend/venv/` — e está certo.

A pasta `venv` tem centenas de arquivos e pesa muito. Não faz sentido mandar pro GitHub porque qualquer pessoa pode recriar o venv com um único comando.

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

```powershell
pip install fastapi uvicorn mysql-connector-python python-dotenv
```

### Salvando as dependências

O arquivo `requirements.txt` fica dentro de `backend/` e lista tudo que o projeto precisa.

```powershell
pip freeze > requirements.txt
```

> **Como usar depois:** quem clonar o projeto roda dentro da pasta backend `pip install -r requirements.txt` e já tem tudo instalado.

---

## Parte 4 — Salvando no GitHub

Agora vamos registrar a instalação no seu repositório

`Lembrando que deve criar seu resitório sem o readme`


```Command Prompt
git remote set-url origin "caminho do seu repositorio"
git add .
git commit -m "criando estrutura basica"
git push
```
no processo vai er que validar suas credenciais
---

## Estrutura do projeto até agora

```
estudio-tatuagem-api/
├── aulas/                      ← material das aulas
├── backend/
│   ├── venv/                   ← ambiente virtual (não vai pro GitHub)
│   └── requirements.txt        ← bibliotecas instaladas
├── frontend/
├── .gitignore                  ← diz ao Git o que ignorar
└── REGRAS.md                   ← regras de código do projeto
```

---

## Se der erro — resolução de problemas

### Erro ao instalar as bibliotecas (pydantic-core / Rust not found)

Esse erro acontece quando o Python usado para criar o venv não é o oficial.
Nessas máquinas o Inkscape instala um Python próprio que aparece primeiro e atrapalha.

**Como resolver:**

1. Apague o venv que foi criado errado:
```powershell
Remove-Item -Recurse -Force backend\venv
```

2. Recrie o venv forçando o Python oficial:
```powershell
C:\Python310\python.exe -m venv backend\venv --system-site-packages
```

3. Ative e instale normalmente:
```powershell
.\backend\venv\Scripts\Activate.ps1
pip install fastapi uvicorn mysql-connector-python python-dotenv
```

---

### Erro ao ativar o venv (política de execução)

```
não é possível carregar o arquivo ... Activate.ps1 porque a execução de scripts foi desabilitada
```

**Como resolver** (fazer só uma vez por máquina):
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
Confirme com `S` e tente ativar de novo.

---

## Toda vez que voltar ao laboratório

As máquinas do laboratório não salvam o que foi feito. Então **a cada aula** você vai precisar repetir esses passos antes de continuar.

### Passo 1 — Clone o repositório de novo

```powershell
cd Desktop
git clone https://github.com/usuario/estudio-tatuagem-api.git
cd estudio-tatuagem-api
```

> Se a pasta já existir no Desktop do dia anterior, apague e clone de novo — assim você garante que está com a versão mais atualizada.

### Passo 2 — Crie e ative o venv

```powershell
C:\Python310\python.exe -m venv backend\venv --system-site-packages
.\backend\venv\Scripts\Activate.ps1
```

### Passo 3 — Instale as dependências

Toda vez que você cria um venv novo, ele começa vazio — os pacotes instalados antes não existem mais.
Por isso precisamos reinstalar a cada aula.

```powershell
pip install -r backend\requirements.txt
```

Esse comando lê o `requirements.txt` e reinstala tudo de uma vez automaticamente.

Pronto. A partir daqui é só continuar de onde parou.

---

## Próxima aula

Na **Aula 4** vamos criar o primeiro arquivo Python e fazer a API responder pela primeira vez.
