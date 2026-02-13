                     CSN6214 OPERATING SYSTEMS
                       ASSIGNMENT TT2L - GROUP 02 (2530)

1. HOW TO COMPILE (MAKE) AND RUN
--------------------------------------------------------------------------------
Prerequisites:
- G++ Compiler (supporting C++11 or later)
- Linux Environment (Required for <sys/mman.h>, <unistd.h>, and POSIX threads)

Compilation Commands:
Since the project uses POSIX threads and real-time extensions for shared memory, you must link the 'pthread' and 'rt' libraries.

   1. Compile the Server:
      g++ server.cpp -o server -lpthread -lrt

   2. Compile the Client:
      g++ client.cpp -o client -lpthread -lrt

Alternatively, if you have a Makefile:
   make

Running the System:
   1. Start the Server first (Initializes Shared Memory and FIFO):
      ./server

   2. Start the Client (Player 1 / Host):
      ./client

   3. Start Additional Clients (Players 2-5):
      Open new terminal tabs and run:
      ./client


2. EXAMPLE COMMANDS & INTERACTION FLOW
--------------------------------------------------------------------------------
Step 1: Server Startup
   Command: ./server
   Output: "[SYSTEM] Shared Memory Created... SERVER RUNNING."

Step 2: Host Connection (Player 1)
   Command: ./client
   Interaction:
      >>> You are Player 1 (HOST) <<<
      Enter Number of Players (3-5): 3

Step 3: Joining Players
   Command: ./client
   Interaction:
      >>> Connecting... <<<
      (Wait for lobby to fill)

Step 4: Symbol Selection
   Interaction:
      >>> YOUR TURN TO CHOOSE SYMBOL! <<<
      Enter a letter (A-Z): X
      (Host chooses first, followed by Player 2, then Player 3)

Step 5: Gameplay (Making a Move)
   Interaction:
      >>> YOUR TURN! Enter grid number: 5
      (Input must be a valid grid number shown on the Reference Grid)

Step 6: Server Shutdown (Save & Exit)
   Command: Press Ctrl+C in the Server terminal.
   Action: Saves current scores to 'scores.txt' and unlinks shared memory.


3. GAME RULES SUMMARY
--------------------------------------------------------------------------------
Objective:
   Form a continuous line of your symbol horizontally, vertically, or diagonally to win.

Setup:
   - 3 to 5 players supported.
   - The grid size scales automatically based on the player count.

Turn Mechanics:
   - Players take turns entering a grid number to place their symbol.
   - A Reference Grid is displayed above the board to show valid numbers.

Time Limit:
   - Each player has exactly 20 seconds to make a move.
   - If the timer expires, the turn is forcibly skipped to the next player.

Winning & Drawing:
   - Victory: The first player to complete a line wins 1 point.
   - Draw: If the grid fills up with no winner, the game ends in a Draw.

Scores:
   - Scores are persistent and saved to 'scores.txt' automatically.


4. MODE SUPPORTED
--------------------------------------------------------------------------------
Deployment Mode: 
   Single Machine Mode.
   - The Server and all Clients run on the same physical machine.

Architecture:
   Hybrid Architecture (Multi-Process + Multi-Thread).
   - Server: Parent process manages memory and spawns threads.
   - Child Processes: Created via fork() to handle each client connection.
   - Threads: Internal 'Logger' and 'Scheduler' threads run in the background.

Communication (IPC):
   - Shared Memory: Used for game board, scores, and state updates.
   - Named Pipes (FIFO): Used as a gatekeeper for initial connections.
   - Process-Shared Mutexes: Used for synchronization.
