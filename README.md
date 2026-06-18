# TicketZone 🎟️ — Sistema de Vendas de Ingressos

Sistema Android em Java com autenticação de usuários e CRUD completo de ingressos, usando banco de dados SQLite nativo. Interface com tema roxo/dourado inspirada em ticketeiras profissionais.

---

## Estrutura do Projeto

```
app/src/main/
├── java/com/example/ticketapp/
│   ├── activities/
│   │   ├── LoginActivity.java          ← Tela de login
│   │   ├── RegisterActivity.java       ← Tela de cadastro
│   │   ├── MainActivity.java           ← Lista de ingressos (CRUD + Busca)
│   │   └── TicketFormActivity.java     ← Formulário adicionar/editar ingresso
│   ├── adapters/
│   │   └── TicketAdapter.java          ← RecyclerView adapter com badges de status
│   ├── database/
│   │   └── DatabaseHelper.java         ← SQLiteOpenHelper (banco de dados)
│   └── models/
│       ├── User.java                   ← Modelo de usuário
│       └── Ticket.java                 ← Modelo de ingresso
└── res/
    ├── layout/
    │   ├── activity_login.xml
    │   ├── activity_register.xml
    │   ├── activity_main.xml
    │   ├── activity_ticket_form.xml
    │   └── item_ticket.xml
    ├── menu/
    │   └── main_menu.xml
    ├── drawable/
    │   ├── bg_header.xml               ← Gradiente roxo (toolbar e cards)
    │   ├── bg_badge.xml                ← Badge dourado (categoria)
    │   ├── bg_badge_green.xml          ← Badge disponível
    │   ├── bg_badge_orange.xml         ← Badge poucos ingressos
    │   ├── bg_badge_red.xml            ← Badge esgotado
    │   ├── bg_search.xml               ← Fundo da barra de busca
    │   ├── ic_ticket.xml, ic_edit.xml,
    │   │   ic_delete.xml, ic_add.xml,
    │   │   ic_back.xml, ic_search.xml,
    │   │   ic_logout.xml, ic_clear.xml ← Ícones vetoriais
    │   └── ...
    ├── mipmap-*/                        ← Ícones do launcher (tema roxo/dourado)
    └── values/
        ├── colors.xml
        ├── strings.xml
        └── themes.xml
```

---

## Como importar no Android Studio

1. Extraia o ZIP baixado
2. Abra o Android Studio (Canary ou versão estável)
3. **File → Open** → selecione a pasta `TicketApp`
4. Aguarde o Gradle sincronizar — clique em **Sync Now** se solicitado
5. Conecte um dispositivo ou inicie um emulador (API 24+)
6. Clique em **Run ▶**

> **Versão mínima recomendada:** Android Studio Hedgehog (2023.1.1) ou superior.
> O projeto inclui `gradle/wrapper/gradle-wrapper.properties` que fixa automaticamente o Gradle 8.9 — não é necessário configurar manualmente.

---

## Banco de Dados (SQLite)

### Tabela `users`

| Coluna   | Tipo    | Restrição        | Descrição              |
|----------|---------|------------------|------------------------|
| id       | INTEGER | PK AUTOINCREMENT | Identificador único    |
| name     | TEXT    | NOT NULL         | Nome completo          |
| email    | TEXT    | UNIQUE NOT NULL  | E-mail de login        |
| password | TEXT    | NOT NULL         | Senha do usuário       |

### Tabela `tickets`

| Coluna     | Tipo    | Restrição        | Descrição                    |
|------------|---------|------------------|------------------------------|
| id         | INTEGER | PK AUTOINCREMENT | Identificador único          |
| event_name | TEXT    | NOT NULL         | Nome do evento               |
| location   | TEXT    |                  | Local do evento              |
| event_date | TEXT    |                  | Data do evento (dd/mm/aaaa)  |
| category   | TEXT    |                  | Categoria (Show, Teatro etc) |
| price      | REAL    | NOT NULL         | Valor unitário do ingresso   |
| quantity   | INTEGER | NOT NULL         | Quantidade disponível        |

---

## Funcionalidades

### Autenticação
- ✅ Cadastro com validação (nome, e-mail válido, senha ≥ 6 chars, confirmação)
- ✅ Detecção de e-mail duplicado antes de salvar
- ✅ Login com verificação no banco de dados
- ✅ Sessão persistida com SharedPreferences (mantém login ao fechar o app)
- ✅ Logout com diálogo de confirmação

### CRUD de Ingressos
- ✅ **Create** — Cadastrar novo ingresso com evento, local, data, categoria, preço e quantidade
- ✅ **Read** — Listar todos os ingressos em cards com header roxo
- ✅ **Update** — Editar ingresso existente (campos pré-preenchidos)
- ✅ **Delete** — Excluir ingresso com confirmação
- ✅ **Search** — Barra de busca sempre visível, filtra em tempo real por nome do evento ou local

### Interface
- ✅ Tema roxo `#6A0DAD` e dourado `#FFD700`
- ✅ Badge de status colorido por quantidade: **verde** (disponível) / **laranja** (poucos) / **vermelho** (esgotado)
- ✅ Dropdown de categorias: Show, Festival, Teatro, Cinema, Esporte, Stand-up, Outro
- ✅ Ícones launcher personalizados no tema do app

---

## Dependências (`app/build.gradle`)

```gradle
implementation 'androidx.appcompat:appcompat:1.7.0'
implementation 'com.google.android.material:material:1.12.0'
implementation 'androidx.recyclerview:recyclerview:1.3.2'
implementation 'androidx.cardview:cardview:1.0.0'
implementation 'androidx.coordinatorlayout:coordinatorlayout:1.2.0'
implementation 'androidx.constraintlayout:constraintlayout:2.2.0'
implementation 'androidx.core:core:1.13.1'
```

> O banco SQLite é nativo do Android — não precisa de dependência extra.

---

## Versões do build

| Componente        | Versão  |
|-------------------|---------|
| AGP               | 8.7.3   |
| Gradle            | 8.9     |
| compileSdk        | 35      |
| targetSdk         | 35      |
| minSdk            | 24      |
| Java              | 11      |

---

## Como visualizar o banco de dados

**Método 1 — App Inspection (recomendado, somente emulador):**
1. Rode o app no emulador
2. **View → Tool Windows → App Inspection**
3. Aba **Database Inspector** → selecione `com.example.ticketapp`
4. Explore as tabelas `users` e `tickets`

**Método 2 — ADB + DB Browser for SQLite:**
```bash
adb shell run-as com.example.ticketapp \
    cp /data/data/com.example.ticketapp/databases/TicketApp.db /sdcard/
adb pull /sdcard/TicketApp.db .
# Abra TicketApp.db no DB Browser for SQLite (sqlitebrowser.org)
```

---

## Observações

- **Senha**: armazenada em texto puro para fins didáticos. Em produção, use hashing (BCrypt ou SHA-256).
- **Sessão**: gerenciada com SharedPreferences. Em produção, considere tokens JWT ou Firebase Auth.
- **Banco de dados**: arquivo SQLite local em `/data/data/com.example.ticketapp/databases/TicketApp.db`.
- **Busca**: filtra simultaneamente por `event_name` e `location` usando `LIKE` no SQLite.
