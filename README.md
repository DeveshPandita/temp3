# Multi-Threaded Chat Server

This project implements a multi-threaded chat server in C++ using POSIX sockets and standard threading libraries. The server supports user authentication, broadcast messages, private messages, and group messaging (with group creation, joining, and leaving).

---

## Group Members

- Suvrat Pal (211089)
- Devesh Pandita (210330)
- Om Kothawade (210682)

---

## Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Compilation](#compilation)
- [Running the Server](#running-the-server)
- [Usage Instructions](#usage-instructions)
- [Files Description](#files-description)
- [Troubleshooting](#troubleshooting)

---

## Features

- **User Authentication:**  
  Clients must log in using a valid username and password. Credentials are stored in a `users.txt` file in the format `username:password`.

- **Broadcast Messaging:**  
  The command `/broadcast <message>` sends a message to all connected clients except the sender.

- **Private Messaging:**  
  The command `/msg <recipient> <message>` sends a private message to the specified user.

- **Group Management & Messaging:**  
  - **Create Group:** `/create_group <group_name>` creates a new group with the creator as the first member.
  - **Join Group:** `/join_group <group_name>` adds the client to an existing group.
  - **Leave Group:** `/leave_group <group_name>` removes the client from a group.
  - **Group Message:** `/group_msg <group_name> <message>` sends a message to all members of the specified group (except the sender).

- **Multi-threading:**  
  Each client connection is handled on a separate thread, allowing multiple clients to communicate simultaneously.

---

## Project Structure

- **chat_server.cpp**  
  Contains the full source code for the chat server. It includes:
  - Socket creation and binding to a port.
  - Loading user credentials from `users.txt`.
  - Accepting client connections and handling each in a separate thread.
  - Implementations of messaging functions (broadcast, private, and group messaging).

- **users.txt**  
  A text file that stores user credentials. Each line should be in the format:  
  
  username:password
  

---

## Prerequisites

- **Operating System:**  
  Linux or any POSIX-compliant system.

- **Compiler:**  
  A C++ compiler that supports C++11 or higher (e.g., `g++`).

- **Libraries:**  
  Standard C++ libraries, POSIX sockets, and threading libraries (no third-party libraries are required).

---

## Compilation

To compile the chat server, open a terminal in the project directory and run:

bash
g++ -std=c++11 -pthread -o chat_server chat_server.cpp


- `-std=c++11` enables C++11 features.
- `-pthread` links the POSIX threading library.

---

## Running the Server

1. **Prepare the `users.txt` File:**

   Create a file named `users.txt` in the same directory as the executable. Each line should contain a username and password separated by a colon. For example:

   
   alice:password123
   bob:qwerty456
   charlie:secure789

2. **Start the Server:**

   Run the compiled server executable:

   bash
   ./chat_server
   

   The server will bind to port `12345` (as defined in the code) and start listening for incoming connections. You should see an output message like:

   
   Server started on port 12345...
   

3. **Connect a Client:**

   Use any TCP client (such as `telnet` or `nc`) to connect to the server. For example, using `telnet`:

   bash
   telnet localhost 12345
   

   Or using `nc` (netcat):

   bash
   nc localhost 12345
   

---

## Usage Instructions

Once connected, follow these steps:

1. **Authentication:**
   - **Enter username:** When prompted, type your username (must exist in `users.txt`).
   - **Enter password:** When prompted, type the corresponding password.
   - If authentication fails, the server will close your connection.

2. **Chat Commands:**

   - **Broadcast Message:**  
     bash
     /broadcast Hello everyone!
     
     Sends "Hello everyone!" to all connected users (except yourself).

   - **Private Message:**  
     bash
     /msg bob Hi Bob, how are you?
     
     Sends a private message to the user `bob`.

   - **Create Group:**  
     bash
     /create_group mygroup
     
     Creates a new group named `mygroup` with you as its first member.

   - **Join Group:**  
     bash
     /join_group mygroup
     
     Joins the existing group `mygroup`.

   - **Leave Group:**  
     bash
     /leave_group mygroup
     
     Leaves the group `mygroup`.

   - **Group Message:**  
     bash
     /group_msg mygroup Hello group!
     
     Sends a message to all members of the group `mygroup` (except yourself).

   - **Exit:**  
     bash
     /exit
     
     Disconnects from the chat server.

---

## Files Description

- **chat_server.cpp:**  
  - **Main Function:**  
    Initializes the server, loads user credentials from `users.txt`, binds the server to a port, listens for connections, and spawns a new thread to handle each client.
  
  - **handle_client():**  
    Authenticates the user, registers the client, processes incoming commands, and handles client disconnection.
  
  - **Messaging Functions:**  
    - `send_message()`: Sends a message to a specific client.
    - `broadcast_message()`: Sends a message to all clients except the sender.
    - `send_private_message()`: Sends a private message to a specified user.
    - `send_group_message()`: Sends a message to all members of a specified group.

- **users.txt:**  
  Stores user credentials. Each line should have the format `username:password`.

---

## Troubleshooting

- **Port Conflicts:**  
  Ensure that port `12345` is not already in use and is allowed through your firewall.

- **Compilation Errors:**  
  Verify that your compiler supports C++11 and that all required header files (such as `<arpa/inet.h>`, `<unistd.h>`) are available.

- **Authentication Issues:**  
  Ensure that the `users.txt` file exists in the same directory as the executable and that the credentials are correctly formatted.

- **Connection Problems:**  
  If clients cannot connect, confirm that the server is running and that you are using the correct port and IP address.

---
