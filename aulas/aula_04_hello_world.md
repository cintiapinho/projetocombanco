# Aula 4 — Primeiro endpoint

## O que vamos fazer nessa aula

1. Entender o que é o FastAPI e o uvicorn
2. Criar o arquivo `main.py`
3. Fazer a API responder no navegador
4. Conhecer a documentação automática

---

## Parte 1 — O que é o FastAPI e o uvicorn?

Lembra da analogia do restaurante da Aula 1?

- **FastAPI** é o garçom — ele recebe os pedidos (requisições) e sabe o que fazer com cada um
- **uvicorn** é a porta do restaurante — ele fica "ouvindo" as chamadas que chegam e passa para o FastAPI

Você sempre usa os dois juntos. O uvicorn abre a porta, o FastAPI atende.

---

## Parte 2 — Criando o main.py

Crie um arquivo chamado `main.py` dentro da pasta `backend/`.

```python
from fastapi import FastAPI  # importa o FastAPI

app = FastAPI()              # cria a aplicação


@app.get("/")                # define que essa função responde ao endereço "/"
def inicio():                # quando alguém acessar "/", essa função vai rodar
    return {"mensagem": "API do estúdio de tatuagem funcionando!"}
```

### O que cada parte faz

| Parte | O que é |
|-------|---------|
| `from fastapi import FastAPI` | Importa o FastAPI que instalamos |
| `app = FastAPI()` | Cria a sua API — é como "ligar" o garçom |
| `@app.get("/")` | Diz: "quando alguém fizer um GET no endereço `/`, execute a função abaixo" |
| `return {...}` | O que a API vai devolver — um JSON |

---

## Parte 3 — Rodando a API

Com o venv ativado, entre na pasta `backend` e execute:

```powershell
cd backend
uvicorn main:app --reload
```

### O que significa esse comando

| Parte | Significado |
|-------|-------------|
| `uvicorn` | O servidor que vai rodar a API |
| `main` | O nome do arquivo Python (`main.py`) |
| `app` | O nome da variável que criamos (`app = FastAPI()`) |
| `--reload` | Reinicia automaticamente quando você salvar o arquivo |

Se tudo estiver certo, vai aparecer isso no terminal:

```
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process
```

---

## Parte 4 — Acessando no navegador

Abra o navegador e acesse:

```
http://127.0.0.1:8000
```

Você vai ver o JSON que a sua API retornou:

```json
{"mensagem": "API do estúdio de tatuagem funcionando!"}
```

> `127.0.0.1` é o endereço do seu próprio computador.
> `8000` é a porta onde o uvicorn está ouvindo.

---

## Parte 5 — Documentação automática

Essa é uma das melhores funcionalidades do FastAPI.
Acesse no navegador:

```
http://127.0.0.1:8000/docs
```

O FastAPI gera automaticamente uma página onde você pode:
- Ver todos os endpoints da API
- Testar cada endpoint direto pelo navegador
- Ver o formato dos dados que cada endpoint retorna

Não precisamos criar essa página — o FastAPI faz isso sozinho.

---

## Para encerrar o servidor

No terminal, pressione **CTRL + C**.

---

## Estrutura do projeto até agora

```
estudio-tatuagem-api/
├── aulas/
├── backend/
│   ├── main.py          ← novo
│   └── requirements.txt
├── frontend/
│   ├── venv/
├── .gitignore
└── REGRAS.md
```

---

## Próxima aula

Na **Aula 5** vamos conectar a API ao banco de dados e criar o CRUD completo de clientes.
