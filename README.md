# Sistema de Gestão de Biblioteca

Um sistema completo, moderno e robusto para o controle de acervo, gerenciamento de usuários e motores dinâmicos de empréstimos e devoluções. O projeto adota uma arquitetura nativa em ASP.NET Core MVC com renderização de Views Razor do lado do servidor, integrando persistência de dados local segura e controle estrito de regras de negócio.

## 🚀 Visão Geral e Contexto
Este sistema foi desenvolvido para substituir processos manuais e planilhas descentralizadas, eliminando a perda recorrente de informações e otimizando o fluxo de atendimento diário.

### Objetivos Principais
* Disponibilização de consulta de acervo em tempo real com cálculo dinâmico de disponibilidade.
* Redução drástica do tempo médio para o registro de empréstimos e devoluções.
* Garantia da integridade física e lógica dos dados por meio de histórico e logs automatizados.

---

## 🛠️ Tecnologias e Arquitetura

O projeto adota o padrão de arquitetura **MVC (Model-View-Controller)** puro do ecossistema .NET:

* **Backend:** .NET Core / ASP.NET Core MVC
* **Persistência de Dados:** Entity Framework Core
* **Banco de Dados:** SQLite (`library.db`)
* **Frontend:** Razor Views, Bootstrap 5, JavaScript (ES6+)
* **Gerenciamento de Estado:** ASP.NET Core Session State

---

## 📂 Estrutura do Projeto

A organização de pastas segue rigorosamente as convenções estabelecidas pelo framework ASP.NET Core MVC:

```text
SISTEMA-BIBLIOTECA-MAIN/
│
├── Controllers/              # Controladores responsáveis pelas regras de requisição e respostas
│   ├── AtendenteController.cs
│   ├── AuthController.cs
│   ├── AuthenticatedController.cs
│   ├── BibliotecarioController.cs
│   ├── ClientesController.cs
│   ├── EmprestimosController.cs
│   ├── FuncionariosController.cs
│   ├── HomeController.cs
│   ├── LivrosController.cs
│   └── UsuariosController.cs
│
├── Data/                     # Contexto de banco de dados e configurações do EF Core
│   └── LibraryContext.cs
│
├── Migrations/               # Histórico de evoluções e controle de esquema do banco de dados
│
├── Models/                   # Modelos de domínio e entidades do banco de dados
│   ├── Cliente.cs
│   ├── Emprestimo.cs
│   ├── Livro.cs
│   └── Usuario.cs
│
├── Services/                 # Serviços auxiliares e lógicas isoladas do sistema
│
├── ViewModels/               # Modelos específicos para transporte e exibição de dados nas Views
│   └── DashboardViewModel.cs
│
├── Views/                    # Interfaces do usuário em formato Razor (.cshtml)
│   ├── Atendente/
│   ├── Auth/
│   ├── Bibliotecario/
│   ├── Clientes/
│   ├── Emprestimos/
│   ├── Funcionarios/
│   ├── Home/
│   ├── Livros/
│   ├── Shared/
│   └── Usuarios/
│
├── wwwroot/                  # Arquivos estáticos globais (CSS nativos, JavaScript e bibliotecas)
│
├── appsettings.json          # Configurações globais de conexão e ambiente
├── library.db                # Base de dados SQLite integrada
└── Program.cs                # Arquivo de inicialização, serviços e middlewares da aplicação
