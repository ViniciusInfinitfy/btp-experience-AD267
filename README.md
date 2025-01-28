# BTP-Experience2025-AD267
Extending with ABAP Cloud and SAP Cloud Application Programming Model

## Description
This repository contains the material for the SAP BTP Experience 2025 session called AD267 - Extending with ABAP Cloud and SAP Cloud Application Programming Model.

## Overview

### Extending SAP systems
SAP emphasizes maintaining a clean core by utilizing the various extensibility options illustrated in the diagram below. In this hands-on exercise, we will leverage developer extensibility and side-by-side extensibility, demonstrating how seamless communication between these two approaches can be.
![image](https://github.com/user-attachments/assets/3d86fa51-dd78-4547-9b7d-db7f7d1d88f0)

### Architecture of a CAP application
A CAP-based project, structured as a Multi-Target Application (MTA), can be divided into three layers: database, service, and presentation (if required). In this hands-on exercise, we will explore how these layers interact and demonstrate how easily they can communicate within a scalable and modular architecture.

![image](https://github.com/user-attachments/assets/fcebdd07-8048-4343-bf6f-b8ea8f1991c3)

SAP CAP (Cloud Application Programming) follows a convention-over-configuration approach, enabling developers to build scalable and enterprise-grade applications efficiently. In this hands-on exercise, we will explore how CAP simplifies data modeling, service definition, and extensibility, demonstrating its seamless integration with SAP BTP and other enterprise solutions.

![image](https://github.com/user-attachments/assets/a1ec1477-3d9b-4417-8f23-8abdb180a3b7)

### Exercise Overview
This hands-on exercise demonstrates how to build an Airline Management System using SAP CAP. It covers data modeling, CSV data loading, and REST services for managing passengers, reservations, flights, and companies. Key business rules like CPF validation, minimum age, seat availability, and pricing constraints ensure data integrity. Finally, the system is tested to validate functionality, showcasing CAP’s scalability and enterprise integration.

## Requirements 

To carry out this hands-on exercise, you need to:  

- **Set up a CAP development environment**, including **Node.js (LTS version)** and **SAP Business Application Studio** or **VS Code** with the **SAP CDS Extension**.  
- **Install CAP CLI (`@sap/cds`)** using `npm install -g @sap/cds`.  
- **Have a database ready**, such as **SQLite** for local development or **SAP HANA** for cloud deployment.  
- **Use a REST client** like **Postman** or **HTTP test files** to validate the implemented services.  
- **Prepare CSV files** to load initial data into the system.  
- **Ensure access to SAP BTP**, if deploying in a cloud environment.  

Go to **Getting Started - Preparation** to check the installation details and environment setup before proceeding with the first exercise.

### Business Scenario

In this hands-on workshop, we will implement an **Airline Management System** using **SAP CAP** to manage operations like passenger registration, flight scheduling, and reservations. The application will include entities such as **Passenger, Aircraft, Reservation, and Flight Schedule**, adhering to business rules like CPF validation, minimum age, and seat availability.  

You will define the data model, load initial data via **CSV files**, and create **REST services** for CRUD operations and queries. These services will enforce validations to ensure data accuracy and business compliance. Finally, the system will be tested using tools like **Postman**, showcasing CAP’s ability to build scalable, enterprise-grade applications.  
