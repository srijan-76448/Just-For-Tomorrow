# AI & Programming Examples
This repository contains solutions for introductory Artificial Intelligence concepts implemented in Prolog and Python, along with a classic game implementation.

## Introduction to Prolog: _Family Tree_
This section demonstrates basic Prolog facts and rules to define family relationships, specifically focusing on parental and grandparental connections.

### Problem Description
The goal is to establish a family tree using Prolog predicates. We define individuals by gender and then specify parent/2 relationships. From these basic facts, we infer more complex relationships like father/2, mother/2, sibling/2, and grandparent/2.

### Program
```Prolog
% Facts: Gender of individuals
male(john).
male(peter).
male(david).
female(susan).
female(mary).
female(lisa).
female(anne).

% Facts: Parent relationships (Parent, Child)
parent(john, mary).     % John is parent of Mary
parent(susan, mary).    % Susan is parent of Mary
parent(john, peter).    % John is parent of Peter
parent(susan, peter).   % Susan is parent of Peter
parent(david, susan).   % David is parent of Susan
parent(anne, susan).    % Anne is parent of Susan
parent(david, lisa).    % David is parent of Lisa
parent(anne, lisa).     % Anne is parent of Lisa

% Rule: Father (F is father of C if F is male and F is parent of C)
father(F, C) :-
    male(F),
    parent(F, C).

% Rule: Mother (M is mother of C if M is female and M is parent of C)
mother(M, C) :-
    female(M),
    parent(M, C).

% Rule: Sibling (X and Y are siblings if they share the same father and mother, and are not the same person)
sibling(X, Y) :-
    father(F, X),
    father(F, Y),
    mother(M, X),
    mother(M, Y),
    X \= Y. % Ensure X and Y are not the same person

% Rule: Grandparent (GP is grandparent of GC if GP is parent of P, and P is parent of GC)
grandparent(GP, GC) :-
    parent(GP, P),
    parent(P, GC).

% Rule: Grandfather (GF is grandfather of GC if GF is male and GF is grandparent of GC)
grandfather(GF, GC) :-
    male(GF),
    grandparent(GF, GC).

% Rule: Grandmother (GM is grandmother of GC if GM is female and GM is grandparent of GC)
grandmother(GM, GC) :-
    female(GM),
    grandparent(GM, GC).
```


### How to Run
 * Save the code as family_tree.pl.
 * Open a Prolog interpreter (e.g., SWI-Prolog).
 * Load the file: [family_tree].
 * Query the relationships:
   * ?- sibling(mary, peter).
   * ?- grandparent(GP, mary).
   * ?- father(F, mary).

##Graph Traversal: *BFS & DFS in Python*
This section provides Python implementations for two fundamental graph traversal algorithms: Breadth-First Search (BFS) and Depth-First Search (DFS).

### Problem Description
Given a graph represented as an adjacency list, implement functions to perform BFS and DFS starting from a specified node. The traversal should print the nodes in the order they are visited.
Graph Representation
The graph is represented using a dictionary where keys are nodes and values are lists of their direct neighbors.

```python
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}
```

### Program
```Python
from collections import deque

# Graph representation (Adjacency List)
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

def bfs(graph, start_node):
    """
    Performs Breadth-First Search on a graph.
    Args:
        graph (dict): The graph represented as an adjacency list.
        start_node: The node to start the traversal from.
    Returns:
        str: A space-separated string of visited nodes.
    """
    visited = set()
    queue = deque([start_node])
    result = []

    while queue:
        node = queue.popleft()
        if node not in visited:
            result.append(node)
            visited.add(node)
            for neighbor in graph[node]:
                if neighbor not in visited:
                    queue.append(neighbor)
    return " ".join(result)

def dfs(graph, start_node):
    """
    Performs Depth-First Search on a graph.
    Args:
        graph (dict): The graph represented as an adjacency list.
        start_node: The node to start the traversal from.
    Returns:
        str: A space-separated string of visited nodes.
    """
    visited = set()
    stack = [start_node]
    result = []

    while stack:
        node = stack.pop()
        if node not in visited:
            result.append(node)
            visited.add(node)
            # Add neighbors to stack in reverse order for consistent output (e.g., A C F E B D)
            # The exact order depends on how neighbors are added to the stack.
            for neighbor in sorted(graph[node], reverse=True):
                if neighbor not in visited:
                    stack.append(neighbor)
    return " ".join(result)

if __name__ == "__main__":
    print("BFS Traversal:", bfs(graph, 'A'))
    print("DFS Traversal:", dfs(graph, 'A'))
```

### How to Run
 * Save the code as graph_traversal.py.
 * Run from your terminal: python graph_traversal.py
 * Expected Output:
   BFS Traversal: A B C D E F
DFS Traversal: A C F E B D

## Classic Game: *Tic-Tac-Toe in Python*

This section presents a command-line Tic-Tac-Toe game implementation in Python for two players.

### Problem Description
Develop a two-player Tic-Tac-Toe game where players take turns marking cells on a 3x3 board. The game should detect wins (three in a row, column, or diagonal) and draws.

### Program 
```Python
def print_board(board):
    """Prints the current state of the Tic-Tac-Toe board."""
    for i, row in enumerate(board):
        print("|".join(row))
        if i < 2:
            print("-----")

def check_win(board, player):
    """Checks if the given player has won the game."""
    # Check rows
    for row in board:
        if all(s == player for s in row):
            return True
    # Check columns
    for col in range(3):
        if all(board[row][col] == player for row in range(3)):
            return True
    # Check diagonals
    if all(board[i][i] == player for i in range(3)): # Main diagonal
        return True
    if all(board[i][2 - i] == player for i in range(3)): # Anti-diagonal
        return True
    return False

def is_board_full(board):
    """Checks if the board is completely filled."""
    for row in board:
        for cell in row:
            if cell == ' ':
                return False
    return True

def tic_tac_toe():
    """Main function to run the Tic-Tac-Toe game."""
    board = [[' ' for _ in range(3)] for _ in range(3)]
    players = ['X', 'O']
    current_player_idx = 0

    print("Welcome to Tic-Tac-Toe!")
    print_board(board)

    while True:
        player = players[current_player_idx]
        print(f"\nPlayer {player}'s turn.")

        try:
            row = int(input("Enter row (0, 1, 2): "))
            col = int(input("Enter column (0, 1, 2): "))
        except ValueError:
            print("Invalid input. Please enter a number between 0 and 2.")
            continue

        if not (0 <= row <= 2 and 0 <= col <= 2):
            print("Row or column out of bounds. Please enter values between 0 and 2.")
            continue

        if board[row][col] == ' ':
            board[row][col] = player
        else:
            print("That cell is already taken. Please choose an empty cell.")
            continue

        print_board(board)

        if check_win(board, player):
            print(f"Player {player} wins! Congratulations!")
            break
        elif is_board_full(board):
            print("It's a draw! The board is full.")
            break

        current_player_idx = 1 - current_player_idx # Switch player (0 to 1, 1 to 0)

if __name__ == "__main__":
    tic_tac_toe()
```

### How to Run
 * Save the code as tic_tac_toe.py.
 * Run from your terminal: python tic_tac_toe.py
 * Follow the on-screen prompts to play the game.
