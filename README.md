# Desafio Clean Architecture

1. Instale as dependências:

   ```bash
   go install github.com/ktr0731/evans@latest
   go install -tags 'mysql' github.com/golang-migrate/migrate/v4/cmd/migrate@latest
   go mod tidy
   ```

2. Execute o Mysql e RabbitMQ:

   ```bash
   docker-compose up -d
   ```

3. Execute as migrations:

   ```bash
   migrate -path=internal/infra/database/migrations -database "mysql://root:root@tcp(localhost:3306)/orders" -verbose up
   ```

4. Execute os servidores:

   ```bash
   go run cmd/ordersystem/wire_gen.go cmd/ordersystem/main.go
   ```

### Testando Endpoints
## 1 - API REST
Instale a dependência REST Clien no vscode.
Na pasta /api temos os arquivos para testes dos nossos endpoints REST na porta 8080.
- create_order.http (POST /order para criar Orders)
- list_orders.http  (GET /order para requisitar a lista de Orders)

## 2 - GraphQL
Em http://localhost:8082, podemos executar os comandos GraphQL.

Criando Orders:
```sql
mutation createOrder {
  createOrder(input: {id: "bola2", Price: 10.00, Tax: 1.00}), {
    id
    Price
    Tax
    FinalPrice
  }
}
```

Listando Orders:
```sql
query listOrders {
  listOrders {
    id
    Price
    Tax
    FinalPrice
  }
}
```

## 3 - gRPC
Executando o cliente gRPC:
```bash
evans -r repl -p 8081
```

Conectando-se ao package pb:
```bash
package pb
```

Conectando-se ao Service de Orders:
```bash
service OrderService
```

Criando uma Order:
```bash
call CreateOrder
```

Listando as Orders:
```bash
call ListOrders
```