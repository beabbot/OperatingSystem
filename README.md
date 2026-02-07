# OperatingSystem
================================================================================
                            OPERATING SYSTEM GAME PROJECT
================================================================================

1. HOW TO COMPILE
=================
This project uses a Makefile for easy compilation. Ensure you have 'g++' installed.

Method A: Using Make (Recommended)
----------------------------------
Open a terminal in the project directory and run:
   $ make

This will generate two executables:
   - server
   - client

Method B: Manual Compilation
----------------------------
If 'make' is not available, you can compile the files manually using g++:

   $ g++ server.cpp -o server -Wall -pthread -lrt
   $ g++ client.cpp -o client -Wall -pthread -lrt

*Note: The flags -pthread and -lrt are required for POSIX threads and shared memory.*

To clean up compiled files:
   $ make clean


2. HOW TO RUN (EXAMPLE COMMANDS)
================================
The game requires one server instance and multiple client instances (one per player).
Run each command in a separate terminal window.

Step 1: Start the Server
------------------------
The server must be running first to manage the shared memory and game state.
   Terminal 1:
   $ ./server

Step 2: Start the Host (Player 1)
---------------------------------
The first client to connect acts as the "Host" and sets up the game configuration.
   Terminal 2:
   $ ./client
   >>> You are Player 1 (HOST) <<<
   Enter Number of Players (3-5): 3

Step 3: Start Joining Players
-----------------------------
Run additional clients for the remaining players.
   Terminal 3 (Player 2):
   $ ./client
   >>> Connecting... <<<

   Terminal 4 (Player 3):
   $ ./client
   >>> Connecting... <<<

Once all required players (e.g., 3) have joined, the game will automatically proceed to the "Symbol Selection" phase.


3. GAME RULES SUMMARY
=====================
This is a turn-based multiplayer strategy grid game.

Objective:
   Be the first player to fill an entire row, column, or diagonal with your chosen symbol.

Setup:
   1. The Host selects the number of players (3 to 5).
   2. The grid size adapts based on the number of players:
      - 3 Players: 4x4 Grid
      - 4 Players: 5x5 Grid
      - 5 Players: 6x6 Grid

Phases:
   A. Symbol Selection: 
      Players take turns choosing a unique letter (A-Z) to represent them on the board.
   
   B. Gameplay:
      Players take turns entering a grid number (1-16, 1-25, or 1-36) to place their symbol.
      - If a spot is taken, the move is invalid.
      - There is a **20-second time limit** per turn. If a player fails to move, their turn is skipped.

Winning Condition:
   The game ends immediately if a player achieves a full line (Horizontal, Vertical, or Diagonal) of their symbol.
   If the board fills up with no winner, the game ends in a Draw.


4. MODES & FEATURES SUPPORTED
=============================
* Local Multiplayer IPC:
  Uses Shared Memory (shm) and Named Pipes (FIFO) for inter-process communication between the server and multiple client processes.

* Dynamic Grid Modes:
  - 3-Player Mode (4x4 Board)
  - 4-Player Mode (5x5 Board)
  - 5-Player Mode (6x6 Board)

* Timeout System:
  A scheduler thread on the server enforces a 20-second turn limit to prevent stalling.

* Persistence & Logging:
  - Scores are saved to 'scores.txt' upon server shutdown (Ctrl+C).
  - Game events are logged in real-time to 'game.log'.
