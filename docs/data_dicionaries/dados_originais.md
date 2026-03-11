# Conjunto de dados públicos de comércio eletrônico brasileiro da *Olist*

## Fonte dos dados

Os dados originais utilizados neste projeto foram obtidos a partir de um *dataset* público disponibilizado na plataforma *[Kaggle](https://www.kaggle.com/)*.

Para acessar o conjunto de dados completo, incluindo sua descrição, metadados e arquivos associados, clique [aqui](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=olist_customers_dataset.csv).

> Para realizar o *download* do *dataset*, é necessário possuir conta registrada na *Kaggle*.

## Licença e autoria

* *Olist* (Proprietário)

* André Sionek (Administrador)

* Francisco Magioli (Editor)

* Leo Dabague (Editor)

O *dataset* disponibilizado está licenciado sob os termos da licença `Creative Commons Attribution–NonCommercial–ShareAlike 4.0 International (CC BY-NC-SA 4.0)`. Para mais informações sobre a licença, clique [aqui](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en).

## Temporalidade

A cobertura temporal deste *dataset* compreende o período de 2016-12-31 (formato YYYY-MM-DD) a 2018-05-31.

Vale resaltar que a última atualização do conjunto de dados na plataforma *Kaggle* ocorreu há quatro anos atrás, não havendo, desde então, inclusão de novos dados ou registros de manutenções. 

## Descrição do *dataset*

Este é um conjunto de dados público brasileiro de *e-commerce* com pedidos realizados na *[Olist Store](https://www.olist.com/)*. O conjunto de dados contém informações de 100 mil pedidos feitos entre 2016 e 2018 em diversos *marketplaces* no Brasil. 

Estes são dados comerciais reais, que foram anonimizados, e as referências às empresas e parceiros no texto da análise foram substituídas pelos nomes das grandes casas de *Game of Thrones*.

### Contexto

Este conjunto de dados foi cedido pela *Olist*, a maior loja de departamentos em *marketplaces* brasileiros. A *Olist* conecta pequenos negócios de todo o Brasil a canais de distribuição de forma descomplicada e com um único contrato. Esses comerciantes podem vender seus produtos pela Loja *Olist* e enviá-los diretamente aos clientes utilizando os parceiros logísticos da *Olist*.

### Estrutura do *dataset*

O *dataset* foi estruturado e disponibilizado em 9 arquivos no formato CSV, organizados de forma a representar a modelagem de dados ilustrado abaixo:

![Modelagem de dados](https://i.imgur.com/HRhd2Y0.png)

## Clientes (olist_customers_dataset.csv)

Este conjunto de dados contém informações sobre os clientes e suas respectivas localizações. 

**Regra de negócio:** Cada pedido é associado a um `customer_id` exclusivo, o que significa que um mesmo cliente pode receber identificadores diferentes para compras distintas. Para contornar essa limitação, o dataset inclui o campo `customer_unique_id`, cuja finalidade é permitir a identificação de clientes recorrentes.

**Granularidade:** Cada linha representa um identificador de cliente associado a um pedido.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência | Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|---------------|
| `customer_id` | Chave para o conjunto de dados de pedidos. Cada pedido possui um `customer_id` único | STRING | Não | 0 | PK |  |  |
| `customer_unique_id` | Identificador único de um cliente | STRING | Não | 0 | Business Key | customers.customer_id (N:1) customers.costumer_unique_id |  |
| `customer_zip_code_prefix` | Primeiros cinco dígitos do CEP do cliente | INTEGER | Não | 0 |  | Pode ser associado a `geolocation.geolocation_zip_code_prefix` | customers (1:1*) geolocation |
| `customer_city` | Nome da cidade do cliente | STRING | Não | 0 |  |  |  |
| `customer_state` | Unidade Federativa do cliente | STRING | Não | 0 |  |  |  |

---

## Vendedores (olist_sellers_dataset.csv)

Este conjunto de dados inclui informações sobre os vendedores que processaram pedidos feitos na *Olist*.

**Granularidade:** Cada linha representa um vendedor

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência | Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|---------------|
| `seller_id` | Identificador único do vendedor | STRING | Não | 0 | PK |  |  |
| `seller_zip_code_prefix` | Primeiros 5 dígitos do CEP do vendedor | INTEGER | Não | 0 |  | Pode ser associado a `geolocation.geolocation_zip_code_prefix` | sellers (1:1*) geolocation |
| `seller_city` | Nome da cidade do vendedor | STRING | Não | 0 |  |  |  |
| `seller_state` | Unidade Federativa do vendedor | STRING | Não | 0 |  |  |  |

---

## Pedidos (olist_orders_dataset.csv)

Este é o conjunto de dados principal. A partir de cada pedido, você pode encontrar todas as outras informações.

**Granularidade:** Cada linha representa um pedido.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência | Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|---------------|
| `order_id` | Identificador único do pedido | STRING | Não | 0 | PK |  |  |
| `customer_id` | Chave para o conjunto de dados do cliente. Cada pedido possui um `customer_id` único | STRING | Não | 0 | FK | `customers.customer_id` | customers (1:N) orders |
| `order_status` | Referência ao status do pedido (entregue, enviado, etc.) | STRING | Não | 0 |  |  |  |
| `order_purchase_timestamp` |  Exibe o registro de data e hora da compra | DATETIME | Não | 0 |  |  |  |
| `order_approved_at` | Exibe o registro de data e hora da aprovação do pagamento | DATETIME | Não | 0 |  |  |  |
| `order_delivered_carrier_date` | Exibe o registro de data e hora da postagem do pedido. Indica quando ele foi entregue ao parceiro logístico | DATETIME | Sim | 2 |  |  |  |
| `order_delivered_customer_date` | Mostra a data real de entrega do pedido ao cliente | DATETIME | Sim | 3 |  |  |  |
| `order_estimated_delivery_date` | Exibe a data de entrega estimada que foi informada ao cliente no momento da compra | DATETIME | Não | 0 |  |  |  |

---

## Itens do pedido (olist_order_items_dataset.csv)

Este conjunto de dados inclui informações sobre os itens comprados em cada pedido. 

**Regra de negócio:** O pedido com o `ID = 00143d0f86d6fbd9f9b38ab440ac16f5` contém 3 itens (do mesmo produto). O frete de cada item é calculado de acordo com suas dimensões e peso. Para obter o valor total do frete de cada pedido, basta somá-los:
  * **O valor total do item do pedido é:** 21.33 * 3 = 63.99
  * **O valor total do frete é:** 15.10 * 3 = 45.30
  * **O valor total do pedido (item do pedido + frete) é:** 45.30 + 63.99 = 109.29

**Granularidade:** Cada linha representa um item dentro de um pedido.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |  Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|----------------|
| `order_id` | Identificador único do pedido | STRING | Não | 0 | PK (composta), FK | `orders.order_id` | orders (1:N) order_items |
| `order_item_id` | Número sequencial que identifica a quantidade de itens incluídos no mesmo pedido | INTEGER | Não | 0 | PK (composta) |  |  |
| `product_id` | Identificador único do produto | STRING | Não | 0 | FK | `products.product_id` | products (1:N) order_items | 
| `seller_id` | Identificador único do vendedor | STRING | Não | 0 | FK | `sellers.seller_id` | sellers (1:N) order_items |
| `shipping_limit_date` | Mostra a data limite de envio do vendedor para que ele entregue o pedido ao parceiro logístico | DATETIME | Não | 0 |  |  |  |
| `price` | Preço do item | FLOAT | Não | 0 |  |  |  |
| `freight_value` | Valor do frete por item (se um pedido tiver mais de um item, o valor do frete será dividido entre os itens) | FLOAT | Não | 0 |  |  |  |

---

## Produtos (olist_products_dataset.csv)

Este conjunto de dados inclui informações sobre os produtos vendidos pela *Olist*.

**Granularidade:** Cada linha representa um produto.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |  Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|----------------|
| `product_id` | Identificador único do produto | STRING | Não | 0 | PK |  |  |
| `product_category_name` | Categoria do produto | STRING | Sim | 2 | FK | `product_category_name_translation.product_category_name` | product_category_name_translation (1:N) products |
| `product_name_lenght` | Número de caracteres extraídos do nome do produto | INTEGER | Sim | 2 |  |  |  |
| `product_description_lenght` | Número de caracteres extraídos da descrição do produto | INTEGER | Sim | 2 |  |  |  |
| `product_photos_qty` | Número de fotos de produtos publicadas |INTEGER  | Sim | 2 |  |  |  |
| `product_weight_g` | Peso do produto medido em gramas | INTEGER | Não | 0 |  |  |  |
| `product_length_cm` | Comprimento do produto medido em centímetros | INTEGER | Não | 0 |  |  |  |
| `product_height_cm` | Altura do produto medido em centímetros | INTEGER | Não | 0 |  |  |  |
| `product_width_cm` | Largura do produto medida em centímetros | INTEGER | Não | 0 |  |  |  |

--- 

## Pagamentos (olist_order_payments_dataset.csv)

Este conjunto de dados inclui informações sobre as opções de pagamento dos pedidos.

**Granularidade:** Cada linha representa um pedido.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |  Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|----------------|
| `order_id` | Identificador único de um pedido | STRING | Não | 0 | PK (composta), FK |  `orders.order_id` | orders (1:N) order_payments |
| `payment_sequential` | Um cliente pode pagar um pedido com mais de um método de pagamento. Nesse caso, será criada uma sequência para processar todos os pagamentos | INTEGER | Não | 0 | PK (composta) |  |  |
| `payment_type` | Método de pagamento escolhido pelo cliente | STRING | Não | 0 |  |  |  |
| `payment_installments` | Número de parcelas escolhido pelo cliente | INTEGER | Não | 0 |  |  |  |
| `payment_value` | Valor da transação | FLOAT | Não | 0 |  |  |  |

---

## Avaliações (olist_order_reviews_dataset.csv)

Este conjunto de dados inclui informações sobre as avaliações feitas pelos clientes.

**Regra de negócio:** Após um cliente comprar um produto na **Olist Store**, o vendedor é notificado para processar o pedido. Assim que o cliente recebe o produto, ou quando a data de entrega estimada se aproxima, ele recebe uma pesquisa de satisfação por e-mail, onde pode deixar sua opinião sobre a experiência de compra e escrever comentários.

**Granularidade:** Cada linha representa uma avaliação.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |  Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|----------------|
| `review_id` | Identificador único de avaliação | STRING | Não | 0 | PK |  |  |
| `order_id` | Identificador único do pedido | STRING | Não | 0 | FK | `orders.order_id` | orders (1:0..1) order_reviews |
| `review_score` | Nota de 1 a 5 dada pelo cliente em uma pesquisa de satisfação | INTEGER | Não | 0 |  |  |  |
| `review_comment_title` | Título do comentário deixado pelo cliente | STRING | Sim | 88 |  |  |  |
| `review_comment_message` | Mensagem de comentário da avaliação deixada pelo cliente | STRING | Sim | 59 |  |  |  |
| `review_creation_date` | Mostra a data em que a pesquisa de satisfação foi enviada ao cliente | DATETIME | Não | 0 |  |  |  |
| `review_answer_timestamp` | Exibe o registro de data e hora da resposta da pesquisa de satisfação | DATETIME | Não | 0 |  |  |  |

---

## Geolocalização (olist_geolocation_dataset.csv)

Este conjunto de dados contém informações sobre os CEPs brasileiros e suas coordenadas de latitude e longitude.

**Granularidade:** Cada linha representa uma localização.

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência | Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|---------------|
| `geolocation_zip_code_prefix` | Primeiros 5 dígitos do código postal | INTEGER | Não | 0 |  | Pode ser associado a `customers.customer_zip_code_prefix` e `sellers.seller_zip_code_prefix` |  |
| `geolocation_lat` | Latitude | FLOAT | Não | 0 |  |  |  |
| `geolocation_lng` | Longitude | FLOAT | Não | 0 |  |  |  |
| `geolocation_city` | Nome da cidade | STRING | Não | 0 |  |  |  |
| `geolocation_state` | Unidade Federativa | STRING | Não | 0 |  |  |  |

> ⚠️ A tabela `olist_geolocation_dataset.csv` apresenta problemas estruturais relevantes do ponto de vista de modelagem relacional. Embora apresenta representar uma entidade geográfica baseado no prefixo (cinco primeiros dígitos) do CEP, ela não apresenta uma chave primária natural clara. O campo `geolocation_zip_code_prefix` e as coordenadas geográficas também não são únicas, pois aparecem em múltiplas linhas e, portanto, não identificam registros de forma consistente.
> Essa ausência de unicidade impede a definição de uma chave primária confiável e inviabiliza a criação de chaves estrangeiras formais a partir das tabelas de clientes e vendedores, que se comunicam coma a geolocalização apenas por meio do prefixo do CEP. Como um mesmo prefixo pode corresponder a vários registros, o relacionamento trona-se não determinístico.  

---

## Categorias de produtos traduzidas em inglês (product_category_name_translation.csv)

Traduz o nome da categoria do produto para inglês.

**Granularidade:** Cada linha representa uma tradução

| Campo | Descrição | Tipo de dado | Nullable | % Null | Tipo de chave | Referência |  Cardinalidade |
|-------|-----------|--------------|----------|--------|---------------|------------|----------------|
| `product_category_name` | Nome da categoria em português | STRING | Não | 0 | PK |  |  |
| `product_category_name_english` | Nome da categoria em inglês | STRING | Não | 0 |  |  |  |


