# Repository2RP
![GitHub](https://img.shields.io/github/license/IgorDamascenoM/Repository2RP?style=social)

# Sobre o Projeto
Repositório criado para apresentação do teste de conhecimento

# Modelo Conceitual

![Diagrama (2)](https://user-images.githubusercontent.com/96548834/147158113-1c94fc78-8de3-4d08-aa17-4ad3a63e94ec.jpeg)

# Criação da tabelas 

Foi utilizado o SQL Server para criação das tabelas

# Tabelas

```SQL

CREATE TABLE [dbo].[Person.Person](
	[BusinessEntityID] [int] IDENTITY(1,1) NOT NULL,
	[PersonType] [varchar](50) NULL,
	[NameStyle] [varchar](50) NULL,
	[Title] [varchar](50) NULL,
	[FirstName] [varchar](50) NULL,
	[MiddleName] [varchar](50) NULL,
	[LastName] [varchar](50) NULL,
	[Suffix] [varchar](2000) NULL,
	[EmailPromotion] [varchar](50) NULL,
	[AdditionalContactInfo] [varchar](255) NULL,
	[Demographics] [varchar](255) NULL,
	[rowguid] [varchar](255) NULL,
	[ModifiedDate] [datetime] NULL,
 CONSTRAINT [PK_Person.Person] PRIMARY KEY CLUSTERED 

```
```SQL
CREATE TABLE [dbo].[Production.Product](
	[ProductID] [int] NOT NULL,
	[Name] [varchar](50) NULL,
	[ProductNumber] [varchar](50) NULL,
	[MakeFlag] [varchar](50) NULL,
	[FinishedGoodsFlag] [varchar](50) NULL,
	[Color] [varchar](50) NULL,
	[SafetyStockLevel] [int] NULL,
	[ReorderPoint] [varchar](50) NULL,
	[StandardCost] [float] NULL,
	[ListPrice] [float] NULL,
	[Size] [varchar](50) NULL,
	[SizeUnitMeasureCode] [varchar](50) NULL,
	[WeightUnitMeasureCode] [varchar](50) NULL,
	[Weight] [varchar](50) NULL,
	[DaysToManufacture] [int] NULL,
	[ProductLine] [varchar](50) NULL,
	[Class] [varchar](50) NULL,
	[Style] [varchar](50) NULL,
	[ProductSubcategoryID] [varchar](50) NULL,
	[ProductModelID] [varchar](50) NULL,
	[SellStartDate] [varchar](50) NULL,
	[SellEndDate] [varchar](50) NULL,
	[DiscontinuedDate] [varchar](50) NULL,
	[rowguid] [varchar](50) NULL,
	[ModifiedDate] [datetime] NULL,
 CONSTRAINT [PK_Production.Product] PRIMARY KEY CLUSTERED
```
```SQL
CREATE TABLE [dbo].[Sales.Customer](
	[CustomerID] [int] NOT NULL,
	[PersonID] [int] NULL,
	[StoreID] [int] NULL,
	[TerritoryID] [int] NULL,
	[AccountNumber] [varchar](50) NULL,
	[rowguid] [varchar](50) NULL,
	[ModifiedDate] [datetime] NULL,
 CONSTRAINT [PK_Sales.Customer] PRIMARY KEY CLUSTERED
```
```SQL
CREATE TABLE [dbo].[Sales.SalesOrderDetail](
	[SalesOrderID] [int] NOT NULL,
	[SalesOrderDetailID] [int] NOT NULL,
	[CarrierTrackingNumber] [varchar](50) NULL,
	[OrderQty] [int] NULL,
	[ProductID] [int] NULL,
	[SpecialOfferID] [int] NULL,
	[UnitPrice] [float] NULL,
	[UnitPriceDiscount] [float] NULL,
	[LineTotal] [float] NULL,
	[rowguid] [varchar](50) NULL,
	[ModifiedDate] [datetime] NULL,
 CONSTRAINT [pk_orderdetail] PRIMARY KEY CLUSTERED 
```
```SQL
CREATE TABLE [dbo].[Sales.SalesOrderHeader](
	[SalesOrderID] [int] NOT NULL,
	[RevisionNumber] [int] NULL,
	[OrderDate] [datetime] NULL,
	[DueDate] [datetime] NULL,
	[ShipDate] [datetime] NULL,
	[Status] [int] NULL,
	[OnlineOrderFlag] [bit] NULL,
	[SalesOrderNumber] [varchar](50) NULL,
	[PurchaseOrderNumber] [varchar](50) NULL,
	[AccountNumber] [varchar](50) NULL,
	[CustomerID] [int] NULL,
	[SalesPersonID] [int] NULL,
	[TerritoryID] [int] NULL,
	[BillToAddressID] [int] NULL,
	[ShipToAddressID] [int] NULL,
	[ShipMethodID] [int] NULL,
	[CreditCardID] [int] NULL,
	[CreditCardApprovalCode] [varchar](50) NULL,
	[CurrencyRateID] [int] NULL,
	[SubTotal] [float] NULL,
	[TaxAmt] [float] NULL,
	[Freight] [float] NULL,
	[TotalDue] [float] NULL,
	[Comment] [varchar](50) NULL,
	[rowguid] [varchar](50) NULL,
	[ModifiedDate] [datetime] NOT NULL,
 CONSTRAINT [PK_Sales.SalesOrderHeader] PRIMARY KEY CLUSTERED 
```
```SQL
CREATE TABLE [dbo].[Sales.SpecialOfferProduct](
	[SpecialOfferID] [int] NOT NULL,
	[ProductID] [int] NOT NULL,
	[rowguid] [varchar](50) NOT NULL,
	[ModifiedDate] [datetime] NOT NULL,
 CONSTRAINT [pk_specialoffer] PRIMARY KEY CLUSTERED 
```
# Criando o relacionamento entre tabelas
```SQL
-- Chave estrangeira PersonID na tabela Sales.Costumer

alter table [Sales.Customer]
add foreign key (PersonID)
references [Person.person] (BusinessEntityID)

-- Chave estrangeira CustomerID na tabela Sales.SalesOrderHeader

alter table [Sales.SalesOrderHeader]
add foreign key (CustomerID)
references [Sales.Customer] (CustomerID)

-- Chave estrangeira ProductID na tabela SpecialOfferProduct

alter table [Sales.SpecialOfferProduct]
add foreign key (ProductID)
references [Production.Product] (ProductID)

-- Chaves estrangeiras ProductID, SpecialOfferID e SalesOrderID na tabela SaleOrderDetail

alter table [Sales.SalesOrderDetail]
add foreign key (SalesOrderID)
references [Sales.SalesOrderHeader] (SalesOrderID)

alter table [Sales.SalesOrderDetail]
add foreign key (ProductID)
references [Production.Product] (ProductID)

alter table [Sales.SalesOrderDetail]
add foreign key (SpecialOfferID,ProductID)
references [Sales.SpecialOfferProduct] (SpecialOfferID,ProductID)

-- criando chave primaria para a tabela SpecialOfferProduct

alter table [Sales.SpecialOfferProduct]
add CONSTRAINT pk_specialoffer primary key(SpecialOfferID,ProductID)

-- criando chave primaria para a tabela SalesOrderDetail

alter table [Sales.SalesOrderDetail]
add CONSTRAINT pk_orderdetail primary key(SalesOrderID,SalesOrderDetailID)
```
# Desafios
Desafio 1
```SQL
--Escreva uma query que retorna a quantidade de linhas na tabela Sales.SalesOrderDetail pelo 
--campo SalesOrderID, desde que tenham pelo menos três linhas de detalhes.
select
	count (*) as qtdlinhas
from (

	select 
		count (SalesOrderDetailID) as qtd, 
		SalesOrderID
	from [Sales.SalesOrderDetail]
	group by SalesOrderID

) as a

where qtd >= 3
```
Desafio 2
```SQL
--Escreva uma query que ligue as tabelas Sales.SalesOrderDetail, Sales.SpecialOfferProduct e 
--Production.Product e retorne os 3 produtos (Name) mais vendidos (pela soma de OrderQty), 
--agrupados pelo número de dias para manufatura (DaysToManufacture).
select * from 
	(
	select c.DaysToManufacture, c.Name, SUM (a.OrderQty) as TotalVendas,
	
	ROW_NUMBER() over(partition by c.DaysToManufacture order by SUM (a.OrderQty)desc) as posicao
	from [Sales.SalesOrderDetail] as a
	inner join [Sales.SpecialOfferProduct] as b
	on a.ProductID = b.ProductID
	and a.SpecialOfferID = b.SpecialOfferID
	inner join [Production.Product] as c
	on b.ProductID = c.ProductID
	Group by c.Name, c.DaysToManufacture
	) as tableposition
where posicao <= 3

```
Desafio 3
```SQL
--Escreva uma query ligando as 
--tabelas Person.Person, Sales.Customer e Sales.SalesOrderHeader de forma a obter 
--uma lista de nomes de clientes e uma contagem de pedidos efetuados.

select c.BusinessEntityID, c.FirstName, c.LastName, COUNT (SalesOrderID) as compras from [Sales.SalesOrderHeader] as a

inner join [Sales.Customer] as b

on a.CustomerID = b.CustomerID

inner join [Person.Person] as c

on b.PersonID = c.BusinessEntityID

group by c.BusinessEntityID, c.FirstName, c.LastName
```
Desafio 4
```SQL
--Escreva uma query usando as tabelas Sales.SalesOrderHeader, Sales.SalesOrderDetail e Production.Product,
--de forma a obter a soma total de produtos (OrderQty) por ProductID e OrderDate.

select SUM (OrderQty) as SomaTotal, c.Name, c.ProductID, a.OrderDate from [Sales.SalesOrderHeader] as a
inner join [Sales.SalesOrderDetail] as b
on a.SalesOrderID = b.SalesOrderID
inner join [Production.Product] as c
on b.ProductID = c.ProductID

group by c.ProductID, a.OrderDate, c.Name

order by c.Name
```
Desafio 5
```SQL
--Escreva uma query mostrando os campos SalesOrderID, OrderDate e TotalDue da tabela 
--Sales.SalesOrderHeader. Obtenha apenas as linhas onde a ordem tenha sido feita 
--durante o mês de setembro/2011 e o total devido esteja acima de 1.000. Ordene pelo total devido decrescente.

select SalesOrderID,  OrderDate, TotalDue
from [Sales.SalesOrderHeader]
 
where 
	Status = 1
and OrderDate between '2011-09-01' and '2011-09-30'
and TotalDue > 1000
order by TotalDue desc, OrderDate

```
# Arquivos usados para realizar a atividade
[2RPNET.zip](https://github.com/IgorDamascenoM/Repository2RP/files/7765439/2RPNET.zip)

# Autor
Igor Damasceno Mota














