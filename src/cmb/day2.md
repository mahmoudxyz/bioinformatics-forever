# Day 2

## Design Tic-Tac-Toe with Python

**Project source:** [Hyperskill - Tic-Tac-Toe](https://hyperskill.org/projects/73/stages/403/implement)

### Project Structure

This project is divided into multiple stages on Hyperskill, each with specific instructions and requirements. I'm sharing the final stage here, which integrates all previous components. The final stage instructions may seem brief as they build on earlier stages where the game logic was developed incrementally.

The complete input/output specifications can be found in the link above.

### Sample Execution
```
---------
|       |
|       |
|       |
---------
3 1
---------
|       |
|       |
| X     |
---------
1 1
---------
| O     |
|       |
| X     |
---------
3 2
---------
| O     |
|       |
| X X   |
---------
0 0
Coordinates should be from 1 to 3!
1 2
---------
| O O   |
|       |
| X X   |
---------
3 3
---------
| O O   |
|       |
| X X X |
---------
X wins
```


### Code

```python 

xo_arr = [[" "] * 3 for _ in range(3)]

def display_game(arr):
    row_one =  " ".join(xo_arr[0])
    row_two =  " ".join(xo_arr[1])
    row_three =  " ".join(xo_arr[2])

    print("---------")
    print(f"| {row_one} |")
    print(f"| {row_two} |")
    print(f"| {row_three} |")
    print("---------")


# This could be made in different(shorter) way, I think
# maybe make list of set all combinations for wining 
# and then check if it in or not 
def is_win(s):
    symbol_win =  xo_arr[0] == 3 * s
    symbol_win =  symbol_win or xo_arr[1] == 3 * s
    symbol_win =  symbol_win or xo_arr[2] == 3 * s

    symbol_win =  symbol_win or (xo_arr[0][0] == s and xo_arr[0][1] == s and xo_arr[0][2] == s)
    symbol_win =  symbol_win or (xo_arr[1][0] == s and xo_arr[1][1] == s and xo_arr[1][2] == s)
    symbol_win =  symbol_win or (xo_arr[2][0] == s and xo_arr[2][1] == s and xo_arr[2][2] == s)

    symbol_win =  symbol_win or (xo_arr[0][0] == s and xo_arr[1][1] == s and xo_arr[2][2] == s)
    symbol_win =  symbol_win or (xo_arr[0][2] == s and xo_arr[1][1] == s and xo_arr[2][0] == s)

    return symbol_win


symbol = "X"

display_game(xo_arr)


while True: 

    move = input()
    
    row_coordinate = move[0]
    column_coordinate = move[2]

    if not (row_coordinate.isdigit() and column_coordinate.isdigit()):
        print("You should enter numbers!")
        continue
    else:
        row_coordinate = int(row_coordinate)
        column_coordinate = int(column_coordinate)

    if not (1 <= row_coordinate <= 3 and 1 <= column_coordinate <= 3):
        print("Coordinates should be from 1 to 3!")
        continue

    elif xo_arr[row_coordinate - 1][column_coordinate - 1] == "X" or xo_arr[row_coordinate - 1][column_coordinate - 1] == "O":
        print("This cell is occupied! Choose another one!")
        continue

    xo_arr[row_coordinate - 1][column_coordinate - 1] = symbol

    if symbol == "X":
        symbol = "O"
    else:
        symbol = "X"

    display_game(xo_arr)


    x_count =  xo_arr.count("X")
    o_count =  xo_arr.count("O")

    o_win = is_win("O")
    x_win = is_win("X")


    if x_win:
        print("X wins")
        break

    elif o_win:
        print("O wins")
        break
    elif  " " not in xo_arr[0] and " " not in xo_arr[1] and " " not in xo_arr[2] :
        print("Draw")
        break

```