#include <iostream>
#include <string>
using namespace std;

// Structure to store expense details
struct Expense {
    int id;
    string description;
    double amount;
    Expense* next;
};

// Class to manage the list of expenses
class BillSplitter {
private:
    Expense* head;
    int expenseCount;

public:
    BillSplitter() : head(nullptr), expenseCount(0) {}

    // Add a new expense
    void addExpense(const string& description, double amount) {
        Expense* newExpense = new Expense;
        newExpense->id = ++expenseCount;
        newExpense->description = description;
        newExpense->amount = amount;
        newExpense->next = head;
        head = newExpense;
        cout << "Expense added successfully!" << endl;
    }

    // Remove an expense by id
    void removeExpense(int id) {
        if (!head) {
            cout << "No expenses to remove!" << endl;
            return;
        }

        Expense* temp = head;
        Expense* prev = nullptr;

        // If head node itself holds the id to be deleted
        if (temp != nullptr && temp->id == id) {
            head = temp->next; // Changed head
            delete temp;       // Free old head
            cout << "Expense removed successfully!" << endl;
            return;
        }

        // Search for the id to be deleted, keep track of the previous node
        while (temp != nullptr && temp->id != id) {
            prev = temp;
            temp = temp->next;
        }

        // If id was not present in linked list
        if (temp == nullptr) {
            cout << "Expense not found!" << endl;
            return;
        }

        // Unlink the node from linked list
        prev->next = temp->next;
        delete temp; // Free memory
        cout << "Expense removed successfully!" << endl;
    }

    // Update an existing expense by id
    void updateExpense(int id, const string& newDescription, double newAmount) {
        Expense* temp = head;

        while (temp != nullptr) {
            if (temp->id == id) {
                temp->description = newDescription;
                temp->amount = newAmount;
                cout << "Expense updated successfully!" << endl;
                return;
            }
            temp = temp->next;
        }

        cout << "Expense not found!" << endl;
    }

    // View all expenses
    void viewExpenses() const {
        if (!head) {
            cout << "No expenses to display!" << endl;
            return;
        }

        Expense* temp = head;
        while (temp != nullptr) {
            cout << "ID: " << temp->id << ", Description: " << temp->description << ", Amount: $" << temp->amount << endl;
            temp = temp->next;
        }
    }

    // Calculate total expenses
    double calculateTotal() const {
        double total = 0.0;
        Expense* temp = head;

        while (temp != nullptr) {
            total += temp->amount;
            temp = temp->next;
        }

        return total;
    }

    // Split the bill among a given number of people
    void splitBill(int numberOfPeople) const {
        if (numberOfPeople <= 0) {
            cout << "Number of people must be greater than 0!" << endl;
            return;
        }

        double total = calculateTotal();
        double share = total / numberOfPeople;
        cout << "Total expenses: $" << total << endl;
        cout << "Each person should pay: $" << share << endl;
    }

    // Destructor to clean up the linked list
    ~BillSplitter() {
        Expense* current = head;
        while (current != nullptr) {
            Expense* next = current->next;
            delete current;
            current = next;
        }
    }
};

// Main function to demonstrate the bill splitter functionalities
int main() {
    BillSplitter billSplitter;
    int choice;
    int id;
    string description;
    double amount;
    int numberOfPeople;

    do {
        cout << "\nBill Splitter Menu:" << endl;
        cout << "1. Add Expense" << endl;
        cout << "2. Remove Expense" << endl;
        cout << "3. Update Expense" << endl;
        cout << "4. View Expenses" << endl;
        cout << "5. Split Bill" << endl;
        cout << "6. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter description: ";
                cin.ignore();
                getline(cin, description);
                cout << "Enter amount: ";
                cin >> amount;
                billSplitter.addExpense(description, amount);
                break;
            case 2:
                cout << "Enter expense ID to remove: ";
                cin >> id;
                billSplitter.removeExpense(id);
                break;
            case 3:
                cout << "Enter expense ID to update: ";
                cin >> id;
                cout << "Enter new description: ";
                cin.ignore();
                getline(cin, description);
                cout << "Enter new amount: ";
                cin >> amount;
                billSplitter.updateExpense(id, description, amount);
                break;
            case 4:
                billSplitter.viewExpenses();
                break;
            case 5:
                cout << "Enter number of people: ";
                cin >> numberOfPeople;
                billSplitter.splitBill(numberOfPeople);
                break;
            case 6:
                cout << "Exiting..." << endl;
                break;
            default:
                cout << "Invalid choice! Please try again." << endl;
        }
    } while (choice != 6);

    return 0;
}
