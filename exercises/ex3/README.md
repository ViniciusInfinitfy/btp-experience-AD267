# CAP Exercício 3: Implementar serviços para gerenciar as entidades

## **Objetivo**  
Nesta parte do exercício, você irá criar os serviços necessários para o **Sistema de Gestão Aérea**. Será definido um arquivo de serviço inicialmente: **`airline.cds`**.

O para o sistema de aviação será capaz de expor entidades como **Aeronave**, **Companhia**, **Aeroporto**, entre outras.

---

## **Passo 1: Criar os Arquivos de Serviço**
1. Na pasta **`/srv`**, crie os arquivos:

   ![image](https://github.com/user-attachments/assets/eecb1982-aa3f-4886-83cf-f54ecb8e8fca)

---

## **Passo 2: Definir o Serviço `AirlineService` no Arquivo `airline.cds`**
Abra o arquivo **`airline.cds`** e adicione as seguintes definições:

```cds
using btpexp.airlines as nm from '../db/airline';

service AirlineService @(path: '/airline') {
    @readonly
    entity Aeronave            as projection on nm.Aeronave;

    @readonly
    entity Companhia           as projection on nm.Companhia
                                  order by
                                      razao_social;

    @readonly
    entity Aeroporto           as projection on nm.Aeroporto
                                  order by
                                      nome;

    @readonly
    entity Conexao             as projection on nm.Conexao;

    @readonly
    entity PropriedadeAeronave as projection on nm.PropriedadeAeronave
                                  order by
                                      proprietario;

    @cds.query.limit: {
        default: 50,
        max    : 100
    }
    entity HorarioVoo          as projection on nm.HorarioVoo;
    annotate nm.HorarioVoo with { modifiedAt @odata.etag }

    entity Passageiro          as projection on nm.Passageiro;
    annotate nm.Passageiro with { modifiedAt @odata.etag }

    entity ReservaPassagem     as projection on nm.ReservaPassagem;
    annotate nm.ReservaPassagem with { modifiedAt @odata.etag }
}
```

O arquivo **`airline.cds`** define o **serviço REST** chamado **`AirlineService`**, que expõe várias entidades relacionadas ao sistema de gestão aérea.

---

### **Definição do serviço**

![image](https://github.com/user-attachments/assets/0ee81e8b-e5e5-44f3-8fa8-d59f34c542f6)

- **`using btpexp.airlines as nm`**: Importa as definições das entidades do arquivo **`airline.cds`** na pasta **`/db`** e atribui o alias **`nm`** para facilitar o acesso às entidades.
- **`service AirlineService`**: Define o serviço chamado **`AirlineService`**, que estará acessível na URL base **`/airline`**.

---

### **Projeções das entidades**
As entidades são expostas como **projeções** para limitar quais dados serão expostos pelo serviço.

![image](https://github.com/user-attachments/assets/5ab66fbe-8716-4c5f-991b-c41e96d3ab4a)

- **`@readonly`**: Indica que esta entidade só pode ser lida (operações **GET**), sem permitir **criação, atualização ou exclusão**.
- **`projection on nm.Aeronave`**: Cria uma projeção baseada na entidade **`Aeronave`**.

Esse padrão é repetido para todas as entidades:

![image](https://github.com/user-attachments/assets/a6ed51e5-8d40-4c0e-b6f8-218b986e1535)

- A entidade **`Companhia`** é projetada, e os resultados serão ordenados pelo campo **`razao_social`**.

---

### **Limitação de resultados retornados**

![image](https://github.com/user-attachments/assets/24c460ac-9229-4688-863e-a89cc69cef90)

- **`@cds.query.limit`**: Limita o número de registros retornados por padrão.
  - **`default: 50`**: Retorna até 50 registros por padrão.
  - **`max: 100`**: Limita o número máximo de registros retornados a 100.

---

### **Controle de concorrência**

![image](https://github.com/user-attachments/assets/525a5fef-3715-4c66-8f3f-82c254d03e5f)

- **`@odata.etag`**: Permite o controle de concorrência otimista. Isso é útil para evitar conflitos quando múltiplos usuários tentam modificar os mesmos dados simultaneamente.

---

## Passo 3: Teste Inicial 

Agora, para verificar se esse serviço está devidamente configurado e bem localizado, vá no terminal e execute o comando:

```sh
cds watch
```

![image](https://github.com/user-attachments/assets/6bf2393a-a588-440e-958a-3911f2d1dfdc)

---

Verifique se carregou to servidor normalmente. Após isso, acesse o link do localhost e visualize os serviços criados na tela inicial.

![image](https://github.com/user-attachments/assets/10a2bb67-8905-4d40-813c-fa33eef2ce49)

---

O próximo passo pode envolver a criação de **handlers** (em arquivos JS) para adicionar validações personalizadas. Suiga para o próximo exercício: [CAP Exercício 4: Aplicar regras de negócio e validações](https://github.com/ViniciusInfinitfy/btp-experience-AD267/tree/main/exercises/ex4)
