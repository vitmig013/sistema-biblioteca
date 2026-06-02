Aqui está o arquivo `README.md` estruturado exclusivamente com base nas especificações, metas e regras descritas no documento de requisitos (`projeto.md`):

---

# Modernização do Sistema de Gestão de Acervo e Empréstimos

Este repositório contém a documentação e a especificação técnica para o desenvolvimento do Sistema de Gestão de Acervo e Empréstimos. O projeto foi idealizado para modernizar o ecossistema de atendimento, substituindo o controle manual por uma solução automatizada e robusta.

## 📋 Informações do Projeto

* **Versão Atual:** 2.5 (Revisada para Desenvolvimento).
* **Data da Última Revisão:** 13/05/2026.
* **Status:** Pronto para Implementação.
* **Responsáveis/Equipe:** Beatriz Gasparini, Enzo Lima, Jailton Sousa, Leandro de Sá, Matheus Osmédio, Matheus Souza e Nicolas Urata.

---

## 🎯 Visão Estratégica e Contexto

O objetivo central é substituir o controle manual de mais de 70 empréstimos diários, mitigando falhas operacionais e extinguindo o retrabalho no atendimento.

### Pontos de Dor Atuais (Problemas Identificados)

* **Inconsistência Crítica:** Registros rasurados ou perdidos provocam prejuízo direto ao patrimônio da universidade.
* **Gargalo Operacional:** A busca manual por fichas físicas causa filas extensas e gera insatisfação para os alunos.
* **Cegueira de Inventário:** Ausência de controle quantitativo sobre os títulos mais procurados ou exemplares danificados.

### Indicadores de Sucesso (KPIs)

* Redução do tempo médio de registro de um empréstimo para menos de 30 segundos.
* Eliminar por completo a perda de informações sobre livros não devolvidos por meio de validações e rotinas de backup.
* Disponibilização de uma ferramenta de consulta de acervo em tempo real.

---

## 👥 Perfis de Acesso e Stakeholders

O sistema divide-se em três níveis de permissão específicos para a operação:

1. **Atendente:** Responsável pela operação diária na frente de atendimento, executando cadastros de usuários, movimentações de empréstimos, devoluções e consultas.
2. **Bibliotecário:** Atua na gestão estratégica e manutenção geral do acervo. Possui todas as permissões do atendente, além de gerenciar livros (cadastrar/inativar), gerenciar funcionários e emitir relatórios.
3. **Aluno (Usuário):** Beneficiário final do serviço, habilitado a realizar consultas de disponibilidade e verificar seu histórico por intermédio de um atendente.

---

## ⚙️ Requisitos Funcionais (MoSCoW)

A priorização dos requisitos do sistema foca primordialmente na entrega do Produto Mínimo Viável (MVP):

| ID | Requisito | Descrição Técnica | Prioridade |
| --- | --- | --- | --- |
| **RF01** | Cadastro de Usuários | Coleta e armazenamento de ID, Nome, Telefone, Matrícula, E-mail e Logradouro. | **Must-have** |
| **RF02** | Gestão de Acervo | Permite o cadastro e a edição de Livros (Título, Autor, Gênero, Quantidade). | **Must-have** |
| **RF03** | Motor de Empréstimo | Registra a retirada de livros vinculando o ID do Livro, o Usuário e a Data da transação. | **Must-have** |
| **RF04** | Motor de Devolução | Registra a entrega, atualiza o status do item e faz a conferência de prazos. | **Must-have** |
| **RF05** | Cálculo de Multas | Gera boleto ou bloqueio automatizado na conta do usuário caso o prazo de devolução exceda 10 dias. | **Must-have** |
| **RF06** | Sistema de Bloqueio | Impede transações comerciais para usuários com pendências financeiras ou livros em atraso. | **Must-have** |
| **RF07** | Catálogo Digital | Mecanismo de busca rápida por título, autor ou gênero integrada ao banco de dados local. | **Should-have** |

---

## 🧠 Regras de Negócio (RN)

As regras abaixo determinam o comportamento obrigatório e as validações lógicas no backend do sistema:

* **RN01 (Unicidade):** Um usuário está proibido de possuir dois cadastros ativos simultaneamente (Chave única indexada: Matrícula/CPF).
* **RN02 (Limite de Itens):** Limite máximo de até 5 livros sob posse simultânea por usuário.
* **RN03 (Trava de Título):** O usuário fica impedido de retirar duas cópias idênticas do mesmo livro ao mesmo tempo.
* **RN04 (Temporalidade):** O prazo regulamentar padrão para a devolução de qualquer item é de exatamente 10 dias corridos.
* **RN05 (Bloqueio):** Caso conste um único livro com entrega em atraso, a funcionalidade "Realizar Empréstimo" deve ser imediatamente bloqueada para o ID daquele cliente.

---

## 🔄 Fluxos de Uso Detalhados

### 1. Cenário: Empréstimo com Sucesso

1. O atendente identifica o cliente por meio de sua matrícula. Se não houver registro prévio, realiza o cadastro aplicando validações de Regex para e-mail e telefone.
2. O sistema verifica se o perfil do cliente possui bloqueios ou se já atingiu a cota limite de 5 livros simultâneos.
3. O atendente insere o ID do livro e o sistema realiza a validação da disponibilidade de exemplares em estoque.
4. O sistema agenda a data de devolução prevista (Data Atual + 10 dias), atualiza o status do livro para "Ocupado" e salva o registro.

### 2. Cenário: Devolução com Atraso

1. O atendente realiza a busca informando o ID do livro ou a matrícula do aluno.
2. O sistema executa o cruzamento de dados comparando a data atual com a data de devolução esperada.
3. Constatado o atraso, a aplicação calcula o valor da multa devida e emite o registro financeiro.
4. O cadastro do estudante é marcado automaticamente com o status "Bloqueado" e permanece restrito até a confirmação de liquidação do débito.

---

## 🗄️ Arquitetura de Dados e Requisitos Não Funcionais

### Modelo de Entidades Primárias

* **Livro:** `id_livro`, `titulo`, `autor`, `genero`, `qtd_estoque`, `status`.
* **Cliente:** `id_cliente`, `nome`, `matricula` (Unique), `email` (Validado), `status_bloqueio`.
* **Empréstimo:** `id_emprestimo`, `id_livro`, `id_cliente`, `data_retirada`, `data_previsao_devolucao`.

### Requisitos Não Funcionais (RNF)

* **Desempenho:** Banco de dados local estruturado e otimizado para comportar o volume de até 2.000 livros e 5.000 clientes cadastrados.
* **Interface:** Design com foco em produtividade operacional para estações desktop, contemplando atalhos de teclado e visualização limpa.
* **Segurança:** Autenticação e acesso controlado estritamente via login e senha criptografada para funcionários e bibliotecários.

---

## ⚠️ Gestão de Riscos e Mitigação

* **Risco de Integridade (Perda de dados locais):** Mitigado por meio do estabelecimento de uma rotina de backup semanal executada fora do servidor local.
* **Barreira Cultural (Adaptação ao sistema):** Mitigado pelo desenvolvimento de uma interface gráfica intuitiva e aplicação de treinamento assistido com os operadores durante as primeiras semanas.
* **Restrição de Escopo (Prazo curto e equipe reduzida):** Mitigado pelo foco rigoroso no MVP, priorizando a entrega e validação estrita dos requisitos cruciais "Must-have" (RF01 ao RF06).
