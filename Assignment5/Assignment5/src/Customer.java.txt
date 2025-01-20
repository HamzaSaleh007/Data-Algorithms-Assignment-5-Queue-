/**
 * Customer class
 * I Hamza Saleh,000887384 certify that this material is my original work. No other person's work has been used without due acknowledgement.
 */
public class Customer {
    public int items;
    public int serviceTime;
    public int serviceStartTime;

    public Customer(int items) {
        this.items = items;
        this.serviceTime = 45 + 5 * items; ;
    }

    public int getServiceStartTime() {
        return serviceStartTime;
    }

    public void setServiceStartTime(int serviceStartTime) {
        this.serviceStartTime = serviceStartTime;
    }

    public int getItems() {
        return items;
    }
    public int getServiceTime() {
        return serviceTime;
    }

    public String toString(){
        return "Customer with " + items + " items" + " waiting time: " + serviceTime + " s";
    }
}
