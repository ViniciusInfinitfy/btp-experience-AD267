# **CAP Exercício 2: Carregar Dados Iniciais usando Arquivos `.csv`**

## **Objetivo**  
Nesta aula, você irá carregar os dados iniciais para as entidades do **Sistema de Gestão Aérea** utilizando **arquivos CSV**. Os dados serão inseridos automaticamente no banco de dados **SQLite** durante o processo de deploy.

---

## **Passo 1: Baixar os Arquivos CSV do Repositório**
Os arquivos CSV contendo os dados iniciais já estão disponíveis no repositório. Você pode baixá-los [clicando aqui](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/csv%20records) e adicionando-os à pasta **`db/data`** no seu projeto.

![image](https://github.com/user-attachments/assets/f852dba8-62a6-4f79-9513-60c0e4ade21f)

---

## **Passo 2: Executar o Deploy e Carregar os Dados**
Depois de adicionar os arquivos CSV na pasta correta, execute o comando de deploy:

```sh
cds deploy --to sqlite
```

📌 **O que esse comando faz:**  
- Cria as tabelas no banco de dados com base no modelo **`airline.cds`** e **`cep.cds`**.
- Carrega automaticamente os dados dos arquivos CSV.

---

## **Passo 3: Verificar os Dados Carregados**
Após o deploy, inicie o servidor CAP:

```sh
cds watch
```

Acesse o navegador:
```
http://localhost:4004
```
E verifique as entidades como `Companhia`, `Passageiro` e `Aeronave` para garantir que os dados foram carregados.

---

## **Conclusão**
Agora que os dados foram carregados, você pode seguir para o [**CAP Exercício 3**](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex3) para implementar os serviços REST e explorar as operações no sistema. Se encontrar problemas, verifique os passos acima!
