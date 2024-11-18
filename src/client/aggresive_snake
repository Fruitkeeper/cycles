#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
#include <thread>

using namespace std;

// Define the dimensions of the game grid
const int WIDTH = 20;
const int HEIGHT = 20;

// Represents a position on the grid
struct Position {
    int x, y;

    // Compare two positions for equality
    bool operator==(const Position &other) const {
        return x == other.x && y == other.y;
    }
};

// Represents a snake, consisting of a vector of positions
struct Snake {
    vector<Position> body; // The body of the snake, with the head at the front
};

// Snake logic class
class SnakeLogic {
    Snake mySnake; // The player's snake

public:
    SnakeLogic(Position start) {
       
        if (start.x % 2 == 0) {
            mySnake.body = {{start.x, start.y}, {start.x - 1, start.y}};
        } else {
            mySnake.body = {start}; // Initialize with only one segment
        }
    }

    // Determine if a position is inside the grid boundaries
    bool isInsideGrid(Position pos) const {
        return pos.x >= 0 && pos.x < WIDTH && pos.y >= 0 && pos.y < HEIGHT;
    }

    // Check if a position is free (not occupied by the snake itself)
    bool isCellFree(Position pos) const {
        for (const auto &segment : mySnake.body) {
            if (segment == pos) {
                return false;
            }
        }
        return true;
    }

    // Get the movement vector for a given direction
    Position getDirectionVector(char dir) {
        switch (dir) {
            case 'w': return {0, -1}; // Up
            case 'a': return {-1, 0}; // Left
            case 's': return {0, 1};  // Down
            case 'd': return {1, 0};  // Right
            default: return {0, 0};   // No movement
        }
    }

    // Choose the next move for the snake to aggressively chase a target
    Position chooseMove(Position target) {
        Position myHead = mySnake.body[0]; // Head of the snake

        // Possible moves: up, left, down, right
        vector<Position> moves = {
            {myHead.x, myHead.y - 1}, // Up
            {myHead.x - 1, myHead.y}, // Left
            {myHead.x, myHead.y + 1}, // Down
            {myHead.x + 1, myHead.y}  // Right
        };

        Position bestMove = myHead; // Default to staying in place
        int bestDistance = INT_MAX; // Start with the maximum possible distance

        for (const auto &move : moves) {
            if (isInsideGrid(move) && isCellFree(move)) {
                int distance = abs(move.x - target.x) + abs(move.y - target.y);
                if (distance < bestDistance) {
                    bestDistance = distance; // Update the best distance
                    bestMove = move;         // Update the best move
                }
            }
        }

        return bestMove;
    }

    // Update the snake's position after a move
    void update(Position newHead) {
        mySnake.body.insert(mySnake.body.begin(), newHead); // Add the new head position
        mySnake.body.pop_back(); // Remove the tail segment to maintain length
    }

    // Get the current head position of the snake
    Position getHead() const {
        return mySnake.body[0];
    }
};

// Draw the game grid and display the snake
void drawGame(const SnakeLogic &snake) {
    vector<vector<char>> grid(HEIGHT, vector<char>(WIDTH, ' ')); // Initialize the grid with empty spaces

    // Draw the snake on the grid
    for (const auto &segment : snake.getHead()) { 
        grid[segment.y][segment.x] = 'o'; // Mark snake body segments
    }

    // Draw the snake head
    grid[snake.getHead().y][snake.getHead().x] = 'O';

    // Clear the console and display the grid
    system("cls");
    for (int y = 0; y < HEIGHT; ++y) {
        for (int x = 0; x < WIDTH; ++x) {
            cout << grid[y][x];
        }
        cout << endl;
    }
}

// Main loop for controlling the snake
void gameLoop(SnakeLogic &snake, Position target) {
    while (true) {
        Position nextMove = snake.chooseMove(target); // Determine the next move
        snake.update(nextMove);                     // Update the snake's position
        drawGame(snake);                            // Draw the updated game state
        this_thread::sleep_for(chrono::milliseconds(200)); // Pause to control the game speed
    }
}

int main() {
    // Initialize the snake
    SnakeLogic snake({WIDTH / 2, HEIGHT / 2}); // Start the snake in the center

    // Define a target for the snake to chase (e.g., a static point on the grid)
    Position target = {WIDTH / 4, HEIGHT / 4};

    // Start the game loop
    gameLoop(snake, target);

    return 0;
}
