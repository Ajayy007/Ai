# Ai
import math
import random

def print_board(board):
    for row in board:
        print(" ".join(row))
    print()

def is_winner(board, player):
    # Check rows, columns, and diagonals
    for i in range(3):
        if all(cell == player for cell in board[i]) or all(board[j][i] == player for j in range(3)):
            return True
    if all(board[i][i] == player for i in range(3)) or all(board[i][2 - i] == player for i in range(3)):
        return True
    return False

def is_board_full(board):
    return all(all(cell != '-' for cell in row) for row in board)

def get_available_moves(board):
    return [(i, j) for i in range(3) for j in range(3) if board[i][j] == '-']

def minimax(board, depth, maximizing_player):
    if is_winner(board, 'X'):
        return -1
    elif is_winner(board, 'O'):
        return 1
    elif is_board_full(board):
        return 0

    if maximizing_player:
        max_eval = -math.inf
        for move in get_available_moves(board):
            board[move[0]][move[1]] = 'O'
            eval = minimax(board, depth + 1, False)
            board[move[0]][move[1]] = '-'
            max_eval = max(max_eval, eval)
        return max_eval
    else:
        min_eval = math.inf
        for move in get_available_moves(board):
            board[move[0]][move[1]] = 'X'
            eval = minimax(board, depth + 1, True)
            board[move[0]][move[1]] = '-'
            min_eval = min(min_eval, eval)
        return min_eval

def get_best_move(board):
    best_move = None
    best_eval = -math.inf
    for move in get_available_moves(board):
        board[move[0]][move[1]] = 'O'
        eval = minimax(board, 0, False)
        board[move[0]][move[1]] = '-'
        if eval > best_eval:
            best_eval = eval
            best_move = move
    return best_move

def play_game():
    board = [['-' for _ in range(3)] for _ in range(3)]
    print_board(board)

    while True:
        # Player's move
        row, col = map(int, input("Enter your move (row and column, separated by space): ").split())
        if board[row][col] == '-':
            board[row][col] = 'X'
        else:
            print("Invalid move. Try again.")
            continue

        print_board(board)

        if is_winner(board, 'X'):
            print("Congratulations! You win!")
            break
        elif is_board_full(board):
            print("It's a tie!")
            break

        # AI's move
        print("AI's move:")
        ai_row, ai_col = get_best_move(board)
        board[ai_row][ai_col] = 'O'

        print_board(board)

        if is_winner(board, 'O'):
            print("AI wins! Better luck next time.")
            break
        elif is_board_full(board):
            print("It's a tie!")
            break

play_game()
