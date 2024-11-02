# Reducing-Quorum-Sizes-in-Oblivious-Paxos

import numpy as np
from random import choice

class GridQuorum:
    def __init__(self, rows, cols, threshold):
        """
        Initialize the grid with specified rows and columns.

        Parameters:
        - rows (int): Number of rows in the grid.
        - cols (int): Number of columns in the grid.
        - threshold (int): Minimum number of nodes required in intersection of Q1 and Q2 for quorum.
        """
        self.rows = rows
        self.cols = cols
        self.threshold = threshold
        self.grid = np.arange(1, rows * cols + 1).reshape(rows, cols)  # Create grid of nodes with unique IDs
        self.quorum_q1 = []  # Stores nodes in Q1 (columns)
        self.quorum_q2 = []  # Stores nodes in Q2 (rows)
        
    def select_quorum(self):
        """
        Randomly select Q1 as a column and Q2 as a row to form a quorum.
        """
        col = choice(range(self.cols))
        row = choice(range(self.rows))
        self.quorum_q1 = self.grid[:, col].tolist()  # Q1 as one column
        self.quorum_q2 = self.grid[row, :].tolist()  # Q2 as one row
        print(f"Selected Column for Q1: {self.quorum_q1}")
        print(f"Selected Row for Q2: {self.quorum_q2}")
        
    def check_quorum_intersection(self):
        """
        Check if Q1 and Q2 intersect at or above the threshold.
        
        Returns:
        - (bool): True if quorum intersection meets threshold, otherwise False.
        """
        intersection = set(self.quorum_q1).intersection(self.quorum_q2)
        print(f"Intersection of Q1 and Q2: {intersection}")
        return len(intersection) >= self.threshold
    
    def run_consensus(self):
        """
        Simulate a consensus attempt using the selected grid quorum structure.
        """
        print("Attempting to reach consensus with grid quorum...")
        self.select_quorum()
        
        if self.check_quorum_intersection():
            print("Consensus achieved! Quorum requirements met.")
        else:
            print("Consensus failed. Intersection does not meet quorum threshold.")
            
# Example usage:
# Create a 4x4 grid with a threshold of 1 for quorum intersection (for demonstration)
rows, cols = 4, 4
threshold = 1
grid_quorum = GridQuorum(rows, cols, threshold)

# Run a consensus attempt
grid_quorum.run_consensus()
