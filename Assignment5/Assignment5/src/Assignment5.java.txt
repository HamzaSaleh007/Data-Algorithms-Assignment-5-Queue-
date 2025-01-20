/**
 * I Hamza Saleh,000887384 certify that this material is my original work. No other person's work has been used without due acknowledgement.
 * This program simulates a grocery store checkout system with express and normal lines.
 * It reads customer data from a file and enqueues them into the appropriate line based on the number of items they have.
 * The program then simulates the checkout process and prints the status of each queue at 1-minute intervals.
 */


import java.io.File;
import java.util.ArrayList;
import java.util.Scanner;

public class Assignment5 {

    public static void main(String[] args) {
        try {
            // Read customer data from file
            File file = new File("src/CustomerData.txt");
            Scanner scanner = new Scanner(file);

            // assign the values from the file to the variables
            int express = scanner.nextInt(); // Number of express lines
            int normal = scanner.nextInt(); // Number of normal lines
            int maxItems = scanner.nextInt(); // Max items for express checkout

            // Create queues for express and normal lines, and wait time lists for each
            ArrayList<LinkedQueue<Customer>> expressQueues = new ArrayList<>();
            ArrayList<LinkedQueue<Customer>> normalQueues = new ArrayList<>();
            ArrayList<Integer> expressWaiting = new ArrayList<>();
            ArrayList<Integer> normalWaiting = new ArrayList<>();

            // Initialize the express queues and wait times
            for (int i = 0; i < express; i++) {
                expressQueues.add(new LinkedQueue<>());
                expressWaiting.add(0);
            }

            // Initialize the normal queues and wait times
            for (int i = 0; i < normal; i++) {
                normalQueues.add(new LinkedQueue<>());
                normalWaiting.add(0);
            }

            // Enqueue customers into the appropriate line
            while (scanner.hasNextInt()) {
                int items = scanner.nextInt();
                Customer customer = new Customer(items);
                if (items <= maxItems && !expressQueues.isEmpty()) {
                    enqueueCustomer(customer, expressQueues, expressWaiting);
                } else {
                    enqueueCustomer(customer, normalQueues, normalWaiting);
                }
            }
            scanner.close();

            // Print the initial state of the queues (A)
            printPartA(expressQueues, expressWaiting, normalQueues, normalWaiting);

            // Start the checkout process
            CheckoutProcess(expressQueues, expressWaiting, normalQueues, normalWaiting);

        } catch (Exception e) {
            e.getMessage();
        }
    }


    /**
     * Enqueue a customer into the shortest line
     *
     * @param customer the customer to be enqueued
     * @param queues   the list of queues to choose from
     * @param waitTimes the list of wait times for each queue
     */
    private static void enqueueCustomer(Customer customer, ArrayList<LinkedQueue<Customer>> queues, ArrayList<Integer> waitTimes) {
        int shortestTime = 0;
        for (int i = 1; i < waitTimes.size(); i++) {
            if (waitTimes.get(i) < waitTimes.get(shortestTime)) {
                shortestTime = i;
            }
        }
        queues.get(shortestTime).enqueue(customer);
        int newTime = waitTimes.get(shortestTime) + (45 + 5 * customer.getItems());
        waitTimes.set(shortestTime, newTime);
    }

    /**
     * Simulate the checkout process
     *
     * @param expressQueues the list of express queues
     * @param expressWaitTimes the list of wait times for each express queue
     * @param normalQueues the list of normal queues
     * @param normalWaitTimes the list of wait times for each normal queue
     */
    private static void CheckoutProcess(ArrayList<LinkedQueue<Customer>> expressQueues, ArrayList<Integer> expressWaitTimes,
                                        ArrayList<LinkedQueue<Customer>> normalQueues, ArrayList<Integer> normalWaitTimes) {
        int currentTime = 0;
        int maxTime = Math.max(maxWaitTime(expressWaitTimes), maxWaitTime(normalWaitTimes));

        while (currentTime <= maxTime) {
            processQueues(currentTime, expressQueues, expressWaitTimes);
            processQueues(currentTime, normalQueues, normalWaitTimes);

            if (currentTime % 60 == 0) {
                printPartB(currentTime, expressQueues, normalQueues);
            }
            else if (currentTime == maxTime) {
                printPartB(currentTime, expressQueues, normalQueues);
                System.out.println("All customers have been serviced");
            }
            currentTime++;
        }
    }

    /**
     * Process the queues at a given time
     *
     * @param currentTime the current time
     * @param queues the list of queues to process
     * @param waitTimes the list of wait times for each queue
     */
    private static void processQueues(int currentTime, ArrayList<LinkedQueue<Customer>> queues, ArrayList<Integer> waitTimes) {
        for (int i = 0; i < queues.size(); i++) {
            LinkedQueue<Customer> queue = queues.get(i);
            if (!queue.isEmpty()) {
                Customer customer = queue.peek();
                int serviceTimeRemaining = customer.getServiceTime() - (currentTime - customer.getServiceStartTime());
                if (serviceTimeRemaining <= 0) {
                    queue.dequeue(); // Customer has been fully serviced and leaves the queue
                    if (!queue.isEmpty()) {
                        queue.peek().setServiceStartTime(currentTime); // Set start time for the next customer in line
                        // Adjust the wait time for the next customer
                        waitTimes.set(i, queue.peek().getServiceTime());
                    } else {
                        waitTimes.set(i, 0);
                    }
                }
            }
        }
    }


    /**
     * Find the maximum wait time from a list of wait times
     *
     * @param waitTimes the list of wait times
     * @return the maximum wait time
     */
    private static int maxWaitTime(ArrayList<Integer> waitTimes) {
    int max = 0;
        for (int time : waitTimes) {
            if (time > max) {
                max = time;
            }
        }
        return max;
    }

    /**
     * Print the status of the queues at a given time
     *
     * @param currentTime the current time
     * @param expressQueues the list of express queues
     * @param normalQueues the list of normal queues
     */
    private static void printPartB(int currentTime, ArrayList<LinkedQueue<Customer>> expressQueues, ArrayList<LinkedQueue<Customer>> normalQueues) {
        System.out.println("Part B - Status of the queues at time " + currentTime + "s :");

        int queueNum = 1;
        for (LinkedQueue<Customer> queue : expressQueues) {
            System.out.println("Express Queue #" + queueNum + ": " + queue.size() + " customers");
            queueNum++;
        }
        for (LinkedQueue<Customer> queue : normalQueues) {
            System.out.println("Normal Queue #" + queueNum + ": " + queue.size() + " customers");
            queueNum++;
        }
    }

    /**
     * Print the initial state of the queues
     *
     * @param expressQueues the list of express queues
     * @param expressWaitTimes the list of wait times for each express queue
     * @param normalQueues the list of normal queues
     * @param normalWaitTimes the list of wait times for each normal queue
     */
    private static void printPartA(ArrayList<LinkedQueue<Customer>> expressQueues, ArrayList<Integer> expressWaitTimes,
                                   ArrayList<LinkedQueue<Customer>> normalQueues, ArrayList<Integer> normalWaitTimes) {
        System.out.println("PART A - Checkout lines and time estimates for each line:");
        int queueNum = 1;
        for (int i = 0; i < expressQueues.size(); i++) {
            System.out.println("CheckOut(Express) # " + queueNum + " (Est Time = " + expressWaitTimes.get(i) + " s) = " + expressQueues.get(i));
            queueNum++;
        }
        for (int i = 0; i < normalQueues.size(); i++) {
            System.out.println("CheckOut(Normal) # " + queueNum + " (Est Time = " + normalWaitTimes.get(i) + " s) = " + normalQueues.get(i));
            queueNum++;
        }

        // time to clear store of all customers
        int timeToClear = Math.max(maxWaitTime(expressWaitTimes), maxWaitTime(normalWaitTimes));
        System.out.println("Time to clear store of all customers = " + timeToClear + " s");
    }
}