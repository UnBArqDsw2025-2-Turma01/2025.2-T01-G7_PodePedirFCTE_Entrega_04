# Documento de Arquitetura de Software

## Introdução

Este documento tem como objetivo descrever a arquitetura do sistema PoderPedirFCTE, com foco nos casos de uso desenvolvidos para o projeto. Com isso, é possível fornecer uma visão detalhada das interações entre os atores e o sistema, de forma que são destacados os principais fluxos e funcionalidades que compõem este sistema.

## Visão Geral

O PodePedirFCTE é um sistema que permite que os usuários, estudantes da UnB campus Gama, possam fazer pedidos de delivery para estabelecimentos próximos à faculdade. Dessa forma, os alunos possuem um novo conjunto de opções para seus almoços e lanches além de fomentar o comércio local.

Para a criação do diagrama de casos de uso, foi utilizada como ferramenta o site LucidChart.

## Diagrama de Casos de Uso

Ao modelar os casos de uso é possível ter uma melhor noção de como serão as funcionalidades do aplicativo que será desenvolvido. O diagrama a seguir ilustra os casos de uso e seus relacionamentos no contexto do app PodePedirFCTE.

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://lucid.app/documents/embedded/570ecf74-6248-4340-b38f-22b754ed6db6" id="UeKSK8r~XTxZ"></iframe></div>

<img src="./assets/diagrama-caso-de-uso.png" alt="Diagrama de Casos de Uso" style="max-width:100%; height:auto;">
<div align="center">
  <font size="3">Figura 1 – Diagrama de Casos de Uso do sistema PodePedirFCTE</font>
</div>
<div align="center">
  <p>Autores:
    <a href="https://github.com/anabborges">Ana Clara</a>, 
    <a href="https://github.com/anajoyceamorim">Ana Joyce</a>, 
    <a href="https://github.com/fabinsz">Fábio</a>, 
    <a href="https://github.com/gaubiela">Gabriela</a>, 
    <a href="https://github.com/storch7">Guilherme Storch</a>, 
    <a href="https://github.com/luizfaria1989">Luiz Guilherme</a>, 
    <a href="https://github.com/Nathan-bs">Nathan</a>, 
    <a href="https://github.com/rodrigoFAmaral">Rodrigo</a>
  </p>
</div>

### Página Original

A página original do diagrama de casos de uso para a segunda entrega do trabalho de arquitetura pode ser acessada [aqui](https://unbarqdsw2025-2-turma01.github.io/2025.2-T01-G7_PodePedirFCTE_Entrega_02/#/Modelagem/ModelagemOrganizacional/DiagramaDeCasosDeUso), em que é possível também ver o diagrama, mas com melhores explicações sobre os relacionamentos que compõem um diagrama de casos de uso.

### Principais Atores

* **Aluno:** Representa um estudante da faculdade, é ele quem faz os pedidos no aplicativo.
* **Fornecedor:** Representa um restaurante que está cadastrado no aplicativo.
* **Entregador:** É o usuário do app que tem como responsabilidade entregar os pedidos feitos para o restaurante para o aluno.

### Principais Casos de Uso

1. **Avaliar Restaurante**
* Permite que o aluno avalie o restaurante em que foi feito seu pedido com uma escala de 1 a 5 estrelas.

2. **Avaliar Entregador**
* Permite que o aluno avalie o entregador que foi responsável por entregar seu pedido com uma escala de 1 a 5 estrelas.

3. **Buscar Por Restaurantes**
* Permite que o aluno busque, por meio do preenchimento em uma barra de pesquisa, restaurantes presentes no aplicativo.

4. **Realizar Pedido** *(<<include>> Realizar Pagamento)*
* Permite que o aluno escolha um ou mais itens de um cardápio de um restaurante e faça uma encomenda. Esse caso de uso inclui o caso de uso realizar pagamento, o qual é necessário para a conclusão do pedido.

5. **Acompanhar Pedido**
* Permite que o aluno veja o status do seu pedido, pode ser: pedido recebido pelo restaurante, em preparo, em rota de entrega, entregue.

6. **Visualizar Histórico de Pedidos**
* Permite que o aluno veja os pedidos que foram feitos anteriormente na sua conta.

7. **Visualizar Histórico de Pagamentos**
* Permite que o aluno veja os históricos de pagamentos que foram feitos na sua conta.

8. **Visualizar Recibos**
* Permite que o aluno veja os recibos dos pagamentos dos pedidos que foram feitos em sua conta.

9. **Fazer Login**
* Permite que o usuário (seja ele aluno, fornecedor ou entregador) entre na sua conta do aplicativo. É necessário um e-mail e senha, mas também pode ser feito por outros serviços como Google e Apple ID.

10. **Gerenciar Perfil**
* Permite que o usuário (seja ele aluno, fornecedor ou entregador) gerencie as informações principais do seu perfil, como nome, telefone e e-mail.

11. **Gerenciar Pedidos**
* Permite que o fornecedor visualize e gerencie os diferentes pedidos que foram feitos no seu restaurante.

12. **Gerenciar Cardápio** *(<<include>> Gerenciar Itens do Cardápio)*
* Permite que o fornecedor gerencie as configurações do seu cardápio, como itens em destaque, itens em promoção ou conjunto de itens. Esse caso de uso inclui o caso de uso Gerenciar Itens do Cardápio, em que um item específico pode ter suas configurações alteradas pelo fornecedor.

13. **Preparar Pedidos** *(<<include>> Atualizar Status do Pedido)*
* Permite que o fornecedor organize no app quais pedidos estão sendo preparados e estão na lista para serem preparados. Este caso de uso inclui o caso de uso Atualizar Status do Pedido, em que o fornecedor atualiza o estado do pedido para "em preparo", podendo ser visualizado pelo usuário.

14. **Acessar Relatórios de Vendas**
* Permite que o fornecedor tenha uma visão geral das vendas dos pedidos de seu estabelecimento, com gráficos e informações importantes sobre o faturamento do restaurante.

15. **Responder a Avaliações de Clientes**
* Permite que o fornecedor responda, com texto, as avaliações realizadas pelos alunos.

16. **Gerencia Fidelidade com Restaurantes**
* Permite que o entregador se fidelize com um restaurante em específico ou um grupo de restaurantes específicos. Dessa forma, este entregador será responsável por fazer entregas apenas para os restaurantes que tem fidelidade.

17. **Atualiza Status da Entrega**
* Permite que o entregador mude o status de entrega do pedido para "em rota de entrega" quando estiver a caminho do estudante.

18. **Gerenciar Entregas**
* Permite que o entregador visualize as entregas que estão sendo feitas e serão feitas por ele.

19. **Consultar Extrato**
* Permite que o entregador visualize o seu extrato, mostrando o quanto recebeu por cada entrega e também um balanço final do valor total.

20. **Reportar Problema na Entrega**
* Permite que o entregador informe no app algum problema relacionado à entrega do pedido. Entre alguns dos problemas é possível citar: cliente não veio buscar o pedido, restaurante com atraso no pedido.

21. **Consultar Histórico de Entregas**
* Permite que o entregador visualize as entregas que foram feitas anteriormente por ele.

## Relacionamentos Entre os Casos de Uso

Os seguintes relacionamentos foram identificados

* "Realizar Pedido" inclui "Realizar Pagamento"
* "Gerenciar Cardápio" inclui "Gerenciar Itens do Cardápio"
* "Preparar Pedidos" inclui "Atualizar Status do Pedido"

## Considerações Finais

Com o diagrama de casos de uso apresentado neste documento é possível ter uma visão geral das principais funcionalidades do aplicativo. Servindo como um guia do que deverá ser implementado ao realizar cada um dos casos de uso. Além disso, é importante destacar que mesmo fornecendo uma boa noção de como alguns dos requisitos serão implementados, os outros documentos do DAS também são essenciais para que o projeto seja feito de maneira organizada e coerente com o que está sendo proposto com o app PodePedirFCTE.

## Quadro de Participações

| **Membro da equipe** | **Função** |
| :------------- | :--------- |
| [Luiz](https://github.com/luizfaria1989) | Documentação da página |

## Referências

> 1. Agenda System. Visualização Caso de Uso. Disponível em: https://unbarqdsw2024-2.github.io/2024.2_G6_Agenda_Entrega_04/#/./itens-das/4.1.4.CasosDeUso. Acesso em: 17/11/2025.
> 2. PodePedirFCTE. Diagrama de Casos de Uso. Disponível em: https://unbarqdsw2025-2-turma01.github.io/2025.2-T01-G7_PodePedirFCTE_Entrega_02/#/Modelagem/ModelagemOrganizacional/DiagramaDeCasosDeUso. Acesso em: 17/11/2025.

## Histórico de Versões

| **Data**       | **Versão** | **Descrição**                         | **Autor**                                      | **Revisor**                                      | **Data da Revisão** |
| :--------: | :----: | :-------------------------------- | :----------------------------------------: | :----------------------------------------: | :-------------: |
| 17/11/2025 |  `0.1`   | Criação da página. | [`@Luiz`](https://github.com/luizfaria1989) | [`@Ana Joyce`](github.com/anajoyceamorim) |   20/11/2025    |
| 17/11/2025 |  `0.2`   | Documentação da página. | [`@Luiz`](https://github.com/luizfaria1989) | [`@Ana Joyce`](github.com/anajoyceamorim) |   20/11/2025    |
| 18/11/2025 |  `0.3`   | Corrige erros de escrita. | [`@Luiz`](https://github.com/luizfaria1989) | [`@Ana Joyce`](github.com/anajoyecamorim) |   20/11/2025    |
