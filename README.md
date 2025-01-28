# **BTP-Experience2025-CAP**  
Construindo um Sistema de Gestão Aérea com SAP Cloud Application Programming Model  

## **Descrição**  
Este repositório contém os materiais para a sessão **SAP BTP Experience 2025**, focada na construção de um **Sistema de Gestão Aérea** utilizando o **SAP Cloud Application Programming Model (CAP)**.  

## **Visão Geral**  

### **Expandindo Sistemas SAP**  
A SAP enfatiza a importância de manter um **clean core** utilizando diversas opções de extensibilidade. Neste exercício prático, focaremos na **extensibilidade para desenvolvedores** e na **extensibilidade side-by-side**, demonstrando como essas abordagens se integram de forma eficiente.  

![image](https://github.com/user-attachments/assets/3d86fa51-dd78-4547-9b7d-db7f7d1d88f0)  

### **Arquitetura de uma Aplicação CAP**  
Um **projeto baseado em CAP**, estruturado como uma **Multi-Target Application (MTA)**, é dividido em três camadas:  
- **Banco de Dados** – Define entidades e modelos de dados.  
- **Serviço** – Implementa a lógica de negócios e expõe APIs.  
- **Apresentação** – (Opcional) Fornece uma camada de UI para os usuários finais.  

Este exercício prático explora como essas camadas interagem dentro de uma **arquitetura escalável e modular**.  

![image](https://github.com/user-attachments/assets/fcebdd07-8048-4343-bf6f-b8ea8f1991c3)  

O SAP CAP segue uma abordagem **convention-over-configuration**, permitindo que os desenvolvedores criem **aplicações empresariais escaláveis** de forma eficiente. Este exercício cobre **modelagem de dados, definição de serviços e extensibilidade**, demonstrando a **integração fluida com o SAP BTP** e outros sistemas empresariais.  

![image](https://github.com/user-attachments/assets/a1ec1477-3d9b-4417-8f23-8abdb180a3b7)  

### **Visão Geral do Exercício**  
Este exercício prático orienta a criação de um **Sistema de Gestão Aérea** usando **SAP CAP**. Você irá:  
- **Definir o modelo de dados** (`schema.cds`) com entidades como **Passageiros, Voos, Reservas e Aeronaves**.  
- **Carregar dados iniciais** utilizando arquivos `.csv`.  
- **Implementar serviços REST** para gerenciar passageiros, reservas, voos e companhias.  
- **Aplicar regras de negócio** como **validação de CPF, idade mínima, disponibilidade de assentos e restrições de preços**.  
- **Testar o sistema** utilizando **arquivos de teste HTTP**, garantindo **integridade dos dados e escalabilidade do sistema**.  

---

## **Requisitos**  

Para completar este exercício prático, você precisa:  

- **Configurar um ambiente de desenvolvimento CAP**, incluindo **Node.js (versão LTS)** e **SAP Business Application Studio** ou **VS Code** com a **extensão SAP CDS**.  
- **Instalar o CAP CLI (`@sap/cds`)** via `npm install -g @sap/cds`.  
- **Ter um banco de dados pronto**, como **SQLite** para desenvolvimento local ou **SAP HANA** para implantação na nuvem.  
- **Usar um cliente REST**, **arquivos de teste HTTP**, para validar os serviços implementados.  
- **Preparar arquivos CSV** para carregar dados iniciais no sistema.  
- **Garantir acesso ao SAP BTP**, caso o sistema seja implantado em um ambiente na nuvem.  

Acesse **Getting Started - Preparation** para verificar os detalhes da instalação e garantir que o ambiente está pronto antes de iniciar o primeiro exercício.  

### **Cenário de Negócio**  

Neste exercício prático, implementaremos um **Sistema de Gestão Aérea** usando **SAP CAP**, permitindo a gestão eficiente de **cadastro de passageiros, agendamento de voos e reservas**.  

A aplicação inclui **entidades principais** como **Passageiro, Aeronave, Reserva e Horário de Voo**, garantindo conformidade com regras de negócio como **validação de CPF, idade mínima e disponibilidade de assentos**.  

Você irá:  
- **Definir o modelo de dados** e configurar associações.  
- **Carregar dados iniciais** usando **arquivos CSV**.  
- **Desenvolver serviços REST** para operações CRUD e consultas de voos.  
- **Implementar lógica de validação** para garantir conformidade com as regras de negócio.  
- **Testar o sistema** usando **Postman**, demonstrando como o SAP CAP permite a construção de **aplicações empresariais escaláveis e flexíveis**.  

Esta experiência prática destaca como o **SAP CAP possibilita o desenvolvimento eficiente e flexível de soluções empresariais**, mantendo uma **arquitetura clean core**.

## Exercícios
### [**SAP CAP** - Preparação](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex0)

- [**CAP Exercício 0:** Configuração do ambiente de desenvolvimento CAP](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex99)
- [**CAP Exercício 1:** Definir o modelo de dados (`schema.cds`) para o Sistema de Gestão Aérea](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex1)
- [**CAP Exercício 2:** Carregar dados iniciais usando arquivos `.csv` para companhias, aeronaves, aeroportos e conexões](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex2)
- [**CAP Exercício 3:** Implementar serviços REST para gerenciar passageiros, reservas e voos](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex3)
- [**CAP Exercício 4:** Aplicar regras de negócio e validações (CPF, idade mínima, disponibilidade de assentos, preços)](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex4)
- [**CAP Exercício 5:** Testar os serviços usando Postman ou arquivos de teste HTTP](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex5)
