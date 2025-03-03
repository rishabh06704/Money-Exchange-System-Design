
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

---

### **High-Level System Architecture Diagram**

```mermaid
graph TD;
    
    subgraph Users
        A[Admin] -->|Login| S[System]
        B[Manager] -->|Login| S
        C[Exchange Agent] -->|Login| S
    end

    subgraph System Components
        S[Currency Exchange System] -->|Validates| DB[(Database)]
        S -->|Fetches Rates| API[Exchange Rate API]
        S -->|Checks Compliance| OFAC[OFAC Compliance API]
        S -->|Logs Transactions| LOGS[Audit Logging]
        S -->|Generates Reports| REP[Reporting Module]
    end

    subgraph Data Storage
        DB -.->|Stores| UsersTable[(Users Table)]
        DB -.->|Stores| TransactionsTable[(Transactions Table)]
        DB -.->|Stores| RatesTable[(Exchange Rates Table)]
        DB -.->|Stores| ComplianceTable[(KYC Compliance)]
    end

    API -.->|Fetches real-time rates| RatesTable
    OFAC -.->|Verifies user compliance| ComplianceTable
    LOGS -.->|Records system activity| TransactionsTable
    REP -.->|Generates insights| TransactionsTable
```
## **System Workflow**
The **Money Exchange System** operates through a structured workflow:
1. **User Authentication:** Admins, Managers, and Exchange Agents log in with secure credentials.
2. **Customer Registration & Verification:** Customer details are validated, and KYC/OFAC checks are performed.
3. **Currency Exchange Processing:**
   - Retrieves current exchange rate and commission.
   - Computes the final amount and records the transaction.
4. **Transaction Storage & Reporting:**
   - Transactions are logged into the database.
   - Admins generate daily/monthly reports for financial auditing.
     
---

## **Use Case Diagram**

The **Use Case Diagram** highlights how the system interacts with different **users and external services**.
![Use-Case-Diagram](./diagrams/use-case-diagram.png)


---


## **Business Logic & Compliance**
The **Money Exchange System (MEXS)** ensures **secure, accurate, and compliant** currency exchange operations. The system follows strict **regulatory standards** while efficiently processing financial transactions.

### ** Transaction Processing**
- **Dynamic Exchange Rate Retrieval:**  
  - The system fetches the latest exchange rates from an **external API** before processing transactions.
  - A configurable **commission rate** is applied to determine the final exchange amount.
  
- **Currency Exchange Calculation:**  
  - Computes the **final amount** using the formula:  
    \[
    \text{Final Amount} = (\text{Exchange Rate} \times \text{Original Amount}) - \text{Commission}
    \]
  - Ensures accuracy with proper **rounding mechanisms** based on the currency's decimal precision.

- **Multi-Method Payment Processing:**  
  - Customers can complete transactions using **cash, credit card, or checks**.
  - The system tracks transaction **timestamps**, amounts, and payment methods.

### ** Regulatory Compliance Enforcement**
- **KYC (Know Your Customer) & Identity Verification:**  
  - Customers must present valid identification (**Passport, Driving License, National ID**) before initiating a transaction.
  - System **validates customer details** against stored records.

- **OFAC Sanction List Check:**  
  - Every customer is **screened in real time** against the **Office of Foreign Assets Control (OFAC) list** to ensure compliance.
  - Transactions are **blocked automatically** if a match is found.

- **Fraud Prevention Mechanisms:**  
  - Detects **suspicious transactions** (e.g., unusually large exchanges, multiple rapid transactions).
  - Logs flagged transactions for **further review** by compliance officers.

### ** Transaction Auditing & Reporting**
- **Daily and Monthly Reports:**  
  - Admins generate reports showing **total transactions, exchanged amounts, commission earned, and flagged transactions**.
  - Reports help in **financial tracking and compliance audits**.

- **Secure Data Storage:**  
  - All **transactions and customer data** are stored securely in an **encrypted database**.
  - Follows **data retention policies** for auditability.

---

## **Design Decisions**
Key architectural and design choices for **scalability and security:**
- **RBAC Implementation:** Restricts system access based on user roles.
- **Database Normalization:** Ensures data consistency and eliminates redundancy.
- **Separation of Concerns:** Divides responsibilities into authentication, business logic, and storage layers.
- **Compliance Integration:** Incorporates external APIs for **OFAC checks and real-time exchange rates**.

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

**Class Model Definitions**

- **PersonalInfo:** Represents a person’s country information.
- **Profile:** Links customers to various forms of identification (Passport, DrivingLicense, NationalID).
- **Identification:** Abstract class representing different identification forms, characterized by ID number, issue date, and expiry date.
- **Passport:** Subclass of Identification, details issuing country.
- **DrivingLicense:** Subclass of Identification, includes issuing state and license class.
- **NationalID:** Subclass of Identification, includes issuing authority.
- **CurrencyExchange:** Represents transaction details of currency exchanges, including amount, exchange rate, commission, timestamp, and transaction ID.
- **Currency:** Represents different currencies with attributes like name, symbol, ISO code, and decimal places.
- **Rate:** Base class for ExchangeRate and CommissionRate, capturing the effective date.
- **ExchangeRate:** Subclass of Rate, storing currency exchange rates.
- **CommissionRate:** Subclass of Rate, storing commission values.
- **ExchangeRateArchive:** Keeps historical exchange rate records.
- **ExchangeRateHistory:** Stores past exchange rate data with timestamps.
- **Statistics:** Captures exchange volume and transaction count by country.
- **DailyStatistics:** Subclass of Statistics, records daily transactions.
- **MonthlyStatistics:** Subclass of Statistics, records monthly transactions.
- **Country:** Represents a country with name, alpha-2/3 codes, and region.
- **ExchangeLocation:** Stores exchange office details like name, address, and phone number.
- **Transaction:** Represents the method used for a currency exchange transaction.

---

## **Design Class Diagram**

The **Design Class Diagram** expands upon the Class Diagram by incorporating methods, relationships, and detailed behaviors of each class, ensuring a clear implementation structure.

```mermaid
classDiagram
    class Person {
        - name: String
        - passportNumber: String
        + getName(): String
        + setName(name: String)
        + getPassportNumber(): String
        + setPassportNumber(passportNumber: String)
    }

    class Customer {
        - passportIssuingCountry: String
        - email: String
        - phoneNumber: String
        - address: String
        - dateOfBirth: Date
        + getPassportIssuingCountry(): String
        + setPassportIssuingCountry(passportIssuingCountry: String)
        + getEmail(): String
        + setEmail(email: String)
        + getPhoneNumber(): String
        + setPhoneNumber(phoneNumber: String)
        + getAddress(): String
        + setAddress(address: String)
        + getDateOfBirth(): Date
        + setDateOfBirth(dateOfBirth: Date)
    }

    class PersonalInfo {
        - country: String
        + getCountry(): String
        + setCountry(country: String)
    }

    class Profile {
        - identification: Identification
        + getIdentification(): Identification
        + setIdentification(identification: Identification)
    }

    class Identification {
        - idNumber: String
        - issueDate: Date
        - expiryDate: Date
        + getIdNumber(): String
        + setIdNumber(idNumber: String)
        + getIssueDate(): Date
        + setIssueDate(issueDate: Date)
        + getExpiryDate(): Date
        + setExpiryDate(expiryDate: Date)
    }

    class Passport {
        - issuingCountry: String
        + getIssuingCountry(): String
        + setIssuingCountry(issuingCountry: String)
    }

    class DrivingLicense {
        - issuingState: String
        - licenseClass: String
        + getIssuingState(): String
        + setIssuingState(issuingState: String)
        + getLicenseClass(): String
        + setLicenseClass(licenseClass: String)
    }

    class NationalID {
        - issuingAuthority: String
        + getIssuingAuthority(): String
        + setIssuingAuthority(issuingAuthority: String)
    }

    class CurrencyExchange {
        - amount: Float
        - exchangeRate: Float
        - commission: Float
        - exchangeDateTime: DateTime
        - transactionId: String
        - exchangeLocation: ExchangeLocation
        - sourceCurrency: Currency
        - targetCurrency: Currency
        - commissionRate: Float
        - methodOfTransaction: String
        + getAmount(): Float
        + setAmount(amount: Float)
        + getExchangeRate(): Float
        + setExchangeRate(exchangeRate: Float)
        + getCommission(): Float
        + setCommission(commission: Float)
        + getExchangeDateTime(): DateTime
        + setExchangeDateTime(exchangeDateTime: DateTime)
        + getTransactionId(): String
        + setTransactionId(transactionId: String)
        + getExchangeLocation(): ExchangeLocation
        + setExchangeLocation(exchangeLocation: ExchangeLocation)
        + getSourceCurrency(): Currency
        + setSourceCurrency(sourceCurrency: Currency)
        + getTargetCurrency(): Currency
        + setTargetCurrency(targetCurrency: Currency)
        + getCommissionRate(): Float
        + setCommissionRate(commissionRate: Float)
        + getMethodOfTransaction(): String
        + setMethodOfTransaction(methodOfTransaction: String)
    }

    class Currency {
        - name: String
        - symbol: String
        - isoCode: String
        - decimalPlaces: Int
        + getName(): String
        + setName(name: String)
        + getSymbol(): String
        + setSymbol(symbol: String)
        + getIsoCode(): String
        + setIsoCode(isoCode: String)
        + getDecimalPlaces(): Int
        + setDecimalPlaces(decimalPlaces: Int)
    }

    class Rate {
        - effectiveDate: Date
        + getEffectiveDate(): Date
        + setEffectiveDate(effectiveDate: Date)
    }

    class ExchangeRate {
        - rate: Float
        + getRate(): Float
        + setRate(rate: Float)
    }

    class CommissionRate {
        - rate: Float
        + getRate(): Float
        + setRate(rate: Float)
    }

    class ExchangeRateArchive {
        - historicalRates: List<ExchangeRate>
        + getHistoricalRates(): List<ExchangeRate>
        + setHistoricalRates(historicalRates: List<ExchangeRate>)
    }

    class ExchangeRateHistory {
        - currency: Currency
        - exchangeRate: Float
        - timestamp: DateTime
        + getCurrency(): Currency
        + setCurrency(currency: Currency)
        + getExchangeRate(): Float
        + setExchangeRate(exchangeRate: Float)
        + getTimestamp(): DateTime
        + setTimestamp(timestamp: DateTime)
    }

    class Statistics {
        - exchangeAmountByCountry: Map<Country, Float>
        - transactionCountByCountry: Map<Country, Int>
        + getExchangeAmountByCountry(): Map<Country, Float>
        + setExchangeAmountByCountry(exchangeAmountByCountry: Map<Country, Float>)
        + getTransactionCountByCountry(): Map<Country, Int>
        + setTransactionCountByCountry(transactionCountByCountry: Map<Country, Int>)
    }

    class DailyStatistics {
        - date: Date
        + getDate(): Date
        + setDate(date: Date)
    }

    class MonthlyStatistics {
        - month: Int
        - year: Int
        + getMonth(): Int
        + setMonth(month: Int)
        + getYear(): Int
        + setYear(year: Int)
    }

    class Country {
        - name: String
        - alpha2Code: String
        - alpha3Code: String
        - region: String
        + getName(): String
        + setName(name: String)
        + getAlpha2Code(): String
        + setAlpha2Code(alpha2Code: String)
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
### **Design Class Model Enhancements:**
- **Encapsulation of Attributes:** Getter and setter methods for controlled data access.
- **Inheritance & Polymorphism:** Parent classes like `Identification` extend into subclasses like `Passport`, `DrivingLicense`, and `NationalID`.
- **Composition and Associations:**
  - `Customer` **has-a** `Profile`, which **has-a** `Identification`.
  - `CurrencyExchange` **has-a** `ExchangeLocation` and involves `SourceCurrency` & `TargetCurrency`.
- **Transaction Methods:**
  - `calculateFinalAmount()` – Computes the final amount after commission.
  - `validateIdentification()` – Ensures uniqueness and compliance.
  - `generateReport()` – Aggregates statistics on daily/monthly transactions.
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


## **Future Enhancements**

- **Multi-Currency Support**: Enhancing exchange calculations for multi-currency transactions.
- **Fraud Detection Mechanism**: Implementing AI-driven fraud detection in compliance with global financial regulations.
- **Automated Exchange Rate Updates**: Real-time integration with global exchange rate providers.

---

## **Repository Structure**

```plaintext
money-exchange-system-design/
│
├── README.md                       # Comprehensive project overview and documentation
├── diagrams/                       # UML diagrams and architectural designs
│   ├── use-case-diagram.png        # file for Use Case Diagram
│   ├── class-diagram.md            # Mermaid file for Class Diagram
│   ├── design-class-diagram.md     # Mermaid file for Design Class Diagram
│   ├── architecture-diagram.png    # High-level system architecture overview
│   ├── expanded-sequence.png       # Additional sequence diagram for profile creation
└── docs/                           # Additional documentation and analysis
    ├── project-report.pdf          # Detailed report
   
```

---

