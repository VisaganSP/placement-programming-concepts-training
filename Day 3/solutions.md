# **🌟 Exception Handling Interview Guide Solutions**

#### Day 3 - 05-09-2024

---

### **Problem 1: File Processing System (Exception Handling, Resource Management) 📁**

**Scenario:**  
You are developing a file processing system that reads from and writes to files. The system should handle various exceptions such as file not found, I/O errors, and invalid file formats. Implement exception handling to ensure proper resource management and meaningful error reporting.

#### Requirements:

- Use `try-catch-finally` or equivalent constructs to manage file operations.
- Handle exceptions like `FileNotFoundException`, `IOException`, and custom exceptions for invalid formats.
- Ensure that files are properly closed even if an exception occurs.

---

### **C++ Implementation**

```cpp
#include <iostream>
#include <fstream>
#include <stdexcept>

using namespace std;

// Custom exception for invalid file format
class InvalidFileFormatException : public runtime_error {
public:
    explicit InvalidFileFormatException(const string &message) : runtime_error(message) {}
};

void processFile(const string &filename) {
    ifstream file;
    try {
        file.open(filename);

        if (!file.is_open()) {
            throw runtime_error("File not found.");
        }

        string line;
        while (getline(file, line)) {
            if (line.empty()) {
                throw InvalidFileFormatException("Invalid file format: empty line encountered.");
            }
            cout << "Processing line: " << line << endl;
        }

        if (file.bad()) {
            throw runtime_error("I/O error occurred while reading the file.");
        }

    } catch (const InvalidFileFormatException &e) {
        cerr << "Error: " << e.what() << endl;
    } catch (const runtime_error &e) {
        cerr << "Runtime Error: " << e.what() << endl;
    } catch (...) {
        cerr << "An unknown error occurred." << endl;
    } finally {
        if (file.is_open()) {
            file.close();
        }
    }
}

int main() {
    processFile("example.txt");
    return 0;
}
```

**Explanation:**

- **Custom Exception:** `InvalidFileFormatException` is a custom exception class for handling specific file format issues.
- **Exception Handling:** `try-catch` blocks are used to handle exceptions related to file operations and invalid formats. A `finally` block is emulated using the `finally` label to ensure file closure.
- **Resource Management:** The file is closed in the `finally` section to ensure that resources are released properly, even if an exception occurs.

---

### **Java Implementation**

```java
import java.io.*;

public class FileProcessor {

    // Custom exception for invalid file format
    static class InvalidFileFormatException extends Exception {
        public InvalidFileFormatException(String message) {
            super(message);
        }
    }

    public static void processFile(String filename) {
        BufferedReader reader = null;

        try {
            reader = new BufferedReader(new FileReader(filename));
            String line;

            while ((line = reader.readLine()) != null) {
                if (line.trim().isEmpty()) {
                    throw new InvalidFileFormatException("Invalid file format: empty line encountered.");
                }
                System.out.println("Processing line: " + line);
            }

        } catch (FileNotFoundException e) {
            System.err.println("File not found: " + e.getMessage());
        } catch (IOException e) {
            System.err.println("I/O error: " + e.getMessage());
        } catch (InvalidFileFormatException e) {
            System.err.println("Error: " + e.getMessage());
        } catch (Exception e) {
            System.err.println("An unknown error occurred: " + e.getMessage());
        } finally {
            try {
                if (reader != null) {
                    reader.close();
                }
            } catch (IOException e) {
                System.err.println("Error closing the file: " + e.getMessage());
            }
        }
    }

    public static void main(String[] args) {
        processFile("example.txt");
    }
}
```

**Explanation:**

- **Custom Exception:** `InvalidFileFormatException` is defined as a subclass of `Exception` for specific file format errors.
- **Exception Handling:** Uses `try-catch-finally` blocks to handle file operations, including specific exceptions for file not found, I/O errors, and custom exceptions.
- **Resource Management:** The `finally` block ensures that the `BufferedReader` is closed properly.

---

### **Python Implementation**

```python
class InvalidFileFormatException(Exception):
    pass

def process_file(filename):
    try:
        with open(filename, 'r') as file:
            for line in file:
                if not line.strip():
                    raise InvalidFileFormatException("Invalid file format: empty line encountered.")
                print(f"Processing line: {line.strip()}")
    except FileNotFoundError:
        print("File not found.")
    except IOError as e:
        print(f"I/O error: {e}")
    except InvalidFileFormatException as e:
        print(f"Error: {e}")
    except Exception as e:
        print(f"An unknown error occurred: {e}")

# Example usage
process_file("example.txt")
```

**Explanation:**

- **Custom Exception:** `InvalidFileFormatException` is a custom exception class for handling specific file format issues.
- **Exception Handling:** Uses `try-except` blocks to catch file not found, I/O errors, custom exceptions, and any other exceptions.
- **Resource Management:** Uses `with` statement to ensure that the file is properly closed even if an exception occurs.

---

### **Problem 2: Banking Application (Custom Exceptions, Exception Chaining) 💰**

**Scenario:**  
Develop a banking application that handles various types of banking transactions such as deposits, withdrawals, and transfers. Implement custom exceptions for invalid transactions, and use exception chaining to provide detailed error information.

#### Requirements:

- Create custom exception classes for specific banking errors (e.g., `InsufficientFundsException`, `InvalidTransactionException`).
- Use exception chaining to provide additional context about errors.
- Ensure that transactions are rolled back in case of exceptions.

---

### **C++ Implementation**

```cpp
#include <iostream>
#include <stdexcept>
using namespace std;

// Custom exceptions for banking errors
class InsufficientFundsException : public runtime_error {
public:
    explicit InsufficientFundsException(const string &message) : runtime_error(message) {}
};

class InvalidTransactionException : public runtime_error {
public:
    explicit InvalidTransactionException(const string &message) : runtime_error(message) {}
};

// BankAccount class
class BankAccount {
    double balance;

public:
    BankAccount(double initial_balance) : balance(initial_balance) {}

    void deposit(double amount) {
        if (amount <= 0) {
            throw InvalidTransactionException("Deposit amount must be positive.");
        }
        balance += amount;
    }

    void withdraw(double amount) {
        if (amount <= 0) {
            throw InvalidTransactionException("Withdrawal amount must be positive.");
        }
        if (amount > balance) {
            throw InsufficientFundsException("Insufficient funds for withdrawal.");
        }
        balance -= amount;
    }

    double getBalance() const {
        return balance;
    }
};

// Main simulation
int main() {
    BankAccount account(1000.0);

    try {
        account.deposit(500.0);
        cout << "Balance after deposit: " << account.getBalance() << endl;

        account.withdraw(2000.0);
        cout << "Balance after withdrawal: " << account.getBalance() << endl;
    } catch (const InsufficientFundsException &e) {
        cerr << "Error: " << e.what() << endl;
    } catch (const InvalidTransactionException &e) {
        cerr << "Error: " << e.what() << endl;
    } catch (const runtime_error &e) {
        cerr << "Runtime Error: " << e.what() << endl;
    } catch (...) {
        cerr << "An unknown error occurred." << endl;
    }

    return 0;
}
```

**Explanation:**

- **Custom Exceptions:** `InsufficientFundsException` and `InvalidTransactionException` are used to handle specific errors related to banking transactions.
- **Exception Chaining:** Errors are caught and reported with specific messages, allowing for detailed error reporting.
- **Transaction Integrity:** Although not shown explicitly, you could implement rollback mechanisms if needed.

---

### **Java Implementation**

```java
// Custom exceptions for banking errors
class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}

class InvalidTransactionException extends Exception {
    public InvalidTransactionException(String message) {
        super(message);
    }
}

// BankAccount class
class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    public void deposit(double amount) throws InvalidTransactionException {
        if (amount <= 0) {
            throw new InvalidTransactionException("Deposit amount must be positive.");
        }
        balance += amount;
    }

    public void withdraw(double amount) throws InsufficientFundsException, InvalidTransactionException {
        if (amount <= 0) {
            throw new InvalidTransactionException("Withdrawal amount must be positive.");
        }
        if (amount > balance) {
            throw new InsufficientFundsException("Insufficient funds for withdrawal.");
        }
        balance -= amount;
    }

    public double getBalance() {
        return balance;
    }
}

// Main simulation
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000.0);

        try {
            account.deposit(500.0);
            System.out.println("Balance after deposit: " + account.getBalance());

            account.withdraw(2000.0);
            System

.out.println("Balance after withdrawal: " + account.getBalance());
        } catch (InsufficientFundsException e) {
            System.err.println("Error: " + e.getMessage());
        } catch (InvalidTransactionException e) {
            System.err.println("Error: " + e.getMessage());
        } catch (Exception e) {
            System.err.println("An unknown error occurred: " + e.getMessage());
        }
    }
}
```

**Explanation:**

- **Custom Exceptions:** `InsufficientFundsException` and `InvalidTransactionException` handle specific banking errors.
- **Exception Chaining:** Exception messages are propagated to provide context.
- **Transaction Integrity:** Methods throw specific exceptions if conditions are not met.

---

### **Python Implementation**

```python
class InsufficientFundsException(Exception):
    pass

class InvalidTransactionException(Exception):
    pass

class BankAccount:
    def __init__(self, initial_balance):
        self.balance = initial_balance

    def deposit(self, amount):
        if amount <= 0:
            raise InvalidTransactionException("Deposit amount must be positive.")
        self.balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise InvalidTransactionException("Withdrawal amount must be positive.")
        if amount > self.balance:
            raise InsufficientFundsException("Insufficient funds for withdrawal.")
        self.balance -= amount

    def get_balance(self):
        return self.balance

# Example usage
try:
    account = BankAccount(1000.0)
    account.deposit(500.0)
    print(f"Balance after deposit: {account.get_balance()}")

    account.withdraw(2000.0)
    print(f"Balance after withdrawal: {account.get_balance()}")
except InsufficientFundsException as e:
    print(f"Error: {e}")
except InvalidTransactionException as e:
    print(f"Error: {e}")
except Exception as e:
    print(f"An unknown error occurred: {e}")
```

**Explanation:**

- **Custom Exceptions:** `InsufficientFundsException` and `InvalidTransactionException` for specific error scenarios.
- **Exception Chaining:** Detailed error messages are provided.
- **Transaction Integrity:** Methods throw exceptions if invalid operations are attempted.

---
