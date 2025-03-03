
# Money Exchange System (MEXS) - System Design Documentation

## **Project Overview**

The Money Exchange System (MEXS) is a financial transaction system designed for international airports, enabling secure and compliant currency exchanges. This project focuses solely on system design, covering architectural decisions, business logic, and UML diagrams.This system design project was developed as part of the Information Systems Analysis and Design course (INFO 620). It is a group effort by a team of 5, focusing on a comprehensive Money Exchange System (MEXS) for international currency exchanges.

## **System Objectives**

- **Secure Authentication & Role-Based Access Control (RBAC):** Ensure only authorized users can access system functionalities.
- **Real-time Currency Exchange Management:** Dynamic exchange rate retrieval and commission calculation.
- **Regulatory Compliance:** Integrate **OFAC checks** and enforce **KYC** (Know Your Customer) requirements.
- **Transaction Tracking & Reporting:** Log exchange transactions and generate **daily/monthly reports**.

---

## **System Architecture**

The architecture is designed for **scalability, security, and compliance**, consisting of:

- **Authentication Layer:** Secure user login with **RBAC** (Admin, Manager, Exchange Agent).
- **Business Logic Layer:** Implements **exchange rate retrieval, commission calculations, and OFAC checks**.
- **Data Storage Layer:** Relational database storing **user profiles, transactions, exchange rates, and statistics**.
- **External Integrations:** Fetching **real-time exchange rates** and validating customers against the **OFAC database**.

### **High-Level System Architecture Diagram**



---

## **Use Case Diagram**

The **Use Case Diagram** highlights how the system interacts with different **users and external services**.
![Use-Case-Diagram](./diagrams/use-case-diagram.png)


---

## **Class Diagram**

This **Class Diagram** illustrates the **object-oriented design** for the system.

```mermaid
classDiagram
    class Person {
        name
        passportNumber
    }

    class Customer {
        passportIssuingCountry
        email
        phoneNumber
        address
        dateOfBirth
    }

    class PersonalInfo {
        country
    }

    class Profile {
        identification
    }

    class Identification {
        idNumber
        issueDate
        expiryDate
    }

    class Passport {
        issuingCountry
    }

    class DrivingLicense {
        issuingState
        licenseClass
    }

    class NationalID {
        issuingAuthority
    }

    class CurrencyExchange {
        amount
        exchangeRate
        commission
        exchangeDateTime
        transactionId
        exchangeLocation
        sourceCurrency
        targetCurrency
        commissionRate
        methodOfTransaction
    }

    class Currency {
        name
        symbol
        isoCode
        decimalPlaces
    }

    class Rate {
        effectiveDate
    }

    class ExchangeRate {
        rate
    }

    class CommissionRate {
        rate
    }

    class ExchangeRateArchive {
        historicalRates
    }

    class ExchangeRateHistory {
        currency
        exchangeRate
        timestamp
    }

    class Statistics {
        exchangeAmountByCountry
        transactionCountByCountry
    }

    class DailyStatistics {
        date
    }

    class MonthlyStatistics {
        month
        year
    }

    class Country {
        name
        alpha2Code
        alpha3Code
        region
    }

    class ExchangeLocation {
        name
        address
        phoneNumber
    }

    class Transaction {
        method
    }

    Person <|-- Customer
    Person <|-- PersonalInfo
    Rate <|-- ExchangeRate
    Rate <|-- CommissionRate
    Statistics <|-- DailyStatistics
    Statistics <|-- MonthlyStatistics
    Identification <|-- Passport
    Identification <|-- DrivingLicense
    Identification <|-- NationalID

    Customer "1" -- "1..*" CurrencyExchange
    CurrencyExchange "1..*" -- "2" Currency
    CurrencyExchange "1..*" -- "1" ExchangeRate
    CurrencyExchange "1..*" -- "1" CommissionRate
    CurrencyExchange "1..*" -- "1" ExchangeLocation
    CurrencyExchange "1" -- "1" Transaction
    Currency "1..*" -- "1" Country
    ExchangeRate "1..*" -- "1" ExchangeRateArchive
    CurrencyExchange "1..*" -- "1" DailyStatistics
    CurrencyExchange "1..*" -- "1" MonthlyStatistics
    Currency "1" -- "1..*" ExchangeRateHistory
    Profile "1" -- "1" Identification
    Customer "1" -- "1" Profile
```

## **Design Class Diagram**

The **Design Class Diagram** expands upon the Class Diagram by incorporating methods, relationships, and detailed behaviors of each class, ensuring a clear implementation structure.



---

## **Sequence Diagram**

The **Sequence Diagram** below demonstrates the **customer profile creation process**, including validation, uniqueness checks, and **OFAC compliance verification**.

```mermaid
sequenceDiagram
    participant CEA as Currency Exchange Agent
    participant CES as Currency Exchange System
    participant OFAC as OFAC Database
    participant DB as Database
    CEA->>CES: Initiate user profile creation
    CES->>CEA: Display input form
    CEA->>CES: Enter customer details
    CES->>DB: Validate mandatory fields
    DB->>CES: Missing fields (if any)
    CES->>CEA: Display error message
    CEA->>CES: Confirm name & identification details
    CES->>DB: Validate uniqueness
    DB->>CES: Check for duplicate records
    CES->>OFAC: Perform OFAC check
    OFAC->>CES: Return check status
    CES-->>CEA: Display success/error message
```

---

## **Business Logic and Compliance**

- **Real-Time Exchange Rate Retrieval:** Fetches updated exchange rates and applies a configurable commission.
- **Transaction Processing:** Ensures **accurate calculations** for exchanged amounts based on currency and region.
- **OFAC & KYC Compliance:** Verifies **customer identity and financial status** before allowing transactions.
- **Historical Data Storage:** Archives **previous exchange rates and transactions** for reporting and analysis.

---

## **Repository Structure**

```plaintext
money-exchange-system-design/
│
├── README.md                       # Comprehensive project overview and documentation
├── diagrams/                        # UML diagrams and architectural designs
│   ├── use-case-diagram.mmd         # Mermaid file for Use Case Diagram
│   ├── class-diagram.mmd            # Mermaid file for Class Diagram
│   ├── sequence-diagram.mmd         # Mermaid file for Sequence Diagram
│   ├── architecture-diagram.png     # High-level system architecture overview
│   ├── expanded-sequence.png        # Additional sequence diagram for profile creation
└── docs/                            # Additional documentation and analysis
    ├── requirements.md              # Functional and Non-Functional Requirements
    ├── design-decisions.md          # Architectural choices and design patterns
    ├── business-logic.md            # Business rules, data flow, and compliance
    ├── system-workflow.md           # Step-by-step explanation of key processes
```

---

## **Future Enhancements**

- **Multi-Currency Support**: Enhancing exchange calculations for multi-currency transactions.
- **Fraud Detection Mechanism**: Implementing AI-driven fraud detection in compliance with global financial regulations.
- **Automated Exchange Rate Updates**: Real-time integration with global exchange rate providers.



