# Visão Lógica

A visão lógica define a arquitetura do software, detalhando seus componentes, classes e as interações entre eles. Utilizando diagramas de classes e componentes, essa visão oferece um mapa claro de como o código é organizado e como a lógica de negócio é implementada, sendo um guia fundamental para a equipe de desenvolvimento.

## Objetivo

O objetivo da visão lógica é traduzir os requisitos funcionais do sistema em uma estrutura tangível. No contexto do projeto PodePedirFCTE, um aplicativo para conectar alunos da FCTE a restaurantes locais no Gama, a visão lógica decompõe o sistema em módulos, classes e objetos. Diagramas de classes são utilizados para representar visualmente essas estruturas e seus relacionamentos, enquanto pacotes organizam o código de forma coesa. As interfaces definem os contratos de comunicação entre os módulos, e uma documentação rigorosa, aliada a revisões constantes, assegura que a implementação final atenda aos objetivos do projeto e mantenha um alto padrão de qualidade.

## Diagrama de Pacotes

A arquitetura do sistema **PodePedirFCTE** foi estruturada visando o desacoplamento entre as camadas de apresentação (Apps) e a lógica de negócio (Backend), utilizando um núcleo compartilhado para garantir a consistência dos dados.

A organização dos pacotes reflete a divisão de responsabilidades do sistema, permitindo o desenvolvimento paralelo entre as equipes de *frontend* e *backend*, além de facilitar a manutenção evolutiva.

O diagrama abaixo ilustra a hierarquia e as dependências entre os subsistemas:

<p align="center">
  <img src="../assets/diagrama-de-pacotes.png" alt="Diagrama de Pacotes">
</p>

### Detalhamento dos Subsistemas

Abaixo detalha-se a responsabilidade arquitetural de cada grande pacote identificado na Visão Lógica.

#### 1. SharedKernel (Núcleo Compartilhado)
O `SharedKernel` atua como a base de contratos do sistema. Ele não possui dependências de outros pacotes, garantindo estabilidade (princípio de arquitetura limpa).
* **DTOs (Data Transfer Objects):** Define a estrutura dos dados que trafegam entre os clientes e o servidor (ex: `PedidoDTO`, `UsuarioDTO`), garantindo que ambos os lados "falem a mesma língua".
* **Enums:** Centraliza as constantes de domínio (ex: `StatusPedido`), evitando duplicidade de regras de estado espalhadas pelo código.

#### 2. Backend (Lógica de Negócio e Orquestração)
O pacote `Backend` concentra a inteligência do sistema. Ele não se comunica diretamente com a interface do usuário, mas expõe serviços para serem consumidos. A estrutura interna segue uma divisão por contextos delimitados (*Bounded Contexts*):
* **Gerenciadores:** Módulos responsáveis por domínios específicos (`Contas`, `Restaurantes`, `Pagamentos`, `Entregas`).
* **Orquestrador de Pedidos:** Atua como o coordenador do fluxo principal. Ele interage com múltiplos gerenciadores para validar, cobrar e despachar um pedido, mantendo a consistência da transação.
* **Dependências:** O Backend depende do `SharedKernel` para tipagem de dados, mas desconhece a existência dos aplicativos clientes.

#### 3. Camada de Aplicação (Clientes)
Os pacotes `AppEstudante`, `AppRestaurante` e `AppEntregador` representam os clientes (Frontend/Mobile).
* **Responsabilidade:** Focam exclusivamente na experiência do usuário (UX/UI) e na captura de dados. Não contêm regras de negócio críticas.
* **Isolamento:** São completamente independentes entre si. Uma alteração no app do Estudante não impacta o app do Entregador.
* **Comunicação:** Dependem do `SharedKernel` para estruturar as requisições e se comunicam com o `Backend` via chamadas de rede (API), sem dependência direta de código-fonte (acoplamento lógico, não físico).

## Diagrama de Classes

Esta seção apresenta a estrutura estática do sistema **PodePedirFCTE**, detalhando as classes, seus atributos e os relacionamentos estabelecidos para atender aos requisitos funcionais e regras de negócio.

A modelagem abaixo reflete as decisões de design tomadas para garantir a integridade dos dados de usuários, pedidos e pagamentos dentro da arquitetura proposta.

### Visualização Geral

O diagrama a seguir evidencia as relações de herança, agregação e dependência entre as entidades principais do domínio.

<p align="center">
  <img src="../assets/diagrama-de-classes/diagrama-de-classes.png" alt="Diagrama de Classes">
</p>

> **Nota:** Métodos de acesso (*getters* e *setters*) foram omitidos do diagrama para priorizar a legibilidade dos atributos e operações de negócio.

### Detalhamento das Classes Significativas

Abaixo descrevemos as responsabilidades de cada classe mapeada na Visão Lógica.

#### 1. Núcleo de Usuários (Herança)

**Usuário**  

<p align="center">
  <img src="../assets/diagrama-de-classes/usuario.png" alt="Usuário">
</p>  

- **Responsabilidade:** Classe base (superclasse) que generaliza os dados comuns a todos os atores do sistema.
- **Atributos:** Contém as credenciais de acesso e dados pessoais básicos necessários para a autenticação.

**Aluno**

<p align="center">
  <img src="../assets/diagrama-de-classes/aluno.png" alt="Aluno">
</p>  

- **Tipo:** Especialização de `Usuário`.
- **Responsabilidade:** Representa o consumidor final da plataforma. Possui atributos específicos para validação acadêmica e histórico de pedidos.

**Entregador**

<p align="center">
  <img src="../assets/diagrama-de-classes/entregador.png" alt="Entregador">
</p>  

- **Tipo:** Especialização de `Usuário`.
- **Responsabilidade:** Representa o responsável pela logística. Seus atributos focam em dados de transporte e disponibilidade.

**Fornecedor**

<p align="center">
  <img src="../assets/diagrama-de-classes/fornecedor.png" alt="Fornecedor">
</p>  

- **Tipo:** Especialização de `Usuário`.
- **Responsabilidade:** Representa os estabelecimentos comerciais. Gerencia cardápios e recebe pedidos.

#### 2. Núcleo de Pedidos e Produtos

**Cardapio**

<p align="center">
  <img src="../assets/diagrama-de-classes/cardapio.png" alt="Cardápio">
</p>  

- **Responsabilidade:** Agrega os itens disponíveis para venda. Pertence exclusivamente a um `Fornecedor`.

**Item**

<p align="center">
  <img src="../assets/diagrama-de-classes/item.png" alt="Item">
</p> 

- **Responsabilidade:** Representa a unidade mínima de venda (produto), contendo preço, descrição e disponibilidade.

**Pedido**

<p align="center">
  <img src="../assets/diagrama-de-classes/pedido.png" alt="Pedido">
</p> 

- **Responsabilidade:** Entidade central da transação. Relaciona um `Aluno` a itens de um `Fornecedor` e a um `Entregador`.
- **Observação:** O ciclo de vida do pedido é gerido através do `Enum_Status_Entrega`.

#### 3. Núcleo Financeiro

**Pagamento**

<p align="center">
  <img src="../assets/diagrama-de-classes/pagamento.png" alt="Pagamento">
</p> 

- **Responsabilidade:** Gerencia a transação financeira entre as partes.
- **Relacionamentos:** Conecta-se a Alunos e Fornecedores via agregação para processar os valores devidos.

**CartaoPagamento**

<p align="center">
  <img src="../assets/diagrama-de-classes/cartao-pagamento.png" alt="Cartão Pagamento">
</p> 

- **Responsabilidade:** Armazena de forma segura (tokenizada) os métodos de pagamento do aluno.

#### 4. Enumeradores (Tipos Auxiliares)

Classes auxiliares para garantir consistência de estados e tipos no sistema:

* **Enum_Tipo:** Tipologias de cartões (Crédito, Débito, etc.).
* **Enum_Status_Pagamento:** Estados da transação (Pendente, Aprovado, Recusado).
* **Enum_Status_Entrega:** Máquina de estados do pedido (Preparando, Saiu para Entrega, Entregue).
* **Enum_Tipo_Pagamento:** Formas de pagamento aceitas (Pix, Cartão, Dinheiro).

## Diagrama de Sequência

Esta seção detalha como os elementos estruturais definidos no Diagrama de Classes e de Pacotes colaboram para realizar as funcionalidades do sistema.

Os diagramas de sequência abaixo ilustram o fluxo de mensagens entre os objetos, evidenciando a interação entre as camadas de visualização, controle e persistência identificadas na arquitetura.

### 1. Fluxos Principais de Negócio (Pedidos e Entregas)

Focam na interação crítica entre o `AppEstudante`, `AppEntregador` e o `Orquestrador de Pedidos` no Backend.

**Acompanhar Pedido**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/acompanhar-pedido.png" alt="Acompanhar Pedido">
</p>


**Atualizar Status da Entrega**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/status-da-entrega.png" alt="Atualizar Status da Entrega">
</p>

**Histórico de Pedidos**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/historico-de-pedidos.png" alt="Histórico de Pedidos">
</p>

**Histórico de Entregas**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/historico-de-entregas.png" alt="Histórico de Entregas">
</p>

### 2. Gestão de Usuários e Busca

Demonstra como o sistema manipula dados cadastrais e consultas, validando a comunicação com o `Gerenciador de Contas`.

**Gerenciar Perfil**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/gerenciar-perfil.png" alt="Gerenciar Perfil">
</p>

**Buscar Restaurantes**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/buscar-restaurantes.png" alt="Buscar Restaurantes">
</p>

### 3. Fluxos de Suporte e Financeiro

Detalhamento das interações para processos de feedback e verificação financeira.

**Avaliar Entregador**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/avaliar-entregador.png" alt="Avaliar Entregador">
</p>

**Reportar Problema na Entrega**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/reportar-problema-entrega.png" alt="Reportar Problema na Entrega">
</p>

**Consultar Extrato**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/consultar-extrato.png" alt="Consultar Extrato">
</p>

**Visualizar Recibos**

<p align="center">
  <img src="../assets/diagrama-de-sequencia/visualizar-recibos.png" alt="Visualizar Recibos">
</p>


## Diagrama de Estados

Algumas classes identificadas no Diagrama de Classes possuem um comportamento dinâmico complexo, onde a resposta a um evento depende do estado atual do objeto. Esta seção detalha as máquinas de estado das entidades críticas para o negócio.

### 1. Ciclo de Vida do Pedido
A classe `Pedido` é a entidade central do sistema. Seu estado determina quais operações são permitidas no Backend (ex: um pedido "Em Rota" não pode ser "Cancelado" sem penalidade).

O diagrama abaixo ilustra as transições de estado do pedido desde a criação até a entrega final.

<p align="center">
  <img src="../assets/diagrama-de-estados/visualizacao-cliente.png" alt="Ciclo de Vida do Pedido">
</p>

**Regras de Transição:**
* **Preparação:** O pedido só entra em preparação após a confirmação do gateway de pagamento.
* **Rota:** A transição para "Em Rota" dispara notificações automáticas para o Cliente.
* **Finalização:** O ciclo se encerra definitivamente com a confirmação de entrega ou o cancelamento justificado.

### 2. Disponibilidade Logística (Entregador)
Para o módulo de alocação de entregas, é crucial monitorar o estado operacional do `Entregador`. Este diagrama define a lógica utilizada pelo algoritmo de *dispatch* para enviar ofertas.

<p align="center">
  <img src="../assets/diagrama-de-estados/entregador.png" alt="Entregador">
</p>

**Impacto Arquitetural:**
* O sistema só envia notificações de "Nova Corrida" para entregadores no estado **Disponível**.
* O estado **Offline** impede o recebimento de qualquer oferta.
* Os estados intermediários (**A caminho**, **Coletando**, **Entregando**) bloqueiam o recebimento de novas ofertas simultâneas (dependendo da regra de negócio de *stacking*).

### 3. Gestão de Identidade (Conta)
A segurança do sistema depende do estado da `Conta` do usuário. Este fluxo controla o acesso e a validade das credenciais.

<p align="center">
  <img src="../assets/diagrama-de-estados/conta.png" alt="Entregador">
</p>

**Regras de Negócio:**
* Contas **Aguardando Confirmação** (pendentes de token de e-mail) têm acesso restrito apenas à funcionalidades de leitura pública.
* O estado **Inativa** preserva os dados históricos (integridade referencial) mas bloqueia novos logins.

## Referências

> 1. UNIVERSIDADE DE BRASÍLIA (UnB). Arquitetura e Desenho de Software - Aula Arquitetura e DAS - Parte II. Disponível em: https://aprender3.unb.br/pluginfile.php/3178407/mod_page/content/1/Arquitetura%20e%20Desenho%20de%20Software%20-%20Aula%20Arquitetura%20e%20DAS%20-%20Parte%20II%20-%20Profa.%20Milene.pdf. Acesso em: 20/11/2025.
> 2. PodePedirFCTE. Projeto. Disponível em: https://unbarqdsw2025-2-turma01.github.io/2025.2-T01-G7_PodePedirFCTE_Entrega_02/#/Projeto/Projeto. Acesso em: 20/11/2025.
> 3. PodePedirFCTE. Diagrama de Classes. Disponível em: https://unbarqdsw2025-2-turma01.github.io/2025.2-T01-G7_PodePedirFCTE_Entrega_02/#/./Modelagem/ModelagemEstatica/DiagramaDeClasses. Acesso em: 20/11/2025.
> 4. PodePedirFCTE. Diagrama de Pacotes. Disponível em: https://unbarqdsw2025-2-turma01.github.io/2025.2-T01-G7_PodePedirFCTE_Entrega_02/#/Modelagem/ModelagemOrganizacional/DiagramaDePacotes. Acesso em: 20/11/2025.
> 5. PodePedirFCTE. Diagrama de Sequência. Disponível em: https://unbarqdsw2025-2-turma01.github.io/2025.2-T01-G7_PodePedirFCTE_Entrega_02/#/Modelagem/ModelagemDinamica/DiagramaDeSequencia. Acesso em: 20/11/2025.
> 6. PodePedirFCTE. Diagrama de Estados. Disponível em: https://unbarqdsw2025-2-turma01.github.io/2025.2-T01-G7_PodePedirFCTE_Entrega_02/#/Modelagem/ModelagemDinamica/DiagramaDeEstados. Acesso em: 20/11/2025.

## Histórico de Versões

| **Data**       | **Versão** | **Descrição**                         | **Autor**                                      | **Revisor**                                      | **Data da Revisão** |
| :--------: | :----: | :-------------------------------- | :----------------------------------------: | :----------------------------------------: | :-------------: |
| 20/11/2025 |  `0.1`   | Criação da página e adição de conteúdo | [`@Ana Joyce`](https://github.com/anajoyceamorim) | [`@Ana Clara`](https://github.com/anabborges) |   20/11/2025    |
