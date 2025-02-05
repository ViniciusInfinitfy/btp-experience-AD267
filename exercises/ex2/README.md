# **CAP Exercício 2: Carregar Dados Iniciais usando Arquivos `.csv`**

## **Objetivo**  
Nesta aula, você irá carregar os dados iniciais para as entidades do **Sistema de Gestão Aérea** utilizando **arquivos CSV**. Os dados serão inseridos automaticamente no banco de dados **SQLite** durante o processo de deploy.

---

## **Passo 1: Baixar os Arquivos CSV do Repositório**
Os arquivos CSV contendo os dados iniciais já estão disponíveis no repositório. Você pode baixá-los [clicando aqui](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/csv%20records) e adicionando-os à pasta **`db/data`** no seu projeto.

![image](https://github.com/user-attachments/assets/1021e09f-633b-4c1f-ac56-2c2b34a13cbd)

Caso você deseja adicionar automaticamente esses arquivos `.csv`, basta você executar o seguinte comando no terminal: `cds add data`. Assim, ele irá criar a pasta data/ e criar os arquivos com os nomes padronizados com as entidades feitas. Entretando eles não estarão com nenhum dado.

---

## **Passo 2: Executar o Deploy e Carregar os Dados**
Depois de adicionar os arquivos CSV na pasta correta, execute o comando de deploy:

```sh
cds deploy --to sqlite
```

📌 **O que esse comando faz:**  
- Cria as tabelas no banco de dados com base no modelo **`airline.cds`**.
- Carrega automaticamente os dados dos arquivos CSV.

---

## **Passo 3: Verificar os Dados Carregados**
Após o deploy, inicie o servidor CAP:

```sh
cds watch
```

Você irá perceber que ele carregou todos os arquivos csv's, porém, eles estão sendo todos puxados da memória da aplicação. Ou seja, se nós realizassemos uma alteração em um dos dados, e reiniciassemos o servidor, seria como se não tivessse acontecido nada.

![image](https://github.com/user-attachments/assets/ed86dc64-7017-452d-983d-a36f61c00476)

Por isso, precisamos realizar uma configuração no arquivo `package.json`, que é o arquivo de configuração do nosso projeto.

Digite esse código dentro do arquivo `package.json`, para configurar essa captura e armazenamento de dados em nosso projeto:

```sh
"cds": {
    "requires": {
      "db": {
        "kind": "sqlite",
        "credentials": {
          "database": "db.sqlite"
        }
      }
    }
  }
```

Pronto, dessa forma, quando nós iniciarmos o projeto, os dados serão armazenados e alterados diretamente do banco de dados sqlite.

![image](https://github.com/user-attachments/assets/40895da0-304e-4b8c-9942-603a1f28b93f)

![image](https://github.com/user-attachments/assets/ed0bb54e-bb70-4775-810d-eb59393ce991)

---

## **Conclusão**
Agora que os dados foram carregados, você pode seguir para o [**CAP Exercício 3**](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex3) para implementar os serviços REST e explorar as operações no sistema. Se encontrar problemas, verifique os passos acima!
