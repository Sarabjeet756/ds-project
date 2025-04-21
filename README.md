#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Book {
    int id;
    char title[100];
    char author[100];
    int available; // 1 = available, 0 = borrowed
    struct Book* next;
} Book;

Book* head = NULL;

Book* createBook(int id, char title[], char author[]) {
    Book* newBook = (Book*)malloc(sizeof(Book));
    newBook->id = id;
    strcpy(newBook->title, title);
    strcpy(newBook->author, author);
    newBook->available = 1;
    newBook->next = NULL;
    return newBook;
}

void addBook() {
    int id;
    char title[100], author[100];
    printf("Enter Book ID: ");
    scanf("%d", &id);
    printf("Enter Title: ");
    getchar(); // flush
    fgets(title, sizeof(title), stdin);
    title[strcspn(title, "\n")] = '\0';
    printf("Enter Author: ");
    fgets(author, sizeof(author), stdin);
    author[strcspn(author, "\n")] = '\0';

    Book* newBook = createBook(id, title, author);
    newBook->next = head;
    head = newBook;
    printf("Book added successfully!\n");
}

void displayBooks() {
    Book* temp = head;
    if (!temp) {
        printf("No books available.\n");
        return;
    }
    while (temp) {
        printf("\nID: %d\nTitle: %s\nAuthor: %s\nStatus: %s\n", temp->id, temp->title, temp->author, temp->available ? "Available" : "Borrowed");
        temp = temp->next;
    }
}

void searchBook() {
    char title[100];
    printf("Enter title to search: ");
    getchar();
    fgets(title, sizeof(title), stdin);
    title[strcspn(title, "\n")] = '\0';
    Book* temp = head;
    while (temp) {
        if (strcmp(temp->title, title) == 0) {
            printf("Book found: ID %d, Author: %s, Status: %s\n", temp->id, temp->author, temp->available ? "Available" : "Borrowed");
            return;
        }
        temp = temp->next;
    }
    printf("Book not found.\n");
}

void deleteBook() {
    int id;
    printf("Enter Book ID to delete: ");
    scanf("%d", &id);
    Book *temp = head, *prev = NULL;
    while (temp) {
        if (temp->id == id) {
            if (prev == NULL)
                head = temp->next;
            else
                prev->next = temp->next;
            free(temp);
            printf("Book deleted successfully!\n");
            return;
        }
        prev = temp;
        temp = temp->next;
    }
    printf("Book not found.\n");
}

void borrowBook() {
    int id;
    printf("Enter Book ID to borrow: ");
    scanf("%d", &id);
    Book* temp = head;
    while (temp) {
        if (temp->id == id) {
            if (temp->available) {
                temp->available = 0;
                printf("Book borrowed successfully!\n");
            } else {
                printf("Book already borrowed.\n");
            }
            return;
        }
        temp = temp->next;
    }
    printf("Book not found.\n");
}

void returnBook() {
    int id;
    printf("Enter Book ID to return: ");
    scanf("%d", &id);
    Book* temp = head;
    while (temp) {
        if (temp->id == id) {
            if (!temp->available) {
                temp->available = 1;
                printf("Book returned successfully!\n");
            } else {
                printf("Book is not borrowed.\n");
            }
            return;
        }
        temp = temp->next;
    }
    printf("Book not found.\n");
}

int main() {
    int choice;
    do {
        printf("\n--- Library Menu ---\n");
        printf("1. Add Book\n2. Display Books\n3. Search Book\n4. Delete Book\n5. Borrow Book\n6. Return Book\n0. Exit\nEnter choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1: addBook(); break;
            case 2: displayBooks(); break;
            case 3: searchBook(); break;
            case 4: deleteBook(); break;
            case 5: borrowBook(); break;
            case 6: returnBook(); break;
            case 0: printf("Exiting...\n"); break;
            default: printf("Invalid choice.\n");
        }
    } while (choice != 0);
    return 0;
}
9. Outputs
Sample Output:
mathematica
Copy
Edit
--- Library Menu ---
1. Add Book
2. Display Books
...
Enter choice: 1
Enter Book ID: 101
Enter Title: C Programming
Enter Author: Dennis Ritchie
Book added successfully!
