import tkinter as tk
from tkinter import messagebox

def is_valid(board, row, col, num):
    
    for i in range(9):
        if board[row][i] == num or board[i][col] == num:
            return False
    
    
    start_row, start_col = 3 * (row // 3), 3 * (col // 3)
    for i in range(3):
        for j in range(3):
            if board[start_row + i][start_col + j] == num:
                return False
    return True


def solve_sudoku(board):
    for row in range(9):
        for col in range(9):
            if board[row][col] == 0:
                for num in range(1, 10):
                    if is_valid(board, row, col, num):
                        board[row][col] = num
                        if solve_sudoku(board):
                            return True
                        board[row][col] = 0
                return False
    return True

def get_board():
    board = []
    for row in range(9):
        current_row = []
        for col in range(9):
            value = entries[row][col].get()
            # Handle empty or invalid input
            if value.isdigit() and 1 <= int(value) <= 9:
                current_row.append(int(value))
            else:
                current_row.append(0)
        board.append(current_row)
    return board

def set_board(board):
    for row in range(9):
        for col in range(9):
            entries[row][col].delete(0, tk.END)
            if board[row][col] != 0:
                entries[row][col].insert(0, str(board[row][col]))


def solve():
    board = get_board()
    if solve_sudoku(board):
        set_board(board)
        messagebox.showinfo("Success", "Sudoku Solved Successfully!")
    else:
        messagebox.showerror("Error", "No solution exists!")

# Clear button action
def clear_board():
    for row in range(9):
        for col in range(9):
            entries[row][col].delete(0, tk.END)


root = tk.Tk()
root.title("Sudoku Solver")


entries = [[tk.Entry(root, width=3, font=('Arial', 16), justify='center') for _ in range(9)] for _ in range(9)]


for i in range(9):
    for j in range(9):
        entries[i][j].grid(row=i, column=j, padx=2, pady=2)
        # Add thicker border for 3x3 grids
        if i % 3 == 2 and i != 8:
            entries[i][j].grid(pady=(2, 5))
        if j % 3 == 2 and j != 8:
            entries[i][j].grid(padx=(2, 5))


tk.Button(root, text="Solve", command=solve, font=('Arial', 12), width=10).grid(row=10, column=0, columnspan=4, pady=10)
tk.Button(root, text="Clear", command=clear_board, font=('Arial', 12), width=10).grid(row=10, column=5, columnspan=4, pady=10)

root.mainloop()
