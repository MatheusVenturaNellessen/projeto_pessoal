# Conjunto de dados públicos de comércio eletrônico brasileiro da *Olist*

## Fonte dos dados

Os dados originais utilizados neste projeto foram obtidos a partir de um *dataset* público disponibilizado na plataforma *[Kaggle](https://www.kaggle.com/)*.

Para acessar o conjunto de dados completo, incluindo sua descrição, metadados e arquivos associados, clique [aqui](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=olist_customers_dataset.csv).

> Para realizar o *download* do *dataset*, é necessário possuir conta registrada na plataforma.

## Licença e autoria

* *Olist* (Proprietário)

* André Sionek (Administrador)

* Francisco Magioli (Editor)

* Leo Dabague (Editor)

O *dataset* disponibilizado está licenciado sob os termos da licença `Creative Commons Attribution–NonCommercial–ShareAlike 4.0 International (CC BY-NC-SA 4.0)`. Para mais detalhes, clique [aqui](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en).

## Temporalidade

A cobertura temporal deste *dataset* compreende o período de 2016-12-31 (formato YYYY-MM-DD) a 2018-05-31.

vale resaltar que a última atualização do conjunto de dados na plataforma *Kaggle* ocorreu há quatro anos atrás, não havendo, desde então, inclusão de novos dados ou registros de manutenções. 

## Descrição do *dataset*

Este é um conjunto de dados público brasileiro de e-commerce com pedidos realizados na *Olist Store*. O conjunto de dados contém informações de 100 mil pedidos feitos entre 2016 e 2018 em diversos *marketplaces* no Brasil. 

Estes são dados comerciais reais, que foram anonimizados, e as referências às empresas e parceiros no texto da análise foram substituídas pelos nomes das grandes casas de *Game of Thrones*.

### Contexto

Este conjunto de dados foi cedido pela *Olist*, a maior loja de departamentos em *marketplaces* brasileiros. A *Olist* conecta pequenos negócios de todo o Brasil a canais de distribuição de forma descomplicada e com um único contrato. Esses comerciantes podem vender seus produtos pela Loja *Olist* e enviá-los diretamente aos clientes utilizando os parceiros logísticos da *Olist*.

### Estrutura do *dataset*

O *dataset* foi estruturado e disponibilizado em 9 arquivos no formato CSV, organizados de forma a representar a modelagem de dados ilustrado abaixo:

![Modelagem de dados](https://i.imgur.com/HRhd2Y0.png)

## Customers (olist_customers_dataset.csv)

Este conjunto de dados contém informações sobre os clientes e suas respectivas localizações. 

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `customer_id` | Chave para o conjunto de dados de pedidos. Cada pedido possui um `customer_id` único | STRING | Não | 0 | PK |  |
| `customer_unique_id` | Identificador único de um cliente | STRING | Não | 0 | Business Key |  |
| `customer_zip_code_prefix` | Primeiros cinco dígitos do CEP do cliente | INTEGER | Não | 0 | FK | `geolocation.geolocation_zip_code_prefix` |
| `customer_city` | Nome da cidade do cliente | STRING | Não | 0 |  |  |
| `customer_state` | Unidade Federativa do cliente | STRING | Não | 0 |  |  |

> **Regra de negócio:** Cada pedido é associado a um `customer_id` exclusivo, o que significa que um mesmo cliente pode receber identificadores diferentes para compras distintas. Para contornar essa limitação, o dataset inclui o campo `customer_unique_id`, cuja finalidade é permitir a identificação de clientes recorrentes.

> **Granularidade:** Cada linha representa um identificador de cliente associado a um pedido.

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**

## Geolocation (olist_geolocation_dataset.csv)

Este conjunto de dados contém informações sobre os CEPs brasileiros e suas coordenadas de latitude e longitude.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `geolocation_zip_code_prefix` | Primeiros 5 dígitos do código postal | INTEGER | Não | 0 | PK |  |
| `geolocation_lat` | Latitude | FLOAT | Não | 0 |  |  |
| `geolocation_lng` | Longitude | FLOAT | Não | 0 |  |  |
| `geolocation_city` | Nome da cidade | STRING | Não | 0 |  |  |
| `geolocation_state` | Unidade Federativa | STRING | Não | 0 |  |  |

> **Granularidade:** Cada linha representa uma localização.

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**

---

## Order Items (olist_order_items_dataset.csv)

Este conjunto de dados inclui informações sobre os itens comprados em cada pedido. 

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `order_id` | Identificador único do pedido | STRING | Não | 0 | PK (composta) |  |
| `order_item_id` | Número sequencial que identifica a quantidade de itens incluídos no mesmo pedido | INTEGER | Não | 0 | PK (composta) |  |
| `product_id` | Identificador único do produto | STRING | Não | 0 | FK | `products.product_id` |
| `seller_id` | Identificador único do vendedor | STRING | Não | 0 | FK | `sellers.seller_id` |
| `shipping_limit_date` | Mostra a data limite de envio do vendedor para que ele entregue o pedido ao parceiro logístico | DATETIME | Não | 0 |  |  |
| `price` | Preço do item | FLOAT | Não | 0 |  |  |
| `freight_value` | Valor do frete por item (se um pedido tiver mais de um item, o valor do frete será dividido entre os itens) | FLOAT | Não | 0 |  |  |

> **Regra de negócio:** O pedido com o `ID = 00143d0f86d6fbd9f9b38ab440ac16f5` contém 3 itens (do mesmo produto). O frete de cada item é calculado de acordo com suas dimensões e peso. Para obter o valor total do frete de cada pedido, basta somá-los:
> * **O valor total do item do pedido é:** 21.33 * 3 = 63.99
> * **O valor total do frete é:** 15.10 * 3 = 45.30
> * **O valor total do pedido (item do pedido + frete) é:** 45.30 + 63.99 = 109.29

> **Granularidade:** Cada linha representa um item dentro de um pedido.

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**

---

## Order Payments (olist_order_payments_dataset.csv)

Este conjunto de dados inclui informações sobre as opções de pagamento dos pedidos.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `order_id` | Identificador único de um pedido | STRING | Não | 0 | PK |  |
| `payment_sequential` | Um cliente pode pagar um pedido com mais de um método de pagamento. Nesse caso, será criada uma sequência para processar todos os pagamentos | INTEGER | Não | 0 |  |  |
| `payment_type` | Método de pagamento escolhido pelo cliente | STRING | Não | 0 |  |  |
| `payment_installments` | Número de parcelas escolhido pelo cliente | INTEGER | Não | 0 |  |  |
| `payment_value` | Valor da transação | FLOAT | Não | 0 |  |  |

> **Granularidade:** Cada linha representa um pedido.

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**

---

## Orders Reviews (olist_order_reviews_dataset.csv)

Este conjunto de dados inclui informações sobre as avaliações feitas pelos clientes.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `review_id` | Identificador único de avaliação | STRING | Não | 0 | PK |  |
| `order_id` | Identificador único do pedido | STRING | Não | 0 | FK | `orders.order_id` |
| `review_score` | Nota de 1 a 5 dada pelo cliente em uma pesquisa de satisfação | INTEGER | Não | 0 |  |  |
| `review_comment_title` | Título do comentário deixado pelo cliente | STRING | Sim | 88 |  |  |
| `review_comment_message` | Mensagem de comentário da avaliação deixada pelo cliente | STRING | Sim | 59 |  |  |
| `review_creation_date` | Mostra a data em que a pesquisa de satisfação foi enviada ao cliente | DATETIME | Não | 0 |  |  |
| `review_answer_timestamp` | Exibe o registro de data e hora da resposta da pesquisa de satisfação | DATETIME | Não | 0 |  |  |

> **Regra de negócio:** Após um cliente comprar um produto na **Olist Store**, o vendedor é notificado para processar o pedido. Assim que o cliente recebe o produto, ou quando a data de entrega estimada se aproxima, ele recebe uma pesquisa de satisfação por e-mail, onde pode deixar sua opinião sobre a experiência de compra e escrever comentários.

> **Granularidade:** Cada linha representa uma avaliação.

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**

---

## Orders (olist_orders_dataset.csv)

Este é o conjunto de dados principal. A partir de cada pedido, você pode encontrar todas as outras informações.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `order_id` | Identificador único do pedido | STRING | Não | 0 | PK |  |
| `customer_id` | Chave para o conjunto de dados do cliente. Cada pedido possui um `customer_id` único | STRING | Não | 0 | FK | `customers.customer_id` |
| `order_status` | Referência ao status do pedido (entregue, enviado, etc.) | STRING | Não | 0 |  |  |
| `order_purchase_timestamp` |  Exibe o registro de data e hora da compra | DATETIME | Não | 0 |  |  |
| `order_approved_at` | Exibe o registro de data e hora da aprovação do pagamento | DATETIME | Não | 0 |  |  |
| `order_delivered_carrier_date` | Exibe o registro de data e hora da postagem do pedido. Indica quando ele foi entregue ao parceiro logístico | DATETIME | Sim | 2 |  |  |
| `order_delivered_customer_date` | Mostra a data real de entrega do pedido ao cliente | DATETIME | Sim | 3 |  |  |
| `order_estimated_delivery_date` | Exibe a data de entrega estimada que foi informada ao cliente no momento da compra | DATETIME | Não | 0 |  |  |

> **Granularidade:** Cada linha representa um pedido.

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**

---

## Products (olist_products_dataset.csv)

Este conjunto de dados inclui informações sobre os produtos vendidos pela *Olist*.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `product_id` | Identificador único do produto | STRING | Não | 0 | PK |  |
| `product_category_name` | Categoria do produto | STRING | Sim | 2 |  |  |
| `product_name_lenght` | Número de caracteres extraídos do nome do produto | INTEGER | Sim | 2 |  |  |
| `product_description_lenght` | Número de caracteres extraídos da descrição do produto | INTEGER | Sim | 2 |  |  |
| `product_photos_qty` | Número de fotos de produtos publicadas |INTEGER  | Sim | 2 |  |  |
| `product_weight_g` | Peso do produto medido em gramas | INTEGER | Não | 0 |  |  |
| `product_length_cm` | Comprimento do produto medido em centímetros | INTEGER | Não | 0 |  |  |
| `product_height_cm` | Altura do produto medido em centímetros | INTEGER | Não | 0 |  |  |
| `product_width_cm` | Largura do produto medida em centímetros | INTEGER | Não | 0 |  |  |

> **Granularidade:** Cada linha representa um produto.

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**

--- 

## Sellers (olist_sellers_dataset.csv)

Este conjunto de dados inclui informações sobre os vendedores que processaram pedidos feitos na *Olist*.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `seller_id` | Identificador único do vendedor | STRING | Não | 0 | PK |  |
| `seller_zip_code_prefix` | Primeiros 5 dígitos do CEP do vendedor | INTEGER | Não | 0 | FK | `geolocation.geolocation_zip_code_prefix` |
| `seller_city` | Nome da cidade do vendedor | STRING | Não | 0 |  |  |
| `seller_state` | Unidade Federativa do vendedor | STRING | Não | 0 |  |  |

> **Granularidade:** Cada linha representa um vendedor

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**

## Product Category Name Translation (product_category_name_translation.csv)

Traduz o nome da categoria do produto para inglês.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |
|-------|-----------|--------------|----------|--------|---------------|------------|
| `product_category_name` | Nome da categoria em português | STRING | Não | 0 |  |  |
| `product_category_name_english` | Nome da categoria em inglês | STRING | Não | 0 |  |  |

> **Granularidade:** Cada linha representa uma tradução

> **Dimensão dos dados:**
> * **Total de registros:**
> * **Total de colunas:**
