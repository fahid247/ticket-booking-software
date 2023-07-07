#include <stdio.h>

void adminDashboard() {
    printf("Admin Dashboard\");
}

void userDashboard() {
    printf("User Dashboard\");
}

void newUserDashboard() {
    printf("New User Dashboard\");
}

int main() {
    int choice;
    
    printf("Train Ticket Management Program\");
    
    while (1) {
        printf("\Dashboard Options:\");
        printf("1. Admin\");
        printf("2. User\");
        printf("3. New User\");
        printf("0. Exit\");
        
        printf("\Enter your choice: ");
        scanf("%d", &choice);
        
        switch (choice) {
            case 1:
                adminDashboard();
                break;
            case 2:
                userDashboard();
                break;
            case 3:
                newUserDashboard();
                break;
            case 0:
                printf("Exiting...\");
                return 0;
            default:
                printf("Invalid choice. Please try again.\");
                break;
        }
    }
    
    return 0;
}
