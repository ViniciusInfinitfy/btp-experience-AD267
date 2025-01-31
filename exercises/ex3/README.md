# CAP Exercício 3: Implementar serviços para gerenciar as entidades

## **Objetivo**  
Nesta parte do exercício, você irá criar os serviços necessários para o **Sistema de Gestão Aérea** e o **serviço de consulta de CEP**. Serão definidos dois arquivos de serviço inicialmente: **`airline.cds`** e **`cep.cds`**.

O para o sistema de aviação será capaz de expor entidades como **Aeronave**, **Companhia**, **Aeroporto**, entre outras. O serviço de CEP permitirá a consulta de endereços.

---

## **Passo 1: Criar os Arquivos de Serviço**
1. Na pasta **`/srv`**, crie os arquivos:

   ![image](https://github.com/user-attachments/assets/03c01bc4-c0a4-4058-8d50-9c4dcfc701d3)

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

## **Passo 3: Definir o Serviço `CepService` no Arquivo `cep.cds`**
Abra o arquivo **`cep.cds`** e adicione a seguinte definição:

```cds
using processo_seletivo.esquema as nm from '../db/cep';

service CepService @(path: '/viacep') {
    @readonly
    entity Cep as projection on nm.Cep;
}
```

--- 

O arquivo **`cep.cds`** define o **serviço REST** chamado **`CepService`**, que expõe uma entidade para consulta de dados de endereços baseados em CEPs.

---

### **Definição do serviço**

![image](https://github.com/user-attachments/assets/499249c8-7321-4522-9fb4-477534805294)

- **`using btpexp.esquema as nm`**: Importa a definição da entidade **`Cep`** do arquivo **`cep.cds`** localizado na pasta **`/db`** e atribui o alias **`nm`**.
- **`service CepService`**: Define o serviço chamado **`CepService`**, que estará acessível na URL base **`/viacep`**.

---

### **Projeção da entidade `Cep`**

![image](https://github.com/user-attachments/assets/069a3f4c-9a32-438a-b4b7-5851981c2fa2)

- **`@readonly`**: Indica que a entidade **`Cep`** só pode ser lida (operações **GET**), sem permitir **criação, atualização ou exclusão** de registros.
- **`projection on nm.Cep`**: Cria uma projeção baseada na entidade **`Cep`**, limitando os dados que podem ser expostos pelo serviço REST.

---

## Passo 3: Teste Inicial 

Agora, para verificar se esses serviços estão devidamente configurados e bem localizados, vá no terminal e execute o comando:

```sh
cds watch
```

![image](https://github.com/user-attachments/assets/a9240b6f-b8a2-47a8-8718-ea55a8bd142e)

---

Verifique se carregou to servidor normalmente. Após isso, acesse o link do localhost e visualize os serviços criados na tela inicial.

![image](https://github.com/user-attachments/assets/0db534b7-14c4-4bac-bcb1-957058d5f480)

---

O próximo passo pode envolver a criação de **handlers** (em arquivos JS) para adicionar validações personalizadas. Suiga para o próximo exercício: [CAP Exercício 4: Aplicar regras de negócio e validações](https://github.com/ViniciusInfinitfy/btp-experience-AD267/tree/main/exercises/ex4)
