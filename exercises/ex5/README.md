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

Crie uma pasta chamada `tests` na raiz do projeto, onde armazenaremos os arquivos de teste.

(imagem da pasta sendo criada com os arquivos)

### Estrutura esperada:

```bash
/tests
  ├── passageiro.http
  ├── reserva_passagem.http
  ├── horario_voo.http
```

---

## Passo 2: Criar os arquivos de teste HTTP

### Arquivo `passageiro.http`

#### Criar um novo passageiro

```http
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
GET http://localhost:4004/airline/Passageiro
```

#### Atualizar o nome do passageiro

```http
PATCH http://localhost:4004/airline/Passageiro('550e8400-e29b-41d4-a716-446655440000')
Content-Type: application/json

{
  "nome": "João Silva Atualizado"
}
```

#### Deletar um passageiro

```http
DELETE http://localhost:4004/airline/Passageiro('550e8400-e29b-41d4-a716-446655440000')
```

---

### Arquivo `reserva_passagem.http`

#### Criar uma reserva válida

```http
POST http://localhost:4004/airline/ReservaPassagem
Content-Type: application/json

{
  "id_horario_voo": "550e8400-e29b-41d4-a716-446655440001",
  "id_passageiro": "550e8400-e29b-41d4-a716-446655440000",
  "assento": "12A",
  "classe": "Econômica",
  "status": "Confirmada",
  "data_reserva": "2025-01-01",
  "preco": 1200.50
}
```

#### Consultar todas as reservas

```http
GET http://localhost:4004/airline/ReservaPassagem
```

#### Atualizar o status da reserva

```http
PATCH http://localhost:4004/airline/ReservaPassagem('550e8400-e29b-41d4-a716-446655440003')
Content-Type: application/json

{
  "status": "Cancelada"
}
```

#### Deletar uma reserva de passagem

```http
DELETE http://localhost:4004/airline/ReservaPassagem('550e8400-e29b-41d4-a716-446655440003')
```

---

### Arquivo `horario_voo.http`

#### Criar um horário de voo

```http
POST http://localhost:4004/airline/HorarioVoo
Content-Type: application/json

{
  "id_companhia": "550e8400-e29b-41d4-a716-446655440004",
  "id_conexao": "550e8400-e29b-41d4-a716-446655440005",
  "id_aeronave": "550e8400-e29b-41d4-a716-446655440006",
  "nr_voo": "AB123",
  "partida_prevista": "10:00:00",
  "chegada_prevista": "13:00:00",
  "data": "2025-01-15",
  "nr_assentos_executivo": 10,
  "nr_assentos_economico": 100,
  "nr_assentos_max": 110
}
```

#### Consultar horários de voo

```http
GET http://localhost:4004/airline/HorarioVoo
```

#### Atualizar horário de partida

```http
PATCH http://localhost:4004/airline/HorarioVoo('550e8400-e29b-41d4-a716-446655440007')
Content-Type: application/json

{
  "partida_prevista": "11:00:00"
}
```

#### Deletar um horário de voo

```http
DELETE http://localhost:4004/airline/HorarioVoo('550e8400-e29b-41d4-a716-446655440007')
```

---

## Passo 3: Executar os testes no VS Code

1. Abra o VS Code e vá até a pasta `tests`.

2. Abra um dos arquivos HTTP, como `passageiro.http`.

3. Clique em **Send Request** para executar o teste. O resultado da requisição será exibido em um painel no VS Code.

---

## Passo 4: Verificar os resultados

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
