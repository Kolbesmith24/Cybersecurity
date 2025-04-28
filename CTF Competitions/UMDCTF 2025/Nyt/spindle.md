# Spindle - UMDCTF2025 Writeup

---

## Challenge Overview
We are provided with:
- A website link.
- A `.txt` file containing a large list of words.

![image](https://github.com/user-attachments/assets/c714bf73-2d06-4193-92f0-d7e222c1f12d)


Our goal is to interact with the website and solve the challenge to retrieve the flag.

---

## Reconnaissance Phase
Upon visiting the provided website, we are presented with a word puzzle game.

The objective appears to involve rearranging letters to form the word **"FANUM"**.

![image](https://github.com/user-attachments/assets/ab881719-d296-4dd6-b597-7d52ed971242)

By interacting with the puzzle, we observe that:
- We can click letters to select them.
- Only certain moves (likely adjacent or specific selections) are permitted.
- Letters can be swapped under certain conditions, but the behavior is not as simple as swapping any two letters.

### Inspecting the Source Code
Right-clicking the webpage and selecting **"View Page Source"** reveals the script responsible for handling the game's logic.

![image](https://github.com/user-attachments/assets/e92c1dd5-eee4-439c-a4ec-07da85137fb9)


From the script, we learn:
- A 5x5 letter grid is generated.
- When two letters are clicked, the move is **validated on the server-side**.
- If the move is valid, the grid updates.
- A move counter is incremented.
- Successfully forming the target word reveals the flag, while exceeding a move limit results in failure.

### Reviewing the `.txt` File

The `.txt` file contains a **large dictionary of English words**, likely intended for use within the puzzle.

![image](https://github.com/user-attachments/assets/1ad2195f-cdc1-43e3-83af-bb1643e1f914)

---

## Exploitation Phase
### Understanding the Game Mechanics
We begin by analyzing the movement rules.
The grid at the start is:

```
R1 - F N E H A
R2 - A K E L B
R3 - V M T K E
R4 - O G U Y U
R5 - P J P F N
```


After experimentation, **we discover**:
- We are **not swapping individual characters**.
- Instead, we are **reversing entire words** by selecting the **first and last letter** of a word.
- This reversal **also flips** the letters within the word.

In short: **clicking on two endpoints reverses the entire segment**.

---
### Initial Manual Attempts
Through manual trial-and-error, we attempt to create "FANUM".

Successfully forming FANUM in **41 moves**:

![image](https://github.com/user-attachments/assets/ab9aa6a8-14ea-49d6-9599-4a2720eb44c0)


However, the game indicates failure due to exceeding the move limit.

---

Another attempt yields **31 moves**, and we confirm FANUM is approved horizontally and vertically:

![image](https://github.com/user-attachments/assets/52bcce49-023e-4da0-b3c9-7bf9de818c56)
![image](https://github.com/user-attachments/assets/88c3df84-a000-4347-93cd-4b7129d3d386)

Attempting to form "MUNAF" (FANUM reversed) **does not** yield success either:

![image](https://github.com/user-attachments/assets/7b0a8d0f-14f1-4985-834e-6dcf1b411106)


Despite finding **5–6 different methods** to reach FANUM in around **10 moves**, they all still exceed the hidden move limit.

![image](https://github.com/user-attachments/assets/cec3c557-a5ac-42b2-a93a-8b63ab404410)

At this point, it becomes clear that **solving the puzzle manually** would be **extremely time-consuming and inefficient**.

---

### Automating the Solution
To automate the solution process, I employed a Python script.  
The script was designed to:
- Search for known words (from `dictionary.txt`) within the grid.
- Reverse the detected words according to game mechanics.
- Optimize moves to form **"FANUM"** as efficiently as possible.

#### Script Overview
The script consists of:
- **Grid Utilities**: Finding words horizontally/vertically and applying reversals.
- **Heuristics**: Using a **longest common subsequence** (LCS) metric to prioritize paths closer to forming "FANUM".
- **Solver**: Implements a **priority queue (heap)** to search the solution space efficiently using an A*-like approach.

Here’s a summary of the key functions:

|Function|Purpose|
|:--|:--|
|`find_all_words()`|Find all possible valid words in the current grid.|
|`reverse_in_grid()`|Reverse the letters between two clicked positions (following puzzle rules).|
|`target_in_grid()`|Check if the target word ("FANUM") appears in the grid.|
|`longest_common_subsequence()`|Used for smarter move prioritization.|
|`solve()`|Main driver that searches for the sequence of moves that form "FANUM".|

The complete Python script used is:
```python
import heapq
import time
from termcolor import colored

# --- Grid utilities ---


def find_all_words(grid, word_set):
    found = []
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # Horizontal and vertical only
    for i in range(5):
        for j in range(5):
            for dx, dy in directions:
                word = ""
                path = []
                x, y = i, j
                while 0 <= x < 5 and 0 <= y < 5:
                    word += grid[x][y]
                    path.append((x, y))
                    if word in word_set and len(word) >= 2:
                        found.append((path[0], path[-1], word))
                    x += dx
                    y += dy
    return found


def reverse_in_grid(grid, start, end):
    new_grid = [row[:] for row in grid]  # Deep copy
    i1, j1 = start
    i2, j2 = end
    if i1 == i2:  # Horizontal
        if j1 > j2:
            j1, j2 = j2, j1
        segment = grid[i1][j1 : j2 + 1]
        new_grid[i1][j1 : j2 + 1] = segment[::-1]
    elif j1 == j2:  # Vertical
        if i1 > i2:
            i1, i2 = i2, i1
        segment = [grid[x][j1] for x in range(i1, i2 + 1)]
        for idx, x in enumerate(range(i1, i2 + 1)):
            new_grid[x][j1] = segment[::-1][idx]
    else:
        raise ValueError("Only horizontal or vertical moves are allowed.")
    return new_grid


def target_in_grid(grid, target):
    for row in grid:
        if target in "".join(row):
            return True
    for col in zip(*grid):
        if target in "".join(col):
            return True
    return False


# --- Heuristics ---


def longest_common_subsequence(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m):
        for j in range(n):
            if s1[i] == s2[j]:
                dp[i + 1][j + 1] = dp[i][j] + 1
            else:
                dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j])
    return dp[m][n]


def smart_heuristic(grid, target):
    max_lcs = 0
    for row in grid:
        row_str = "".join(row)
        max_lcs = max(max_lcs, longest_common_subsequence(row_str, target))
    for col in zip(*grid):
        col_str = "".join(col)
        max_lcs = max(max_lcs, longest_common_subsequence(col_str, target))
    return len(target) - max_lcs


# --- Visualization ---


def print_grid_with_highlight(grid, start, end):
    highlighted_grid = [row[:] for row in grid]
    i1, j1 = start
    i2, j2 = end

    if i1 == i2:  # Horizontal
        if j1 > j2:
            j1, j2 = j2, j1
        for j in range(j1, j2 + 1):
            if (i1, j) == start:
                highlighted_grid[i1][j] = colored(highlighted_grid[i1][j], "red")
            elif (i1, j) == end:
                highlighted_grid[i1][j] = colored(highlighted_grid[i1][j], "green")
            else:
                highlighted_grid[i1][j] = colored(highlighted_grid[i1][j], "yellow")
    elif j1 == j2:  # Vertical
        if i1 > i2:
            i1, i2 = i2, i1
        for i in range(i1, i2 + 1):
            if (i, j1) == start:
                highlighted_grid[i][j1] = colored(highlighted_grid[i][j1], "red")
            elif (i, j1) == end:
                highlighted_grid[i][j1] = colored(highlighted_grid[i][j1], "green")
            else:
                highlighted_grid[i][j1] = colored(highlighted_grid[i][j1], "yellow")

    for row in highlighted_grid:
        print(" ".join(row))
    print()


# --- Solver ---


def solve(grid, target_word, word_list):
    word_set = set(word_list)
    visited = set()
    heap = []
    node_counter = 0
    solution_moves = []

    heapq.heappush(heap, (0, grid, []))

    while heap:
        cost, current_grid, path = heapq.heappop(heap)
        node_counter += 1
        if node_counter % 10000 == 0:
            print(
                f"[Progress] Processed {node_counter} nodes, queue size: {len(heap)}, current path length: {len(path)}"
            )

        grid_tuple = tuple(tuple(row) for row in current_grid)
        if grid_tuple in visited:
            continue
        visited.add(grid_tuple)

        if target_in_grid(current_grid, target_word):
            print(f"[Finished] Found target after processing {node_counter} nodes.")
            solution_moves = path  # Store the solution path
            break

        for start, end, word in find_all_words(current_grid, word_set):
            new_grid = reverse_in_grid(current_grid, start, end)
            new_path = path + [(start, end)]
            new_cost = len(new_path) + smart_heuristic(new_grid, target_word)
            heapq.heappush(heap, (new_cost, new_grid, new_path))

    # --- Visualization after finding the solution ---
    if solution_moves:
        print("\nSolution steps:")
        current_grid = [row[:] for row in grid]
        for idx, move in enumerate(solution_moves):
            start, end = move
            print(f"Step {idx + 1}: reverse {start} to {end}")
            print_grid_with_highlight(
                current_grid, start, end
            )  # Print grid after each move
            current_grid = reverse_in_grid(current_grid, start, end)  # Apply the move

    else:
        print(
            f"[End] Exhausted all nodes, target not found after {node_counter} nodes."
        )
    return solution_moves


# --- Main ---

if __name__ == "__main__":
    initial_grid = [
        ["F", "N", "E", "H", "A"],
        ["A", "K", "E", "L", "B"],
        ["V", "M", "T", "K", "E"],
        ["O", "G", "U", "Y", "U"],
        ["P", "J", "P", "F", "N"],
    ]

    word_list = open("./dictionary.txt").read().splitlines()
    target = "FANUM"

    start_time = time.time()
    solution = solve(initial_grid, target, word_list)
    end_time = time.time()

    print(f"Solve function took {end_time - start_time:.2f} seconds.")
# Credit to Thr for initial script
```


---

#### Script Results

Running the script, it successfully generates a sequence of moves:

![image](https://github.com/user-attachments/assets/67558b76-3926-432a-9a6e-14fe172b35a4)
![image](https://github.com/user-attachments/assets/30b9baa6-4cda-4121-b385-118e97a9c456)



Testing the resulting moves in the browser **successfully reveals the flag**:

![image](https://github.com/user-attachments/assets/653030c8-fc2d-4b0b-9854-8b1664811345)


---
