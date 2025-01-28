# **Use o SAP Cloud Application Programming Model com Extensões de IDE**  

## **Visão Geral**  
O **SAP CAP (Cloud Application Programming)** é um modelo de desenvolvimento projetado para criar **aplicações empresariais escaláveis e preparadas para a nuvem**. Ele se integra perfeitamente ao **SAP BTP** e pode ser executado em **nuvem pública, privada ou on-premise**.  

Nesta parte do exercício prático, você será guiado na construção de um **Sistema de Gestão Aérea** utilizando **SAP CAP**.  

O sistema incluirá entidades como **Passageiros, Voos, Reservas e Aeronaves**, juntamente com validações e regras de negócio para garantir a **consistência dos dados**. Utilizando o **SAP CAP**, você implementará serviços para gerenciar operações essenciais, como **cadastro de passageiros, agendamento de voos e reservas de assentos**.  

## **Cenário de Negócio**  
Nesta parte do exercício prático, implementaremos um **Sistema de Gestão Aérea** que permitirá a **gestão eficiente das operações de voo**. O sistema possibilitará que os usuários **cadastrem passageiros, gerenciem horários de voos e realizem reservas**, garantindo o cumprimento de regras como **validação de CPF, idade mínima e disponibilidade de assentos**.  

Você começará definindo o modelo de dados utilizando **Core Data Services (CDS)** e gerando os artefatos de desenvolvimento necessários. **Serviços REST** serão criados para gerenciar passageiros e reservas, garantindo que as regras de negócio sejam aplicadas. O sistema terá suporte para operações transacionais e rascunhos (**draft-enabled**), permitindo **consistência de dados e extensibilidade**.  

Este exercício prático demonstra a **flexibilidade e escalabilidade** do **SAP CAP** na construção de **aplicações empresariais robustas**.  

## **Exercícios**  

| Exercício | Descrição |
|-----------|------------|
| **CAP Exercício 0** | Configuração do ambiente de desenvolvimento CAP |
| **CAP Exercício 1** | Definir o modelo de dados (`schema.cds`) para o Sistema de Gestão Aérea |
| **CAP Exercício 2** | Carregar dados iniciais usando arquivos `.csv` para companhias, aeronaves, aeroportos e conexões |
| **CAP Exercício 3** | Implementar serviços REST para gerenciar passageiros, reservas e voos |
| **CAP Exercício 4** | Aplicar regras de negócio e validações (CPF, idade mínima, disponibilidade de assentos, preços) |
| **CAP Exercício 5** | Testar os serviços usando Postman ou arquivos de teste HTTP |  
