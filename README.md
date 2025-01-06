package basic;

import java.io.*;
import java.text.*;
import java.util.*; // Importing for SimpleDateFormat

// Travel Planner Class
class TravelPlanner {
    private String destination;
    private Date startDate;
    private Date endDate;
    private List<String> activities;
    private double budget;
    private double expenses;

    public TravelPlanner(String destination, Date startDate, Date endDate, double budget) {
        this.destination = destination;
        this.startDate = startDate;
        this.endDate = endDate;
        this.activities = new ArrayList<>();
        this.budget = budget;
        this.expenses = 0;
    }

    // Add activity to itinerary
    public void addActivity(String activity) {
        activities.add(activity);
    }

    // Display itinerary
    public void displayItinerary() {
        System.out.println("Itinerary for " + destination + ":");
        System.out.println("Start Date: " + startDate);
        System.out.println("End Date: " + endDate);
        System.out.println("Activities:");
        for (String activity : activities) {
            System.out.println("- " + activity);
        }
    }

    // Set expenses
    public void addExpense(double amount) {
        if (expenses + amount <= budget) {
            expenses += amount;
            System.out.println("Expense added: " + amount);
        } else {
            System.out.println("Error: Exceeds budget!");
        }
    }

    // Display budget and expenses
    public void displayBudgetStatus() {
        System.out.println("Budget: " + budget);
        System.out.println("Expenses: " + expenses);
        System.out.println("Remaining Budget: " + (budget - expenses));
    }

    // Save trip data to file
    public void saveTripData() {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("trip_data.txt"))) {
            writer.write("Destination: " + destination + "\n");
            writer.write("Start Date: " + startDate + "\n");
            writer.write("End Date: " + endDate + "\n");
            writer.write("Budget: " + budget + "\n");
            writer.write("Expenses: " + expenses + "\n");
            writer.write("Activities: " + String.join(", ", activities) + "\n");
            System.out.println("Trip data saved!");
        } catch (IOException e) {
            System.out.println("Error saving trip data: " + e.getMessage());
        }
    }
}

// Main class
public class Main {
    public static void main(String[] args) {
        // Sample trip creation
        Scanner scanner = new Scanner(System.in);
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");

        try {
            System.out.print("Enter destination: ");
            String destination = scanner.nextLine();

            System.out.print("Enter start date (yyyy-MM-dd): ");
            String startDateStr = scanner.nextLine();
            Date startDate = dateFormat.parse(startDateStr);  // Using SimpleDateFormat to parse the date

            System.out.print("Enter end date (yyyy-MM-dd): ");
            String endDateStr = scanner.nextLine();
            Date endDate = dateFormat.parse(endDateStr);  // Using SimpleDateFormat to parse the date

            System.out.print("Enter budget: ");
            double budget = scanner.nextDouble();
            scanner.nextLine();  // Consume newline left by nextDouble()

            // Create TravelPlanner object
            TravelPlanner planner = new TravelPlanner(destination, startDate, endDate, budget);

            // Add activities
            while (true) {
                System.out.print("Enter activity (or type 'done' to finish): ");
                String activity = scanner.nextLine();
                if (activity.equalsIgnoreCase("done")) break;
                planner.addActivity(activity);
            }

            // Add expenses
            while (true) {
                System.out.print("Enter expense amount (or type 'done' to finish): ");
                String expenseInput = scanner.nextLine();
                if (expenseInput.equalsIgnoreCase("done")) break;
                try {
                    double expense = Double.parseDouble(expenseInput);
                    planner.addExpense(expense);
                } catch (NumberFormatException e) {
                    System.out.println("Invalid amount. Please try again.");
                }
            }

            // Display itinerary and budget status
            planner.displayItinerary();
            planner.displayBudgetStatus();

            // Save trip data to file
            planner.saveTripData();

        } catch (ParseException e) {
            System.out.println("Invalid date format. Please use 'yyyy-MM-dd'.");
        }

        scanner.close();
    }
}
