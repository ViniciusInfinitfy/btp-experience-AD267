# CAP Exercício 0: Configuração do ambiente de desenvolvimento CAP 

## **Objetivo**  
Este exercício orienta a configuração do ambiente necessário para o desenvolvimento do **Sistema de Gestão Aérea** utilizando **SAP CAP (Cloud Application Programming Model)**.  

Ao final desta etapa, você terá seu ambiente pronto para modelar dados, criar serviços REST e testar a aplicação.  

## **Pré-requisitos**  
Antes de começar, certifique-se de que você tem:  
- **Node.js (versão LTS)** instalado.  
- **Editor de código**, como **VS Code**.  
- **CAP CLI (`@sap/cds`)** instalado via `npm install -g @sap/cds`.  
- **Banco de dados SQLite** configurado para desenvolvimento local.   

Se algum desses itens não estiver instalado, acesse [Preparação](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex0).  

## **Passo 1: Criar um Novo Projeto CAP**  
Agora, crie um projeto básico para garantir que o ambiente esteja funcionando corretamente:  

No terminal, digite:
```sh
cds init airline-management
cd airline-management
```

Este comando criará a estrutura inicial do projeto CAP.  

Para testar, execute:  
```sh
cds watch
```
Se tudo estiver certo, você verá a mensagem:  
```
[cds] - server listening on http://localhost:4004
```
Isso significa que seu ambiente está pronto! 

---

## **Conclusão**  
Agora que seu ambiente está configurado, você pode seguir para o [**CAP Exercício 1: Definir o modelo de dados (schema.cds)**](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex1), onde definiremos o modelo de dados para o **Sistema de Gestão Aérea**.  

Se tiver algum problema, revise os passos acima ou peça ajuda durante a sessão.
