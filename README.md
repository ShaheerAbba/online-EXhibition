//# online-EXhibition
//this is the project on online Exhibition .It has one base class and two derived classes.It is the project of OOP c++.
#include <iostream>
#include <string>

using namespace std;
class Exhibition {
protected:
    string name ;
    string releaseDate;
    int duration;
    double ticketPrice;
    bool seats[50]; // an array to cheak the seat avaikability

public:
    Exhibition (string n, string rd, int d, double tp)
        : name(n), releaseDate(rd), duration(d), ticketPrice(tp) {
        for (int i = 0; i < 50; i++) {
            seats[i] = true; // Initially, all seats are available
        }
    }
    virtual void displayDetails() {
        cout << "Name: " << name << endl;
        cout << "Release Date: " << releaseDate << endl;
        cout << "Duration: " << duration << " minutes" << endl;
        cout << "Ticket Price: $" << ticketPrice << endl;
    }

    virtual string getName() {
        return name;
    }

    virtual double getTicketPrice() {
        return ticketPrice;
    }

    virtual void changeTicketPrice(double newPrice) {
        ticketPrice = newPrice;
        cout << "Ticket price changed successfully!" << endl;
    }

    void displayAvailableSeats() {
        cout << "Available seats for " << name << ":" << endl;
        for (int i = 0; i < 50; i++) {
            if (seats[i]) {
                cout << "Seat " << i + 1 << " is available" << endl;
            }
        }
    }

    bool bookSeat(int seatNumber) {
        if (seatNumber >= 1 && seatNumber <= 50) {
            if (seats[seatNumber - 1]) {
                seats[seatNumber - 1] = false; 
                return true;
            } else {
                cout << "Seat " << seatNumber << " is already reserved. Please choose another seat." << endl;
                return false;
            }
        } else {
            cout << "Invalid seat number. Please choose a seat between 1 and 50." << endl;
            return false;
        }
    }
};

class UserClass : public Exhibition {
public:
    UserClass(string n, string rd, int d, double tp)
        : Exhibition (n, rd, d, tp) {}

    void displayDetails() override {
        cout << "=== Exhibition Details ===" << endl;
        Exhibition::displayDetails();
    }
};

class AdminClass : public Exhibition {
public:
    AdminClass(string n, string rd, int d, double tp)
        : Exhibition (n, rd, d, tp) {}

    void displayDetails() override {
        cout << "=== Exhibition Details ===" << endl;
        Exhibition::displayDetails();
    }
};

int main() {
    const string adminPassword = "abbas";
    const int maxExhibitions= 3;

    Exhibition* exhibitions[maxExhibitions];
    int numExhibitions = 0;

    int choice;
    do {

        cout << "\n=== Online Exhibition Booking System ===" << endl;
        cout << "1. Add new Exhibition (admin only)" << endl;
        cout << "2. Delete Exhibition (admin only)" << endl;
        cout << "3. Change ticket price (admin only)" << endl;
        cout << "4. Currently playing online  Exhibition list" << endl;
        cout << "5. Available seats list" << endl;
        cout << "6. Book a ticket" << endl;
        cout << "7. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                cout << "Enter the password: ";
                string password;
                cin >> password;
                if (password == adminPassword) {
                    if (numExhibitions >= maxExhibitions) {
                        cout << "Maximum number of online Ehibition reached. Delete old Exhibitions before adding new ones." << endl;
                    } else {
                        string name, releaseDate;
                        int duration;
                        double ticketPrice;
                        cout << "Enter the name of the exhibition: ";
                        cin.ignore();
                        getline(cin, name);
                        cout << "Enter the release date: ";
                        cin.ignore();
                        getline(cin, releaseDate);
                        cout << "Enter the duration (in minutes): ";
                        cin >> duration;
                        cout << "Enter the ticket price: ";
                        cin >> ticketPrice;

                        exhibitions [numExhibitions] = new AdminClass(name, releaseDate, duration, ticketPrice);
                        numExhibitions++;
                        cout << "Exhibition added successfully!" << endl;
                    }
                } else {
                    cout << "Invalid password. Please try again." << endl;
                }
                break;
            }
            case 2: {
                cout << "Enter the password: ";
                string password;
                cin >> password;
                if (password == adminPassword) {
                    string name;
                    cout << "Enter the exhibition name to delete: ";
                    cin.ignore();
                    getline(cin, name);

                    bool found = false;    
                    for (int i = 0; i < numExhibitions; i++) {
                        if (exhibitions[i]->getName() == name) {
                            delete exhibitions[i];
                            exhibitions [i] = exhibitions [numExhibitions - 1];
                            numExhibitions--;
                            found = true;
                            break;
                        }
                    }

                    if (found) {
                        cout << "Exhibition deleted successfully!" << endl;
                    } else {
                        cout << "Exhibition with this name is not found. Please try again." << endl;
                    }
                } else {
                    cout << "Invalid password. Please try again." << endl;
                }
                break;
            }
            case 3: {
                cout << "Enter the password: ";
                string password;
                cin >> password;
                if (password == adminPassword) {
                    string name;
                    cout << "Enter the exhibition name to change ticket price: ";
                    cin.ignore();
                    getline(cin, name);

                    bool found = false;
                    for (int i = 0; i < numExhibitions; i++) {
                        if (exhibitions [i]->getName () == name) {
                            double newPrice;
                            cout << "Enter the new ticket price: ";
                            cin >> newPrice;
                            exhibitions [i]->changeTicketPrice(newPrice);
                            found = true;
                            break;
                        }
                    }

                    if (!found) {
                        cout << "Exhibition with this name is not found. Please try again." << endl;
                    }
                } else {
                    cout << "Invalid password. Please try again." << endl;
                }
                break;
            }
            case 4: {
                cout << "=== Currently Playing Online Exhibition ===" << endl;
                for (int i = 0; i < numExhibitions; i++) {
                    exhibitions [i]->displayDetails();
                    cout << endl;
                }
                break;
            }
            case 5: {
                string name;
                cout << "Enter the exhibition name to display available seats: ";
                cin.ignore();
                getline(cin, name);

                bool found = false;
                for (int i = 0; i < numExhibitions; i++) {
                    if (exhibitions[i]->getName () == name) {
                        exhibitions [i]->displayAvailableSeats();
                        found = true;
                        break;
                    }
                }

                if (!found) {
                    cout << "Exhibition with this name is not found. Please try again." << endl;
                }
                break;
            }
            case 6: {
            	int totalbill=0;
			    string name;
			    cout << "Enter the exhibition name: ";
			    cin.ignore();
			    getline(cin, name);
			
			    string Name;
			    cout << "Enter your name: ";
			    getline(cin, Name);
			
			    string contact;
			    cout << "Enter your contact number: ";
			    getline(cin, contact);
			
			    bool found = false;
			    for (int i = 0; i < numExhibitions; i++) {
			        if (exhibitions [i]->getName() == name) {
			            exhibitions [i]->displayAvailableSeats();
			              
			            int numSeats;
			            cout << "Enter the number of seats you want to book: ";
			            cin >> numSeats;
			            
			            int seatNumber[50];
			            bool bookingSuccessful = true; // Initialize bookingSuccessful to true
			           
			            for (int j = 0; j < numSeats; j++) {
			                cout << "Enter seat number " << j + 1 << ": ";
			                cin >> seatNumber[j];
			                
							if (! exhibitions [i]->bookSeat(seatNumber[j])) {
			                    bookingSuccessful = false; 
			                    
			                    break; 
			                }
			            }
			
			            if (bookingSuccessful) {
			                cout << "Dear " << Name << ", your ticket is booked successfully." << endl;
			                cout << "Exhibition: " << name << endl;
			                cout << "Seat numbers: ";
			                for (int j = 0; j < numSeats; j++) {
			                    cout << seatNumber[j] << " ";
			                    totalbill+=seatNumber[j];
			                }
			                cout << endl;
			                cout << "Contact: " << contact << endl;
			                cout<<"total bill of booking is $ "<<totalbill<<endl;
			            } else {
			                cout << "Seat booking failed. Please try again." << endl;
			            }
			
			            found = true;
			           
			            break;
			        }
			    }
			
			    if (!found) {
			        cout << "Exhibition with this name is not found. Please try again." << endl;
			    }
			    break;
			}
            case 7: {
                for (int i = 0; i < numExhibitions; i++) {
                    delete exhibitions [i];
                }
                cout << "Exiting the program." << endl;
                break;
            }
            default:
                cout << "Invalid choice. Please try again." << endl;
                break;
        }
    } while (choice != 7);

    return 0;
}
