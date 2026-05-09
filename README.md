// bank management system

#include <iostream>
#include <fstream>
using namespace std;

class BankAccount {
private:
    int accNo;
    char name[50];
    float balance;

public:
    void createAccount() {
        cout << "Enter Account Number: ";
        cin >> accNo;

        cout << "Enter Name: ";
        cin >> name;

        cout << "Enter Initial Balance: ";
        cin >> balance;
    }

    void showAccount() {
        cout << "\nAccount Number: " << accNo;
        cout << "\nName: " << name;
        cout << "\nBalance: " << balance << endl;
    }

    void deposit() {
        float amt;
        cout << "Enter amount to deposit: ";
        cin >> amt;
        balance += amt;

        cout << "Amount Deposited Successfully.\n";
    }

    void withdraw() {
        float amt;
        cout << "Enter amount to withdraw: ";
        cin >> amt;

        if (amt <= balance) {
            balance -= amt;
            cout << "Withdrawal Successful.\n";
        }
        else {
            cout << "Insufficient Balance.\n";
        }
    }

    int getAccNo() {
        return accNo;
    }

    void writeToFile() {
        ofstream file("bank.dat", ios::binary | ios::app);
        file.write((char*)this, sizeof(*this));
        file.close();
    }

    void readFromFile(int num) {
        ifstream file("bank.dat", ios::binary);

        while (file.read((char*)this, sizeof(*this))) {
            if (accNo == num) {
                showAccount();
                return;
            }
        }

        cout << "Account not found.\n";
        file.close();
    }
};

int main() {
    BankAccount b;
    int choice, acc;

    do {
        cout << "\n===== BANK MANAGEMENT SYSTEM =====";
        cout << "\n1. Create Account";
        cout << "\n2. Deposit";
        cout << "\n3. Withdraw";
        cout << "\n4. Balance Inquiry";
        cout << "\n5. Exit";

        cout << "\nEnter Choice: ";
        cin >> choice;

        switch(choice) {

        case 1:
            b.createAccount();
            b.writeToFile();
            break;

        case 2:
            b.deposit();
            break;

        case 3:
            b.withdraw();
            break;

        case 4:
            cout << "Enter Account Number: ";
            cin >> acc;
            b.readFromFile(acc);
            break;

        case 5:
            cout << "Thank You.\n";
            break;

        default:
            cout << "Invalid Choice.\n";
        }

    } while(choice != 5);

    return 0;
}