# Android CRUD App com Login — SQLite

Sistema Android em Java com autenticação de usuários e CRUD completo de produtos, usando banco de dados SQLite nativo.

---

## Estrutura do Projeto

```
app/src/main/
├── java/com/example/crudapp/
│   ├── activities/
│   │   ├── LoginActivity.java         ← Tela de login
│   │   ├── RegisterActivity.java      ← Tela de cadastro
│   │   ├── MainActivity.java          ← Lista de produtos (CRUD)
│   │   └── ProductFormActivity.java   ← Formulário adicionar/editar
│   ├── adapters/
│   │   └── ProductAdapter.java        ← RecyclerView adapter
│   ├── database/
│   │   └── DatabaseHelper.java        ← SQLiteOpenHelper (banco de dados)
│   └── models/
│       ├── User.java                  ← Modelo de usuário
│       └── Product.java               ← Modelo de produto
└── res/
    ├── layout/
    │   ├── activity_login.xml
    │   ├── activity_register.xml
    │   ├── activity_main.xml
    │   ├── activity_product_form.xml
    │   └── item_product.xml
    ├── menu/
    │   └── main_menu.xml
    ├── drawable/
    │   ├── ic_logo.xml
    │   ├── ic_edit.xml
    │   └── ic_delete.xml
    └── values/
        ├── colors.xml
        ├── strings.xml
        └── themes.xml
```

---

## Como importar no Android Studio 2023.1.1

1. Abra o Android Studio
2. **File → Open** → selecione a pasta `AndroidCRUD`
3. Aguarde o Gradle sincronizar
4. Conecte um dispositivo ou inicie um emulador (API 24+)
5. Clique em **Run ▶**

---

## Banco de Dados (SQLite)

### Tabela `users`
| Coluna   | Tipo    | Descrição             |
|----------|---------|-----------------------|
| id       | INTEGER | Chave primária (auto) |
| name     | TEXT    | Nome do usuário       |
| email    | TEXT    | E-mail (único)        |
| password | TEXT    | Senha                 |

### Tabela `products`
| Coluna      | Tipo    | Descrição             |
|-------------|---------|-----------------------|
| id          | INTEGER | Chave primária (auto) |
| name        | TEXT    | Nome do produto       |
| description | TEXT    | Descrição             |
| price       | REAL    | Preço                 |
| quantity    | INTEGER | Quantidade em estoque |

---

## Funcionalidades

### Autenticação
- ✅ Cadastro de usuários com validação (nome, e-mail, senha ≥ 6 chars)
- ✅ Login com verificação no banco de dados
- ✅ Sessão persistida com SharedPreferences
- ✅ Logout com confirmação

### CRUD de Produtos
- ✅ **Create** — Adicionar novo produto
- ✅ **Read** — Listar todos os produtos
- ✅ **Update** — Editar produto existente
- ✅ **Delete** — Excluir produto com confirmação
- ✅ **Search** — Busca por nome ou descrição

---

## Dependências (app/build.gradle)

```gradle
implementation 'androidx.appcompat:appcompat:1.6.1'
implementation 'com.google.android.material:material:1.10.0'
implementation 'androidx.recyclerview:recyclerview:1.3.2'
implementation 'androidx.cardview:cardview:1.0.0'
implementation 'androidx.coordinatorlayout:coordinatorlayout:1.2.0'
```

> O banco SQLite é nativo do Android — não precisa de dependência extra.

---

## Observações

- **Senha**: armazenada em texto puro para simplicidade. Em produção, use hashing (BCrypt, SHA-256).
- **Sessão**: gerenciada com SharedPreferences. Em produção, considere tokens JWT ou Firebase Auth.
- **Banco de dados**: arquivo SQLite local no dispositivo (`/data/data/com.example.crudapp/databases/CrudApp.db`).
