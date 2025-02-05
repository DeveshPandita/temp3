# Multi-Threaded Chat Server

This project implements a **multi-threaded chat server** in C++ using **sockets and threading libraries**. The server supports **user authentication, broadcast messages, private messages, and group messaging** (including group creation, joining, and leaving).

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
- [How the Code Operates](#how-the-code-operates)
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

- **Compiler:** A C++ compiler supporting **C++11 or higher** (e.g., `g++`).
- **Libraries:** Standard C++ libraries, sockets, and threading libraries.

---

## Compilation

To compile the chat server, run:

BASH
'make'


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

   BASH
   '.\server_grp'
   

   You should see:

   
   Server started on port 12345...
   

3. **Connect the Client**  
   Launch the client:

   BASH
   '.\client_grp'
   

---

## Usage Instructions

Once connected, use the following commands:

- **Broadcast Message:**  
  BASH
  '/broadcast Hello everyone!'
  
  Sends "Hello everyone!" to all users.

- **Private Message:**  
  BASH
  '/msg bob Hi Bob, how are you?'
  
  Sends a private message to `bob`.

- **Create Group:**  
  BASH
  '/create_group mygroup'
  
  Creates a group `mygroup`.

- **Join Group:**  
  BASH
  '/join_group mygroup'
  
  Joins the group `mygroup`.

- **Leave Group:**  
  BASH
  '/leave_group mygroup'
  
  User leaves the group `mygroup`.

- **Send Group Message:**  
  BASH
  '/group_msg mygroup Hello group!'
  
  Sends "Hello group!" to all members of group `mygroup`.

- **Exit:**  
  BASH
  '/exit'
  
  Disconnects from the server.

---

## Files Description

- **`server_grp.cpp`**
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

## How the Code Operates

### 1. User Authentication

- *Initialization:*  
  When the server starts up, it reads from the users.txt file and loads valid user credentials into a map for quick access using the load_credentials() function.

- *Login Process:*  
  As soon as a client connects, the server prompts for a username and password. These credentials are verified against the preloaded data within the client_handler() function.

### 2. Client Management

- *Thread Handling:*  
  Each client connection is managed in its own thread (using std::thread), which allows multiple clients to interact simultaneously without blocking the system.

- *Tracking:*  
  A synchronized map (referred to as socket_to_user) is maintained to keep track of active users and their corresponding sockets. This mapping is essential for efficient message routing.

### 3. Messaging

- *Broadcast Messaging:*  
  The /broadcast command lets users send messages to all connected clients, promoting open communication across the network. This functionality is implemented in the broadcast() function.

- *Private Messaging:*  
  With the /msg command, users can send direct, private messages to specific individuals. This feature, handled by the private_message() function, ensures that sensitive information remains confidential.

- *Group Messaging and Management:*
  - *Group Creation:*  
    Users can form new groups with the /create_group command, which is managed by the create_group() function. This supports collaborative discussions among users.
    
  - *Joining Groups:*  
    The /join_group command enables users to join existing groups, allowing them to participate in focused conversations. The process is implemented in the join_group() function.
    
  - *Group Communication:*  
    Within a group, the /group_msg command is used to send messages to all members of that group. This targeted messaging is managed by the group_message() function.
    
  - *Leaving Groups:*  
    Users can opt out of groups using the /leave_group command, giving them control over their group memberships. This process is handled by the leave_group() function.

### 4. Connection Handling

- *Disconnection:*  
  When a client disconnects or issues the /exit command, the server cleans up by removing the user from active lists and any groups they belong to. This cleanup is managed by the handle_disconnection() function.

- *Notifications:*  
  The server may also notify other users of join and leave events, keeping everyone informed about changes in the active participant list.

---

## Troubleshooting

- **Port Conflicts:**  
  Ensure that **port 12345** is available.

- **Compilation Errors:**  
  - Ensure **C++11 support** is enabled:
    BASH
    'g++ -std=c++11 -pthread -o server_grp chat_server.cpp'
    

- **Authentication Issues:**  
  - Check `users.txt` for valid usernames/passwords.

- **Connection Problems:**  
  - Verify that the **server is running**.
    

---
