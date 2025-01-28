# Use SAP Cloud Application Programming Model with IDE Extensions

## Overview  
**SAP CAP (Cloud Application Programming)** is a development model designed to build scalable, cloud-ready business applications, services, and extensions. It integrates seamlessly with **SAP BTP** and can run on public or private cloud, as well as on-premise environments.  

This part of the hands-on workshop will guide you through building an **Airline Management System** using SAP CAP.  

The system will include entities such as **Passengers, Flights, Reservations, and Aircraft**, along with validations and business rules to ensure data consistency. Using SAP CAP, you will implement services to handle core operations like passenger registration, flight scheduling, and seat reservations.  

## Business Scenario  
In this part of the hands-on workshop, the scenario we will implement is an **Airline Management System** that enables efficient management of flight operations. The system will allow users to register passengers, manage flight schedules, and handle reservations, ensuring compliance with rules such as CPF validation, minimum age, and seat availability.  

You will begin by defining the data model using **Core Data Services (CDS)** and generating the required development artifacts. REST services will be created to manage passengers and reservations while ensuring business rules are enforced. The system will be draft-enabled and support transactional operations, allowing data consistency and extensibility.  

This hands-on exercise showcases the flexibility and scalability of SAP CAP for building enterprise-grade applications.  

## Exercises

| Exercise | Description |
|----------|------------|
| **CAP Exercise 0** | Setting up the CAP development environment |
| **CAP Exercise 1** | Define the data model (`schema.cds`) for the Airline Management System |
| **CAP Exercise 2** | Load initial data using `.csv` files for companies, aircraft, airports, and connections |
| **CAP Exercise 3** | Implement REST services for managing passengers, reservations, and flights |
| **CAP Exercise 4** | Apply business rules and validations (CPF, minimum age, seat availability, pricing) |
| **CAP Exercise 5** | Test the services using Postman or HTTP test files |
