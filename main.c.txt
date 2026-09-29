#include <stdio.h>

#define SIZE 8
#define EMPTY -1

// Primary hash function
int hash1(int key) { return key % SIZE; }

// Second hash function for double hashing
int hash2(int key) { return 1 + 2 * (key % 4); }

// Initialize hash table
void initialize(int table[]) {
  for (int i = 0; i < SIZE; i++)
    table[i] = EMPTY;
}

// Display hash table
void displayTable(int table[]) {
  printf("\nIndex\tValue\n");
  printf("----------------\n");

  for (int i = 0; i < SIZE; i++) {
    if (table[i] == EMPTY)
      printf("%d\tEMPTY\n", i);
    else
      printf("%d\t%d\n", i, table[i]);
  }
}

// Linear Probing
void linearProbing(int keys[], int n, int table[]) {
  initialize(table);

  printf("\n========== LINEAR PROBING ==========\n");

  for (int i = 0; i < n; i++) {
    int key = keys[i];
    int index = hash1(key);
    int probes = 1;

    printf("\nKey %d: %d", key, index);

    while (table[index] != EMPTY) {
      index = (index + 1) % SIZE;
      probes++;
      printf(" -> %d", index);
    }

    table[index] = key;

    printf("  Inserted at %d", index);
    printf("  Probes = %d\n", probes);
  }

  printf("\nFinal Hash Table:");
  displayTable(table);
}

// Quadratic Probing
void quadraticProbing(int keys[], int n, int table[]) {
  initialize(table);

  printf("\n========== QUADRATIC PROBING ==========\n");

  for (int i = 0; i < n; i++) {
    int key = keys[i];
    int h = hash1(key);
    int index = h;
    int j = 0;
    int probes = 1;

    printf("\nKey %d: %d", key, index);

    while (table[index] != EMPTY) {
      j++;
      index = (h + j * j) % SIZE;
      probes++;
      printf(" -> %d", index);
    }

    table[index] = key;

    printf("  Inserted at %d", index);
    printf("  Probes = %d\n", probes);
  }

  printf("\nFinal Hash Table:");
  displayTable(table);
}

// Double Hashing
void doubleHashing(int keys[], int n, int table[]) {
  initialize(table);

  printf("\n========== DOUBLE HASHING ==========\n");

  for (int i = 0; i < n; i++) {
    int key = keys[i];
    int h1 = hash1(key);
    int h2 = hash2(key);
    int index = h1;
    int j = 0;
    int probes = 1;

    printf("\nKey %d: h1=%d, h2=%d", key, h1, h2);
    printf("\nProbe sequence: %d", index);

    while (table[index] != EMPTY) {
      j++;
      index = (h1 + j * h2) % SIZE;
      probes++;
      printf(" -> %d", index);
    }

    table[index] = key;

    printf("  Inserted at %d", index);
    printf("  Probes = %d\n", probes);
  }

  printf("\nFinal Hash Table:");
  displayTable(table);
}

// Search using Linear Probing
int searchLinear(int table[], int key) {
  int index = hash1(key);
  int probes = 1;

  while (table[index] != EMPTY && probes <= SIZE) {
    if (table[index] == key)
      return probes;

    index = (index + 1) % SIZE;
    probes++;
  }

  return -probes;
}

// Search using Quadratic Probing
int searchQuadratic(int table[], int key) {
  int h = hash1(key);
  int index = h;
  int j = 0;
  int probes = 1;

  while (table[index] != EMPTY && probes <= SIZE) {
    if (table[index] == key)
      return probes;

    j++;
    index = (h + j * j) % SIZE;
    probes++;
  }

  return -probes;
}

// Search using Double Hashing
int searchDouble(int table[], int key) {
  int h1 = hash1(key);
  int h2 = hash2(key);
  int index = h1;
  int j = 0;
  int probes = 1;

  while (table[index] != EMPTY && probes <= SIZE) {
    if (table[index] == key)
      return probes;

    j++;
    index = (h1 + j * h2) % SIZE;
    probes++;
  }

  return -probes;
}

// Display search result
void displaySearchResult(int key, int probes) {
  if (probes > 0)
    printf("Key %d: FOUND, Probes = %d\n", key, probes);
  else
    printf("Key %d: NOT FOUND, Probes = %d\n", key, -probes);
}

int main() {
  int keys[] = {23, 43, 13, 33, 53, 63, 73};
  int n = sizeof(keys) / sizeof(keys[0]);

  int linearTable[SIZE];
  int quadraticTable[SIZE];
  int doubleTable[SIZE];

  printf("============================================\n");
  printf("       RAILWAY BOOKING HASHING SYSTEM\n");
  printf("============================================\n");

  printf("\nInput Booking IDs:\n");

  for (int i = 0; i < n; i++)
    printf("%d ", keys[i]);

  printf("\n\nHash Table Size = %d", SIZE);
  printf("\nPrimary Hash Function: h(k) = k %% %d", SIZE);
  printf("\nLoad Factor = %d/%d = %.2f\n", n, SIZE, (float)n / SIZE);

  // Perform insertions
  linearProbing(keys, n, linearTable);

  quadraticProbing(keys, n, quadraticTable);

  doubleHashing(keys, n, doubleTable);

  // Search operations
  int searchKeys[] = {53, 63, 73, 80};
  int searchCount = 4;

  printf("\n\n============================================\n");
  printf("             SEARCH RESULTS\n");
  printf("============================================\n");

  printf("\n--- Linear Probing ---\n");

  for (int i = 0; i < searchCount; i++)
    displaySearchResult(searchKeys[i],
                        searchLinear(linearTable, searchKeys[i]));

  printf("\n--- Quadratic Probing ---\n");

  for (int i = 0; i < searchCount; i++)
    displaySearchResult(searchKeys[i],
                        searchQuadratic(quadraticTable, searchKeys[i]));

  printf("\n--- Double Hashing ---\n");

  for (int i = 0; i < searchCount; i++)
    displaySearchResult(searchKeys[i],
                        searchDouble(doubleTable, searchKeys[i]));

  printf("\n============================================\n");
  printf("                 END\n");
  printf("============================================\n");

  return 0;
}