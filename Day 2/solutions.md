# **🌟 OOPS Concepts Interview Guide Solutions**

#### Day 2 - 22-08-2024

---

### **Problem 1: Hotel Management System (Inheritance, Polymorphism, Design Patterns) 🏨**

**Scenario:**  
You are tasked with building a hotel management system that deals with different types of rooms (like Deluxe, Suite, and Standard) and services (like Food Service and Cleaning Service). Each room has unique properties (like room size, amenities) and services. Implement this system with proper use of **inheritance** and **polymorphism**. Also, use a **design pattern** to manage the allocation of rooms efficiently.

#### Requirements:

- Implement the `Room` base class and derive specific room types (`Deluxe`, `Suite`, `Standard`).
- Implement a `Service` base class and derive specific service types (`FoodService`, `CleaningService`).
- Use the **Factory Design Pattern** for creating room objects based on the room type requested by the user.

🛠️ **Ensure**:

- Each room type has its unique properties.
- Services are applied **polymorphically** to different room types.
- Room allocation is handled by a `RoomFactory`.

---

## **Solution Explanation**

### **C++ Implementation**

```cpp
#include <iostream>
#include <memory>
#include <vector>
using namespace std;

// Abstract Room class
class Room {
public:
    virtual void getDetails() = 0;  // Abstract method
    virtual void applyService(const string &service) = 0;  // Abstract method
    virtual ~Room() = default;
};

// Derived Room classes
class DeluxeRoom : public Room {
public:
    void getDetails() override {
        cout << "Deluxe Room: Large room with a king-size bed and Jacuzzi.\n";
    }

    void applyService(const string &service) override {
        cout << service << " is being applied to the Deluxe Room.\n";
    }
};

class SuiteRoom : public Room {
public:
    void getDetails() override {
        cout << "Suite Room: Large living area with luxury facilities.\n";
    }

    void applyService(const string &service) override {
        cout << service << " is being applied to the Suite Room.\n";
    }
};

class StandardRoom : public Room {
public:
    void getDetails() override {
        cout << "Standard Room: A compact room with basic amenities.\n";
    }

    void applyService(const string &service) override {
        cout << service << " is being applied to the Standard Room.\n";
    }
};

// Factory Design Pattern for Room allocation
class RoomFactory {
public:
    static shared_ptr<Room> allocateRoom(const string &roomType) {
        if (roomType == "Deluxe") {
            return make_shared<DeluxeRoom>();
        } else if (roomType == "Suite") {
            return make_shared<SuiteRoom>();
        } else if (roomType == "Standard") {
            return make_shared<StandardRoom>();
        }
        return nullptr;
    }
};

int main() {
    vector<shared_ptr<Room>> hotelRooms;

    // Allocating rooms using the factory
    hotelRooms.push_back(RoomFactory::allocateRoom("Deluxe"));
    hotelRooms.push_back(RoomFactory::allocateRoom("Suite"));
    hotelRooms.push_back(RoomFactory::allocateRoom("Standard"));

    // Applying services to all rooms
    for (auto &room : hotelRooms) {
        room->getDetails();
        room->applyService("Cleaning Service");
    }

    return 0;
}
```

**Explanation:**

- **Inheritance and Polymorphism:** The `Room`class is an abstract base class with pure virtual methods. `DeluxeRoom`, `SuiteRoom`, and `StandardRoom` derive from `Room`, implementing specific details and service applications.
- **Factory Design Pattern:** `RoomFactory` provides a static method `allocateRoom` to instantiate the appropriate room based on the `roomType` parameter.This centralizes room creation logic and abstracts the instantiation process.

---

### **Java Implementation**

```java
// Abstract Room class
abstract class Room {
    abstract void getDetails();
    abstract void applyService(String service);
}

// DeluxeRoom class
class DeluxeRoom extends Room {
    void getDetails() {
        System.out.println("Deluxe Room: Large room with king-size bed and Jacuzzi.");
    }

    void applyService(String service) {
        System.out.println(service + " is being applied to the Deluxe Room.");
    }
}

// SuiteRoom class
class SuiteRoom extends Room {
    void getDetails() {
        System.out.println("Suite Room: Large living area with luxury facilities.");
    }

    void applyService(String service) {
        System.out.println(service + " is being applied to the Suite Room.");
    }
}

// StandardRoom class
class StandardRoom extends Room {
    void getDetails() {
        System.out.println("Standard Room: A compact room with basic amenities.");
    }

    void applyService(String service) {
        System.out.println(service + " is being applied to the Standard Room.");
    }
}

// Factory for Room creation
class RoomFactory {
    public static Room allocateRoom(String roomType) {
        if (roomType.equalsIgnoreCase("Deluxe")) {
            return new DeluxeRoom();
        } else if (roomType.equalsIgnoreCase("Suite")) {
            return new SuiteRoom();
        } else if (roomType.equalsIgnoreCase("Standard")) {
            return new StandardRoom();
        }
        return null;
    }
}

public class Main {
    public static void main(String[] args) {
        Room[] hotelRooms = new Room[3];

        hotelRooms[0] = RoomFactory.allocateRoom("Deluxe");
        hotelRooms[1] = RoomFactory.allocateRoom("Suite");
        hotelRooms[2] = RoomFactory.allocateRoom("Standard");

        for (Room room : hotelRooms) {
            room.getDetails();
            room.applyService("Cleaning Service");
        }
    }
}
```

**Explanation:**

- **Inheritance and Polymorphism:** The `Room` abstract class defines the interface for all room types. `DeluxeRoom`, `SuiteRoom`, and `StandardRoom` extend `Room`, providing specific details and services.
- **Factory Design Pattern:** `RoomFactory` uses a static method `allocateRoom` to return instances of the appropriate `roomType` based on the roomType string, encapsulating the creation logic.

---

### **Python Implementation**

```python
from abc import ABC, abstractmethod

# Abstract Room class
class Room(ABC):
    @abstractmethod
    def get_details(self):
        pass

    @abstractmethod
    def apply_service(self, service):
        pass

# DeluxeRoom class
class DeluxeRoom(Room):
    def get_details(self):
        print("Deluxe Room: Large room with king-size bed and Jacuzzi.")

    def apply_service(self, service):
        print(f"{service} is being applied to the Deluxe Room.")

# SuiteRoom class
class SuiteRoom(Room):
    def get_details(self):
        print("Suite Room: Large living area with luxury facilities.")

    def apply_service(self, service):
        print(f"{service} is being applied to the Suite Room.")

# StandardRoom class
class StandardRoom(Room):
    def get_details(self):
        print("Standard Room: A compact room with basic amenities.")

    def apply_service(self, service):
        print(f"{service} is being applied to the Standard Room.")

# Factory for Room creation
class RoomFactory:
    @staticmethod
    def allocate_room(room_type):
        if room_type == "Deluxe":
            return DeluxeRoom()
        elif room_type == "Suite":
            return SuiteRoom()
        elif room_type == "Standard":
            return StandardRoom()
        return None

# Main simulation
hotel_rooms = [RoomFactory.allocate_room("Deluxe"),
               RoomFactory.allocate_room("Suite"),
               RoomFactory.allocate_room("Standard")]

for room in hotel_rooms:
    room.get_details()
    room.apply_service("Cleaning Service")
```

**Explanation:**

- **Inheritance and Polymorphism:** The `Room` class is an abstract base class with abstract methods. `DeluxeRoom`, `SuiteRoom`, and `StandardRoom` implement these methods, providing specific details and service applications.
- **Factory Design Pattern:** `RoomFactory` uses a static method `allocate_room` to create instances of different room types based on the room_type string, simplifying the room creation process.

---

### **Problem 2: Vehicle Fleet Management System (Abstraction, Interface, Strategy Pattern) 🚚**

**Scenario:**  
Develop a vehicle fleet management system for a logistics company. The company manages different types of vehicles (like **Truck**, **Van**, **Motorbike**) to deliver different types of packages (like fragile goods, electronics, perishable items). Each vehicle uses different **delivery strategies** based on their capacities and the type of package. Implement this system using **interfaces**, **inheritance**, and the **Strategy Design Pattern** to define the delivery behavior dynamically.

#### Requirements:

- Define a `Vehicle` interface with abstract methods for delivering packages.
- Create `Truck`, `Van`, and `Motorbike` classes that implement the `Vehicle` interface.
- Implement different **delivery strategies** for fragile and perishable goods.
- Use the **Strategy Pattern** to assign different delivery behaviors based on the type of goods.

---

### **C++ Implementation**

```cpp
#include <iostream>
#include <memory>
#include <string>
using namespace std;

// Strategy interface
class DeliveryStrategy {
public:
    virtual void deliver(const string &goods) = 0;
    virtual ~DeliveryStrategy() = default;
};

// Concrete Strategies for delivery
class FragileDelivery : public DeliveryStrategy {
public:
    void deliver(const string &goods) override {
        cout << "Delivering fragile goods: " << goods << " with extra care.\n";
    }
};

class PerishableDelivery : public DeliveryStrategy {
public:
    void deliver(const string &goods) override {
        cout << "Delivering perishable goods: " << goods << " quickly before it spoils.\n";
    }
};

// Abstract Vehicle class (interface)
class Vehicle {
protected:
    unique_ptr<DeliveryStrategy> deliveryStrategy;

public:
    virtual void deliverGoods(const string &goods) = 0;
    virtual ~Vehicle() = default;

    void setDeliveryStrategy(unique_ptr<DeliveryStrategy> strategy) {
        deliveryStrategy = move(strategy);
    }
};

// Concrete Vehicles
class Truck : public Vehicle {
public:
    void deliverGoods(const string &goods) override {
        cout << "Truck is assigned for delivery.\n";
        deliveryStrategy->deliver(goods);
    }
};

class Van : public Vehicle {
public:
    void deliverGoods(const string &goods) override {
        cout << "Van is assigned for delivery.\n";
        deliveryStrategy->deliver(goods);
    }
};

class Motorbike : public Vehicle {
public:
    void deliverGoods(const string &goods) override {
        cout << "Motorbike is assigned for delivery.\n";
        deliveryStrategy->deliver(goods);
    }
};

// Main simulation
int main() {
    Truck truck;
    Van van;
    Motorbike motorbike;

    // Set and use strategies
    truck.setDeliveryStrategy(make_unique<FragileDelivery>());
    truck.deliverGoods("Glassware");

    van.setDeliveryStrategy(make_unique<PerishableDelivery>());
    van.deliverGoods("Fresh Vegetables");

    motorbike.setDeliveryStrategy(make_unique<FragileDelivery>());
    motorbike.deliverGoods("Electronics");

    return 0;
}
```

1. **Strategy Pattern Implementation:**

   - The `DeliveryStrategy` interface defines a common method `deliver` for different types of delivery behaviors.
   - `FragileDelivery` and `PerishableDelivery` are concrete implementations of this strategy, each implementing the `deliver` method to handle goods in specific ways.

2. **Vehicle Interface:**

   - The `Vehicle` class is an abstract base class with a pure virtual function `deliverGoods`. This ensures that all derived classes must implement this function, adhering to the `Vehicle` interface.
   - The `deliveryStrategy` pointer is used to dynamically assign a delivery behavior to each vehicle.

3. **Concrete Vehicles:**

   - `Truck`, `Van`, and `Motorbike` classes inherit from `Vehicle` and implement the `deliverGoods` function. Each vehicle uses the strategy assigned to it to deliver goods.
   - The `setDeliveryStrategy` function allows assigning different delivery behaviors at runtime, demonstrating the Strategy Pattern in action.

4. **Main Simulation:**
   - In the `main` function, instances of `Truck`, `Van`, and `Motorbike` are created.
   - Delivery strategies are assigned to these vehicles using the `setDeliveryStrategy` function.
   - Each vehicle then delivers goods using its assigned strategy, showcasing polymorphism and dynamic behavior assignment.

---

### **Java Implementation**

```java
// Strategy interface
interface DeliveryStrategy {
    void deliver(String goods);
}

// Concrete Strategies for delivery
class FragileDelivery implements DeliveryStrategy {
    public void deliver(String goods) {
        System.out.println("Delivering fragile goods: " + goods + " with extra care.");
    }
}

class PerishableDelivery implements DeliveryStrategy {
    public void deliver(String goods) {
        System.out.println("Delivering perishable goods: " + goods + " quickly before it spoils.");
    }
}

// Abstract Vehicle class (interface)
abstract class Vehicle {
    protected DeliveryStrategy deliveryStrategy;

    public void setDeliveryStrategy(DeliveryStrategy strategy) {
        this.deliveryStrategy = strategy;
    }

    abstract void deliverGoods(String goods);
}

// Concrete Vehicles
class Truck extends Vehicle {
    @Override
    void deliverGoods(String goods) {
        System.out.println("Truck is assigned for delivery.");
        deliveryStrategy.deliver(goods);
    }
}

class Van extends Vehicle {
    @Override
    void deliverGoods(String goods) {
        System.out.println("Van is assigned for delivery.");
        deliveryStrategy.deliver(goods);
    }
}

class Motorbike extends Vehicle {
    @Override
    void deliverGoods(String goods) {
        System.out.println("Motorbike is assigned for delivery.");
        deliveryStrategy.deliver(goods);
    }
}

// Main simulation
public class Main {
    public static void main(String[] args) {
        Truck truck = new Truck();
        Van van = new Van();
        Motorbike motorbike = new Motorbike();

        // Set and use strategies
        truck.setDeliveryStrategy(new FragileDelivery());
        truck.deliverGoods("Glassware");

        van.setDeliveryStrategy(new PerishableDelivery());
        van.deliverGoods("Fresh Vegetables");

        motorbike.setDeliveryStrategy(new FragileDelivery());
        motorbike.deliverGoods("Electronics");
    }
}
```

1. **Strategy Interface:**

   - The `DeliveryStrategy` interface defines a common method `deliver` that all concrete strategies must implement.

2. **Concrete Strategy Implementations:**

   - `FragileDelivery` and `PerishableDelivery` are classes that implement the `DeliveryStrategy` interface, each providing specific behavior for delivering goods.

3. **Abstract Vehicle Class:**

   - The `Vehicle` abstract class includes an abstract method `deliverGoods` and a `DeliveryStrategy` object, allowing the strategy to be set at runtime.

4. **Concrete Vehicle Classes:**

   - `Truck`, `Van`, and `Motorbike` classes extend the `Vehicle` class and implement the `deliverGoods` method. Each vehicle uses the assigned delivery strategy to perform its task.

5. **Main Simulation:**
   - In the `main` method, instances of `Truck`, `Van`, and `Motorbike` are created and assigned specific delivery strategies.
   - The vehicles then deliver goods using the assigned strategies, demonstrating how the Strategy Pattern allows flexible behavior assignment.

---

### **Python Implementation**

```python
from abc import ABC, abstractmethod

# Strategy interface
class DeliveryStrategy(ABC):
    @abstractmethod
    def deliver(self, goods):
        pass

# Concrete Strategies for delivery
class FragileDelivery(DeliveryStrategy):
    def deliver(self, goods):
        print(f"Delivering fragile goods: {goods} with extra care.")

class PerishableDelivery(DeliveryStrategy):
    def deliver(self, goods):
        print(f"Delivering perishable goods: {goods} quickly before it spoils.")

# Abstract Vehicle class (interface)
class Vehicle(ABC):
    def __init__(self):
        self.delivery_strategy = None

    def set_delivery_strategy(self, strategy):
        self.delivery_strategy = strategy

    @abstractmethod
    def deliver_goods(self, goods):
        pass

# Concrete Vehicles
class Truck(Vehicle):
    def deliver_goods(self, goods):
        print("Truck is assigned for delivery.")
        self.delivery_strategy.deliver(goods)

class Van(Vehicle):
    def deliver_goods(self, goods):
        print("Van is assigned for delivery.")
        self.delivery_strategy.deliver(goods)

class Motorbike(Vehicle):
    def deliver_goods(self, goods):
        print("Motorbike is assigned for delivery.")
        self.delivery_strategy.deliver(goods)

# Main simulation
truck = Truck()
van = Van()
motorbike = Motorbike()

# Set and use strategies
truck.set_delivery_strategy(FragileDelivery())
truck.deliver_goods("Glassware")

van.set_delivery_strategy(PerishableDelivery())
van.deliver_goods("Fresh Vegetables")

motorbike.set_delivery_strategy(FragileDelivery())
motorbike.deliver_goods("Electronics")
```

1. **Strategy Interface:**

   - `DeliveryStrategy` is an abstract base class (ABC) that defines a common method `deliver` to be implemented by all concrete strategies.

2. **Concrete Strategy Implementations:**

   - `FragileDelivery` and `PerishableDelivery` are concrete classes that implement the `DeliveryStrategy` interface, providing specific delivery behaviors.

3. **Abstract Vehicle Class:**

   - `Vehicle` is an abstract class that holds a `delivery_strategy` and has an abstract method `deliver_goods` that must be implemented by derived classes.

4. **Concrete Vehicle Classes:**
   - `Truck`, `Van`, and `Motorbike` classes extend the `Vehicle` class and implement the `deliver_goods` method, using its assigned strategy, showcasing how the Strategy Pattern enables dynamic behavior assignment.

### Summary of Design Patterns Used

1. **Strategy Pattern**: This pattern is used to define a family of algorithms (delivery strategies) and make them interchangeable. In this case, `DeliveryStrategy` is the interface for different delivery behaviors, and `FragileDelivery` and `PerishableDelivery` are specific implementations.

2. **Abstraction**: `Vehicle` serves as an abstract class (or interface) with the method `deliver_goods` that must be implemented by its subclasses (`Truck`, `Van`, and `Motorbike`). This ensures that all vehicles follow a common interface while allowing different behaviors through the Strategy Pattern.

3. **Inheritance**: `Truck`, `Van`, and `Motorbike` inherit from `Vehicle`, which allows them to share common functionality while implementing specific behaviors.

### Implementation Notes

- **C++**: Uses smart pointers (`unique_ptr`) for managing strategy instances, which simplifies memory management and avoids manual deletion.
- **Java**: Uses abstract classes and interfaces for defining strategies and vehicles. Java's built-in garbage collection handles memory management.

- **Python**: Uses abstract base classes (ABCs) to define the strategy interface and ensure that concrete classes implement the required methods. Python's dynamic typing and memory management features make it straightforward to use.

Feel free to ask if you need further clarifications or additional features for your Vehicle Fleet Management System!

---
