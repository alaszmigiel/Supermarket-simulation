# Supermarket Simulation
This project simulates operations of a supermarket with multiple cash registers, where **customers** interact with **cashiers** in real-time. 
Customers select the shortest queue, wait for their turn, and are served by cashiers. 
The system manages **cashier breaks**, **shift changes**, and synchronizes all processes using **monitors** and **locks** to ensure smooth operation and avoid conflicts when accessing shared resources.


## Key features 
- ***Customer Process***:  The customer enters the supermarket, selects the shortest queue, and waits for their turn to be served.
After being served by a cashier, the customer process completes. Customers are visualized as moving circles, which change colors when being served.
- ***Cashier Process***:  Cashiers serve customers from their respective queues.
After serving customers for a specified period, cashiers can request breaks or shift changes. When this happens, the register temporarily closes, and the queue is blocked for new customers.
- ***Animation***:  Using **JavaFX**, the simulation provides a dynamic visualization of customers and cashiers interacting in real-time.
Cash registers are represented as rectangles, and customers as moving circles, with color changes reflecting different statuses (open, on break, closing, etc.).
- ***Synchronization and Thread Management***: The simulation uses **locks** to ensure that resources like queues and cashier operations are accessed in a thread-safe manner, preventing race conditions and ensuring smooth operation. 

## Project Structure
```sh
SupermarketSimulation/
│── src/main/                    
│   ├── java/com/supermarketsimulation/  
│   │   ├── Client.java                 # Represents a customer 
│   │   ├── Cashier.java                # Represents a cashier and handles operations (serving, breaks, shifts)
│   │   ├── CashRegistersMonitor.java   # Manages the state of all registers and queues
│   │   ├── SupermarketController.java  # Controls simulation logic
│   │   ├── Main.java                   # Main class to start the simulation 
│── resources/com/supermarketsimulation/  
│   ├── SupermarketLayout.fxml          # JavaFX layout for the simulation interface
│── .gitignore                         # Files and directories to be ignored in version control
│── pom.xml                            # Maven project configuration
│── README.md                          # Project documentation (this file)
```

## Technical Requirements 
- Java 21 or higher
- Maven
- JavaFX

## Installation & Usage

### 1. Clone the repository:
```sh
git clone https://github.com/alaszmigiel/Supermarket-simulation.git
cd Supermarket-Simulation
```

### 2. Build the project:
```sh
mvn clean install
```

### 3. Run the simulation:
```sh
mvn javafx:run
```

## Shared Resources & Synchronization:
The simulation relies on several shared resources that are synchronized:
- **Customer Queues**: Each register has its own queue, and the system uses locks to manage safe access to these queues and prevent race conditions.
- **Cash Registers and Operations**: System makes sure that operations like **breaks** and **shifts** are synchronized, and register status changes are handled without conflicts.
- **Synchronization Objects**:
  - **Local Locks**: Each customer queue is protected by a dedicated lock to have thread-safe access.
  - **Global Lock**: A global lock is used for operations that affect multiple queues or the overall system.

## Simulation Workflow
- ***Customer Process***: Customers enter the store, choose the shortest queue, and wait to be served.
As they are served, their circle in the animation turns **green**. Once served, the customer process ends.

- ***Cashier Process***: Cashiers serve customers from their respective queues.
After a set time, the cashier may **request a break**, which turns the register color to **yellow**.
Cashier must wait in line for their turn to take a break and when he becomes first it will be approved when no other cashier is currently on break and no other cashier
is in the process of switching shifts.
Once a **break request is approved**, the register color changes to **orange**, and the queue is temporarily blocked from accepting new customers.
When a cashier returns from their break, the register color turns green, signaling it is open again.

- ***Shift Change***:
After returning from their break, the cashier may **request a shift change**. Register color then turns **dark blue**. A shift change is only allowed if
cashier is the first in line for a shift change and no other cashier is currently changing or on break
Once all customers in the queue are served, the register color changes to **light blue**, meaning that a new cashier will take over the register.

- ***Store Closing***: Once all customers have entered the store, the registers turn light gray, signaling that the store is closing.
After all customers have been served, the register turns **dark gray**, marking the end of the simulation.
At this point, no further break or shift requests can be made, and all cashiers finish their work.

### Simulation States:
- **Register States**:
  - **Green**: Register is open and serving customers.
  - **Yellow**: Cashier has requested a break.
  - **Orange**: Register is closing, serving last customers.
  - **Red**: Cashier is on break.
  - **Dark Blue**: Cashier has requested a shift change.
  - **Light Blue**: Register is closed for a shift change, and a new cashier takes over.
  - **Light Gray**: All customers have entered the store, store is closing.
  - **Dark Gray**: Register is closed, no more customers to serve.
- **Customer States**:
  - **Black**: Customer is choosing a queue or waiting in line.
  - **Green**: Customer is being served.

