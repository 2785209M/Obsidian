Architectural patterns provide the big picture for how software is organized in a system. They define how components are organized and communicate with each other.
**Examples:**
### **Model View Control:**
The primary goal of Model View Control is the separation of concerns. It divides an application into three connected parts (Model, view, controller).
The **Model** is the brain of the application. It manages the data, logic, and rules of the system. It is responsible for interacting with the **store** (typically a database). The model is the **only** component that can query or update the store, which is isolated from the other components.
The **View** is responsible for showing the user what they see. It is a visual representation of the model. This is the visual aspect of the **UI**.
The **Controller** acts as an interface between the model and the view. it is the **User Input** aspect of the **UI**. It listens to User Inpute, and based on this tells the model to update or the view to change.
A benefit of this approach is that the components are **loosely coupled**. They are not completely independent, but you can change the inner workings of one component without affecting the others. For example, you could completely redesign the view component from a web interface to a mobile app interface without changing the model or the controller at all.
![[image-5.png]]
### **Client-Server:**
The **Server** is responsible for providing access or services it hosts to one or more clients through a common interface. The server is also responsible for ensuring the integrity of the information it stores by validating change requests from clients.
The **Client** is responsible for presenting information gathered from the server to end-users and for issuing commands to the server. The end user may be people or other software Components.
The client-server architecture relies heavily on a request-response cycle. The client must (usually) initiate a request for the server to return a response. The server rarely initiates contact with the client.
### **Peer-to-Peer:**
The Peer to peer architecture makes every client both a client and a server, moving data into the clients themselves. The consequence of this is that the responsibility for hosting and managing services is distributed through the system.
Peer-to-Peer is very useful in systems where load needs to be distributed across the network. Good examples of this are file sharing networks, block-chain, and sensor networks. It is much easier for 1000 users to share a 10GB file with each other than it is for one server to share a 10GB file with 1000 users.
**Peer discovery** is performed in unstructured P2P networks using a combination of a publicly available list of the IP addresses of peers in the system and searching across the peer network. When a peer sends a request to another peer that peer sends it on until the data requested is found or the request times out. However, this is only used in older P2P systems as it creates a lot of network traffic.
**Instead**, modern systems use **Structured P2P networks** which act like a giant lookup table (often relying on distributed hash tables), routing requests direclty to the peer holding the data using the most topographically efficient path. This has a time efficiency of O(logN). The worst-case scenario for reaching the end node using this structure is $log_2{N}$, however on average it will take half that many.
 ![[image.png]]
### **Message Oriented Architecture:**
This architecture is like a post office.

This provides the basis for decoupled, asynchronous communication between components. In the pattern, all communication occurs as discrete messages that pass through a central message bus or broker. The broker routes messages to the appropriate client based on a **Routing policy**, which ensures that senders and receivers do not need to be active at the same time.

There are two ways messages can be distributed within this architecture:
**Point-to-Point:** A message is put into a message broker and is addressed to a specific **queue** (like a locked PO box), to be consumed by only **one** receiver.
**Publish-Subscribe:** A producer sends a message to the broker with an instruction to forward the message to every client who is subscribed to a specific topic.

![[image-6.png]]
### Pipe and Filter:
Each Transformation that must be applied to data is called a **Filter**.
Each filter is connected by a **Pipe**. The Pipes serve as a communication channel and a buffer for data being passed between filters. To work together, the adjacent filters need to agree on a common data format passing through the pipe.
Filters are wired into a sequential assembly called the **Pipeline**. Because each filter operates independently and is triggerd by the arrival of data in its pipe, the system naturally processes data concurrently without the need for a central scheduler to manage execution.
#### Variants to the basic pipe and filter model
- push or pull driven data flows
- sequential or concurrent data processing filters
- re-orderable or inter-changeable filters
- branching data filters

![[image-7.png|557]]
### Plugin Architecture:
The plugin Architecture provides a flexible mechanism for extending the functionality of a software system. In the pattern, a software is able to dynamically search for and add components that extend its functionality into the main system.

The Architecture is fundamentaly divided into two parts; the **main application** and the **plugin modules**. The main application is responsible for querying and maintaining a **registry**, which is a database that stores the specifications and state of the plugin modules available to the system.

The main **Application** can query the **Registry** for available plugin specifications. When an appropriate plugin is located, a loader component is used to instantiate and configure the component for use by the main application, using the specification supplied by the registry. The plugin's functionality can then be accessed via a standardized interface.