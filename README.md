#include <stdio.h>
#include <string.h>
#include <stdlib.h>

void adminPanel();
void userPanel();
void newUserRegistration();
int authenticateUser(char *username, char *password);
void exitProgram();
void addEditTrainSchedule();
void viewTrainSchedule();
void bookTickets();
void viewBookedTickets();
void cancelTickets();

struct Train {
    int trainNumber;
    char source[50];
    char destination[50];
    char departureTime[10];
    double ticketCost;
};

struct User {
    char username[50];
    char password[50];
};

#define MAX_BOOKED_TICKETS 100


struct Train trains[100];
int numTrains = 0;

struct User users[100];
int numUsers = 0;

struct BookedTicket {
    int trainNumber;
    char source[50];
    char destination[50];
    char departureTime[10];
    int numTickets;
};

struct BookedTicket bookedTickets[MAX_BOOKED_TICKETS];
int numBookedTickets = 0;

int main() {
    int choice;

    printf("Train Ticket Booking Software\n");

    while (1) {
        printf("\nDashboard Options:\n");
        printf("1. Admin\n");
        printf("2. User\n");
        printf("3. New User Registration\n");
        printf("0. Exit\n");

        printf("\nEnter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                adminPanel();
                break;
            case 2:
                userPanel();
                break;
            case 3:
                newUserRegistration();
                break;
            case 0:
                exitProgram();
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                break;
        }
    }

    return 0;
}

void adminPanel() {
     char adminPassword[50];
    printf("\nAdmin Login:\n");
    printf("Enter admin password: ");
    scanf("%s", adminPassword);

    if (strcmp(adminPassword, "123") == 0) {
        int adminChoice;

        printf("\nAdmin Panel:\n");
        printf("1. Add/Edit Train Schedule\n");
        printf("2. View Booked Tickets\n");
        printf("3. Logout\n");

        printf("\nEnter your choice: ");
        scanf("%d", &adminChoice);

        switch (adminChoice) {
            case 1:
                addEditTrainSchedule();
                break;
            case 2:
                viewBookedTickets();
                break;
            case 3:
                printf("Logging out from admin panel...\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                break;
        }
    } else {
        printf("Admin authentication failed. Please try again.\n");
    }
    }


void userPanel() {
    char username[50];
    char password[50];

    printf("\nUser Login:\n");
    printf("Enter username: ");
    scanf("%s", username);
    printf("Enter password: ");
    scanf("%s", password);

    if (authenticateUser(username, password)) {
        int userChoice;

        printf("\nUser Panel:\n");
        printf("1. View Train Schedule\n");
        printf("2. Book Tickets\n");
        printf("3. View Booked Tickets\n");
        printf("4. Cancel Tickets\n");
        printf("5. Logout\n");

        printf("\nEnter your choice: ");
        scanf("%d", &userChoice);

        switch (userChoice) {
            case 1:
                viewTrainSchedule();
                break;
            case 2:
                bookTickets();
                break;
            case 3:
                viewBookedTickets();
                break;
            case 4:
                cancelTickets();
                break;
            case 5:
                printf("Logging out from user panel...\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                break;
        }
    } else {
        printf("Authentication failed. Please try again.\n");
    }
}

void newUserRegistration() {
    printf("\nNew User Registration:\n");

    if (numUsers >= 100) {
        printf("User registration limit reached.\n");
        return;
    }

    printf("Enter username: ");
    scanf("%s", users[numUsers].username);
    printf("Enter password: ");
    scanf("%s", users[numUsers].password);

    numUsers++;
    printf("Registration successful.\n");
}

int authenticateUser(char *username, char *password) {
    for (int i = 0; i < numUsers; i++) {
        if (strcmp(users[i].username, username) == 0 && strcmp(users[i].password, password) == 0) {
            return 1;
        }
    }
    return 0;
}

void exitProgram() {
    printf("Exiting...\n");
    exit(0);
}


void addEditTrainSchedule() {
    if (numTrains >= 100) {
        printf("Train schedule limit reached.\n");
        return;
    }

    printf("Enter train number: ");
    scanf("%d", &trains[numTrains].trainNumber);
    printf("Enter source: ");
    scanf("%s", trains[numTrains].source);
    printf("Enter destination: ");
    scanf("%s", trains[numTrains].destination);
    printf("Enter departure time: ");
    scanf("%s", trains[numTrains].departureTime);
    printf("Enter ticket cost: ");
    scanf("%lf", &trains[numTrains].ticketCost);  // Read the ticket cost

    numTrains++;
    printf("Train schedule added/edited successfully.\n");
}

void viewTrainSchedule() {
    if (numTrains == 0) {
        printf("No trains available in the schedule.\n");
        return;
    }

    printf("\nTrain Schedule:\n");
    printf("Train No.\tSource\t\tDestination\tDeparture Time\n");
    for (int i = 0; i < numTrains; i++) {
        printf("%d\t\t%s\t\t%s\t\t%s\n", trains[i].trainNumber, trains[i].source, trains[i].destination, trains[i].departureTime);
    }
}

void bookTickets() {
    int selectedTrain;
    int numTickets;

    printf("\nAvailable Trains:\n");
    printf("Train No.\tSource\t\tDestination\tDeparture Time\n");
    for (int i = 0; i < numTrains; i++) {
        printf("%d\t\t%s\t\t%s\t\t%s\n", trains[i].trainNumber, trains[i].source, trains[i].destination, trains[i].departureTime);
    }

    printf("Enter the train number you want to book tickets for: ");
    scanf("%d", &selectedTrain);

    int trainIndex = -1;
    for (int i = 0; i < numTrains; i++) {
        if (trains[i].trainNumber == selectedTrain) {
            trainIndex = i;
             printf("Enter the number of tickets you want to book: ");
    scanf("%d", &numTickets);
    double totalCost = numTickets * trains[trainIndex].ticketCost;
    char confirm;
    printf("Total Cost: %.2lf\n", totalCost);
    printf("Confirm booking and payment? (Y/N): ");
    scanf(" %c", &confirm);

    if (confirm == 'Y' || confirm == 'y') {
        printf("Payment successful! Booking %d tickets for Train %d from %s to %s at %s.\nYour tickets are booked now.\n",
               numTickets, trains[trainIndex].trainNumber, trains[trainIndex].source, trains[trainIndex].destination,
               trains[trainIndex].departureTime);

    } else {
        printf("Booking and payment cancelled.\n");
    }
            break;
        }
    }

    if (trainIndex == -1) {
        printf("Invalid train number. Please try again.\n");
        return;
    }

    printf("Booking %d tickets for Train %d from %s to %s at %s.\n Booking successful,your tickets are booked now ", numTickets, trains[trainIndex].trainNumber,
           trains[trainIndex].source, trains[trainIndex].destination, trains[trainIndex].departureTime);

}


void viewBookedTickets() {


    printf("\nBooked Tickets:\n");
    printf("Train No.\tSource\t\tDestination\tDeparture Time\tNo. of Tickets\n");
    for (int i = 0; i < numBookedTickets; i++) {
        printf("%d\t\t%s\t\t%s\t\t%s\t\t%d\n", bookedTickets[i].trainNumber, bookedTickets[i].source,
               bookedTickets[i].destination, bookedTickets[i].departureTime, bookedTickets[i].numTickets);
    }
     if (numBookedTickets == 0) {
        printf("No booked tickets available.\n");
        return;
    }
}

void cancelTickets() {
    int trainNumber;
    int numTicketsToCancel;

  viewBookedTickets();

    printf("Enter the train number for which you want to cancel tickets: ");
    scanf("%d", &trainNumber);

    int ticketIndex = -1;
    for (int i = 0; i < numBookedTickets; i++) {
        if (bookedTickets[i].trainNumber == trainNumber) {
            ticketIndex = i;
            break;
        }
    }

    if (ticketIndex == -1) {
        printf("No booked tickets found for Train %d.\n", trainNumber);
        return;
    }

    printf("Booked Ticket Details:\n");
    printf("Train No.\tSource\t\tDestination\tDeparture Time\tNo. of Tickets\n");
    printf("%d\t\t%s\t\t%s\t\t%s\t\t%d\n", bookedTickets[ticketIndex].trainNumber, bookedTickets[ticketIndex].source,
           bookedTickets[ticketIndex].destination, bookedTickets[ticketIndex].departureTime, bookedTickets[ticketIndex].numTickets);

    printf("Enter the number of tickets you want to cancel: ");
    scanf("%d", &numTicketsToCancel);

    if (numTicketsToCancel <= 0 || numTicketsToCancel > bookedTickets[ticketIndex].numTickets) {
        printf("Invalid number of tickets to cancel.\n");
        return;
    }
    bookedTickets[ticketIndex].numTickets -= numTicketsToCancel;

    if (bookedTickets[ticketIndex].numTickets == 0) {
        for (int i = ticketIndex; i < numBookedTickets - 1; i++) {
            bookedTickets[i] = bookedTickets[i + 1];
        }
        numBookedTickets--;
    }

    printf("%d tickets canceled for Train %d.\n", numTicketsToCancel, trainNumber);
}
