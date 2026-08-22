## A practice project on implementing Database partitioning using Postgres and Spring boot

### Key-notes
- There are two models here OldProduct & Product
- Hibernate ddl-auto is set to none so that we can handle the schema manipulation, not hibernate and the models are used only to manipulate records.
- The InitializeDBTables is marked with @Component and in its @PostConstruct, it initializes all tables. Here is what the query does:
  - Drop existing tables
  - Create a new table `old_product` with 4 indexes and no partition
  - Create a new table `product` with 4 indexes and partitions.
    - Each partition will contain 10 million data
    - Partition is applied on product id field: `PARTITION BY RANGE(product_id)`
    - Each partition can hold at most 10 million data.
    - There is a total of 10 partition tables. All tables can store a total of 100 millions product.
  - Then the query runs a loop to create 100 million products.
  - Each of the products are stored both in old product table and the partitioned product table.
### Running application
- Run the docker compose file
- Run the application (it may take some minutes to create 100 million product records)
- Hit the endpoint `http://localhost:8080/` to see the results & comparisons of querying in regular old_product table and partitioned product table.

<!-- sync-marker-1 -->
