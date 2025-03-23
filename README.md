// Student Management System
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <conio.h>  // For getch() to hide password input

#define MAX_STUDENTS 100

// Student structure
struct Student {
    int id;
    char fullName[100];
    int age;
    char course[50];
    char address[100];
    char dob[15];  // Format: DD-MM-YYYY
    char contactNumber[15];
};

// Global variables
struct Student students[MAX_STUDENTS];
int student_count = 0;

// Function prototypes
void login();
void mainMenu();
void addStudent();
void displayStudents();
void deleteStudent();
void searchStudent();
void modifyStudent();
void clearScreen();

// Function to clear screen
void clearScreen() {
    system("cls");  // For Windows
    // system("clear"); // Uncomment for Linux/Mac
}

// Login function with hidden password
void login() {
    char username[20], password[20], ch;
    int i = 0;

    printf("================================================================\n");
    printf("          Welcome to the Student Management System! \n");
    printf("================================================================\n");

    while (1) {
        printf("\nEnter Username: ");
        scanf("%s", username);

        printf("Enter Password: ");
        i = 0;
        while (1) {
            ch = getch();
            if (ch == 13) { // Enter key
                password[i] = '\0';
                break;
            } else if (ch == 8 && i > 0) { // Backspace handling
                printf("\b \b");
                i--;
            } else {
                password[i++] = ch;
                printf("*");
            }
        }

        if (strcmp(username, "isha") == 0 && strcmp(password, "1234") == 0) {
            printf("\n\nLogin successful!\n");
            break;
        } else {
            printf("\n\nInvalid username or password. Try again!\n");
        }
    }
}

// Main menu function
void mainMenu() {
    int choice;

    while (1) {
        printf("\n**********************************\n");
        printf("*  STUDENT MANAGEMENT SYSTEM  *\n");
        printf("\n**********************************\n");
        printf("1. Add New Records\n");
        printf("2. Display All Students Records\n");
        printf("3. Delete Records\n");
        printf("4. Search and View Records\n");
        printf("5. Modify Records\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        clearScreen();

        switch (choice) {
            case 1:
                addStudent();
                break;
            case 2:
                displayStudents();
                break;
            case 3:
                deleteStudent();
                break;
            case 4:
                searchStudent();
                break;
            case 5:
                modifyStudent();
                break;
            case 6:
                printf("Exiting program...\n");
                exit(0);
            default:
                printf("Invalid choice! Please try again.\n");
        }
    }
}

// Function to add a student
void addStudent() {
    if (student_count >= MAX_STUDENTS) {
        printf("Cannot add more students! Database is full.\n");
        return;
    }

    struct Student s;
    printf("Enter Student ID: ");
    scanf("%d", &s.id);
    getchar();  // Clear buffer

    printf("Enter Full Name: ");
    fgets(s.fullName, sizeof(s.fullName), stdin);
    s.fullName[strcspn(s.fullName, "\n")] = '\0';

    printf("Enter Age: ");
    scanf("%d", &s.age);

    getchar();  // Clear buffer
    printf("Enter Course: ");
    fgets(s.course, sizeof(s.course), stdin);
    s.course[strcspn(s.course, "\n")] = '\0';

    printf("Enter Address: ");
    fgets(s.address, sizeof(s.address), stdin);
    s.address[strcspn(s.address, "\n")] = '\0';

    printf("Enter Date of Birth (DD-MM-YYYY): ");
    fgets(s.dob, sizeof(s.dob), stdin);
    s.dob[strcspn(s.dob, "\n")] = '\0';

    printf("Enter Contact Number: ");
    fgets(s.contactNumber, sizeof(s.contactNumber), stdin);
    s.contactNumber[strcspn(s.contactNumber, "\n")] = '\0';

    students[student_count++] = s;
    printf("Student record added successfully!\n");
}

// Function to display all students
void displayStudents() {
    if (student_count == 0) {
        printf("No student records available.\n");
        return;
    }

    printf("\n-----------------------------------------------------------------------------------------------------------\n");
    printf("| %-5s | %-20s | %-3s | %-15s | %-30s | %-15s | %-15s |\n", "ID", "Name", "Age", "Course", "Address", "DOB", "Contact No.");
    printf("-----------------------------------------------------------------------------------------------------------\n");
    for (int i = 0; i < student_count; i++) {
        printf("| %-5d | %-20s | %-3d | %-15s | %-30s | %-15s | %-15s |\n",
               students[i].id, students[i].fullName, students[i].age, students[i].course, students[i].address, students[i].dob, students[i].contactNumber);
    }
    printf("-----------------------------------------------------------------------------------------------------------\n");
}

// Function to delete a student
void deleteStudent() {
    int id, found = 0;
    printf("Enter Student ID to delete: ");
    scanf("%d", &id);

    for (int i = 0; i < student_count; i++) {
        if (students[i].id == id) {
            for (int j = i; j < student_count - 1; j++) {
                students[j] = students[j + 1];
            }
            student_count--;
            found = 1;
            printf("Student record deleted successfully!\n");
            break;
        }
    }

    if (!found) {
        printf("Student ID not found!\n");
    }
}

// Function to search for a student
void searchStudent() {
    int id, found = 0;
    printf("Enter Student ID to search: ");
    scanf("%d", &id);

    for (int i = 0; i < student_count; i++) {
        if (students[i].id == id) {
            printf("\nStudent Found!\n");
            printf("\n-----------------------------------------------------------------------------------------------------------\n");
            printf("| %-5s | %-20s | %-3s | %-15s | %-30s | %-15s | %-15s |\n", "ID", "Name", "Age", "Course", "Address", "DOB", "Contact No.");
            printf("-----------------------------------------------------------------------------------------------------------\n");
            printf("| %-5d | %-20s | %-3d | %-15s | %-30s | %-15s | %-15s |\n",
                   students[i].id, students[i].fullName, students[i].age, students[i].course, students[i].address, students[i].dob, students[i].contactNumber);
            printf("-----------------------------------------------------------------------------------------------------------\n");
            found = 1;
            break;
        }
    }

    if (!found) {
        printf("Student ID not found!\n");
    }
}

// Function to modify a student record
void modifyStudent() {
    int id, found = 0;
    printf("Enter Student ID to modify: ");
    scanf("%d", &id);

    for (int i = 0; i < student_count; i++) {
        if (students[i].id == id) {
            printf("Enter new Full Name: ");
            getchar();  // Clear buffer
            fgets(students[i].fullName, sizeof(students[i].fullName), stdin);
            students[i].fullName[strcspn(students[i].fullName, "\n")] = '\0';

            printf("Enter new Age: ");
            scanf("%d", &students[i].age);

            printf("Enter new Course: ");
            getchar();  // Clear buffer
            fgets(students[i].course, sizeof(students[i].course), stdin);
            students[i].course[strcspn(students[i].course, "\n")] = '\0';

            printf("Enter new Address: ");
            fgets(students[i].address, sizeof(students[i].address), stdin);
            students[i].address[strcspn(students[i].address, "\n")] = '\0';

            printf("Enter new Date of Birth (DD-MM-YYYY): ");
            fgets(students[i].dob, sizeof(students[i].dob), stdin);
            students[i].dob[strcspn(students[i].dob, "\n")] = '\0';

            printf("Enter new Contact Number: ");
            fgets(students[i].contactNumber, sizeof(students[i].contactNumber), stdin);
            students[i].contactNumber[strcspn(students[i].contactNumber, "\n")] = '\0';
			     break;
        }
    }

    if (!found) {
        printf("Student ID not found!\n");
    }
}

// Main function
int main() {
    login();
    clearScreen();
    mainMenu();
    return 0;
}
