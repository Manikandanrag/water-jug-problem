# water-jug-problem
## AIM
Solve the Water Jug problem: Given 3 jugs A,B,C with capacities 8,5 and 3 liters respectively but are  not calibrated (i.e. no measuring mark will be there). Jug A is filled with 8 liters of water. By a series of pouring back and forth among the 3 jugs, divide the 8 liters into 2 equal parts i.e.  4 liters in jug A and 4 liters in jug B. 
## STEPS
Step	Action	Resulting State (A, B, C)	Explanation
1	Pour A → B	(3, 5, 0)	Fill jug B (5 L) from A.
2	Pour B → C	(3, 2, 3)	Fill jug C (3 L) from B.
3	Pour C → A	(6, 2, 0)	Empty C into A.
4	Pour B → C	(6, 0, 2)	Pour 2 L from B into C.
5	Pour A → B	(1, 5, 2)	Fill B (5 L) from A.
6	Pour B → C	(1, 4, 3)	Fill C (3 L) from B (only 1 L fits).
7	Pour C → A	(4, 4, 0)	Empty C into A. ✅ Goal reached.
## Program
```
from collections import deque

# capacities of jugs
CAP_A, CAP_B, CAP_C = 8, 5, 3

# initial and goal states
initial = (8, 0, 0)
goal = (4, 4, 0)

# function to pour water from one jug to another
def pour(state, from_jug, to_jug, caps):
    amounts = list(state)
    if amounts[from_jug] == 0 or amounts[to_jug] == caps[to_jug]:
        return None  # nothing to pour

    # calculate pour amount
    pour_amount = min(amounts[from_jug], caps[to_jug] - amounts[to_jug])

    # pour water
    amounts[from_jug] -= pour_amount
    amounts[to_jug] += pour_amount
    return tuple(amounts)

# BFS to find solution
def bfs_water_jug():
    queue = deque([(initial, [])])
    visited = set([initial])
    caps = (CAP_A, CAP_B, CAP_C)

    while queue:
        state, path = queue.popleft()

        # check goal
        if state == goal:
            return path + [state]

        # try all possible pour operations (from, to)
        for i in range(3):
            for j in range(3):
                if i != j:
                    new_state = pour(state, i, j, caps)
                    if new_state and new_state not in visited:
                        visited.add(new_state)
                        queue.append((new_state, path + [state]))

    return None

# run BFS and display solution
solution = bfs_water_jug()

if solution:
    print("Steps to divide water equally:")
    for step in solution:
        print(f"A={step[0]}, B={step[1]}, C={step[2]}")
else:
    print("No solution found.")

```

## OUTPUT
<img width="295" height="169" alt="image" src="https://github.com/user-attachments/assets/0c896969-fbb8-4083-8ea7-e97ee0fa94f0" />
