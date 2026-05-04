# 1. C Programming and Data Types
(a) **1/2**
In programming, the system/compiler won't know how to interpret your data unless you give it a guide. Just giving a selection of bits to the compiler won't mean anything. You have to tell the compiler what those bits represent so it can decode them into usable data.
```
Bit patterns in memory can have different meanings. Data types assign a fixed meaning to bit patterns and the safety of the language ensures this meaning is not lost when we perform computations. An example might be performing the operation +1 woul result in different instructions and modifications of bit patterns depending if an int or a float is being modified.
```

(b) **0/15**
```
#include <stdio.h>
#include <stlib.h>

typedef struct node Node{
	Node node;
	Node leftChild;
	Node rightChild;
};

typedef struct list List{

};

List* list_create(){
malloc(sizeof(Node));
if(Node){

}
}

void list_destroy(List* l){
if(l) free(l);
}

void push_right(List* l, void* value){

}

Node* pop_left(List* l){
if(node.left) free(node.left);
}
```

```
#include <stdlib.h>
#include <stdio.h>

struct node{
	struct node* prev;
	struct node* next;
	void* value;
};

struct list{
	struct node* left;
	struct node* right;
};

List* list_create(){
	List* l = malloc(sizeof(List));
	if(l){
		l->left = NULL;
		l->right = NULL;
	}
	return l;
}

Node* node_create(void*valule){
	Node* n = malloc(sizeof(Node));
	if(n){
		n->value = value;
		n->next = NULL;
		n->prev = NULL;
	}
	return n;
}

void push_right(List* l, void* value){
	if(l==NULL) return;
	Node* n = node_create(value);
	if(l->right){
		n->prev = l->right;
		l->right->next = n;
	}
	l->right = n;
	if (l->left == NULL) l->left = n;
}

Node* pop_right(List* l){
	if(l == NULL || l->right == NULL) return NULL;
	Node* n = l->right;
	l->right = l->right->prev;
	if(l->left == n) l->left = NULL;
	
	return n;
}

void push_left(List* l, void* value){
	if(!l) return;
	Node* n = node_create(value);
	if(l->left){
		n->next = l->left;
		l->left->prev = n;
	}
	l->left = n;
	if(l->right == NULL) l->right = n;
}

Node* pop_left(List* l){
	if(l == NULL || l->left == NULL) return NULL;
	Node* n = l->left;
	l->left = l->left->next;
	if(l->right == n) l->right = NULL;
	return n;
}

void list_destroy(List* l){
	Node* n = pop_left(l);
	while(n){
		free(n);
		n = pop_left(l);
	}
	free(l);
}

void for_each_node(List* l, void(*fun)(Node*)){
	if(l){
		Node* n = l->left;
		while(n){
			(*fun)(n);
			n = n->next;
		}
	}
}

void print(Node* n){
	printf("%s\n", n_.value);
}

int main(){
	List* l = list_create();
	push_right(l, "first");
	push_right(l, second);
	return 0;
}
```
# 2. Memory and Resource Management and Ownership
(a) **0/2**
Dereferencing a void pointer (or a null pointer) would result in a segmentation fault, as the system would be trying to free memory that is already free.
```
A void pointer holds a reference to an unknown value. It is unclear what the bits the pointer is pointing at mean. The programmer first has to re-establish the meaning of the bits by casting the pointer.
```

(b) **1/2**
The stack: The stack is an area of memory managed automatically by the compiler. It lives in RAM. This means the programmer does not have to manually allocate and deallocate it.
The heap: The heap is an area of memory in RAM that is managed manually by the compiler. This means the programmer has to allocate and deallocate memory to the heap in the program (e.g. malloc and free).
```
The stack is automatically managed. Variables are put on the stack when a block is entered and removed in reverse order when the block is exited.
The heap is managed manually by the programmer. The programmer is responsible to call free correclty to prevent memory leaks.
```

(c) **1/3**
C variable can have local, static, or global lifetime.
Local: A local variable has a lifetime that starts and ends at the beginning and end of a function.
Static: A static variable will come to life at the beginning of a process and end when the process finishes.
Global: A global variable will be initialized throughout an application and will die when the application stops.
```
Automatic: Automatic variables are dellocated at the end of the block they were declared in
Static: Static variables are deallocated at the end of the program
Allocated: Allocated variables are manually deallocated by the programmer
```

(d) **2/2**
Returning a pointer to a local variable from a function would result in a Dangling pointer. Since local variables are freed at the end of a function, the pointer would be pointing to an area of memory containing nothing. This means it is a gateway to accidentally overwrite other data or for hackers to take advantage.
```
The pointer has a longer lifetime than the variable. Any access to the original variable would be invalid as it has potentially been deallocated.
```

(e) **1/3**
Memory leaks: forgetting to free memory after it has stopped being useful results in memory leaks. To many memory leaks could result in the heap being completely filled and no new data being allowed.
Allocation size: It is important not to create memory overhead by assigning a component more space than it needs. It is also important to not assign too little memory to a variable, which could lead to an error.
```
Double free errors - trying to free data that has already been freed
Dangling Pointers - Pointers to variables that have been freed
Memory leaks - Never calling free
```

(f) **2/4**
Ownership is the context that each part of a program is responsible for managing its own memory. When the object is initialized is will call the constructor method, which will allocate space in memory for the variables it needs, and when the object reaches the end of its lifetime it will call the destructor method to free all of the memory it allocated during its lifetime. This is beneficial because it takes responsibility away from the programmer and reduces the rate of the above three problems occuring.
```
Ownership is when a specific entity is given responsibility for managing a resource. In C++ this is reinforced by "Resource Allocation Is Initialization" (RAII). Resources are allocated in the constructor of an object and deallocated in the destructor of the object. by creating such an object handler on the stack, the management of the resource is tied to the lifetime of a stack variable.
```

(g) **0/4**
```
typedef struct dbhandle DBhandle{

};

DBConnection* db_open(const char* url){
 \\ ESTABLISH CONNECTION
}

void db_close(DBConnection* connection){
 \\ CLOSE CONNECTION
}

void main(){
	DBConnection* db_open(const char* url);
	void db_close(DBConnection* connection);
}
```

```
DBConnection* db_open(const char* url);
	void db_close(DBConnection* connection);

	struct db_handle {
	DBConnection* connection;
	
	db_handle(const char* url){
		connection = db_open(url);
	}
	
	~db_handle(){
		db_close(connection);
	}
}
```

# 3. Concurrent Systems Programming
(a) **2/2**
Concurrency: Concurrency is the interleaving of task execution on a single processor using scheduling algorithms to give the illusion of simultaneous processing.
Parallelism: Parallelism is the simultaneous execution of tasks on multiple different processors/cores.
```
Concurrency is a programming paradigm; Parallelism is about making
programs faster
Concurrency is about dealing with lots of things at once; Parallelism is about
doing lots of things at once
```

(b) **1/2**
Mutexes are used to protect critical regions when they are being accessed by a thread. A mutex is like a lock with only one key, and only one thread can hold that key at a time.
```
Mutual exclusion which can be ensured by mutexes or semaphores
```

(c) **4/4**
(i) This code is not safe. It initializes two threads to work on a critical region at the same time without a locking mechanism.  This would result in a race condition. This could result in buffer overflow.
(ii) The size of the buffer would be unpredictable due to the race condition.
```
No, this program is not safe, as the buffer is unprotected and being accessed by two threads. It is posssible that bufferptr->pop() could be called on an empty buffer.
As this program has a race condition the output is unpredictable. It might crash or result in a buffer with arbitrary size.
```

(d) **0/16**
```
struct Semaphore{
	int S;
};

void sem_wait(struct semaphore*){
	while(S<=0);
	S--;
}

void signal(struct semaphore*){
	S++;
}

void main(){
	 std::thread();
}
```

```
#include <stdlib.h>
#include <pthread.h>

typedef struct semaphore Semaphore;

struct semaphore{
	int count;
	int size;
	pthread_mutex_t lock;
	pthread_cond_t cond;
};

Semaphore *sem_create(int N){
	Semaphore *sem = (Semaphore *)malloc(sizeof(Semaphore));
	
	if(sem!=NULL){
		sem->count = N;
		sem->size = N;
		pthread_mutex_init(&(sem->lock), NULL);
		pthread_cond_init(&(sem->cond), NULL);
	}
	return sem;
}

void sem_signal(Semaphore *sem){
	pthread_mutex_lock(&(sem->lock));
	if(sem->count < sem->size)
		sem->count++;
	pthread_cond_signal(&(sem->cond));
	pthread_mutex_unlock(&(sem->lock));
}

void sem_wait(Semaphore *sem){
	pthread_mutex_lock(&(sem->lock));
	while(sem->count <= 0)
		pthread_cond_wait(&(sem->cond), &(sem->lock));
		
	sem->count--;
	pthread_cond_signal(&(sem->cond));
	pthread_mutex_unlock(&(sem->lock));
}
```

# 14/60 = 23%

# Keynotes
The programming is worth 35 marks. You must get the programming.