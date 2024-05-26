#bill splitter
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>

struct Person {
    std::string name;
    double amount;

    Person(const std::string& n) : name(n), amount(0.0) {}
};

class BillSplitter {
private:
    std::vector<Person> people;

public:
    void addPerson(const std::string& name) {
        people.push_back(Person(name));
    }

    void removePerson(const std::string& name) {
        for (auto it = people.begin(); it != people.end(); ++it) {
            if (it->name == name) {
                people.erase(it);
                break;
            }
        }
    }

    void recordExpense(const std::string& name, double amount) {
        for (auto& person : people) {
            if (person.name == name) {
                person.amount += amount;
                break;
            }
        }
    }

    void calculateShares() {
        double totalAmount = 0.0;
        for (const auto& person : people) {
            totalAmount += person.amount;
        }

        if (totalAmount == 0.0) {
            std::cout << "No expenses recorded yet.\n";
            return;
        }

        double share = totalAmount / people.size();
        std::cout << "Total amount: $" << std::fixed << std::setprecision(2) << totalAmount << std::endl;
        std::cout << "Share per person: $" << std::fixed << std::setprecision(2) << share << std::endl;

        std::cout << "Individual shares:\n";
        for (const auto& person : people) {
            std::cout << person.name << ": $" << std::fixed << std::setprecision(2) << person.amount << std::endl;
        }
    }

    void displayMenu() {
        std::cout << "\nBill Splitter Menu:\n"
                     "1. Add Person\n"
                     "2. Remove Person\n"
                     "3. Record Expense\n"
                     "4. Calculate Shares\n"
                     "5. Exit\n";
    }

    void processChoice(int choice) {
        switch (choice) {
            case 1: {
                std::string name;
                std::cout << "Enter person's name: ";
                std::cin >> name;
                addPerson(name);
                break;
            }
            case 2: {
                std::string name;
                std::cout << "Enter person's name to remove: ";
                std::cin >> name;
                removePerson(name);
                break;
            }
            case 3: {
                std::string name;
                double amount;
                std::cout << "Enter person's name: ";
                std::cin >> name;
                std::cout << "Enter expense amount: $";
                std::cin >> amount;
                recordExpense(name, amount);
                break;
            }
            case 4:
                calculateShares();
                break;
            case 5:
                std::cout << "Exiting...\n";
                break;
            default:
                std::cout << "Invalid choice. Please try again.\n";
                break;
        }
    }

    void run() {
        int choice;
        do {
            displayMenu();
            std::cout << "Enter your choice: ";
            std::cin >> choice;
            processChoice(choice);
        } while (choice != 5);
    }
};

int main() {
    BillSplitter billSplitter;
    billSplitter.run();
    return 0;
}
