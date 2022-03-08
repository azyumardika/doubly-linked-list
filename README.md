# doubly-linked-list
// Online C compiler to run C program online
#include <stdio.h>
#include <stdlib.h>

// node creation
struct Node {
    int data;
    struct Node* next;
    struct Node* prev;
};

// masukkan node di depan
void insertFront(struct Node** head, int data)
{
  // mengalokasikan memori untuk Node baru
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));

  // tetapkan data ke Node baru
    newNode->data = data;

  // buat Node baru sebagai kepala
    newNode->next = (*head);

  // tetapkan null ke sebelumnya
    newNode->prev = NULL;

  // sebelumnya dari kepala (sekarang kepala adalah Node kedua) adalah Node baru
    if ((*head) != NULL)
        (*head)->prev = newNode;

  // kepala menunjuk ke Node baru
        (*head) = newNode;
}
// masukkan Node setelah Node tertentu
void insertAfter(struct Node* prev_node, int data) {
  // check if previous node is null
  if (prev_node == NULL) {
    printf("previous node cannot be null");
    return;
  }

  // mengalokasikan memori untuk Node baru
  struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));

  // tetapkan data ke Node baru
  newNode->data = data;

  // berikutnya atur dari Node baru ke node sebelumnya
  newNode->next = prev_node->next;

  // atur berikutnya dari node sebelumnya ke Node baru
  prev_node->next = newNode;

  // atur sebelumnya dari Node baru ke node sebelumnya
  newNode->prev = prev_node;

  // atur sebelumnya dari Node baru di sebelah Node baru
  if (newNode->next != NULL)
    newNode->next->prev = newNode;
}

// masukkan Node baru di akhir daftar
void insertEnd(struct Node** head, int data) {
  // allocate memory for node
  struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));

  // tetapkan data ke Node baru
  newNode->data = data;

  // tetapkan null ke Node baru berikutnya
  newNode->next = NULL;

  // simpan Node kepala sementara (untuk digunakan nanti)
  struct Node* temp = *head;

  // jika daftar link kosong, buat Node baru sebagai Node kepala
  if (*head == NULL) {
    newNode->prev = NULL;
    *head = newNode;
    return;
  }

  // jika daftar link tidak kosong, lewati ke akhir daftar link
  while (temp->next != NULL)
    temp = temp->next;

  // sekarang, Node terakhir dari daftar link adalah temp

  // tetapkan berikutnya dari node terakhir (temp) ke Node baru
  temp->next = newNode;

  // tetapkan sebelumnya dari Node baru ke temp
  newNode->prev = temp;
}

// hapus simpul dari daftar link ganda
void deleteNode(struct Node** head, struct Node* del_node) {
  // if head or del is null, deletion is not possible
  if (*head == NULL || del_node == NULL)
    return;

  // jika del_node adalah simpul kepala, arahkan penunjuk kepala ke del_node berikutnya
  if (*head == del_node)
    *head = del_node->next;

  // jika del_node tidak berada di node terakhir, arahkan node sebelumnya di sebelah del_node ke del_node sebelumnya
  if (del_node->next != NULL)
    del_node->next->prev = del_node->prev;

  // jika del_node bukan node pertama, arahkan node berikutnya dari node sebelumnya ke node del_node berikutnya
  if (del_node->prev != NULL)
    del_node->prev->next = del_node->next;

  // bebaskan memori del_node
  free(del_node);
}

// cetak daftar link ganda
void displayList(struct Node* node) {
    struct Node* last;

    while (node != NULL) {
        printf("%d->", node->data);
        last = node;
        node = node->next;
  }
    if (node == NULL)
        printf("NULL\n");
}

int main() {
  // inisialisasi node kosong
    struct Node* head = NULL;
    printf("List : \n");
    insertFront(&head, 1);
    insertEnd(&head, 2);
    insertEnd(&head, 3);
    insertEnd(&head, 4);
    insertEnd(&head, 5);
    displayList(head);
    printf("\nInsertsion : \n");
    printf("Before : \n");
    displayList(head);
    printf("After : \n");
    insertAfter(head->next->next, 15);
    displayList(head);
    printf("\nDelete : \n");
    printf("Before : \n");
    displayList(head);
    printf("After : \n");
    deleteNode(&head, head->next->next);
    displayList(head);
}
