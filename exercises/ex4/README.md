# CAP Exercício 4: Aplicar regras de negócio e validações

## **Objetivo**  
Nesta parte do exercício, você irá adicionar funcionalidades de validação e gerenciamento personalizado para as entidades **`Passageiro`** e **`ReservaPassagem`**, usando arquivos **JavaScript** no backend do **SAP CAP**. Para isso, você configurará os arquivos **`passageiro-srv.js`**, **`reserva_passagem-srv.js`** e **`airline.js`**.

---

## **Passo 1: Criar os arquivos de serviço no backend**
Navegue até a pasta **`/srv`** e crie os seguintes arquivos:

![image](https://github.com/user-attachments/assets/8f11a019-a362-4486-98fc-d7fabf1c07da)

---

## **Passo 2: Implementar o arquivo `passageiro-srv.js`**
Abra o arquivo **`passageiro-srv.js`** e adicione o seguinte código:

```javascript
const cds = require('@sap/cds');

class PassageiroService {
    static init(service) {
        service.before('CREATE', 'Passageiro', async (req) => {
            await PassageiroService.validarCadastro(req);
        });

        service.on('error', (err, req) => {
            PassageiroService.errorHandler(err, req);
        });
    }

    static async validarCadastro(req) {
        const { cpf, data_nascimento, email } = req.data;
        
        if (!/^[0-9]{11}$/.test(cpf)) {
            throw new Error("INVALID_CPF");
        }

        const idadeMinima = 3;
        const idade = PassageiroService.calcularIdade(data_nascimento);
        if (idade < idadeMinima) {
            throw new Error(`Idade mínima permitida: ${idadeMinima} anos.`);
        }

        if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
            throw new Error("EMAIL_INVALIDO");
        }

        const duplicado = await cds.tx(req).run(
            SELECT.one.from('processo_seletivo.airlines.Passageiro').where({ cpf })
        );
        if (duplicado) {
            throw new Error("DUPLICATE_PASSENGER");
        }
    }

    static calcularIdade(dataNascimento) {
        const nascimento = new Date(dataNascimento);
        const hoje = new Date();
        let idade = hoje.getFullYear() - nascimento.getFullYear();
        if (hoje < new Date(hoje.getFullYear(), nascimento.getMonth(), nascimento.getDate())) {
            idade--;
        }
        return idade;
    }

    static errorHandler(err, req) {
        switch (err.message) {
            case "INVALID_CPF":
                req.reject(400, "O CPF informado não é válido.");
                break;
            case "EMAIL_INVALIDO":
                req.reject(400, "O email informado não é válido.");
                break;
            case "DUPLICATE_PASSENGER":
                req.reject(400, "Já existe um passageiro com o mesmo CPF.");
                break;
            default:
                req.reject(500, "Erro inesperado no sistema.");
        }
    }
}

module.exports = PassageiroService;
```

O arquivo **`passageiro-srv.js`** define uma classe chamada **`PassageiroService`**, responsável por realizar validações e tratamentos de erros personalizados para a entidade **`Passageiro`** no contexto do **SAP CAP**. Ele intercepta operações de criação de novos registros (antes de serem persistidos no banco de dados) e aplica regras de negócio, como validações de CPF, idade mínima e duplicidade de registros.

---

### **Classe `PassageiroService`**

![image](https://github.com/user-attachments/assets/6666228b-faf1-47da-9f09-2ea285fe2800)

A classe encapsula todas as funções de validação e tratamento de erros em métodos estáticos, facilitando a reutilização e integração com o serviço **CAP**.

---

### **Função `init(service)`**

![image](https://github.com/user-attachments/assets/8fcfd35b-71e8-4179-98df-6e35ef0edfa5)

- **`service.before('CREATE', 'Passageiro')`**: Antes de criar um novo passageiro, chama a função **`validarCadastro(req)`** para garantir que as regras de negócio sejam respeitadas.
- **`service.on('error')`**: Intercepta erros durante as operações na entidade **`Passageiro`** e os processa na função **`errorHandler`**.

---

### **Função `validarCadastro(req)`**

![image](https://github.com/user-attachments/assets/623e5380-6764-474a-a91d-e7558315744e)

- **Recebe os dados** do passageiro da solicitação **`req.data`** para validação.

---

#### **Validação de CPF**

![image](https://github.com/user-attachments/assets/88aaca57-ed58-443f-90d5-e9a7e66b065f)

- Verifica se o CPF contém exatamente 11 dígitos numéricos.
- Caso não passe na validação, lança o erro **`INVALID_CPF`**.

---

#### **Validação de Idade Mínima**

![image](https://github.com/user-attachments/assets/3a907ffe-9e6c-43d8-93ee-be53ccdd93b2)

- Calcula a idade do passageiro com base na data de nascimento.
- Verifica se a idade é pelo menos **3 anos**.
- Caso não seja, lança um erro com a mensagem personalizada.

**Função auxiliar:**

![image](https://github.com/user-attachments/assets/8ccd8b8a-43f4-4331-b8b1-8db96b699677)

- Calcula a idade atual do passageiro comparando o ano de nascimento com o ano atual.
- Leva em consideração o mês e o dia para evitar erros.

---

#### **Validação de Email**

![image](https://github.com/user-attachments/assets/5ebf2b74-8ef6-4ee3-9132-7eff92d11734)

- Verifica se o email segue um formato válido usando uma expressão regular.

---

#### **Verificação de Passageiro Duplicado**

![image](https://github.com/user-attachments/assets/1a7f9563-1c2e-4cac-ac5a-28215701b4f5)

- Realiza uma consulta no banco de dados para verificar se já existe um passageiro com o mesmo CPF.
- Se encontrar um registro duplicado, lança o erro **`DUPLICATE_PASSENGER`**.

---

### **Função `errorHandler(err, req)`**

![image](https://github.com/user-attachments/assets/308ee114-72d0-4567-8375-415facac8939)

- **Intercepta e trata os erros** gerados durante as operações.
- Cada erro específico é associado a uma mensagem personalizada:
  - **`INVALID_CPF`**: Retorna uma mensagem indicando que o CPF não é válido.
  - **`EMAIL_INVALIDO`**: Indica que o email informado está incorreto.
  - **`DUPLICATE_PASSENGER`**: Indica que já existe um passageiro com o mesmo CPF.
  - **Outros erros**: São tratados como erros internos (código 500).

---

### **Exportação da Classe**

![image](https://github.com/user-attachments/assets/97030a81-411f-41f9-a8e7-b7b0a5c359e7)

- Exporta a classe **`PassageiroService`** para que possa ser usada no arquivo **`airline.js`** e integrada ao serviço **CAP**.

---

## **Passo 3: Implementar o arquivo `reserva_passagem-srv.js`**
Abra o arquivo **`reserva_passagem-srv.js`** e adicione o seguinte código:

```javascript
const cds = require('@sap/cds');

class ReservaPassagemService {
    static init(service) {
        service.before('CREATE', 'ReservaPassagem', async (req) => {
            await ReservaPassagemService.validarReserva(req);
        });

        service.on('error', (err, req) => {
            ReservaPassagemService.errorHandler(err, req);
        });
    }

    static async validarReserva(req) {
        const { id_horario_voo, assento, classe } = req.data;

        // Verificar se o assento está disponível
        const assentoExistente = await cds.tx(req).run(
            SELECT.one.from('processo_seletivo.airlines.ReservaPassagem')
                .where({ id_horario_voo, assento })
        );
        if (assentoExistente) {
            throw new Error("ASSENTO_OCUPADO");
        }

        // Verificar se a classe é válida
        const classesPermitidas = ['Econômica', 'Executiva'];
        if (!classesPermitidas.includes(classe)) {
            throw new Error("CLASSE_INVALIDA");
        }
    }

    static errorHandler(err, req) {
        switch (err.message) {
            case "ASSENTO_OCUPADO":
                req.reject(400, "O assento solicitado já está ocupado.");
                break;
            case "CLASSE_INVALIDA":
                req.reject(400, "A classe informada não é válida.");
                break;
            default:
                req.reject(500, "Erro inesperado no sistema.");
        }
    }
}

module.exports = ReservaPassagemService;
```

O arquivo **`reserva_passagem-srv.js`** define a lógica personalizada para validar e gerenciar reservas de passagens no contexto do **SAP CAP**. Ele intercepta operações de criação de novas reservas, verifica a disponibilidade de assentos e a validade das classes escolhidas pelos passageiros. O tratamento de erros personalizado garante mensagens claras para os usuários quando as validações falham.

---

### **Classe `ReservaPassagemService`**

![image](https://github.com/user-attachments/assets/dab620a3-94f1-4680-a4bf-b45977dc6ed7)

A classe encapsula as validações e os métodos de gerenciamento para a entidade **`ReservaPassagem`**, utilizando métodos estáticos para facilitar a integração com o **CAP**.

---

### **Função `init(service)`**

![image](https://github.com/user-attachments/assets/2988ab18-5d4b-4e88-8732-dbcbc426ee3d)

- **`service.before('CREATE', 'ReservaPassagem')`**: Antes de criar uma nova reserva, chama a função **`validarReserva(req)`** para aplicar as regras de negócio.
- **`service.on('error')`**: Intercepta erros e os redireciona para o método **`errorHandler`**.

---

### **Função `validarReserva(req)`**

![image](https://github.com/user-attachments/assets/99ad2612-745b-4728-a786-85186c6c50d4)

- **Extrai os dados da solicitação (`req.data`)**, como o identificador do voo, o assento solicitado e a classe escolhida.

---

#### **Verificação de Assento Disponível**

![image](https://github.com/user-attachments/assets/f562eb4e-d881-4084-85c2-b368e423341b)

- **Consulta no banco de dados** se o assento solicitado já está ocupado para o voo especificado.
- Se o assento já estiver reservado, lança o erro **`ASSENTO_OCUPADO`**.

---

#### **Verificação da Classe Informada**

![image](https://github.com/user-attachments/assets/fd13d80c-0fc4-4283-8c6c-7a418dacb44d)

- Verifica se a classe escolhida está entre as opções permitidas: **`Econômica`** ou **`Executiva`**.
- Se a classe não for válida, lança o erro **`CLASSE_INVALIDA`**.

---

### **Função `errorHandler(err, req)`**

![image](https://github.com/user-attachments/assets/e94b869e-aecf-447d-8cf8-d792321980ae)

- **Intercepta e trata os erros** gerados durante a operação de criação de reserva.
- Cada erro específico retorna uma mensagem personalizada:
  - **`ASSENTO_OCUPADO`**: Indica que o assento já está reservado.
  - **`CLASSE_INVALIDA`**: Indica que a classe escolhida não é suportada.
  - **Outros erros**: São tratados como erros internos (código 500).

---

### **Exportação da Classe**

![image](https://github.com/user-attachments/assets/2b639aad-9c8b-406e-a009-c2d5828d4d16)

- Exporta a classe **`ReservaPassagemService`** para ser utilizada no arquivo **`airline.js`**, onde será integrada ao **SAP CAP**.

---

## **Passo 4: Integrar os serviços no arquivo `airline.js`**
Abra o arquivo **`airline.js`** e adicione o seguinte código:

```javascript
const cds = require('@sap/cds');
const PassageiroService = require('./passageiro-srv');
const ReservaPassagemService = require('./reserva_passagem-srv');

module.exports = cds.service.impl(function () {
    PassageiroService.init(this);
    ReservaPassagemService.init(this);
});
```

### **O que está acontecendo aqui:**
- Importamos os serviços personalizados de **Passageiro** e **ReservaPassagem**.
- No método **`cds.service.impl()`**, integramos esses serviços ao contexto do **SAP CAP**, permitindo que as validações e regras de negócio sejam aplicadas automaticamente durante as operações.

---

Nesta parte do exercício, você implementou funcionalidades de validação e gerenciamento personalizado para as entidades **`Passageiro`** e **`ReservaPassagem`**, usando arquivos **JavaScript** no backend do **SAP CAP**. Agora, vamos **testar um simples GET** nessas entidades utilizando arquivos **HTTP** diretamente no **VS Code**.

---

## **Testando as entidades com GET usando arquivos HTTP**

### **Passo 1: Criar uma pasta para os arquivos de teste**
Para organizar os testes, crie uma pasta chamada **`tests`** na raiz do projeto:

![image](https://github.com/user-attachments/assets/0990423d-b70a-4a5e-a0d2-d72428da998c)

---

### **Passo 2: Configurar os arquivos de teste HTTP**

#### **Arquivo `passageiro-get.http`**
Este arquivo fará uma requisição **GET** para listar todos os passageiros cadastrados.

```http
### Consultar todos os passageiros
GET http://localhost:4004/airline/Passageiro
```

#### **Arquivo `reserva_get.http`**
Este arquivo fará uma requisição **GET** para listar todas as reservas de passagens.

```http
### Consultar todas as reservas de passagem
GET http://localhost:4004/airline/ReservaPassagem
```

---

### **Passo 3: Executar os testes no VS Code**
1. Certifique-se de que a **extensão REST Client** está instalada.
2. Em um dos arquivos HTTP (por exemplo, **`passageiro-get.http`**) e clique em **`Send Request`** no topo da solicitação para enviar o teste.

![image](https://github.com/user-attachments/assets/db0b1286-912e-4e79-9313-8cc4900eb6af)

---

Se ocorrerem erros, verifique se os serviços foram iniciados corretamente com o comando:

```sh
cds watch
```

Provavelmente a tabela de Passageiros estará vazia, então você pode criar outros arquivos de teste com outros endpoints para fazer essa busca de dados usando o GET.

---

⚠️ **Importante:**  
Estes testes de **GET** são básicos e usados apenas para verificar se os dados estão sendo retornados corretamente. Na [**Aula 5**](https://github.com/ViniciusInfinitfy/btp-experience-AD267/tree/main/exercises/ex5), você aprenderá a testar operações **POST, PUT e DELETE**, aplicando validações mais complexas.
