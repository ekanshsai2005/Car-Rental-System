# Car Rental System

A simple console-based car rental application written in Java. It lets you add cars to a fleet, rent them out, and view the fleet's current status. Data is saved to disk, so your cars and their rental status are still there the next time you run the program.

## Features

- **Add cars** with a model, brand, and price per day
- **Rent a car** by model name (case-insensitive)
- **Display all cars** with their brand, model, price, and availability
- **Persistent storage** using Java serialization (`cars.dat`)
- **Menu-driven interface** that loops until you choose to exit

## Project Structure

| File | Description |
|------|-------------|
| `CarRentalSystem.java` | Contains both the `Car` class and the `CarRentalSystem` class with the `main` method |
| `cars.dat` | Auto-generated data file that stores the car list (created after the first car is added) |

## How It Works

### `Car` class
Represents a single car. It implements `Serializable` so it can be written to and read from a file.

| Field | Type | Purpose |
|-------|------|---------|
| `model` | `String` | Car model name |
| `brand` | `String` | Car brand/manufacturer |
| `pricePerDay` | `double` | Daily rental price |
| `isAvailable` | `boolean` | `true` if the car can be rented |

Key methods:
- `rentCar()` marks the car as unavailable (throws `IllegalStateException` if already rented)
- `returnCar()` marks the car as available again
- `toString()` returns a formatted description of the car

### `CarRentalSystem` class
Manages the collection of cars and the user menu.

- `loadCars()` reads the saved car list from `cars.dat` at startup
- `saveCars()` writes the car list to `cars.dat` after every change
- `addCar(...)` adds a new car to the list and saves
- `rentCar(model)` finds an available car matching the model and rents it
- `displayCars()` prints every car in the fleet
- `main(...)` runs the interactive menu

## Requirements

- Java Development Kit (JDK) 8 or higher

## Running the Program

1. Compile:
   ```bash
   javac CarRentalSystem.java
   ```
2. Run:
   ```bash
   java CarRentalSystem
   ```

## Usage

When the program starts, you will see this menu:

```
1. Add Car
2. Rent Car
3. Display Cars
4. Exit
Choose an option:
```

### Example session

```
Choose an option: 1
Enter model: Civic
Enter brand: Honda
Enter price per day: 45.5

Choose an option: 3

Available Cars:
Brand: Honda, Model: Civic, Price/Day: $45.50, Available: true

Choose an option: 2
Enter model to rent: civic
Car rented successfully.

Choose an option: 4
Exiting...
```

## Known Limitations

- Input is read with `Scanner.next()`, so model and brand names **cannot contain spaces** (e.g. use `Civic`, not `Civic Type R`).
- Entering a non-numeric value at the menu will crash the program with an `InputMismatchException`.
- `returnCar()` exists in the `Car` class but is **not yet exposed in the menu**.
- `displayCars()` shows all cars (rented and available), despite its "Available Cars" heading.
- If you change the `Car` class after saving data, the old `cars.dat` may fail to load. Delete it to start fresh.
- Prices are displayed with a `$` symbol.

## Possible Improvements

- Add a "Return Car" menu option
- Use `Scanner.nextLine()` to support multi-word names
- Validate user input and handle invalid entries gracefully
- Track rental duration and calculate total cost
- Add customer details and rental history

## License

Free to use for learning and personal projects.
