# Conjunto de dados públicos de comércio eletrônico brasileiro da *Olist*

## Fonte dos dados

Os dados originais utilizados neste projeto foram obitidos a partir de um *dataset* público disponibilizado na plataforma *[Kaggle](https://www.kaggle.com/)*.

Para acessar o conjunto de dados completo, incluindo sua descrição, metadados e arquivos associados, clique [aqui](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=olist_customers_dataset.csv).

Para realizar o *download* do *dataset*, é necessário possuir conta registrada na plataforma.

## Licença e autoria

* *Olist* (Proprietário)

* André Sionek (Administrador)

* Francisco Magioli (Editor)

* Leo Dabague (Editor)

O *dataset* disponibilizado está licenciado sob os termos da licença `Creative Commons Attribution–NonCommercial–ShareAlike 4.0 International (CC BY-NC-SA 4.0)`. Para mais detalhes, clique [aqui](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en).

## Temporalidade

A cobertura temporal deste *dataset* compreende o período de 2016-12-31 (formato YYYY-MM-DD) a 2018-05-31.

Resalta-se que a última atualizção do conjunto de dados na plataforma *Kaggle* ocorreu há quatro anos atrás, não havendo, desde então, inclussão de novos dados ou registros de manuntenções. 

## Descrição do *dataset*

Este é um conjunto de dados público brasileiro de e-commerce com pedidos realizados na *Olist Store*. O conjunto de dados contém informações de 100 mil pedidos feitos entre 2016 e 2018 em diversos *marketplaces* no Brasil. 

Estes são dados comerciais reais, que foram anonimizados, e as referências às empresas e parceiros no texto da análise foram substituídas pelos nomes das grandes casas de *Game of Thrones*.

### Contexto

Este conjunto de dados foi cedido pela *Olist*, a maior loja de departamentos em *marketplaces* brasileiros. A *Olist* conecta pequenos negócios de todo o Brasil a canais de distribuição de forma descomplicada e com um único contrato. Esses comerciantes podem vender seus produtos pela Loja *Olist* e enviá-los diretamente aos clientes utilizando os parceiros logísticos da *Olist*.

### Estrutura do *dataset*

O *dataset* foi estruturado e disponibilizado em 9 arquivos no formato CSV, organizados de forma a representar a modelagem de dados ilustrado abaixo:

![Modelagem de dados](https://i.imgur.com/HRhd2Y0.png)

## olist_customers_dataset

### Objetivo

Este conjunto de dados contém informações sobre os clientes e suas respectivas localizações. 

### Regra de negócio

Cada pedido é associado a um `customer_id` exclusivo, o que significa que um mesmo cliente pode receber identificadores diferentes para compras distintas.Para contornar essa limitação, o dataset inclui o campo `customer_unique_id`, cuja finalidade é permitir a identificação de clientes recorrentes.

**Granulariedade:** Cada linha representa um pedido.

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `customer_id` | STRING | Chave para o conjunto de dados de pedidos. Cada pedido possui um `customer_id` único | PK |
| `customer_unique_id` | STRING | Identificador único de um cliente | PK |
| `customer_zip_code_prefix` | INTEGER | Primeiros cinco dígitos do CEP do cliente                                          |  |
| `customer_city` | STRING  | Nome da cidade do cliente |  |
| `customer_state` | STRING | Unidade Federativa do cliente |  |

---

## olist_geolocation_dataset

### Objetivo

Este conjunto de dados contém informações sobre os CEPs brasileiros e suas coordenadas de latitude e longitude.

**Granulariedade:** Cada linha representa uma localização.

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `geolocation_zip_code_prefix` | INTEGER | Primeiros 5 dígitos do código postal | PK |
| `geolocation_lat` | INTEGER | Latitude |  |
| `geolocation_lng` | INTEGER | Longitude |  |
| `geolocation_city` | STRING | Nome da cidade |  |
| `geolocation_state` | STRING | Unidade Federativa |  |

---

## olist_order_items_dataset

### Objetivo

Este conjunto de dados inclui informações sobre os itens comprados em cada pedido. 

### Regra de negócio

O pedido com o `ID = 00143d0f86d6fbd9f9b38ab440ac16f5` contém 3 itens (do mesmo produto). O frete de cada item é calculado de acordo com suas dimensões e peso. Para obter o valor total do frete de cada pedido, basta somá-los:

* **O valor total do item do pedido é:** 21.33 * 3 = 63.99

* **O valor total do frete é:** 15.10 * 3 = 45.30

* **O valor total do pedido (item do pedido + frete) é:** 45.30 + 63.99 = 109.29

**Granulariedade:** Cada linha representa um pedido.

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `order_id` | STRING | Identificador único do pedido | PK |
| `order_item_id` | INTEGER | Número sequencial que identifica a quantidade de itens incluídos no mesmo pedido. | FK |
| `product_id` | STRING | Identificador único do produto | FK |
| `seller_id` | STRING | Identificador único do vendedor | FK |
| `shipping_limit_date` | DATETIME | Mostra a data limite de envio do vendedor para que ele entregue o pedido ao parceiro logístico |  |
| `price` | INTEGER | Preço do item |  |
| `freight_value` | INTEGER | Valor do frete por item (se um pedido tiver mais de um item, o valor do frete será dividido entre os itens) |  |

### Relacionamentos

---

## olist_order_payments_dataset

### Objetivo

Este conjunto de dados inclui informações sobre as opções de pagamento dos pedidos.

**Granulariedade:** Cada linha representa um pedido.

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `order_id` | STRING | Identificador único de um pedido | PK |
| `payment_sequential` | INTEGER | Um cliente pode pagar um pedido com mais de um método de pagamento. Nesse caso, será criada uma sequência para processar todos os pagamentos |  |
| `payment_type` | STRING | Método de pagamento escolhido pelo cliente |  |
| `payment_installments` | INTEGER | Número de parcelas escolhido pelo cliente |  |
| `payment_value` | INTEGER | Valor da transação |  |

---

## olist_order_reviews_dataset

### Objetivo

Este conjunto de dados inclui informações sobre as avaliações feitas pelos clientes.

### Regra de negócio

Após um cliente comprar um produto na **Olist Store**, o vendedor é notificado para processar o pedido. Assim que o cliente recebe o produto, ou quando a data de entrega estimada se aproxima, ele recebe uma pesquisa de satisfação por e-mail, onde pode deixar sua opinião sobre a experiência de compra e escrever comentários.

**Granulariedade:** Cada linha representa uma avalização.

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `review_id` | STRING | Identificador único de avaliação | PK |
| `order_id` | STRING | Identificador único do pedido | FK |
| `review_score` | INTEGER | Nota de 1 a 5 dada pelo cliente em uma pesquisa de satisfação |  |
| `review_comment_title` | STRING | Título do comentário deixado pelo cliente | 88% Null |
| `review_comment_message` | STRING | Mensagem de comentário da avaliação deixada pelo cliente | 59% Null |
| `review_creation_date` | DATETIME | Mostra a data em que a pesquisa de satisfação foi enviada ao cliente |  |
| `review_answer_timestamp` | DATETIME | Exibe o registro de data e hora da resposta da pesquisa de satisfação |  |

## olist_orders_dataset

### Objetivo

Este é o conjunto de dados principal. A partir de cada pedido, você pode encontrar todas as outras informações.

**Granulariedade:** Cada linha representa um pedido.

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `order_id` | STRING | Identificador único do pedido | PK |
| `customer_id` | STRING | Chave para o conjunto de dados do cliente. Cada pedido possui um `customer_id` único | FK |
| `order_status` | STRING | Referência ao status do pedido (entregue, enviado, etc.) |  |
| `order_purchase_timestamp` | DATETIME | Exibe o registro de data e hora da compra |  |
| `order_approved_at` | DATETIME | Exibe o registro de data e hora da aprovação do pagamento |  |
| `order_delivered_carrier_date` | DATETIME | Exibe o registro de data e hora da postagem do pedido. Indica quando ele foi entregue ao parceiro logístico | 2% Null |
| `order_delivered_customer_date` | DATETIME | Mostra a data real de entrega do pedido ao cliente | 3% Null |
| `order_estimated_delivery_date` | DATETIME | Exibe a data de entrega estimada que foi informada ao cliente no momento da compra |  |

---

## olist_products_dataset

### Obejetivo

Este conjunto de dados inclui informações sobre os produtos vendidos pela *Olist*.

**Granularieade:** Cada linha representa um produto.

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `product_id` | STRING | Identificador único do produto | PK |
| `product_category_name` | STRING | Categoria do produto | 2% Null |
| `product_name_lenght` | INTEGER | Número de caracteres extraídos do nome do produto | 2% Null |
| `product_description_lenght` | INTEGER | Número de caracteres extraídos da descrição do produto | 2% Null |
| `product_photos_qty` | INTEGER | Número de fotos de produtos publicadas | 2% Null |
| `product_weight_g` | INTEGER | Peso do produto medido em gramas |  |
| `product_length_cm` | INTEGER | Comprimento do produto medido em centímetros |  |
| `product_height_cm` | INTEGER | Altura do produto medido em centímetros |  |
| `product_width_cm` | INTEGER | Largura do produto medida em centímetros |  |

--- 

## olist_sellers_dataset

### Objetivo

Este conjunto de dados inclui informações sobre os vendedores que processaram pedidos feitos na *Olist*.

**Granulariedade:** Cada linha representa um vendedor

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `seller_id` | STRING | Identificador único do vendedor | PK |
| `seller_zip_code_prefix` | INTEGER | Primeiros 5 dígitos do CEP do vendedor |  |
| `seller_city` | STRING | Nome da cidade do vendedor |  |
| `seller_state` | STRING | Unidade Federativa do vendedor |  |

## product_category_name_translation

### Objetivo

Traduz o nome da categoria do produto para inglês.

**Granulariedade:** Cada linha representa uma tradução

| Campo | Tipo original | Descrição | Observações |
|-------|---------------|-----------|-------------|
| `seller_id` | STRING | Nome da categoria em português |  |
| `seller_zip_code_prefix` | STRING | Nome da categoria em inglês |  |
