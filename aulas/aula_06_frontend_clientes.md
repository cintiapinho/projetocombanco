# Aula 6 — Frontend de Clientes

## O que vamos fazer nessa aula

1. Entender o que é CORS e por que ele existe
2. Configurar o CORS no `main.py`
3. Criar o `clientes.html` com a estrutura da página
4. Criar o `js/clientes.js` com toda a lógica
5. Testar tudo no navegador

---

## Parte 1 — O que é CORS?

Até agora testamos a API pelo `/docs`. Mas numa aplicação real, quem chama a API é uma página HTML.

O problema: o navegador bloqueia por segurança chamadas entre origens diferentes.

**Origem** = protocolo + endereço + porta. Exemplos:
- `file://` → arquivo aberto direto no computador
- `http://127.0.0.1:8000` → API rodando no servidor
- `http://meusite.com` → site em produção

Quando o HTML e a API estão em origens diferentes, o navegador barra a requisição e mostra um erro de **CORS**.

Isso é proposital — imagina se qualquer site pudesse chamar a API do seu banco sem permissão.

**CORS** (Cross-Origin Resource Sharing) é o mecanismo que permite ou bloqueia essas chamadas. Nós configuramos na API quais origens têm permissão.

---

## Parte 2 — Configurando o CORS no main.py

Abra o `backend/main.py` e substitua o conteúdo:

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware  # importa o middleware de CORS
from rotas import clientes

app = FastAPI()

# Configura quais origens têm permissão de chamar a API
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],      # "*" = qualquer origem (usado em desenvolvimento)
    allow_methods=["*"],      # permite todos os métodos (GET, POST, PUT, DELETE)
    allow_headers=["*"],      # permite todos os cabeçalhos
)

app.include_router(clientes.router)


@app.get("/")
def inicio():
    return {"mensagem": "API do estúdio de tatuagem funcionando!"}
```

> Em produção, `allow_origins=["*"]` seria substituído pelo endereço real do seu site,
> por exemplo: `allow_origins=["https://meusite.com"]`.
> Para desenvolvimento, `"*"` libera tudo e facilita os testes.

Salve o arquivo — o uvicorn vai reiniciar automaticamente.

---

## Parte 3 — Criando o HTML

Crie o arquivo `frontend/clientes.html`.

O HTML cuida apenas da **estrutura da página** — títulos, tabela, formulário.
A lógica (buscar dados, cadastrar, excluir) vai ficar no arquivo JS separado.

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Clientes — Estúdio de Tatuagem</title>
  <!-- Bootstrap: biblioteca de estilos prontos -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="container py-4">

  <h1>Clientes</h1>

  <!-- Formulário para cadastrar novo cliente -->
  <form id="form-cliente" class="row g-2 mt-2 mb-4">
    <div class="col-md-3">
      <input type="text" id="nome" class="form-control" placeholder="Nome" required>
    </div>
    <div class="col-md-2">
      <input type="text" id="cpf" class="form-control" placeholder="CPF (só números)" required>
    </div>
    <div class="col-md-2">
      <input type="text" id="telefone" class="form-control" placeholder="Telefone">
    </div>
    <div class="col-md-2">
      <input type="text" id="cidade" class="form-control" placeholder="Cidade">
    </div>
    <div class="col-md-1">
      <input type="text" id="uf" class="form-control" placeholder="UF" maxlength="2">
    </div>
    <div class="col-md-2">
      <button type="submit" class="btn btn-primary w-100">Cadastrar</button>
    </div>
  </form>

  <!-- Tabela onde os clientes vão aparecer -->
  <table class="table table-bordered">
    <thead>
      <tr>
        <th>ID</th>
        <th>Nome</th>
        <th>CPF</th>
        <th>Telefone</th>
        <th>Cidade</th>
        <th>UF</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody id="corpo-tabela">
      <!-- os clientes vão aparecer aqui -->
    </tbody>
  </table>

  <!-- Importa o arquivo JavaScript separado -->
  <script src="js/clientes.js"></script>

</body>
</html>
```

**Teste:** abra o `clientes.html` no navegador agora. A tabela aparece vazia — normal, ainda não criamos o JS.

---

## Parte 4 — Criando o JavaScript

Crie o arquivo `frontend/js/clientes.js`. Vamos construir função por função.

### 4.1 — Listar clientes

Cole no `js/clientes.js` e salve:

```javascript
const API = 'http://127.0.0.1:8000'  // endereço da nossa API

// Busca todos os clientes e preenche a tabela
async function listarClientes() {
  const resposta = await fetch(`${API}/clientes`)  // chama GET /clientes
  const clientes = await resposta.json()           // converte para JSON

  const corpo = document.getElementById('corpo-tabela')
  corpo.innerHTML = ''  // limpa a tabela antes de preencher

  clientes.forEach(c => {
    corpo.innerHTML += `
      <tr>
        <td>${c.idcliente}</td>
        <td>${c.nome}</td>
        <td>${c.cpf}</td>
        <td>${c.telefone ?? '-'}</td>
        <td>${c.cidade ?? '-'}</td>
        <td>${c.uf ?? '-'}</td>
        <td>
          <button class="btn btn-sm btn-danger" onclick="deletarCliente(${c.idcliente})">Excluir</button>
        </td>
      </tr>`
  })
}

listarClientes()  // chama a função quando a página carrega
```

**Teste:** recarregue o `clientes.html` no navegador. Os clientes do banco devem aparecer na tabela.

---

### 4.2 — Cadastrar cliente

Adicione abaixo no `js/clientes.js`:

```javascript
// Envia o formulário e cadastra um novo cliente
document.getElementById('form-cliente').addEventListener('submit', async (e) => {
  e.preventDefault()  // impede o formulário de recarregar a página

  const cliente = {
    nome:         document.getElementById('nome').value,
    cpf:          document.getElementById('cpf').value,
    telefone:     document.getElementById('telefone').value || null,
    datacadastro: new Date().toISOString().split('T')[0],  // data de hoje: AAAA-MM-DD
    cidade:       document.getElementById('cidade').value || null,
    uf:           document.getElementById('uf').value || null
  }

  await fetch(`${API}/clientes`, {
    method: 'POST',                               // método POST
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(cliente)                 // converte o objeto para JSON
  })

  e.target.reset()   // limpa o formulário
  listarClientes()   // atualiza a tabela
})
```

**Teste:** preencha o formulário e clique em Cadastrar. O novo cliente deve aparecer na tabela.

---

### 4.3 — Excluir cliente

Adicione abaixo no `js/clientes.js`:

```javascript
// Deleta um cliente pelo ID
async function deletarCliente(id) {
  if (!confirm('Tem certeza que deseja excluir este cliente?')) return

  await fetch(`${API}/clientes/${id}`, { method: 'DELETE' })
  listarClientes()  // atualiza a tabela
}
```

**Teste:** clique em Excluir em qualquer cliente. Ele deve sumir da tabela.

---

## Como o fetch funciona

O `fetch` é a função JavaScript que faz requisições HTTP — é o equivalente do `/docs`, mas dentro do código.

| O que fizemos     | Método   | Equivalente na API      |
|-------------------|----------|-------------------------|
| Carregar a tabela | `GET`    | `GET /clientes`         |
| Cadastrar         | `POST`   | `POST /clientes`        |
| Excluir           | `DELETE` | `DELETE /clientes/{id}` |

---

## Estrutura do projeto até agora

```
estudio-tatuagem-api/
├── aulas/
├── backend/
│   ├── rotas/
│   │   ├── __init__.py
│   │   └── clientes.py
│   ├── venv/
│   ├── .env
│   ├── database.py
│   ├── main.py              ← atualizado (CORS)
│   └── requirements.txt
├── frontend/
│   ├── js/
│   │   └── clientes.js      ← novo
│   └── clientes.html        ← novo
├── .gitignore
└── REGRAS.md
```

---

## Próxima aula

Na **Aula 7** vamos criar o CRUD de tatuadores, que tem uma chave estrangeira com a tabela `senioridade`.
