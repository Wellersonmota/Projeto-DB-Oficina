# 🛠️ Projeto Integrado: Gestão de E-commerce, Delivery e Ordem de Serviço (OS) Veicular

## 📝 Descrição do Projeto

Este projeto apresenta um **Modelo de Banco de Dados Relacional (EER)** unificado, desenhado para gerenciar um negócio complexo que opera tanto como uma **Loja Virtual com Sistema de Delivery** quanto como uma **Oficina Mecânica** que executa serviços em veículos.

O esquema conceitual integra os fluxos de venda (`Pedido`) e logística (`Entrega` e `Pagamento`) com os fluxos de serviço especializado e manutenção (`OrdemDeServico`, `Veiculo`, `Mecanico` e `Peca`).

## 🗄️ Estrutura do Banco de Dados (Diagrama EER)

O modelo é composto por entidades que cobrem três pilares principais de negócios:

### 1. Clientes e Vendas (Core E-commerce)

| Entidade | Função | Relações Chave |
| :--- | :--- | :--- |
| **`Cliente`** | Cadastro principal (Nome, CPF, Contato). | 1:N com `Pedido` e 1:N com `Veiculo`. |
| **`Pedido`** | Representa uma transação de compra/venda de produtos ou serviços de delivery. | 1:1 com `Entrega` e `Pagamento`. |
| **`Pagamento`** | Detalhes financeiros de um `Pedido`. | |
| **`Entrega`** | Logística e *status* de entrega (Frete, Valor do Pedido). | |

### 2. Gestão de Serviços Especializados (Oficina)

| Entidade | Função | Relações Chave |
| :--- | :--- | :--- |
| **`Veiculo`** | Veículo levado pelo `Cliente` para manutenção. | 1:N com `OrdemDeServico`. |
| **`OrdemDeServico` (OS)** | Documento central para o trabalho executado (Status, Data de Conclusão). | M:N com `Peca` e M:N com `Servico`. |
| **`Servico`** | Tabela de referência para **Mão de Obra**, incluindo o valor de referência. | M:N com `OrdemDeServico`. |
| **`Peca`** | Itens de reposição usados na execução da OS. | M:N com `OrdemDeServico`. |

### 3. Recursos Humanos e Logística Interna

| Entidade | Função | Relações Chave |
| :--- | :--- | :--- |
| **`Mecanico`** | Profissionais responsáveis por executar os serviços. | 1:N com `EquipeMecanica`. |
| **`EquipeMecanica`** | Agrupa os mecânicos e é alocada a uma `OrdemDeServico`. | 1:N com `OrdemDeServico`. |
| **`Responsavel`** | Representa áreas/pessoas envolvidas na gestão dos `Pedidos` ou tarefas genéricas. | M:N com `Pedido`. |

## 💡 Suposições de Modelagem

A complexidade do esquema exigiu as seguintes decisões, que devem ser consideradas na implementação:

1.  **Integração Cliente-Veículo:** Assumiu-se que um `Cliente` pode possuir múltiplos `Veiculos`, e o `Veiculo` é o ponto de partida para a `OrdemDeServico`.
2.  **OS e Pedido:** A `OrdemDeServico` está vinculada ao `Pedido` (1:1), sugerindo que todo serviço de oficina é formalizado através de um pedido no sistema de vendas.
3.  **Cálculo de Custo da OS:** O valor final da `OrdemDeServico` é calculado no *backend* da aplicação, somando-se:
    * Custo das Peças utilizadas (`Peca` * Quantidade na `OrdemDeServico_has_Peca`).
    * Custo da Mão de Obra (`Servico` * Horas/Unidades na `OrdemDeServico_has_Servico`).

## 🛠️ Tecnologias Utilizadas

* **Banco de Dados:** MySQL
* **Modelagem EER:** MySQL Workbench
