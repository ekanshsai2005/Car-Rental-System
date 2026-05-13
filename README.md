import java.io.*;
import java.util.ArrayList;
import java.util.Scanner;

// Car class
class Car implements Serializable {
    private String model;
    private String brand;
    private double pricePerDay;
    private boolean isAvailable;

    public Car(String model, String brand, double pricePerDay) {
        this.model = model;
        this.brand = brand;
        this.pricePerDay = pricePerDay;
        this.isAvailable = true;
    }

    public String getModel() {
        return model;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void rentCar() {
        if (isAvailable) {
            isAvailable = false;
        } else {
            throw new IllegalStateException("Car is already rented.");
        }
    }

    public void returnCar() {
        isAvailable = true;
    }

    @Override
    public String toString() {
        return String.format("Brand: %s, Model: %s, Price/Day: $%.2f, Available: %b", 
                brand, model, pricePerDay, isAvailable);
    }
}

// Car Rental System
public class CarRentalSystem {
    private static final String FILE_NAME = "cars.dat";
    private ArrayList<Car> cars = new ArrayList<>();

    // Load cars from file
    @SuppressWarnings("unchecked")
    public void loadCars() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILE_NAME))) {
            cars = (ArrayList<Car>) ois.readObject();
        } catch (Exception e) {
            System.out.println("No previous data found.");
        }
    }

    // Save cars to file
    public void saveCars() {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(FILE_NAME))) {
            oos.writeObject(cars);
        } catch (IOException e) {
            System.out.println("Failed to save data.");
        }
    }

    // Add a new car
    public void addCar(String model, String brand, double pricePerDay) {
        cars.add(new Car(model, brand, pricePerDay));
        saveCars();
    }

    // Rent a car
    public void rentCar(String model) {
        for (Car car : cars) {
            if (car.getModel().equalsIgnoreCase(model) && car.isAvailable()) {
                car.rentCar();
                System.out.println("Car rented successfully.");
                saveCars();
                return;
            }
        }
        System.out.println("Car not available.");
    }

    // Display available cars
    public void displayCars() {
        System.out.println("\nAvailable Cars:");
        for (Car car : cars) {
            System.out.println(car);
        }
    }

    public static void main(String[] args) {
        CarRentalSystem rentalSystem = new CarRentalSystem();
        rentalSystem.loadCars();

        Scanner sc = new Scanner(System.in);

        while (true) {
            System.out.println("\n1. Add Car\n2. Rent Car\n3. Display Cars\n4. Exit");
            System.out.print("Choose an option: ");
            int choice = sc.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter model: ");
                    String model = sc.next();
                    System.out.print("Enter brand: ");
                    String brand = sc.next();
                    System.out.print("Enter price per day: ");
                    double price = sc.nextDouble();
                    rentalSystem.addCar(model, brand, price);
                    break;
                case 2:
                    System.out.print("Enter model to rent: ");
                    String rentModel = sc.next();
                    rentalSystem.rentCar(rentModel);
                    break;
                case 3:
                    rentalSystem.displayCars();
                    break;
                case 4:
                    System.out.println("Exiting...");
                    sc.close();
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }
}
