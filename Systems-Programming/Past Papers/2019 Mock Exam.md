# 1. C Programming and Data Types
(a) *
In C, strings are represented by arrays of characters of type char. A character is 1 byte. The end of the string is denoted by the special character \0. "Hello World" contains 11 characters plus the special character which makes it 12 bytes.

(b) ***
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// A node in the tree should store a string label
typedef struct node Node;

// Create a Node with the given label. Returns NULL if unsuccessful
Node* node_create(const char* label);
// Destroys the node and connected nodes in the tree
void node_destroy(Node* n);
// Inserts a node in the right place to maintain the order of the tree
void insert(Node* n, const char* label);
// Lookup a node in the tree. Returns NULL if the label is not present
Node* lookup(Node* n, const char* label);

// Define the Struct properly matching the typedef
struct node {
char* label;
Node* left_child;
Node* right_child;
};

// Implement node_create
Node* node_create(const char* label){
if(!label) return NULL;
Node* n = malloc(sizeof(Node));
if(!n) return NULL;

n->label = malloc(strlen(label) + 1);
if (!n->label) {
free(n);
return NULL;
}

strcpy(n->label, label);
n->left_child = NULL;
n->right_child = NULL;

return n;
}

// Implement node_destroy (Fixed return type to void)
void node_destroy(Node* n){
if(!n) return;
node_destroy(n->left_child);
node_destroy(n->right_child);
free(n->label);
free(n);
}

// Implement insert (Fixed variable names, typos, and BST logic)
void insert(Node* n, const char* label){
if(!n) return;
// Compare incoming label with current node's label
int cmp = strcmp(label, n->label);
if(cmp == 0) return; // Duplicate label, do nothing
if(cmp < 0){ // label comes before n->label, go left
if(n->left_child == NULL){
n->left_child = node_create(label);
} else {
insert(n->left_child, label);
}
} else { // label comes after n->label, go right (cmp > 0)
if(n->right_child == NULL){
n->right_child = node_create(label);
} else {
insert(n->right_child, label);
}
}
} 

// Implement lookup (Fixed BST logic to match insert)
Node* lookup(Node* n, const char* label){
if(!n) return NULL;
int cmp = strcmp(label, n->label);
if(cmp < 0) { return lookup(n->left_child, label); }
if(cmp > 0) { return lookup(n->right_child, label); }
// cmp must be 0
return n;
}

// Implement print (Fixed printf and changed to in-order traversal)
void print(Node* n){
if(!n) return;
printf("%s\n", n->label); // Changed print to printf
print(n->left_child);
print(n->right_child);
}

int main() {
Node* n = node_create("Systems");
insert(n, "Programming");
insert(n, "2019");
print(n);
node_destroy(n);
return 0;
}
```

# 2. Memory and Resource Management and Ownership
(a) **
A void pointer in C does not have any associated type information. This means that it can be used to point to any kind of data type in memory. For example, the function qsort uses void pointers to be able to sort arrays of any data type.

(b) The stack is a region of memory in RAM that is managed automatically by the compiler. For this the size of every variable must be known by the program. The heap can be manually allocated to data by the programmer. Programmers allocate heap memory using malloc() and free it using free(). The heap is a finite resource and doesn't get automatically cleared so you have to free memory after you've finished using it.

(c) **
Call-by-value means that an argument is copied into the parameter of a function (i.e a clone is made, and only lives until the end of the function). Arrays are not called by value. Instead a pointer to the first physical memory address of the array is passed into the function. This means that arrays are always global and can be modified inside functions.

(d) **
A segmentation fault is an error that gets thrown by the hardware notifying your operating that your process has attempted to access a restricted area of memory. 
A common strategy to find the source of a segmentation fault is to use a debugger with a back-trace feature which shows the call stack at the time of the segmentation fault.
The most common cause of segmentation faults is the de-referencing of a null pointer.

(e) ***
RAII - "Resource acquisition is initialization"
Ownership means we identify a specific entity that is responsible for managing a location in memory.
The benefits this has over malloc and free are that manual memory management problems are avoided by construction, for example: double free errors or memory leaks.

RAII in C++ ties the management of memory to the lifetime of a variable. A special struct of this type will define a *Constructor* to allocate memory (e.g. malloc) and a *destructor* to unallocate memory (e.g. free). A variable of this struct type will automatically allocate memory when it is constructed and automatically deallocate memory when it is destroyed.

(f) ***
The std::unique_ptr models a uniqe ownership when we can find a unique owner of a resource that is responsible for managing its lifetime. For example, children in a tree. The children will be automatically deallocated when their parent node is deallocated.
The std::shared_ptr models a shared ownership model where no one unique owner of a resource can be defined. An example of this would be a graph without cycles (such as a DAG) where a node can have multiple incoming edges. A node would be automatically deallocated after its last incoming edge disappears, but not before.

(g) ***
```
struct node{
	std::string label;
	std::vector<std::shared_ptr<struct node>> edges;
}
```

The outgoing edges of a node are stored in a vector (an array that can dynamically grow and shrink). As there is no unique ownership (a node can have multiple incoming edges) we use a shared pointer to model the shared ownership.

This design would be problematic if there were cycles in the graph, as then the shared_ptr would form a perpetual cycle and no memory would be released.

An implementation of this in C would need to ensure that all nodes in the graph are freed after they are no longer accessible. For this a counter could be maintained in the node that counts the incoming edges. Once that reaches zero, the node can be freed.

# 3. Concurrent Systems Programming
(a) *
A thread is part of a process and live in that process' address space, and a process can have multiple threads executing simultaneously. No two processes will share an address space. In Linux processes and threads are treated like the same thing.

(b) **
Consider a scenario where you are using threads to remove adjacent elements in a list. If you do not use mutual exclusion, the two threads may cause a corrupted list state. For example if you have a list a->b->c->d, and thread one moves the pointer going from a->b to a->c, this would make the list a->c->d. But thread 2 wouldn't know this had happened and if it tried to delete c by moving the pointer b->c to b->d, nothing would happen, so c wouldn't be deleted even if it was explicitly removed in the code.

(c) **
A future is a handle representing a value that is not yet computed.
This handle can be passed around or stored in memory.
When  the value is required .get() can be called which fetches the value but waits for it to be computed.

The promise is the other side of a future, allowing it to provide a value ("fulfilling a promise") once it has been computed.

Without future and promise the value would have to be explicitly protected by a mutex or another low level primitive, and a condition variable that could be used to wait for the value to be computed.

(d) ***
This interface is not thread safe as it closes the mutex whilst threads are still holding iterators.

This means that is thread 1 got an iterator to the first element in the set by using begin() but didn't read the data yet, thread 2 could come in with erase() and delete the data before thread 1 reads it. This would likely cause the program to crash. This could also happen with end() and find().

It is worth noting that if any data other than the data that thread 1 held a pointer to was deleted then the program would not crash. But it still isn't safe.

It is also impossible to perform compound operations within one mutex. This makes the erase method impossible to use in this interface because to use it you need to find the position of the data you want to erase using find() first.

(e) *** ***
```
struct stack{
	private:
	int values[N];
	int index = 0;
	std::mutex m;
	std::condition_variable cv_not_full;
	std::condition_variiable cv_not_empty;
	
	public:
	void push(int i){
		std::unique_lock<std::mutex> lock(m);
		cv_not_full.wait(lock,[this]{return index < N;});
		values[index] = i;
		index++;
		cv_not_empty.notify_one();
	}
	
	int pop(){
		std::unique_lock<std::mutex> lock(m);
		cv_not_empty.wait(lock[this]{return index > 0;});
		index--;
		int i = values[index];
		cv_not_full.notify_one();
		return i;
	}
}
```

```
bool try_push(int i){
	std::unique_lock<std::mutex> lock(m);
	if (index >= N){
		return false;
	} else{
		values[index]= i;
		index++;
		return true;
	}
}

std::optional<int> try_pop(){
	std::unique_lock<std::mutex> lock(m);
	if(index <= 0){
		return std::optional<int>()
	} else{
		index--;
		return values[index];
	}
}
```

# Key takeaways
Familiarize self with C and C++ programming. Become comfortable with writing basic programs like binary search trees, stacks, queues, etc. with memory management and thread safe optimization. This should not only make you more comfortable with actual programming but doing real programming will teach you the concepts from the knowledge-based questions as well.