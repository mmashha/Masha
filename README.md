# Masha
Объяснение кода:

1. Классы:

   • Passenger: представляет пассажира с его именем, пунктом назначения, датой и временем поездки.

   • Train: представляет поезд с номером поезда, пунктом назначения, временем отправления и ценой.

   • TicketSystem: управляет списком пассажиров и поездов, позволяет добавлять поезда и пассажиров, осуществлять поиск поездов и генерировать счета.

2. Основная логика:

   • В main создается экземпляр TicketSystem, в который добавляются несколько поездов.

   • Пользователь может выбрать различные опции: поиск поездов, бронирование билета или просмотр зарегистрированных пассажиров.

   • При бронировании билета программа ищет подходящий поезд по заданному пункту назначения и отображает доступные варианты.

3. Пользовательский интерфейс:

   • Программа предлагает пользователю меню с выбором действий и обрабатывает ввод пользователя.

▎Примечания:

• Программа написана для работы в консольном режиме.

• Для упрощения кода не использованы механизмы обработки ошибок (например, неверный ввод данных).

• Вы можете расширить функционал программы, добавив дополнительные возможности, такие как удаление пассажиров, изменение данных о поездах и т.д.
#include <iostream>
#include <vector>
#include <string>

using namespace std;

class Passenger {
public:
    string name;
    string destination;
    string travelDate;
    string travelTime;

    Passenger(string n, string dest, string date, string time)
        : name(n), destination(dest), travelDate(date), travelTime(time) {}
};

class Train {
public:
    string trainNumber;
    string destination;
    string departureTime;
    double price;

    Train(string number, string dest, string depTime, double p)
        : trainNumber(number), destination(dest), departureTime(depTime), price(p) {}
};

class TicketSystem {
private:
    vector<Passenger> passengers;
    vector<Train> trains;

public:
    void addTrain(const Train& train) {
        trains.push_back(train);
    }

    void addPassenger(const Passenger& passenger) {
        passengers.push_back(passenger);
    }

    void searchTrains(const string& destination) {
        cout << "Available trains to " << destination << ":\n";
        for (const auto& train : trains) {
            if (train.destination == destination) {
                cout << "Train Number: " << train.trainNumber
                     << ", Departure Time: " << train.departureTime
                     << ", Price: $" << train.price << endl;
            }
        }
    }

    void generateInvoice(const Passenger& passenger, const Train& train) {
        cout << "\nInvoice for " << passenger.name << ":\n";
        cout << "Destination: " << passenger.destination << endl;
        cout << "Travel Date: " << passenger.travelDate << endl;
        cout << "Travel Time: " << passenger.travelTime << endl;
        cout << "Train Number: " << train.trainNumber << endl;
        cout << "Price: $" << train.price << endl;
        cout << "Thank you for your purchase!" << endl;
    }

    void showPassengers() {
        cout << "\nRegistered Passengers:\n";
        for (const auto& passenger : passengers) {
            cout << "Name: " << passenger.name
                 << ", Destination: " << passenger.destination
                 << ", Travel Date: " << passenger.travelDate
                 << ", Travel Time: " << passenger.travelTime << endl;
        }
    }
};

int main() {
    TicketSystem ticketSystem;

    // Добавление поездов
    ticketSystem.addTrain(Train("A123", "CityA", "10:00 AM", 50.0));
    ticketSystem.addTrain(Train("B456", "CityB", "11:00 AM", 75.0));
    ticketSystem.addTrain(Train("C789", "CityA", "12:00 PM", 60.0));

    while (true) {
        cout << "\n1. Search for trains\n2. Book a ticket\n3. Show registered passengers\n4. Exit\n";
        int choice;
        cin >> choice;

        if (choice == 1) {
            string destination;
            cout << "Enter destination: ";
            cin >> destination;
            ticketSystem.searchTrains(destination);
        } else if (choice == 2) {
            string name, destination, travelDate, travelTime, trainNumber;
            cout << "Enter your name: ";
            cin >> name;
            cout << "Enter destination: ";
            cin >> destination;
            cout << "Enter travel date (YYYY-MM-DD): ";
            cin >> travelDate;
            cout << "Enter travel time (HH:MM): ";
            cin >> travelTime;

            // Поиск подходящего поезда
            ticketSystem.searchTrains(destination);

            cout << "Enter the train number you want to book: ";
            cin >> trainNumber;

            // Находим поезд по номеру
            for (const auto& train : ticketSystem.trains) {
                if (train.trainNumber == trainNumber && train.destination == destination) {
                    Passenger passenger(name, destination, travelDate, travelTime);
                    ticketSystem.addPassenger(passenger);
                                        ticketSystem.generateInvoice(passenger, train);
                    break;
                }
            }

        } else if (choice == 3) {
            ticketSystem.showPassengers();
        } else if (choice == 4) {
            break;
        } else {
            cout << "Invalid choice. Please try again.\n";
        }
    }

    return 0;
}
