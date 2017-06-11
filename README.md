# Diagonal Sudoku Solver with Naked Twins

This repository contains the solution to the first project of the Udacity Artificial Intelligence Nanodegree.

## Naked Twins
Constraint propagation is a search strategy for the solution of a constrained optimization problem. During search we simply remove 
from the solution space possibilities which do not respect all constraints. In general, the more constraints we identify the faster this strategy will work because the constraints rule out  
unfeasible solutions from the search space. Naked Twins is a constraint on the peers of tuples of boxes in the sudoku
(reference here `http://www.sudokudragon.com/tutorialnakedtwins.htm`).

It was implemented in the following function:
```python
def naked_twins(values):
    """Eliminate values using the naked twins strategy.
    Args:
        values(dict): a dictionary of the form {'box_name': '123456789', ...}

    Returns:
        the values dictionary with the naked twins eliminated from peers.
    """

    # Find boxes with 2 entries
    candidates = [box for box in values.keys() if len(values[box]) == 2]

    # Collect boxes that have the same elements
    twins = [[box1,box2] for box1 in candidates for box2 in peers[box1] if set(values[box1]) == set(values[box2])]

    for b1,b2 in twins:
        print(b1, b2, values[b1])

    for box1, box2 in twins:

        peers1 = set(peers[box1])
        peers2 = set(peers[box2])

        peers_int = peers1.intersection(peers2)

        # delete the two digits from all common peers
        for peer_box in peers_int:
            for rm_val in values[box1]:
                values = assign_value(values, peer_box, values[peer_box].replace(rm_val,''))

    return values
```


# Diagonal Sudoku
Modifing the original sudoku problem to add additional constraints on the diagonals is easy, 
because we simply add the diagonals in the set of possible peers of a box.

```python
# Add diagonal units among the peers
diagonal_units = [[r+c for r,c in zip(rows,cols)] , [r+c for r,c in zip(rows[::-1],cols)]]
unitlist = row_units + column_units + square_units + diagonal_units
units = dict((s, [u for u in unitlist if s in u]) for s in boxes)
peers = dict((s, set(sum(units[s], [])) - set([s])) for s in boxes)
```

## Code

* `solution.py` - My coded solution in the function `naked_twins`
* `solution_test.py` - some test `python solution_test.py`.
* `PySudoku.py` - Visuzalizes the solution 
* `visualize.py` - Visualizes the solution
