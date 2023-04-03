// # rentalpayment.c
//used to keep details of tenants after they make their rental payments

#include <stdio.h>

#include <stdlib.h>

#include <string.h>

#include <stdbool.h>

#define MAX_TENANTS 100

struct tenant {

    char name[50];

    int phone_number;

    float amount_paid;

};

struct tenant tenants[MAX_TENANTS];

int num_tenants = 0;

// function prototypes

void add_tenant();

void delete_tenant();

void update_tenant();

void print_tenant(struct tenant t, int index);

void print_tenants();

void search_tenant();

bool authenticate();

int main() {

    while (1) {

        if (!authenticate()) {

            printf("Authentication failed. Exiting...\n");

            break;

        }

        printf("\nEnter command (add, list, search, delete, update, quit): ");

        char command[10];

        fgets(command, 10, stdin);

        command[strlen(command)-1] = '\0';

        if (strcmp(command, "add") == 0) {

            add_tenant();

        } else if (strcmp(command, "list") == 0) {

            print_tenants();

        } else if (strcmp(command, "search") == 0) {

            search_tenant();

        } else if (strcmp(command, "delete") == 0) {

            delete_tenant();

        } else if (strcmp(command, "update") == 0) {

            update_tenant();

        } else if (strcmp(command, "quit") == 0) {

            break;

        } else {

            printf("Error: invalid command.\n");

        }

    }

    return 0;

}

void add_tenant() {

    if (num_tenants >= MAX_TENANTS) {

        printf("Error: maximum number of tenants reached.\n");

        return;

    }

    struct tenant new_tenant;

    printf("\nEnter tenant name: ");

    fgets(new_tenant.name, 50, stdin);

    new_tenant.name[strlen(new_tenant.name)-1] = '\0';

    printf("Enter tenant phone number: ");

    scanf("%d", &new_tenant.phone_number);

    getchar();  // Discard newline character

    printf("Enter amount paid: ");

    scanf("%f", &new_tenant.amount_paid);

    getchar();  // Discard newline character

    tenants[num_tenants] = new_tenant;

    num_tenants++;

    printf("Tenant added: %s, phone number: %d, amount paid: %.2f\n", new_tenant.name, new_tenant.phone_number, new_tenant.amount_paid);

}

void delete_tenant() {

    int phone_number;

    printf("\nEnter tenant phone number to delete: ");

    scanf("%d", &phone_number);

    getchar();  // Discard newline character

    for (int i = 0; i < num_tenants; i++) {

        if (tenants[i].phone_number == phone_number) {

            print_tenant(tenants[i], i);

            printf("\nAre you sure you want to delete this tenant? (y/n): ");

            char choice[2];

            fgets(choice, 2, stdin);

            if (choice[0] == 'y') {

                for (int j = i; j < num_tenants - 1; j++) {

                    tenants[j] = tenants[j + 1];

                }

            

                num_tenants--;

                printf("Tenant deleted.\n");

            } else {

                printf("Tenant not deleted.\n");

            }

            return;

        }

    }

    printf("Error: tenant not found.\n");

}

void update_tenant() {

    int phone_number;

    printf("\nEnter tenant phone number to update: ");

    scanf("%d", &phone_number);

    getchar();  // Discard newline character

    for (int i = 0; i < num_tenants; i++) {

        if (tenants[i].phone_number == phone_number) {

            print_tenant(tenants[i], i);

            printf("\nWhich field do you want to update? (name, phone_number, amount_paid): ");

            char field[15];

            fgets(field, 15, stdin);

            field[strlen(field)-1] = '\0';

            if (strcmp(field, "name") == 0) {

                printf("Enter new tenant name: ");

                fgets(tenants[i].name, 50, stdin);

                tenants[i].name[strlen(tenants[i].name)-1] = '\0';

                printf("Tenant name updated.\n");

            } else if (strcmp(field, "phone_number") == 0) {

                printf("Enter new tenant phone number: ");

                scanf("%d", &tenants[i].phone_number);

                getchar();  // Discard newline character

                printf("Tenant phone number updated.\n");

            } else if (strcmp(field, "amount_paid") == 0) {

                printf("Enter new amount paid: ");

                scanf("%f", &tenants[i].amount_paid);

                getchar();  // Discard newline character

                printf("Amount paid updated.\n");

            } else {

                printf("Error: invalid field.\n");

            }

            return;

        }

    }

    printf("Error: tenant not found.\n");

}

void print_tenant(struct tenant t, int index) {

    printf("Index: %d, Name: %s, Phone Number: %d, Amount Paid: %.2f\n", index, t.name, t.phone_number, t.amount_paid);

}

void print_tenants() {

    if (num_tenants == 0) {

        printf("No tenants.\n");

        return;

    }

    for (int i = 0; i < num_tenants; i++) {

        print_tenant(tenants[i], i);

    }

}

void search_tenant() {

    char name[50];

    printf("\nEnter tenant name to search: ");

    fgets(name, 50, stdin);

    name[strlen(name)-1] = '\0';

    bool found = false;

    for (int i = 0; i < num_tenants; i++) {

        if (strcmp(tenants[i].name, name) == 0) {

            print_tenant(tenants[i], i);

            found = true;

        }

    }

    if (!found) {

        printf("Tenant not found.\n");

    }

}

bool authenticate() {

    char username[20], password[20];

    printf("\nEnter username: ");

    fgets(username, 20, stdin);

    username[strlen(username)-1] = '\0';

    printf("Enter password: ");

    fgets(password, 20, stdin);

    password[strlen(password)-1] = '\0';

    if (strcmp(username, "Joshua") == 0 && strcmp(password, "5976") == 0) {

        return true;

    } else {

        return false;

    }

}
