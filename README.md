# Sistema-de-Geren-Coringao
banco de dados FACENS semestre 2

## Codigo:

```SQL
-- Criando a tabela Clientes
CREATE TABLE Clientes (
    ClienteID INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(255),
    Email VARCHAR(255),
    Telefone VARCHAR(20),
    Descricao VARCHAR (1000),
    TotalPedido DOUBLE(10, 2)
);
-- Criandof a tabela Pedidos
CREATE TABLE Pedidos (
    PedidoID INT AUTO_INCREMENT PRIMARY KEY,
    ClienteID INT,
    Produto VARCHAR(255),
    Quantidade INT,
    DataPedido DATE,
    Descricao VARCHAR (1000),
    FOREIGN KEY (ClienteID) REFERENCES Clientes(ClienteID)
);

```
## Etapa 1: Criação de Tabelas e Inserção de Dados

Crie as tabelas "Clientes" e "Pedidos" com campos apropriados. Insira dados de exemplo nas tabelas para simular clientes e pedidos. Certifique-se de incluir uma chave primária em cada tabela.

``` SQL
-- Criando a tabela Clientes
CREATE TABLE Clientes (
    ClienteID INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(255),
    Email VARCHAR(255),
    Telefone VARCHAR(20),
    Descricao VARCHAR (1000),
    TotalPedido DOUBLE(10, 2)
);

```
![image](https://github.com/huankzera/Sistema-de-Geren-Coringao/assets/126423433/852a2c57-48ab-4195-8b10-f1050c5222f4)


``` SQL
-- Criandof a tabela Pedidos
CREATE TABLE Pedidos (
    PedidoID INT AUTO_INCREMENT PRIMARY KEY,
    ClienteID INT,
    Produto VARCHAR(255),
    Quantidade INT,
    DataPedido DATE,
    Descricao VARCHAR (1000),
    FOREIGN KEY (ClienteID) REFERENCES Clientes(ClienteID)
);

```
![image](https://github.com/huankzera/Sistema-de-Geren-Coringao/assets/126423433/899812be-707e-47ff-a76d-3b8e00191a09)

## Etapa 3: Trigger

Crie uma trigger que seja acionada APÓS a inserção de um novo pedido na tabela "Pedidos". A trigger deve calcular o valor total dos pedidos para o cliente correspondente e atualizar um campo "TotalPedidos" na tabela "Clientes" com o valor total. Teste a Trigger inserindo um novo pedido na tabela "Pedidos“.
``` SQL
DELIMITER //
CREATE TRIGGER CalculaTotalPedidos
AFTER INSERT ON Pedidos
FOR EACH ROW
BEGIN
    DECLARE total_pedidos DOUBLE(10, 2);
    SELECT SUM(Quantidade) INTO total_pedidos FROM Pedidos WHERE ClienteID = NEW.ClienteID;
    UPDATE clientes SET TotalPedido = total_pedidos WHERE ClienteID = NEW.ClienteID;
END //
DELIMITER ;

```
![image](https://github.com/huankzera/Sistema-de-Geren-Coringao/assets/126423433/5f2bedb1-b514-4642-a32c-267d61044ee9)


## Etapa 4: View
Crie uma view chamada "PedidosClientes" que combina informações das tabelas "Clientes" e "Pedidos" usando JOIN para mostrar os detalhes dos pedidos e os nomes dos clientes.
``` SQL
CREATE VIEW PedidosClientes AS
SELECT Pedidos.*, Clientes.Nome AS NomeCliente
FROM Pedidos
JOIN Clientes ON Pedidos.ClienteID = Clientes.ClienteID;

```

![image](https://github.com/huankzera/Sistema-de-Geren-Coringao/assets/126423433/4f7f8681-36d3-4908-beb3-57c23948e9a2)


## Etapa 5: Consulta com JOIN

Escreva uma consulta SQL que utiliza JOIN para listar os detalhes dos pedidos de cada cliente, incluindo o nome do cliente e o valor total de seus pedidos. Utilize a view "PedidosClientes" para essa consulta.
``` SQL
SELECT 
    pc.NomeCliente,
    p.PedidoID,
    p.Descricao,
    p.Quantidade,
    p.DataPedido,
    c.TotalPedido
FROM 
    PedidosClientes pc
JOIN 
    Pedidos p ON pc.PedidoID = p.PedidoID
JOIN 
    Clientes c ON pc.ClienteID = c.ClienteID;

```
![image](https://github.com/huankzera/Sistema-de-Geren-Coringao/assets/126423433/a525a8fe-c9e6-473d-a9cc-f1212dbc4be0)






