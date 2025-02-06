# CAP Exercício 5: Testar os serviços usando arquivos de teste HTTP

## Objetivo

Neste exercício, você testará todos os serviços implementados no projeto utilizando arquivos HTTP, abrangendo algumas operações CRUD (criação, leitura, atualização e exclusão) nas entidades principais. Esses testes garantem que as validações e as regras de negócio definidas nos exercícios anteriores estão funcionando corretamente.

---

## Pré-requisitos

1. Tenha os serviços do projeto em execução com o comando:

   ```bash
   cds watch
   ```

2. Tenha a extensão REST Client instalada no VS Code. Se ainda não tiver, instale-a através do Marketplace do VS Code.

---

## Passo 1: Organizar os arquivos de teste

Na pasta chamada `tests` na raiz do projeto, criada na aula anterior, renomeie os nomes dos arquivos para manterem um padrão mostrando apenas qual serviço/entidade será testada.

![image](https://github.com/user-attachments/assets/05a1d64d-57e8-4f18-baec-5974e4dbd9e3)

### Estrutura esperada:

```bash
/tests
  ├── passageiro.http
  ├── reserva_passagem.http
```

---

## Passo 2: Criar os arquivos de teste HTTP

### Arquivo `passageiro.http`

#### Criar um novo passageiro

```http
### Adicionar Passageiro
POST http://localhost:4004/airline/Passageiro
Content-Type: application/json

{
  "cpf": "12345678901",
  "nome": "João Silva",
  "email": "joao.silva@example.com",
  "telefone": "11987654321",
  "data_nascimento": "2015-06-10",
  "endereco": "Rua dos Aviadores, 123"
}
```

#### Consultar todos os passageiros

```http
### Consultar Passageiro
GET http://localhost:4004/airline/Passageiro
```

#### Atualizar o nome do passageiro

Nessa parte de atualizar, você vai precisar identificar alguns campos obrigatórios para o SAP CAP entender qual registro atualizar.

Nesse caso de Passageiro, os campos necessários para atualizar o registro são:
- **id_passageiro**: Nesse campo, ele vai no URL da requisição e o valor dele, você pega usando o método GET
- **cpf**: Ele também vai no URL da requisição. Seu valor você buscará no método GET
- **data_nascimento**: Ele também irá no URL da requisição e seu valor, você buscará no método GET

**IMPORTANTE:** Esses campos obrigatórios variam de entidade para entidade. Você deve saber o que é necessário para sua requisição.

Além disso, por nós estarmos usando a annotation `@odata.etag`, é necessário colocarmos no cabeçalho da requisição um campo chamado `If-Match`. Seu valor vai ser igual ao valor do campo `ModifiedAt` que aparecerá no GET.

**Isso vale para o `DELETE` também**. Caso você não esteja usando essa annotation, você não precisa disso.

![image](https://github.com/user-attachments/assets/33216e64-129d-468b-a975-2cd5d80ed673)

Após isso, você pode colocar no conteúdo que deseja atualizar.

```http
### Atualizar Passageiro
PUT/PATCH http://localhost:4004/airline/Passageiro(id_passageiro='',cpf='',data_nascimento='')
Content-Type: application/json
If-Match: ""

{
  "nome": "João Silva Atualizado"
}
```

**PUT vs PATCH**
- **PUT**: Todo campo que EXISTE na entidade, e você NÃO COLOCAR dentro do body da requisição, ele retornará `null`.
- **PATCH**: Somente os campos que você colocar serão ALTERADAS. Campos que você NÃO COLOCAR, não serão alterados.

#### Deletar um passageiro

```http
### Deletar Passageiro
DELETE http://localhost:4004/airline/Passageiro(id_passageiro='')
If-Match: ""
```

---

### Arquivo `reserva_passagem.http`

#### Criar uma reserva válida

```http
### Criar Reserva
POST http://localhost:4004/airline/ReservaPassagem
Content-Type: application/json

{
  "id_passageiro": "*", // buscar o valor do id_passageiro com o método GET (REMOVER COMENTÁRIO)
  "assento": "12A",
  "classe": "Econômica",
  "status": "Confirmada",
  "data_reserva": "2025-01-01",
  "preco": 1200.50
}
```

#### Consultar todas as reservas

```http
### Buscar Reserva
GET http://localhost:4004/airline/ReservaPassagem
```

#### Atualizar o status da reserva

```http
### Atualizar Reserva
PUT/PATCH http://localhost:4004/airline/ReservaPassagem(id_reserva='')
Content-Type: application/json
If-Match: "" // Se for null o valor do ModifiedAt, pode colocar null sem as aspas (REMOVER COMENTÁRIO)

{
  "status": "Cancelada"
}
```

#### Deletar uma reserva de passagem

```http
### Deletar Reserva
DELETE http://localhost:4004/airline/ReservaPassagem(id_reserva='')
If-Match: "" // Se for null o valor do ModifiedAt, pode colocar null sem as aspas (REMOVER COMENTÁRIO)
```

---

## Passo 3: Verificar os resultados

Para operações bem-sucedidas, você deverá ver códigos de status `200 OK` ou `201 Created`.

Para operações inválidas (como criar uma reserva de assento ocupado), você verá mensagens de erro personalizadas, como:

```json
{
  "error": {
    "code": "400",
    "message": "O assento solicitado já está ocupado."
  }
}
```

---

## Dicas Adicionais

Se houver problemas com dados ou tabelas, você pode sempre executar novamente o deploy:

```bash
cds deploy --to sqlite
```

---

**Finalizamos o Exercício 5!** Agora, você está pronto para expandir os serviços ou realizar testes adicionais conforme necessário.
