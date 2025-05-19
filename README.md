# Sistema de Reservas de Voos com Arquitetura SOA

## Introdução

Este documento apresenta a proposta de arquitetura para um sistema de **reserva de voos** com **integração de serviços externos** (SOA - Service-Oriented Architecture), conforme solicitado na atividade. O sistema permitirá aos clientes pesquisar, selecionar e reservar voos com base em diversos critérios, além de fornecer funcionalidades para o gerenciamento de voos e clientes pela equipe interna da companhia aérea.

---

## 1. Arquitetura SOA (Diagrama de Componentes)

Abaixo está descrita a estrutura da arquitetura proposta, dividida em componentes internos e integrações com serviços externos:

## Arquitetura SOA - Representação Visual (ASCII)

```txt
+-------------------------------------------------------------+
|                  CAMADA DE APRESENTAÇÃO                    |
| +----------------+  +----------------+  +----------------+  |
| |  App Cliente   |  |   Portal Web   |  |    Admin UI    |  |
| +----------------+  +----------------+  +----------------+  |
+-------------------------------------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|                        API GATEWAY                          |
+-------------------------------------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|             BARRAMENTO DE SERVIÇOS (ESB - SOA)              |
+-------------------------------------------------------------+
    |            |               |              |           |
    v            v               v              v           v
+---------------+  +------------+  +-----------+  +--------+  +------------+
|   Serviço     |  |  Serviço   |  |  Serviço  |  | Serviço|  |  Serviço   |
|  Localização  |  |  Reservas  |  |  Pagamento|  | Usuário|  | E-mail     |
+---------------+  +------------+  +-----------+  +--------+  +------------+
    |            |               |              |           |
    v            v               v              v           v
+------------+  +----------------+  +------------+  +--------------+  +------------+
|  Serviço   |  |    Serviço     |  |  Serviço   |  |   Serviço     |  |  Serviço   |
| Admin      |  | Notificações   |  | Check-in   |  | Validação Doc |  |  Preços    |
+------------+  +----------------+  +------------+  +--------------+  +------------+



### 🔹 Componentes Internos

* **Frontend Web/App**: interface para o cliente final.
* **API Gateway**: centraliza o roteamento de chamadas entre os componentes internos e externos.
* **Cadastro de Usuário**: gerencia dados de login, informações pessoais e autenticação.
* **Reserva de Voos**: lógica de seleção de voos, emissão de bilhetes, disponibilidade e pagamentos.
* **Gestão de Voos**: interface administrativa para edição, criação e remoção de voos.
* **Gestão de Assentos**: gerencia assentos disponíveis, bloqueados e reservados.
* **Banco de Dados**: armazena informações de clientes, reservas, voos e logs.

### 🔗 Integração com Serviços SOA Externos

* **API de Localização**: detecta a localização do cliente automaticamente.
* **API de Preços de Voos**: conecta com fornecedores para consultar preços dinâmicos.
* **API de Pagamentos**: realiza a cobrança do cliente via cartão ou boleto.
* **API de E-mail**: envia confirmações de reserva para os clientes.
* **API de Validação de Documentos**: valida documentos como CPF ou passaporte.

---

### 🌐 Como montar o diagrama de componentes:

1. Acesse o site [https://app.diagrams.net](https://app.diagrams.net) ou [Lucidchart](https://www.lucidchart.com)
2. Selecione "UML > Component Diagram"
3. Use caixas rotuladas para representar componentes:

   * Frontend Web/App
   * Cadastro de Usuário
   * Reserva de Voos
   * etc.
4. Conecte os componentes com **setas unidirecionais** (chamadas via API)
5. Agrupe os serviços externos do lado direito com rótulo **Serviços SOA**
6. Destaque o **API Gateway** no centro, como ponto de integração principal

### 🖊️ Exemplo (descrição textual do diagrama):

```
[Frontend Web/App]
      |
      v
[API Gateway]
  |      |        |         |         |
  v      v        v         v         v
[Cadastro de Usuário]
[Reserva de Voos]
[Gestão de Voos]
[Gestão de Assentos]
[Banco de Dados Principal]

------ SERVIÇOS EXTERNOS (SOA) -------
        |      |      |       |
        v      v      v       v
[API de Localização]
[API de Pagamento]
[API de Preços]
[API de E-mail]
```

---

## 2. Requisitos Não Funcionais

### 1. Escalabilidade

* ✅ **Descrição:** O sistema deve suportar milhares de usuários simultâneos.
* ✅ **Justificativa:** Em datas comemorativas ou períodos de promoções, o sistema pode ter pico de acessos.
* ✅ **Relacionamento com SOA:** Serviços podem ser escalados de forma independente (ex: módulo de reserva pode ter mais instâncias que o de cadastro).

### 2. Alta disponibilidade

* ✅ **Descrição:** O sistema precisa operar 24/7 sem interrupções.
* ✅ **Justificativa:** Qualquer parada pode impactar diretamente nas receitas.
* ✅ **Relacionamento com SOA:** Cada serviço pode ter redundância e failover habilitado.

### 3. Segurança

* ✅ **Descrição:** O sistema deve proteger dados sensíveis dos usuários.
* ✅ **Justificativa:** Cadastro, pagamento e documentos precisam estar seguros.
* ✅ **Relacionamento com SOA:** Uso de autenticação via JWT, criptografia e validação de entrada para cada serviço.

### 4. Baixa latência

* ✅ **Descrição:** O tempo de resposta nas buscas e reservas deve ser inferior a 2 segundos.
* ✅ **Justificativa:** Impacta diretamente na experiência do usuário.
* ✅ **Relacionamento com SOA:** Cada serviço pode ser otimizado ou cacheado separadamente.

### 5. Manutenibilidade

* ✅ **Descrição:** A arquitetura deve permitir alterações sem grandes impactos.
* ✅ **Justificativa:** Sistemas de missão crítica precisam evoluir com segurança.
* ✅ **Relacionamento com SOA:** Serviços isolados permitem alteração de um módulo sem afetar os demais.

---

## Considerações Finais

A proposta acima oferece uma arquitetura SOA robusta, flexível e orientada à manutenção, crescimento e segurança. Ela atende tanto as necessidades da companhia aérea quanto as expectativas dos usuários finais e da equipe interna de operações.

> Nenhuma implementação de código é exigida nesta etapa. O foco é a compreensão da estrutura modular via SOA e a documentação de sua arquitetura e requisitos.

---

**Autor:** Kauã Rodrigues
**Curso:** Engenharia de Software
**Disciplina:** Programação / Arquitetura SOA
