# DSA-Project
#include <iostream> 
#include <string> 
using namespace std; 
struct Customer { 
int tokenNumber; 
string name; 
bool isVIP; 
Customer* next; 
}; 
struct HistoryNode { 
string name; 
int tokenNumber; 
HistoryNode* next; 
 70147176 
}; 
 
class BankFlow { 
private: 
    Customer *front, *rear; 
    HistoryNode* historyTop; 
    int tokenCounter; 
 
public: 
    BankFlow() { 
        front = rear = nullptr; 
        historyTop = nullptr; 
        tokenCounter = 100; 
    } 
 
    ~BankFlow() { 
        while (front != nullptr) { 
            Customer* temp = front; 
            front = front->next; 
            delete temp; 
        } 
        while (historyTop != nullptr) { 
            HistoryNode* temp = historyTop; 
            historyTop = historyTop->next; 
            delete temp; 
        } 
    } 
 
    void pushHistory(string name, int token) { 
        HistoryNode* newNode = new HistoryNode(); 
        newNode->name = name; 
        newNode->tokenNumber = token; 
        newNode->next = historyTop; 
        historyTop = newNode; 
    } 
 
    void addCustomer(string name, bool priority) { 
        Customer* newNode = new Customer(); 
        newNode->name = name; 
        newNode->tokenNumber = ++tokenCounter; 
        newNode->isVIP = priority; 
        newNode->next = nullptr; 
 
        if (front == nullptr) { 
            front = rear = newNode; 
        }  
        else if (priority) { 
            newNode->next = front; 
            front = newNode; 
        }  
        else { 
            rear->next = newNode; 
            rear = newNode; 
        } 
        cout << "\n[SUCCESS] Token #" << newNode->tokenNumber << " issued to " << name 
<< endl; 
    } 
 
    void serveCustomer() { 
        if (front == nullptr) { 
            cout << "\n[!] No customers in line.\n"; 
            return; 
        } 
 
        Customer* temp = front; 
        cout << "\n[SERVING] Token #" << temp->tokenNumber << ": " << temp->name << endl; 
 
        pushHistory(temp->name, temp->tokenNumber); 
 
        front = front->next; 
        if (front == nullptr) rear = nullptr; 
        delete temp; 
    } 
 
    void searchCustomer(int token) { 
        Customer* temp = front; 
        int pos = 1; 
        while (temp != nullptr) { 
            if (temp->tokenNumber == token) { 
                cout << "\n[FOUND] " << temp->name << " is at position " << pos << " in the 
queue.\n"; 
                return; 
            } 
            temp = temp->next; 
            pos++; 
        } 
        cout << "\n[!] Token not found in active queue.\n"; 
    } 
 
    void displayQueue() { 
        if (front == nullptr) { 
            cout << "\nQueue is empty.\n"; 
            return; 
        } 
        Customer* temp = front; 
        cout << "\n--- CURRENT QUEUE ---\n"; 
        while (temp != nullptr) { 
            cout << (temp->isVIP ? "[VIP] " : "[REG] ") << "Token " << temp->tokenNumber << ": " 
<< temp->name << endl; 
            temp = temp->next; 
        } 
    } 
 
    void displayHistory() { 
        if (historyTop == nullptr) { 
            cout << "\nNo history available.\n"; 
            return; 
        } 
        HistoryNode* temp = historyTop; 
        cout << "\n--- SERVING HISTORY ---\n"; 
        while (temp != nullptr) { 
            cout << "Token " << temp->tokenNumber << ": " << temp->name << endl; 
            temp = temp->next; 
        } 
    } 
}; 
 
int main() { 
    BankFlow bank; 
    int choice, searchToken; 
    string name; 
    char vipChoice; 
 
    do { 
        cout << "\n============================"; 
        cout << "\n   BANK TOKEN SYSTEM (DSA)  "; 
        cout << "\n============================"; 
        cout << "\n1. Issue Token"; 
        cout << "\n2. Serve Customer"; 
        cout << "\n3. View Queue"; 
        cout << "\n4. Search Token"; 
        cout << "\n5. View History"; 
        cout << "\n6. Exit"; 
        cout << "\nChoice: "; 
        cin >> choice; 
 
        switch (choice) { 
            case 1: 
                cout << "Enter Name: "; 
                cin >> name; 
                cout << "Is VIP? (y/n): "; 
                cin >> vipChoice; 
                bank.addCustomer(name, (vipChoice == 'y' || vipChoice == 'Y')); 
                break; 
            case 2: 
                bank.serveCustomer(); 
                break; 
            case 3: 
                bank.displayQueue(); 
                break; 
            case 4: 
                cout << "Enter Token Number: "; 
                cin >> searchToken; 
                bank.searchCustomer(searchToken); 
                break; 
            case 5: 
                bank.displayHistory(); 
break; 
} 
} while (choice != 6); 
return 0;
