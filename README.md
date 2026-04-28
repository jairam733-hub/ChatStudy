# Ex. No:1 (b) 		Study of Client Server Chat Applications
## Name : JAIRAM J
## Ref No : 25012221
## Aim: 
To perform a study on Client Server Chat Applications
## Introduction:
Client-server chat applications are a category of networked software that enables real-time communication between users over a network. This study explores the key components, architecture, and considerations in the development of client-server chat applications, highlighting their significance and common implementation practices.
Client-server chat applications are software systems that enable real-time communication between users over a network. These applications follow a client-server model, where one component (the server) manages connections and facilitates communication, while the other component (the client) interacts with the server to send and receive messages. Below are the fundamental aspects and components involved in the basics of client-server chat applications:
## 1. Client-Server Model:
•	Server:
•	The server is a central component that listens for incoming connections from clients.
•	It manages the communication channels and facilitates the exchange of messages between clients.
•	It may handle user authentication, message routing, and other core functionalities.
•	Client:
•	Clients are users or devices that connect to the server to participate in the chat.
•	Each client has a unique identity, often represented by a username.
•	Clients interact with the server to send and receive messages.
## 2. Communication Protocols:
•	Communication between clients and servers often relies on established protocols. The choice of protocol influences the behavior of the chat application.
•	TCP (Transmission Control Protocol):
•	Provides reliable, connection-oriented communication.
•	Ensures the ordered and error-free exchange of messages.

•	UDP (User Datagram Protocol):
•	Connectionless and operates in a best-effort mode.
•	Faster but may result in message loss or disorder.
## 3. Socket Programming:
•	Sockets:

•	Sockets serve as communication endpoints.
•	Each client and the server has a socket for sending and receiving data.

•	Functions:
•	Socket programming involves functions for creating, binding, listening, accepting connections, and sending/receiving data through sockets.
## 4. User Authentication:
•	For security and privacy, chat applications often implement user authentication mechanisms.
•	Users are required to provide credentials (e.g., username and password) to access the chat system.
•	More advanced methods like tokens or secure protocols can enhance authentication.
5. Message Routing:
•	The server is responsible for routing messages from one client to another.
•	It ensures that messages are delivered to the intended recipients.
•	Message routing may involve maintaining a list of connected users and their associated sockets.

## Architecture:
## Client-Server Model:
Client-server chat applications typically follow the client-server model, where one entity acts as the server, managing connections and facilitating communication, and one or more entities act as clients, initiating communication with the server.

## Communication Protocols:
The choice of communication protocol is crucial. Many chat applications use TCP (Transmission Control Protocol) for reliable, connection-oriented communication to ensure the ordered and error-free exchange of messages.
User Authentication:
User authentication mechanisms are essential to ensure secure and authorized access to the chat system. This can involve username-password authentication or more advanced methods like tokens.
## Components of Client-Server Chat Applications:
## Server-Side Components:

•	Socket Handling: The server manages incoming client connections using sockets, creating a separate thread or process for each connected client.
•	User Management: Maintaining information about connected users, their status, and handling login/logout functionality.
•	Message Routing: Implementing logic to route messages from one client to another, ensuring proper delivery.

## Considerations in Development:
1.	Concurrency and Multithreading:
•	Chat applications often require handling multiple connections simultaneously. The server must be designed to support concurrency, commonly achieved through multithreading or asynchronous programming.
2.	Security:
•	Ensuring the security of user data and messages is paramount. Encryption techniques, such as SSL/TLS, can be implemented to secure data in transit. Proper user authentication mechanisms help prevent unauthorized access.
3.	Scalability:
•	As the number of users grows, the chat application must be scalable. This involves optimizing server-side architecture to handle increasing loads efficiently.
4.	Persistence:
•	Some chat applications implement message persistence, allowing users to retrieve past messages. This may involve using databases to store and retrieve chat history.

5.	Notification Systems:
•	Implementing real-time notifications to inform users of new messages, user presence changes, or other relevant events.


Client-server chat applications are versatile tools that facilitate real-time communication between users over a network. They incorporate various components, including server-side and client-side elements, and must consider factors such as security, scalability, and concurrency. As technology continues to advance, client-server chat applications remain integral for collaborative communication in various domains.

Client-server chat applications are foundational to real-time communication over networks. They incorporate principles of socket programming, communication protocols, and security mechanisms to provide a seamless user experience. Understanding the basics of client-server chat applications is essential for developers involved in networked application development, as they form the backbone of various collaborative communication systems. As technology evolves, chat applications continue to adapt, incorporating new features and technologies to enhance user interaction and connectivity.

## CODE's..

## 1) CLIENT SIDE

```
import socket
import threading
import time

HOST = "127.0.0.1"
PORT = 12349

def start_client():
    c = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    c.connect((HOST, PORT))

    def receive():
        while True:
            try:
                data = c.recv(1024).decode()
                if not data:
                    break
                print("🖥️ Server:", data)
                if data.lower() == "bye":
                    break
            except:
                break

    def send():
        msgs = [
            "Hi buddy!",
            "How are you doing today?",
            "Can you tell me about the weather condition today?",
            "That sounds awesome!",
            "bye"
        ]

        for m in msgs:
            time.sleep(1.2)
            c.send(m.encode())
            if m.lower() == "bye":
                break

    t1 = threading.Thread(target=receive)
    t2 = threading.Thread(target=send)

    t1.start()
    t2.start()

    t1.join()
    t2.join()

    c.close()
    print("❌ Conversation ended")

start_client()
```

## 2) SERVER SIDE
```
import socket
import threading
import time

HOST = "127.0.0.1"
PORT = 12349

def start_server():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind((HOST, PORT))
    s.listen(1)

    print("🟢 Server is waiting for client...")
    conn, addr = s.accept()
    print("🟢 Client connected:", addr)

    def receive():
        while True:
            try:
                data = conn.recv(1024).decode()
                if not data:
                    break
                print("👤 Client:", data)
                if data.lower() == "bye":
                    break
            except:
                break

    def send():
        msgs = [
            "Hello Jai, welcome!",
            "I'm doing great, thanks for asking 😊",
            "Today's weather is pleasant with a chance of light rain 🌦️",
            "Glad you liked it 👍",
            "bye"
        ]

        for m in msgs:
            time.sleep(1.2)
            conn.send(m.encode())
            if m.lower() == "bye":
                break

    t1 = threading.Thread(target=receive)
    t2 = threading.Thread(target=send)

    t1.start()
    t2.start()

    t1.join()
    t2.join()

    conn.close()
    s.close()
    print("🔴 Server closed")

start_server()
```
## FULL FUNCTIONAL CODE...
```
import socket
import threading
import time

HOST = "127.0.0.1"
PORT = 12349


def start_server():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind((HOST, PORT))
    s.listen(1)

    conn, addr = s.accept()
    print("🟢 Server connected to client")

    def receive():
        while True:
            try:
                data = conn.recv(1024).decode()
                if not data:
                    break
                print("👤 Client:", data)
                if data.lower() == "bye":
                    break
            except:
                break

    def send():
        msgs = [
            "Hello Jai, welcome!",
            "I'm doing great, thanks for asking 😊",
            "Today's weather is pleasant with a chance of light rain 🌦️",
            "Glad you liked it 👍",
            "bye"
        ]   

        for m in msgs:
            time.sleep(1.2)
            conn.send(m.encode())
            if m == "bye":
                break

    t1 = threading.Thread(target=receive)
    t2 = threading.Thread(target=send)

    t1.start()
    t2.start()

    t1.join()
    t2.join()

    conn.close()
    s.close()
    print("🔴 Server closed")


def start_client():
    time.sleep(1)

    c = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    c.connect((HOST, PORT))

    def receive():
        while True:
            try:
                data = c.recv(1024).decode()
                if not data:
                    break
                print("🖥️ Server:", data)
                if data.lower() == "bye":
                    break
            except:
                break

    def send():
        msgs = [
            "Hi buddy!",
            "How are you doing today?",
            "Can you tell me about the weather condition today?",
            "That sounds awesome!",
            "tata"
        ]

        for m in msgs:
            time.sleep(1.2)
            c.send(m.encode())
            if m == "bye":
                break

    t1 = threading.Thread(target=receive)
    t2 = threading.Thread(target=send)

    t1.start()
    t2.start()

    t1.join()
    t2.join()

    c.close()
    print("❌ conversation ended")


server_thread = threading.Thread(target=start_server)
client_thread = threading.Thread(target=start_client)

server_thread.start()
client_thread.start()

server_thread.join()
client_thread.join()

print("✅ Chat session completed successfully")
```

## Output 
<img width="1355" height="649" alt="Screenshot 2026-04-28 160211" src="https://github.com/user-attachments/assets/16bf5c61-0aa3-4465-b56b-6ba8bf3dc56e" />



## Result:
Thus the study on Client Server Chat Applications has been performed

