# Optimized C Programming Exam Cheat Sheet (Test Use)

---

## 🔗 Linked Lists

```c
struct Node { int data; struct Node* next; };
```

- Insert at beginning:

```c
void insertBegin(struct Node** head, int val) {
    struct Node* n = malloc(sizeof(struct Node));
    n->data = val; n->next = *head; *head = n;
}
```

- Insert at end:

```c
void insertEnd(struct Node** head, int val) {
    struct Node* n = malloc(sizeof(struct Node));
    n->data = val; n->next = NULL;
    if (!*head) { *head = n; return; }
    struct Node* t = *head;
    while (t->next) t = t->next;
    t->next = n;
}
```

- Delete node:

```c
void delete(struct Node** head, int val) {
    struct Node *t = *head, *p;
    if (t && t->data == val) { *head = t->next; free(t); return; }
    while (t && t->data != val) { p = t; t = t->next; }
    if (!t) return; p->next = t->next; free(t);
}
```

- Traverse:

```c
void print(struct Node* h) {
    while (h) { printf("%d ", h->data); h = h->next; }
}
```

---

## 📈 Big O Reference

| Notation   | Description      |
| ---------- | ---------------- |
| O(1)       | Constant         |
| O(log n)   | Binary search    |
| O(n)       | Single traversal |
| O(n log n) | Merge sort       |
| O(n^2)     | Nested loops     |

Fastest to slowest: O(1) < O(log n) < O(n) < O(n log n) < O(n^2)

---

## 🔍 Search / Sort Algorithms

- **Binary Search**:

```c
int bs(int arr[], int l, int r, int x) {
 while (l <= r) {
  int m = l + (r - l) / 2;
  if (arr[m] == x) return m;
  else if (arr[m] < x) l = m+1;
  else r = m-1;
 }
 return -1;
}
```

- **Sorting Complexities**:

| Algo            | Best     | Avg      | Worst     | Space | Stable |
|-----------------|----------|----------|-----------|-------|--------|
| Bubble Sort     | O(n)     | O(n^2)   | O(n^2)    | O(1)  | Yes    |
| Selection Sort  | O(n^2)   | O(n^2)   | O(n^2)    | O(1)  | No     |
| Insertion Sort  | O(n)     | O(n^2)   | O(n^2)    | O(1)  | Yes    |
| Merge Sort      | O(n log n)| O(n log n)| O(n log n)| O(n) | Yes    |

---

## 🌲 Trees (Binary Search Tree)

```c
struct TreeNode { int data; struct TreeNode* l, *r; };
```

- Insert:

```c
struct TreeNode* insert(struct TreeNode* r, int val) {
 if (!r) { r = malloc(sizeof(struct TreeNode));
          r->data = val; r->l = r->r = NULL; return r; }
 if (val < r->data) r->l = insert(r->l, val);
 else if (val > r->data) r->r = insert(r->r, val);
 return r;
}
```

- InOrder Traversal:

```c
void inOrder(struct TreeNode* r) {
 if (r) { inOrder(r->l); printf("%d ", r->data); inOrder(r->r); }
}
```

---

## 📁 File I/O

```c
FILE* f = fopen("file.txt", "r");
if (!f) return 1;
fgets(buf, 100, f);
fscanf(f, "%[^,],%d", name, &val);
fclose(f);
```

- Modes: "r", "w", "r+", "a", "w+"
- Skip CSV column: `%*[^,]`

---

## 🧠 Recursion

- Recursive function: calls itself
- Must have **base case**

```c
int fact(int n) { return (n <= 1) ? 1 : n * fact(n-1); }
```

- Stack overflow if no base case
- **Memoization**: store prior results to avoid repeat work

---

## 💾 malloc & Pointers

- Allocate:

```c
int* a = malloc(n * sizeof(int));
free(a);
```

- Pointers:

```c
int x = 5, *p = &x; printf("%d", *p);
```

- Array as pointer: `arr[i] == *(arr + i)`

---

## 📄 Strings

```c
char s[] = "hello"; // ends with \0
printf("%s", s);
scanf("%s", s);
fgets(s, size, stdin);
```

- strcmp(a,b) == 0 if equal
- strcpy(dest, src)

---

## 🧮 Functions

```c
int add(int a, int b);
int add(int a, int b) { return a + b; }
```

- Must declare/prototype before use
- `return` exits function

---

## 🔢 Command Line Args

```c
int main(int argc, char* argv[]) {
 printf("%s", argv[1]);
 int x = atoi(argv[1]);
}
```

- argv[0] = program name
- argv[i] = string argument

---

## 🧠 Structs

```c
typedef struct {
 char name[50]; int age;
} Person;
Person* p = malloc(sizeof(Person));
strcpy(p->name, "Tom"); p->age = 30;
```

---

## 📦 Header Guards

```c
#ifndef FILENAME_H
#define FILENAME_H
// content
#endif
```

- Prevents double inclusion

---

## 🖥️ Linux/GCC Commands

```bash
gcc main.c -o prog
./prog
cd dir
mkdir name
ls
rm file
mv old new
```

- Default output: `./a.out`