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
```SQL

```















