#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>
#include <conio.h> // For getch() function (Windows specific)

#define MAX_USERS 100
#define USERNAME_LENGTH 20
#define PASSWORD_LENGTH 20
#define FILENAME "users.dat"

typedef struct {
    char username[USERNAME_LENGTH];
    char password[PASSWORD_LENGTH];
    int isAdmin;
} User;

User users[MAX_USERS];
int userCount = 0;

// Function prototypes
void loadUsers();
void saveUsers();
void registerUser();
int loginUser();
void adminMenu();
void studentMenu(char *username);
void changePassword(char *username);
int validatePassword(const char *password);
void clearInputBuffer();

int main() {
    loadUsers();
    int choice;
    
    do {
        system("cls"); // Clear screen (Windows specific)
        printf("\n=== College Admission System ===\n");
        printf("1. Register\n");
        printf("2. Login\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        clearInputBuffer();
        
        switch(choice) {
            case 1:
                registerUser();
                break;
            case 2: {
                int userType = loginUser();
                if (userType == 1) {
                    adminMenu();
                } else if (userType == 0) {
                    // Get the username of the logged in student
                    char username[USERNAME_LENGTH];
                    printf("Enter your username: ");
                    fgets(username, USERNAME_LENGTH, stdin);
                    username[strcspn(username, "\n")] = '\0'; // Remove newline
                    studentMenu(username);
                }
                break;
            }
            case 3:
                printf("Exiting program...\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
        }
        printf("\nPress any key to continue...");
        getch();
    } while (choice != 3);
    
    saveUsers();
    return 0;
}

void loadUsers() {
    FILE *file = fopen(FILENAME, "rb");
    if (file == NULL) {
        return; // File doesn't exist yet
    }
    
    userCount = fread(users, sizeof(User), MAX_USERS, file);
    fclose(file);
}

void saveUsers() {
    FILE *file = fopen(FILENAME, "wb");
    if (file == NULL) {
        printf("Error saving user data!\n");
        return;
    }
    
    fwrite(users, sizeof(User), userCount, file);
    fclose(file);
}

void registerUser() {
    if (userCount >= MAX_USERS) {
        printf("Maximum user limit reached!\n");
        return;
    }
    
    User newUser;
    printf("\n=== New User Registration ===\n");
    
    // Get username
    printf("Enter username (max %d chars): ", USERNAME_LENGTH-1);
    fgets(newUser.username, USERNAME_LENGTH, stdin);
    newUser.username[strcspn(newUser.username, "\n")] = '\0'; // Remove newline
    
    // Check if username already exists
    for (int i = 0; i < userCount; i++) {
        if (strcmp(users[i].username, newUser.username) == 0) {
            printf("Username already exists!\n");
            return;
        }
    }
    
    // Get password with validation
    char password[PASSWORD_LENGTH];
    do {
        printf("Enter password (8-20 chars, at least 1 digit, 1 uppercase, 1 lowercase): ");
        // Password input with masking
        int i = 0;
        char ch;
        while (i < PASSWORD_LENGTH-1) {
            ch = getch();
            if (ch == '\r') { // Enter key
                break;
            } else if (ch == '\b' && i > 0) { // Backspace
                printf("\b \b");
                i--;
            } else if (isprint(ch)) {
                password[i++] = ch;
                printf("*");
            }
        }
        password[i] = '\0';
        printf("\n");
        
        if (!validatePassword(password)) {
            printf("Password does not meet requirements!\n");
        }
    } while (!validatePassword(password));
    
    strcpy(newUser.password, password);
    
    // Set user type (admin or student)
    printf("Is this an admin account? (1 for yes, 0 for no): ");
    scanf("%d", &newUser.isAdmin);
    clearInputBuffer();
    
    users[userCount++] = newUser;
    saveUsers();
    printf("\nRegistration successful!\n");
}

int loginUser() {
    char username[USERNAME_LENGTH];
    char password[PASSWORD_LENGTH];
    
    printf("\n=== Login ===\n");
    printf("Username: ");
    fgets(username, USERNAME_LENGTH, stdin);
    username[strcspn(username, "\n")] = '\0'; // Remove newline
    
    printf("Password: ");
    // Password input with masking
    int i = 0;
    char ch;
    while (i < PASSWORD_LENGTH-1) {
        ch = getch();
        if (ch == '\r') { // Enter key
            break;
        } else if (ch == '\b' && i > 0) { // Backspace
            printf("\b \b");
            i--;
        } else if (isprint(ch)) {
            password[i++] = ch;
            printf("*");
        }
    }
    password[i] = '\0';
    printf("\n");
    
    for (int i = 0; i < userCount; i++) {
        if (strcmp(users[i].username, username) == 0 && 
            strcmp(users[i].password, password) == 0) {
            printf("\nLogin successful!\n");
            return users[i].isAdmin;
        }
    }
    
    printf("\nInvalid username or password!\n");
    return -1;
}

void adminMenu() {
    system("cls");
    printf("\n=== Admin Dashboard ===\n");
    printf("1. View all users\n");
    printf("2. Add new admin\n");
    printf("3. Back to main menu\n");
    
    int choice;
    printf("Enter your choice: ");
    scanf("%d", &choice);
    clearInputBuffer();
    
    switch(choice) {
        case 1:
            printf("\n=== Registered Users ===\n");
            for (int i = 0; i < userCount; i++) {
                printf("%d. %s (%s)\n", i+1, users[i].username, 
                       users[i].isAdmin ? "Admin" : "Student");
            }
            break;
        case 2:
            registerUser();
            break;
        case 3:
            break;
        default:
            printf("Invalid choice!\n");
    }
}

void studentMenu(char *username) {
    int choice;
    do {
        system("cls");
        printf("\n=== Student Dashboard ===\n");
        printf("Welcome, %s!\n", username);
        printf("1. Change password\n");
        printf("2. View admission status\n");
        printf("3. Logout\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        clearInputBuffer();
        
        switch(choice) {
            case 1:
                changePassword(username);
                break;
            case 2:
                printf("\nAdmission status: Pending review\n");
                break;
            case 3:
                printf("Logging out...\n");
                break;
            default:
                printf("Invalid choice!\n");
        }
        if (choice != 3) {
            printf("\nPress any key to continue...");
            getch();
        }
    } while (choice != 3);
}

void changePassword(char *username) {
    char newPassword[PASSWORD_LENGTH];
    char currentPassword[PASSWORD_LENGTH];
    
    printf("\n=== Change Password ===\n");
    printf("Enter current password: ");
    
    // Current password input with masking
    int i = 0;
    char ch;
    while (i < PASSWORD_LENGTH-1) {
        ch = getch();
        if (ch == '\r') { // Enter key
            break;
        } else if (ch == '\b' && i > 0) { // Backspace
            printf("\b \b");
            i--;
        } else if (isprint(ch)) {
            currentPassword[i++] = ch;
            printf("*");
        }
    }
    currentPassword[i] = '\0';
    printf("\n");
    
    // Find the user
    int userIndex = -1;
    for (int i = 0; i < userCount; i++) {
        if (strcmp(users[i].username, username) == 0) {
            userIndex = i;
            break;
        }
    }
    
    if (userIndex == -1 || strcmp(users[userIndex].password, currentPassword) != 0) {
        printf("Current password is incorrect!\n");
        return;
    }
    
    // Get new password with validation
    do {
        printf("Enter new password (8-20 chars, at least 1 digit, 1 uppercase, 1 lowercase): ");
        // Password input with masking
        i = 0;
        while (i < PASSWORD_LENGTH-1) {
            ch = getch();
            if (ch == '\r') { // Enter key
                break;
            } else if (ch == '\b' && i > 0) { // Backspace
                printf("\b \b");
                i--;
            } else if (isprint(ch)) {
                newPassword[i++] = ch;
                printf("*");
            }
        }
        newPassword[i] = '\0';
        printf("\n");
        
        if (!validatePassword(newPassword)) {
            printf("Password does not meet requirements!\n");
        }
    } while (!validatePassword(newPassword));
    
    strcpy(users[userIndex].password, newPassword);
    saveUsers();
    printf("\nPassword changed successfully!\n");
}

int validatePassword(const char *password) {
    int length = strlen(password);
    if (length < 8 || length >= PASSWORD_LENGTH) {
        return 0;
    }
    
    int hasUpper = 0, hasLower = 0, hasDigit = 0;
    for (int i = 0; i < length; i++) {
        if (isupper(password[i])) hasUpper = 1;
        if (islower(password[i])) hasLower = 1;
        if (isdigit(password[i])) hasDigit = 1;
    }
    
    return hasUpper && hasLower && hasDigit;
}

void clearInputBuffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}
