# KBA Automobile — Hyperledger Fabric

A blockchain-based automobile management application developed as part of the **Kerala Blockchain Academy (KBA) Hyperledger Fabric course**.

The project demonstrates how Hyperledger Fabric can be used to manage automobile lifecycle data across multiple organizations, including vehicle creation, dealer orders, order matching, private data, and vehicle registration.

---

## 📌 Overview

**KBA Automobile** is a permissioned blockchain application built using **Hyperledger Fabric**.

The network consists of multiple organizations with different responsibilities:

| Organization | Role                           |
| ------------ | ------------------------------ |
| **Org1**     | Manufacturer                   |
| **Org2**     | Dealer                         |
| **Org3**     | Motor Vehicle Department (MVD) |

The application uses smart contracts to control how vehicle and order data is created, queried, updated, and accessed.

---

## 🏗️ Architecture

The application follows this basic flow:


Web Browser
     │
     ▼
HTML / CSS / JavaScript
     │
     ▼
Go + Gin Web Application
     │
     ▼
Fabric Gateway
     │
     ▼
Hyperledger Fabric Network
     │
     ├── Org1 - Manufacturer
     ├── Org2 - Dealer
     └── Org3 - MVD
             │
             ▼
        Chaincode
             │
             ▼
           Ledger


---

## ✨ Features

* Vehicle creation and querying
* Organization-based access control
* Dealer order management
* Private data collections
* Vehicle and order matching
* Vehicle registration
* CouchDB-based rich queries
* CouchDB indexes
* Pagination
* Transaction history
* Chaincode events
* Fabric Gateway integration
* REST API using Gin
* Web-based automobile dashboard

---

## 🛠️ Technologies Used

* **Hyperledger Fabric**
* **Go**
* **Fabric Gateway**
* **Gin Web Framework**
* **CouchDB**
* **Docker**
* **HTML**
* **CSS**
* **JavaScript**
* **Ubuntu / WSL2**

---

## 🔗 Hyperledger Fabric Network

The project uses a Fabric network containing:

* **Org1** — Manufacturer
* **Org2** — Dealer
* **Org3** — MVD
* **Orderer**
* **CouchDB**
* **Channel:** `autochannel`
* **Chaincode:** `KBA-Automobile`

The chaincode contains two smart contracts:

* `CarContract`
* `OrderContract`

---

## 📂 Project Structure


KBA-CHF/
│
├── KBA-Automobile/
│   │
│   ├── Chaincode/
│   │   ├── car-contract.go
│   │   ├── order-contract.go
│   │   ├── main.go
│   │   ├── collections.json
│   │   └── META-INF/
│   │
│   ├── Client/
│   │   ├── client.go
│   │   ├── connect.go
│   │   ├── profile.go
│   │   ├── event.go
│   │   └── main.go
│   │
│   ├── public/
│   │   ├── styles/
│   │   └── scripts/
│   │
│   ├── templates/
│   │   └── index.html
│   │
│   ├── client.go
│   ├── connect.go
│   ├── profile.go
│   ├── main.go
│   ├── go.mod
│   └── go.sum
│
└── fabric-samples/
    └── test-network/


---

## ⚙️ Prerequisites

Before running the project, install the following:

* Windows with **WSL2**
* **Ubuntu 24.04**
* **Docker Desktop**
* **Git**
* **Go**
* **Node.js and npm**
* **jq**
* **Hyperledger Fabric binaries and samples**

Docker Desktop should have **WSL2 integration** enabled for the Ubuntu distribution.

---

# 🚀 Getting Started

## 1. Clone the Repository

Clone the repository and enter the project directory:


git clone <YOUR-GITHUB-REPOSITORY-URL>
cd KBA-CHF


> Replace `<YOUR-GITHUB-REPOSITORY-URL>` with the URL of this repository.

---

## 2. Set Up the Fabric Network

Navigate to the Fabric test network:


cd fabric-samples/test-network


Start the Fabric network, create the channel, enable Certificate Authorities, and use CouchDB:


./network.sh up createChannel -c autochannel -ca -s couchdb


---

## 3. Add Org3

Navigate to the Org3 setup directory:


cd addOrg3


Add Org3 to the network and join it to `autochannel`:


./addOrg3.sh up -c autochannel -ca -s couchdb


Return to the test network directory:


cd ..


---

## 4. Deploy the Chaincode

Deploy the KBA Automobile chaincode to the channel:


./network.sh deployCC \
  -ccn KBA-Automobile \
  -ccp ../../KBA-Automobile/Chaincode/ \
  -ccl go \
  -c autochannel \
  -ccv 7.0 \
  -ccs 7 \
  -cccg ../../KBA-Automobile/Chaincode/collections.json


The chaincode contains the application's automobile and order management logic.

---

# 💻 Running the Web Application

Open a **new Ubuntu terminal** and navigate to the application directory:


cd ~/KBA-CHF/KBA-Automobile


Start the Go application:


go run .


The application starts on:


http://localhost:8080


Open the address in a web browser.

---

## 🌐 Application Interface

The web application provides a simple manufacturer dashboard.

### Create Car

The manufacturer can enter:

* Car ID
* Make
* Model
* Color
* Date of Manufacture
* Manufacturer Name

The application sends the information to the backend, which submits a `CreateCar` transaction through the Fabric Gateway.

### Query Car

A user can enter a Car ID to retrieve the corresponding vehicle information from the Fabric network.

---

# 🔄 Transaction Flow

When a user creates or queries a car, the request moves through the application and Hyperledger Fabric network.

### Create Car Transaction

```text
User
  │
  ▼
Web Browser
  │
  ▼
HTML / JavaScript
  │
  ▼
Gin Web Server
  │
  ▼
Fabric Gateway
  │
  ▼
Fabric Peer
  │
  ▼
CarContract
  │
  ▼
Endorsement
  │
  ▼
Orderer
  │
  ▼
Block
  │
  ▼
Validation & Commit
  │
  ▼
Ledger
```

### Query Car

Queries follow a shorter path because they do not create a new transaction or block.

```text
User
  │
  ▼
Web Browser
  │
  ▼
HTML / JavaScript
  │
  ▼
Gin Web Server
  │
  ▼
Fabric Gateway
  │
  ▼
Fabric Peer
  │
  ▼
CarContract
  │
  ▼
World State
  │
  ▼
Car Data
  │
  ▼
Web Browser
```

### In This Project

* **Create Car** uses `SubmitTransaction()` to submit a transaction to the Fabric network.
* **Query Car** uses `EvaluateTransaction()` to read the current state.
* The **Fabric Gateway** connects the Go application to the Fabric network.
* The **Chaincode** contains the business logic.
* The **Orderer** orders endorsed transactions into blocks.
* Peers **validate and commit** valid transactions to the ledger.
* The **World State** contains the latest state of the automobile data.

# 🔐 Privacy and Access Control

The project demonstrates Fabric's permissioned architecture and privacy features.

### Private Data Collection

Dealer orders are handled using a private data collection named:


OrderCollection


The collection is configured for access by:

* Org1
* Org2

This allows sensitive order information to remain private to authorized organizations.

### Organization-Based Access

Different operations are restricted to specific organizations.

For example:

* **Org1** → automobile creation and manufacturer operations
* **Org2** → dealer order operations
* **Org3** → vehicle registration

These restrictions are implemented through chaincode logic.

---

# 🗃️ CouchDB and Queries

CouchDB is used as the state database for the Fabric network.

The project demonstrates:

* Rich queries
* Range queries
* Pagination
* CouchDB indexes
* Private data queries

The project includes a CouchDB index for efficient vehicle ID queries.

---

# 📜 Smart Contracts

## CarContract

`CarContract` manages automobile-related operations, including:

* Creating cars
* Reading cars
* Deleting cars
* Querying cars
* Range queries
* Pagination
* Transaction history
* Matching dealer orders
* Registering vehicles

## OrderContract

`OrderContract` manages dealer orders, including:

* Creating orders
* Reading orders
* Deleting orders
* Querying orders
* Private data handling

---

# 📡 Events

The project also demonstrates Fabric events.

The client application contains event listeners for:

* Block events
* Chaincode events
* Private block events

Chaincode events allow an application to receive notifications when specific blockchain operations occur.

---

# 🛑 Stopping the Network

To stop the Fabric network:


cd fabric-samples/test-network
./network.sh down


---

## 👩‍💻 Author

**Aswani Vijoy**

Developed as part of the **Kerala Blockchain Academy Hyperledger Fabric course**.
