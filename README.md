# Data-Algorithms-Assignment-5-Queue-
## Description
This project focuses on optimizing the time it takes to process customers at grocery store check-out lines using the `Queue` data structure. The implementation assigns customers to the check-out line with the shortest cumulative service time, taking into account both express and normal lines. The goal is to ensure efficient processing of customers and simulate the time required to clear all customers from the store.

## Features
- **Part A**:
  - Calculate the optimal distribution of customers across check-out lines.
  - Display the initial state of all check-out lines with estimated processing times.
- **Part B**:
  - Simulate customer processing over time and display the number of customers in each line at 30-second intervals until all lines are empty.
- Custom `LinkedQueue<E>` class with:
  - `enqueue(E value)` and `dequeue()` methods.
  - `toString()` method to display the queue contents neatly.
- `Customer` class:
  - Tracks the number of items in a cart.
  - Calculates service time using the formula:  
    ```
    t = 45 + 5 * ni
    ```
    where `t` is the time in seconds, and `ni` is the number of items in the cart.

## File Structure
- `src/`:
  - `LinkedQueue.java`: Generic queue implementation modified with a `toString()` method.
  - `Customer.java`: Represents a customer with properties such as item count and service time.
  - `Main.java`: Implements the logic for both Part A and Part B, including reading input data, simulating queues, and displaying results.
  - `Customer_Example.txt`: Example input file for testing.
  - `Customer.txt`: Dataset for evaluation.
- `README.md`: Documentation for the project.

## How to Run
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/data-algorithms-assignment5.git
2.Open the project in IntelliJ IDEA:
Launch IntelliJ IDEA.
Click File > Open and select the project folder.
Run the project:
3.Build and run Main.java to execute the simulations.   
