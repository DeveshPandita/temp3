# Multi-Threaded Chat Server

This project implements a **multi-threaded chat server** in C++ using **POSIX sockets and threading libraries**. The server supports **user authentication, broadcast messages, private messages, and group messaging** (including group creation, joining, and leaving).

---

## Group Members

- **Suvrat Pal (211089)**
- **Devesh Pandita (210330)**
- **Om Kothawade (210682)**

---

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Compilation](#compilation)
- [Running the Server](#running-the-server)
- [Usage Instructions](#usage-instructions)
- [Files Description](#files-description)
- [How the Code Works](#how-the-code-works)
- [Troubleshooting](#troubleshooting)

---

## Features

- **User Authentication:**  
  Clients must log in using a **valid username and password** stored in the `users.txt` file.

- **Broadcast Messaging:**  
  `/broadcast <message>` sends a message to all connected users.

- **Private Messaging:**  
  `/msg <recipient> <message>` sends a private message to a specific user.

- **Group Messaging & Management:**  
  - `/create_group <group_name>` creates a new group.
  - `/join_group <group_name>` adds the user to a group.
  - `/leave_group <group_name>` removes the user from a group.
  - `/group_msg <group_name> <message>` sends a message to all group members.

- **Multi-threading:**  
  Each client connection is handled on a separate thread.

---

## Prerequisites

- **Operating System:** Linux or any POSIX-compliant system.
- **Compiler:** A C++ compiler supporting **C++11 or higher** (e.g., `g++`).
- **Libraries:** Standard C++ libraries, POSIX sockets, and threading libraries.

---

## Compilation

To compile the chat server, run:

bash
make


This command compiles the server and client executables (`server_grp` and `client_grp`).

---

## Running the Server

1. **Prepare the `users.txt` File**  
   Ensure the `users.txt` file exists in the same directory with credentials:

   
   alice:password123
   bob:qwerty456
   charlie:secure789
   

2. **Start the Server**  
   Run the compiled server:

   bash
   ./server_grp
   

   You should see:

   
   Server started on port 12345...
   

3. **Connect the Client**  
   Launch the client:

   bash
   ./client_grp
   

---

## Usage Instructions

Once connected, use the following commands:

- **Broadcast Message:**  
  bash
  /broadcast Hello everyone!
  
  Sends "Hello everyone!" to all users.

- **Private Message:**  
  bash
  /msg bob Hi Bob, how are you?
  
  Sends a private message to `bob`.

- **Create Group:**  
  bash
  /create_group mygroup
  
  Creates a group `mygroup`.

- **Join Group:**  
  bash
  /join_group mygroup
  
  Joins the group `mygroup`.

- **Leave Group:**  
  bash
  /leave_group mygroup
  
  Leaves the group `mygroup`.

- **Send Group Message:**  
  bash
  /group_msg mygroup Hello group!
  
  Sends "Hello group!" to all members.

- **Exit:**  
  bash
  /exit
  
  Disconnects from the server.

---

## Files Description

- **`chat_server.cpp`**
  - **Main Function (`main`)**
    - Loads user credentials from `users.txt`.
    - Binds the server to **port 12345**.
    - Listens for client connections and spawns a new thread for each client.

  - **`handle_client()`**
    - Authenticates the user.
    - Registers the client.
    - Processes chat commands.

  - **Messaging Functions:**
    - `send_message()`: Sends a message to a specific client.
    - `broadcast_message()`: Sends a message to all users.
    - `send_private_message()`: Sends a private message to a user.
    - `send_group_message()`: Sends a message to a group.

- **`users.txt`**  
  - Stores registered user credentials.

---

## How the Code Works

### **1. Starting the Server**
- The `main()` function initializes the server, loads users from `users.txt`, and listens on **port 12345**.
- When a client connects, a new thread is created for that client.

### **2. Handling Client Connections**
- The `handle_client()` function authenticates the user by checking `users.txt`.
- If authentication fails, the connection is closed.
- On success, the client is added to the `clients` list.

### **3. Message Transmission**
- **Broadcast Messages:**  
  - `/broadcast <message>` is sent to all users except the sender.

- **Private Messages:**  
  - `/msg <recipient> <message>` sends a message to a specific user.

- **Group Messaging:**
  - Users can create groups (`/create_group`).
  - They can join (`/join_group`) and leave (`/leave_group`) groups.
  - Messages sent with `/group_msg <group_name> <message>` go to all group members.

### **4. Multi-threading**
- Each client is handled in a separate thread (`std::thread`).
- `std::mutex` ensures safe access to shared resources (e.g., user lists).

### **5. Client Disconnection**
- When a user disconnects (`/exit`), their socket is closed, and they are removed from the list.

---

## Troubleshooting

- **Port Conflicts:**  
  Ensure that **port 12345** is available.

- **Compilation Errors:**  
  - Ensure **C++11 support** is enabled:
    bash
    g++ -std=c++11 -pthread -o server_grp chat_server.cpp
    

- **Authentication Issues:**  
  - Check `users.txt` for valid usernames/passwords.

- **Connection Problems:**  
  - Verify that the **server is running**.
  - Use `nc` (netcat) for testing:
    bash
    nc localhost 12345
    

---
