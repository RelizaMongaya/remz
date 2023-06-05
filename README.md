#include <stdio.h>

#include <stdlib.h>

#include <string.h>

#define MAX_ITEMS 100

#define MAX_NAME_LENGTH 50

#define FILE_NAME "inventory.csv"

typedef struct {

    char name[MAX_NAME_LENGTH];

    double price;

    int quantity;

} Item;

Item inventory[MAX_ITEMS];

int numItems = 0;

void displayMenu() {

    printf("\n=== Inventory Management System ===\n");

    printf("1. Add Item\n");

    printf("2. Display Inventory\n");

    printf("3. Change Item Price\n");

    printf("4. Exit\n");

    printf("===================================\n");

    printf("Enter your choice: ");

}

void addItem() {

    if (numItems >= MAX_ITEMS) {

        printf("Inventory is full. Cannot add more items.\n");

        return;

    }

    

    Item newItem;

    

    printf("Enter item name: ");

    scanf("%s", newItem.name);

    

    printf("Enter item price: ");

    scanf("%lf", &newItem.price);

    

    printf("Enter item quantity: ");

    scanf("%d", &newItem.quantity);

    

    inventory[numItems] = newItem;

    numItems++;

    

    printf("Item added successfully!\n");

}

void displayInventory() {

    if (numItems == 0) {

        printf("Inventory is empty.\n");

        return;

    }

    

    printf("=== Inventory ===\n");

    

    for (int i = 0; i < numItems; i++) {

        printf("Name: %s\n", inventory[i].name);

        printf("Price: %.2lf\n", inventory[i].price);

        printf("Quantity: %d\n", inventory[i].quantity);

        printf("-----------------\n");

    }

}

void changeItemPrice() {

    if (numItems == 0) {

        printf("Inventory is empty.\n");

        return;

    }

    

    char itemName[MAX_NAME_LENGTH];

    double newPrice;

    

    printf("Enter the name of the item: ");

    scanf("%s", itemName);

    

    printf("Enter the new price: ");

    scanf("%lf", &newPrice);

    

    for (int i = 0; i < numItems; i++) {

        if (strcmp(itemName, inventory[i].name) == 0) {

            inventory[i].price = newPrice;

            printf("Price changed successfully!\n");

            return;

        }

    }

    

    printf("Item not found in inventory.\n");

}

void saveInventoryToFile() {

    FILE *file = fopen(FILE_NAME, "w");

    

    if (file == NULL) {

        printf("Error opening file.\n");

        return;

    }

    

    for (int i = 0; i < numItems; i++) {

        fprintf(file, "%s,%.2lf,%d\n", inventory[i].name, inventory[i].price, inventory[i].quantity);

    }

    

    fclose(file);

    printf("Inventory saved to file.\n");

}

void loadInventoryFromFile() {

    FILE *file = fopen(FILE_NAME, "r");

    

    if (file == NULL) {

        printf("No inventory file found.\n");

        return;

    }

    

    numItems = 0;

    

    while (fscanf(file, "%[^,],%lf,%d\n", inventory[numItems].name, &inventory[numItems].price, &inventory[numItems].quantity) == 3) {

        numItems++;

    }

    

    fclose(file);

    printf("Inventory loaded from file.\n");

}

int main() {

    int choice;

    

    loadInventoryFromFile();

    

    do {

        displayMenu();

        scanf("%d", &choice);

        

        switch (choice) {

            case 1:

                addItem();

                break;

            case 2:

                displayInventory();

                break;

            case 3:

                changeItemPrice();

                break;

            case 4:

                saveInventoryToFile();

                break;

            default:

                printf("Invalid choice. Please try again.\n");

                break;

        }

    } while (choice != 4);

    

    return 0;

}
