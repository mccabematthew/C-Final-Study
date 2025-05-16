# C Programming Exam Cheat Sheet

## NEW TOPICS (PRIORITY)

### Linked Lists
```c
// Node Structure
struct Node {
    int data;             // Value stored in node
    struct Node* next;    // Pointer to next node
};

// Creating a new node
struct Node* createNode(int value) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = value;
    newNode->next = NULL;
    return newNode;
}

// Insert at beginning
void insertAtBeginning(struct Node** head, int value) {
    struct Node* newNode = createNode(value);
    newNode->next = *head;
    *head = newNode;
}

// Insert at end
void insertAtEnd(struct Node** head, int value) {
    struct Node* newNode = createNode(value);
    if (*head == NULL) {
        *head = newNode;
        return;
    }
    struct Node* temp = *head;
    while (temp->next != NULL) {
        temp = temp->next;
    }
    temp->next = newNode;
}

// Delete a node with specific value
void deleteNode(struct Node** head, int key) {
    struct Node *temp = *head, *prev;
    if (temp != NULL && temp->data == key) {
        *head = temp->next;
        free(temp);
        return;
    }
    while (temp != NULL && temp->data != key) {
        prev = temp;
        temp = temp->next;
    }
    if (temp == NULL) return;
    prev->next = temp->next;
    free(temp);
}

// Traverse a linked list
void traverseList(struct Node* head) {
    struct Node* current = head;
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->next;
    }
}
```

**Linked List vs Arrays**
| Linked Lists | Arrays |
|--------------|--------|
| Dynamic size | Fixed size |
| Insertion/deletion is O(1) at known positions | Insertion/deletion is O(n) |
| Random access is O(n) | Random access is O(1) |
| Extra memory for pointers | No extra memory overhead |
| No memory wastage | Might waste memory |

### Big O Notation
- **O(1)** - Constant time: operations that don't depend on input size
- **O(log n)** - Logarithmic time: divide and conquer algorithms (binary search)
- **O(n)** - Linear time: processing each item once (traversing an array)
- **O(n log n)** - Linearithmic time: efficient sorting algorithms (merge sort)
- **O(n²)** - Quadratic time: nested loops (bubble sort)

**Efficiency Ranking (Best to Worst):**
O(1) > O(log n) > O(n) > O(n log n) > O(n²)

### Algorithms

#### Binary Search
```c
int binarySearch(int arr[], int left, int right, int x) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // Check if x is present at mid
        if (arr[mid] == x)
            return mid;
            
        // If x greater, ignore left half
        if (arr[mid] < x)
            left = mid + 1;
            
        // If x smaller, ignore right half
        else
            right = mid - 1;
    }
    
    // Element not present
    return -1;
}
```

#### Sorting Algorithms

| Algorithm | Best Case | Average Case | Worst Case | Space | Stability |
|-----------|-----------|--------------|------------|-------|-----------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Stable |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | Not Stable |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Stable |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Stable |

**Bubble Sort**: Repeatedly steps through the list, compares adjacent elements and swaps them if they are in the wrong order.
```c
void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n-1; i++)
        for (int j = 0; j < n-i-1; j++)
            if (arr[j] > arr[j+1]) {
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
}
```

**Selection Sort**: Repeatedly finds the minimum element from the unsorted part and puts it at the beginning.
```c
void selectionSort(int arr[], int n) {
    for (int i = 0; i < n-1; i++) {
        int min_idx = i;
        for (int j = i+1; j < n; j++)
            if (arr[j] < arr[min_idx])
                min_idx = j;
        int temp = arr[min_idx];
        arr[min_idx] = arr[i];
        arr[i] = temp;
    }
}
```

**Insertion Sort**: Builds the sorted array one item at a time.
```c
void insertionSort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

**Merge Sort**: Divide and conquer algorithm that divides the input array into two halves, sorts them, and then merges.
```c
void merge(int arr[], int l, int m, int r) {
    int i, j, k;
    int n1 = m - l + 1;
    int n2 = r - m;
    
    // Create temp arrays
    int L[n1], R[n2];
    
    // Copy data
    for (i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];
        
    // Merge
    i = 0;
    j = 0;
    k = l;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    
    // Copy remaining elements
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}
```

### Trees

#### Binary Search Tree (BST)
```c
// Node Structure
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Creating a new node
struct TreeNode* createNode(int value) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Insert a node
struct TreeNode* insert(struct TreeNode* root, int value) {
    if (root == NULL)
        return createNode(value);
        
    if (value < root->data)
        root->left = insert(root->left, value);
    else if (value > root->data)
        root->right = insert(root->right, value);
        
    return root;
}

// Search for a value
struct TreeNode* search(struct TreeNode* root, int value) {
    if (root == NULL || root->data == value)
        return root;
        
    if (root->data < value)
        return search(root->right, value);
        
    return search(root->left, value);
}

// Find minimum value node
struct TreeNode* minValueNode(struct TreeNode* node) {
    struct TreeNode* current = node;
    while (current && current->left != NULL)
        current = current->left;
    return current;
}

// Delete a node
struct TreeNode* deleteNode(struct TreeNode* root, int value) {
    if (root == NULL) return root;
    
    if (value < root->data)
        root->left = deleteNode(root->left, value);
    else if (value > root->data)
        root->right = deleteNode(root->right, value);
    else {
        // Node with only one child or no child
        if (root->left == NULL) {
            struct TreeNode* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct TreeNode* temp = root->left;
            free(root);
            return temp;
        }
        
        // Node with two children
        struct TreeNode* temp = minValueNode(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }
    return root;
}

// Tree traversals
void inOrder(struct TreeNode* root) {
    if (root != NULL) {
        inOrder(root->left);
        printf("%d ", root->data);
        inOrder(root->right);
    }
}

void preOrder(struct TreeNode* root) {
    if (root != NULL) {
        printf("%d ", root->data);
        preOrder(root->left);
        preOrder(root->right);
    }
}

void postOrder(struct TreeNode* root) {
    if (root != NULL) {
        postOrder(root->left);
        postOrder(root->right);
        printf("%d ", root->data);
    }
}
```

**BST Properties**:
- Left subtree contains only nodes with values less than the node's value
- Right subtree contains only nodes with values greater than the node's value
- Both left and right subtrees are also BSTs
- No duplicate values

**BST Operations Time Complexity**:
- Search: O(log n) average, O(n) worst case
- Insert: O(log n) average, O(n) worst case
- Delete: O(log n) average, O(n) worst case

## OLD TOPICS (QUICK REFERENCE)

### Linux & GCC Commands
```bash
# Compile
gcc myfile.c -o myprogram

# Execute
./myprogram

# Directory operations
cd directory_name    # Change directory
mkdir new_directory  # Create directory
ls                   # List files
rm file_name         # Remove file
mv old_name new_name # Rename file
```

### Data Types
```c
// Primitive data types
char      // 1 byte
int       // 4 bytes
float     // 4 bytes
double    // 8 bytes
long      // 4 bytes (8 bytes on 64-bit systems)
short     // 2 bytes
```

### Strings
```c
// String declaration
char str[] = "Hello";    // Null-terminated string

// String functions
strlen(str);             // Length (excludes null terminator)
strcmp(str1, str2);      // Returns 0 if identical
strcpy(dest, source);    // Copy string

// String input
scanf("%s", str);        // Input string (unsafe)
fgets(str, size, stdin); // Safer input with size limit
```

### Random Numbers
```c
// Seed the random number generator
srand(time(NULL));

// Generate random number
int randomNum = rand() % 100;    // 0-99
int rangeNum = rand() % 51 + 50; // 50-100
```

### Memory Allocation
```c
// Dynamic memory allocation
int* arr = (int*)malloc(n * sizeof(int));

// Always free memory when done
free(arr);
```

### Pointers
```c
// Pointer declaration and initialization
int x = 10;
int* ptr = &x;           // Store address of x in ptr

// Dereferencing
*ptr = 20;               // Change value of x to 20
printf("%d", *ptr);      // Print value at address ptr

// Array pointer arithmetic
int arr[5] = {1, 2, 3, 4, 5};
int* p = arr;            // Points to first element
p++;                     // Points to second element
*(arr + 2);              // Access third element (same as arr[2])
```

### Functions
```c
// Function prototype
int add(int a, int b);

// Function definition
int add(int a, int b) {
    return a + b;
}

// Call function
result = add(5, 10);
```

### Command Line Arguments
```c
int main(int argc, char* argv[]) {
    // argc = number of arguments 
    // argv[0] = program name
    // argv[1], argv[2], etc. = additional arguments
    
    // Convert string to int
    int num = atoi(argv[1]);
}
```

### File I/O
```c
// Open a file
FILE* fp = fopen("file.txt", "r");  // Read mode
FILE* fp = fopen("file.txt", "w");  // Write mode
FILE* fp = fopen("file.txt", "a");  // Append mode
FILE* fp = fopen("file.txt", "r+"); // Read/write

// Check if file opened successfully
if (fp == NULL) {
    printf("Error opening file\n");
    return -1;
}

// Read from file
char buffer[100];
fgets(buffer, 100, fp);             // Read line
fscanf(fp, "%d %s", &num, str);     // Formatted read

// Read CSV
fscanf(fp, "%[^,],%d", name, &age); // Read until comma

// Write to file
fprintf(fp, "Hello %s\n", name);    // Formatted write
fputs("Hello world", fp);           // Write string

// Close file
fclose(fp);
```

### Structs
```c
// Struct declaration
struct Person {
    char name[50];
    int age;
};

// With typedef
typedef struct {
    char name[50];
    int age;
} Person;

// Using structs
Person p1;
strcpy(p1.name, "John");
p1.age = 30;

// Dynamic allocation
Person* p2 = (Person*)malloc(sizeof(Person));
strcpy(p2->name, "Jane");
p2->age = 25;
```

### Recursion
```c
// Recursive function example (factorial)
int factorial(int n) {
    // Base case
    if (n <= 1) return 1;
    
    // Recursive case
    return n * factorial(n - 1);
}
```

**Memoization**: Technique to store computed results to avoid redundant calculations in recursive functions.
```c
// Fibonacci with memoization
int fib(int n, int memo[]) {
    if (memo[n] != -1) 
        return memo[n];
    if (n <= 1)
        return n;
    memo[n] = fib(n-1, memo) + fib(n-2, memo);
    return memo[n];
}
```

**Recursion Trade-offs**:
- Advantages: Elegant, concise, better for some problems (tree traversal)
- Disadvantages: Memory-intensive (stack frames), potential stack overflow, sometimes slower
