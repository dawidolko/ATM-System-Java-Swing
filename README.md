# ATM Simulator

> 🏧 **A cash machine with a real ledger** — a Java Swing ATM where every withdrawal, deposit and transfer is written to MySQL, not held in memory

**ATM Simulator** reproduces the flow of a cash machine: insert a card, enter a PIN, check the balance, withdraw, deposit, pay a bill, review the transaction history. The state lives in MySQL — cards, balances and history are three tables — so closing the application and reopening it finds the account exactly where it was left.

It was built for an object-oriented programming course, which is why the window classes, the table model and the database connector are deliberately separate rather than folded into one file.

![Java](https://img.shields.io/badge/Java-Swing-ED8B00?logo=openjdk&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-JDBC-4479A1?logo=mysql&logoColor=white)
![IntelliJ IDEA](https://img.shields.io/badge/IntelliJ%20IDEA-GUI%20Designer-000000?logo=intellijidea&logoColor=white)
![PlantUML](https://img.shields.io/badge/PlantUML-class%20diagram-FABD14)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 🎯 Key Features

- **State in the database, not in the process** — `karty`, `stan_konta` and `tablehistory` hold the cards, the balances and every operation. Restarting the application changes nothing about the accounts.
- **A PIN gate with card selection** — cards are chosen from a list and unlocked with a PIN before any window that touches money will open.
- **Four operations, each its own window** — balance, withdrawal, deposit and bill payment are separate classes, so a change to one cannot break another.
- **History as a table, backed by a model** — `MyTableModel` feeds the history window from the database rather than from a formatted string, so sorting and column widths behave.
- **A random-password generator** — `MainRandomPassword` produces PINs, kept separate from the flow that uses them.
- **A class diagram in the repository** — `Diagram.puml` is the source, `Diagram.png` the render, so the structure can be read before the code is.
- **The full dataset ships with it** — `database/` holds both SQL dumps and CSV exports of all three tables.

---

## 🖼️ Screenshots

| The main screen                                     | Card PIN entry                                   |
| --------------------------------------------------- | ------------------------------------------------ |
| ![The ATM main screen](public/src/atm_main.png)      | ![PIN entry for a selected card](public/src/card_pin.png) |

| Balance                                              | Withdrawal                                       |
| ---------------------------------------------------- | ------------------------------------------------ |
| ![The account balance window](public/src/Balance.png) | ![The cash withdrawal window](public/src/card_cashing.png) |

| Bill payment                                         | Transaction history                              |
| ---------------------------------------------------- | ------------------------------------------------ |
| ![The bill payment window](public/src/card_payment.png) | ![The transaction history table](public/src/history_transaction.png) |

---

## 🗄️ Database

| Table          | What it holds                                            |
| -------------- | -------------------------------------------------------- |
| `karty`        | Cards and their PINs.                                     |
| `stan_konta`   | The balance behind each card.                             |
| `tablehistory` | Every operation: type, amount, timestamp.                 |

The connection is configured in `DatabaseConnector.java`:

```java
private static final String DATABASE_URL = "jdbc:mysql://localhost:3306/atm";
```

---

## 🎬 Demo

[![Watch the demo on YouTube](https://img.youtube.com/vi/PrwN3nVvUKE/hqdefault.jpg)](https://www.youtube.com/watch?v=PrwN3nVvUKE)

---

## 🛠️ Technology Stack

| Technology                | Role                                                        |
| ------------------------- | ----------------------------------------------------------- |
| **Java**                  | Application language.                                        |
| **Swing**                 | Windows, dialogs and tables.                                 |
| **IntelliJ GUI Designer** | `.form` files beside each window class.                      |
| **JDBC**                  | The MySQL connection.                                        |
| **MySQL / MariaDB**       | Cards, balances and history.                                 |
| **PlantUML**              | The class diagram.                                           |

---

## 🚀 Getting Started

### Prerequisites

- JDK 17 or newer
- MySQL or MariaDB
- The MySQL JDBC driver on the classpath

### 1. Clone the repository

```bash
git clone https://github.com/dawidolko/ATM-System-Java-Swing.git
cd ATM-System-Java-Swing
```

### 2. Create the database

```bash
mysql -u root -p -e "CREATE DATABASE atm;"
mysql -u root -p atm < database/karty.sql
mysql -u root -p atm < database/stan_konta.sql
mysql -u root -p atm < database/tablehistory.sql
```

### 3. Adjust the connection

Edit `public/src/DatabaseConnector.java` if your credentials differ from `root` with an empty password.

### 4. Run

Open the project in IntelliJ IDEA (the `.iml` and `.form` files are part of it) and run `Main`. From the command line:

```bash
javac -cp mysql-connector-java.jar -d out public/src/*.java
java -cp out:mysql-connector-java.jar Main
```

---

## 📁 Project Structure

```
ATM-System-Java-Swing/
├── public/src/
│   ├── Main.java                # entry point
│   ├── MenuWindow.java/.form    # card selection
│   ├── PinWindow.java/.form     # PIN gate
│   ├── Dashboard.java/.form     # operation menu
│   ├── BalanceWindow.java/.form # balance
│   ├── PaycheckWindow.java/.form# deposit
│   ├── PaymentWindows.java/.form# withdrawal and bill payment
│   ├── HistoryWindow.java/.form # transaction history
│   ├── MyTableModel.java        # table model behind the history
│   ├── CardItem.java            # one card in the list
│   ├── DatabaseConnector.java   # the JDBC connection
│   ├── MainRandomPassword.java  # PIN generator
│   └── Diagram.puml / .png      # class diagram
├── database/                    # SQL dumps and CSV exports of all three tables
└── docs/                        # course documentation
```

A Polish version of this document is in [README_POLISH.md](README_POLISH.md).

---

## 📄 License

MIT © [Dawid Olko](https://dawidolko.pl)
