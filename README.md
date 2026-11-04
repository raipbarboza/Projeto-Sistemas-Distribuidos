# Sistema Distribuído de Gerenciamento de Tarefas

Projeto acadêmico da disciplina de **Sistemas Distribuídos**.

## 1. Visão geral

O projeto consiste em uma aplicação cliente-servidor na qual **dois clientes independentes** se comunicam com **um servidor central** para cadastrar, consultar e atualizar tarefas. Os dados são mantidos pelo servidor e compartilhados entre os clientes.

O objetivo é demonstrar comunicação em rede, execução simultânea de clientes e acesso a dados compartilhados, incluindo uma demonstração de falha de comunicação.

> **Fonte dos requisitos:** enunciado *Projeto: Sistema Distribuído de Gerenciamento de Tarefas* (Projeto Final Alternativo). As escolhas de implementação indicadas abaixo são propostas da equipe e não exigências adicionais do enunciado.

## 2. Arquitetura

```mermaid
flowchart TB
    S[VM 1 — Servidor central\nAPI e armazenamento]
    C1[VM 2 — Cliente 1]
    C2[VM 3 — Cliente 2]
    C1 <-->|Solicitações e respostas| S
    C2 <-->|Solicitações e respostas| S
```

- **Servidor:** recebe conexões e solicitações, processa operações e mantém os dados das tarefas.
- **Cliente 1:** cadastra, consulta e atualiza tarefas.
- **Cliente 2:** oferece as mesmas funcionalidades e acessa os mesmos dados.
- Os clientes devem funcionar de maneira independente e poder executar simultaneamente.
- **Servidor e clientes devem ser executados em máquinas virtuais.** A proposta inicial é usar três VMs, uma para cada componente.

## 3. Requisitos funcionais obrigatórios

### Servidor

- [ ] RF01 — Inicializar e disponibilizar um serviço de comunicação.
- [ ] RF02 — Receber conexões e solicitações dos clientes.
- [ ] RF03 — Cadastrar novas tarefas.
- [ ] RF04 — Listar as tarefas existentes.
- [ ] RF05 — Atualizar o estado de uma tarefa: `pendente` ou `concluída`.
- [ ] RF06 — Manter os dados compartilhados entre os clientes.
- [ ] RF07 — Responder às solicitações com mensagens adequadas.

### Clientes

- [ ] RF08 — Conectar-se ao servidor.
- [ ] RF09 — Cadastrar uma nova tarefa.
- [ ] RF10 — Consultar a lista de tarefas.
- [ ] RF11 — Alterar o estado de uma tarefa.
- [ ] RF12 — Apresentar mensagens de sucesso ou erro.
- [ ] RF13 — Atualizar as informações exibidas a partir dos dados recebidos do servidor.

## 4. Tecnologias

**Sugeridas no enunciado:** Python, Flask e armazenamento em JSON ou SQLite. O documento também admite implementação direta com sockets em C/C++.

**Proposta inicial de implementação:**

| Camada | Tecnologia | Finalidade |
| --- | --- | --- |
| Servidor | Python + Flask | Disponibilizar uma API HTTP |
| Clientes | Python | Interface de terminal e envio de requisições |
| Comunicação | HTTP + JSON | Troca de mensagens entre processos |
| Armazenamento | SQLite | Manter tarefas no servidor |
| Ambiente | Máquinas virtuais Linux | Executar servidor e clientes em ambientes separados |

A interface em terminal, os endpoints e a escolha por SQLite são decisões propostas, não imposições do enunciado.

## 5. Modelo de dados proposto

Exemplo de tarefa:

```json
{
  "id": 1,
  "titulo": "Estudar Sistemas Distribuídos",
  "concluida": false
}
```

O identificador distingue tarefas; `titulo` descreve a atividade; `concluida` indica seu estado. A representação interna poderá ser ajustada durante a implementação.

## 6. API proposta

| Método | Rota | Descrição |
| --- | --- | --- |
| `GET` | `/tarefas` | Lista as tarefas existentes |
| `POST` | `/tarefas` | Cadastra uma nova tarefa |
| `PATCH` | `/tarefas/{id}` | Altera o estado de uma tarefa |

As rotas e o formato das respostas ainda deverão ser implementados e testados. Não há endpoints funcionais nesta etapa de documentação.

## 7. Estrutura inicial planejada

```text
sistema-distribuido-tarefas/
├── README.md
├── .gitignore
├── requirements.txt
├── servidor/
│   ├── app.py
│   └── database.py
├── cliente/
│   └── main.py
├── testes/
└── docs/
    ├── arquitetura.md
    ├── execucao.md
    └── relatorio-tecnico.md
```

> **Importante:** esta é a estrutura planejada. Os arquivos e diretórios acima não são criados automaticamente pelo README. O mesmo programa cliente poderá ser executado em duas VMs independentes, sujeito à validação dessa interpretação com a professora.

## 8. Preparação do repositório Git

Na pasta do projeto:

```bash
git init
mkdir -p servidor cliente testes docs
python3 -m venv .venv
```

Ative o ambiente virtual:

```bash
source .venv/bin/activate
```

Após criar os arquivos de código e definir as dependências, instale-as com:

```bash
pip install -r requirements.txt
```

Sugestão para o `.gitignore`:

```gitignore
.venv/
__pycache__/
*.py[cod]
.pytest_cache/
.env
*.db
*.sqlite3
```

Registre a documentação inicial:

```bash
git add .
git commit -m "docs: adiciona documentacao inicial do projeto"
```

> O `requirements.txt` deverá ser criado quando as dependências forem definidas. O comando de instalação pressupõe sua existência.

## 9. Execução planejada

A execução definitiva será documentada após a implementação. O fluxo previsto é:

1. Configurar a rede entre as três máquinas virtuais.
2. Iniciar o servidor na VM 1, permitindo acesso pela interface de rede apropriada.
3. Configurar o endereço IP e a porta do servidor nos clientes.
4. Iniciar o Cliente 1 na VM 2 e o Cliente 2 na VM 3.
5. Realizar operações nos dois clientes e verificar os dados compartilhados.

Não são fornecidos comandos de inicialização da aplicação neste momento porque o código ainda não foi implementado.

## 10. Plano de testes

- [ ] T01 — Cliente 1 cadastra uma tarefa com sucesso.
- [ ] T02 — Cliente 2 consulta e visualiza a tarefa criada pelo Cliente 1.
- [ ] T03 — Cliente 2 altera o estado da tarefa para concluída.
- [ ] T04 — Cliente 1 consulta novamente e visualiza o novo estado.
- [ ] T05 — Os dois clientes executam operações enquanto estão conectados.
- [ ] T06 — O servidor é interrompido e o cliente apresenta uma mensagem de erro apropriada.
- [ ] T07 — Após a retomada do servidor, o cliente consegue realizar uma nova solicitação.

O teste de falha é obrigatório no enunciado. Os cenários T06 e T07 são a estratégia proposta para demonstrá-lo.

## 11. Entregáveis obrigatórios

- [ ] Código-fonte do servidor e dos dois clientes.
- [ ] Diagrama da arquitetura e das comunicações.
- [ ] Instruções de execução do servidor e dos clientes.
- [ ] Relatório técnico breve: implementação, testes e problemas encontrados.
- [ ] Demonstração do sistema funcionando, com pelo menos um teste de falha.

## 12. Etapas de desenvolvimento

1. **Documentação e estrutura:** iniciar o Git, registrar requisitos e organizar diretórios.
2. **Servidor:** implementar comunicação, operações sobre tarefas e armazenamento.
3. **Cliente:** implementar menu, solicitações e mensagens de retorno.
4. **Integração:** testar dois clientes independentes acessando os mesmos dados.
5. **Virtualização:** executar os três componentes nas VMs e configurar a rede.
6. **Validação e entrega:** executar testes, registrar evidências, finalizar relatório e demonstrar a falha.

## 13. Critérios de conclusão

O projeto estará pronto para demonstração quando os dois clientes puderem, a partir de suas máquinas virtuais, cadastrar, consultar e atualizar tarefas mantidas pelo servidor; quando as alterações feitas por um cliente forem observáveis pelo outro após consulta; e quando uma falha de comunicação puder ser demonstrada com resposta adequada da aplicação.

---

**Estado do projeto:** documentação inicial — implementação pendente.
