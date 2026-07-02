# Bounce Guard 🏓

A simple paddle-and-ball arcade game built in C++ using [raylib](https://www.raylib.com/). Keep the ball from hitting the bottom of the screen by moving your paddle to intercept it!

![Language](https://img.shields.io/badge/language-C%2B%2B-blue.svg)
![Library](https://img.shields.io/badge/library-raylib-orange.svg)

## 🎮 Gameplay

- A ball bounces around the screen, ricocheting off the left, right, and top walls.
- Control the paddle at the bottom of the screen to keep the ball in play.
- If the ball touches the bottom edge of the screen, it's **Game Over**.
- Press `R` to restart and try again.

## 🕹️ Controls

| Key | Action |
|-----|--------|
| `←` / `→` | Move the paddle left / right |
| `Space` | Start the game |
| `R` | Restart after game over |

## 🛠️ Built With

- **C++**
- **[raylib](https://www.raylib.com/)** — a simple and easy-to-use library for game development

## 📦 Getting Started

### Prerequisites

- A C++ compiler (e.g. `g++`, `clang++`, or MSVC)
- [raylib](https://github.com/raysan5/raylib) installed on your system

### Building

**Linux / macOS**
```bash
g++ main.cpp -o bounce_guard -lraylib -lGL -lm -lpthread -ldl -lrt -lX11
./bounce_guard
```

**Windows (MinGW)**
```bash
g++ main.cpp -o bounce_guard.exe -lraylib -lopengl32 -lgdi32 -lwinmm
bounce_guard.exe
```

> Adjust the linker flags/paths based on how raylib is installed on your system. See the [raylib wiki](https://github.com/raysan5/raylib/wiki) for platform-specific setup instructions.

## 📁 Project Structure

```
.
├── main.cpp     # Game source code
└── README.md    # This file
```
## Quick look
```cpp

#include <iostream>
#include <raylib.h>
using namespace std;

int main() {
    const int SCREEN_WIDTH = 800;
    const int SCREEN_HEIGHT = 600;

    int ball_x = SCREEN_WIDTH / 2;
    int ball_y = SCREEN_HEIGHT / 3;
    int ball_speed_x = 5;
    int ball_speed_y = 5;
    int ball_radius = 15;

    const int paddle_width = 150;
    const int paddle_height = 20;
    int paddle_x = SCREEN_WIDTH / 2 - paddle_width / 2;
    const int paddle_y = SCREEN_HEIGHT - 60;
    const int paddle_speed = 8;

    bool gameOver = false;
    bool gameStarted = false;

    cout << "Hello World" << endl;

    InitWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "My first RAYLIB program!");
    SetTargetFPS(60);

    while (!WindowShouldClose()) {
        if (!gameOver) {
            if (IsKeyDown(KEY_LEFT)) paddle_x -= paddle_speed;
            if (IsKeyDown(KEY_RIGHT)) paddle_x += paddle_speed;

            if (paddle_x < 0) paddle_x = 0;
            if (paddle_x + paddle_width > SCREEN_WIDTH) paddle_x = SCREEN_WIDTH - paddle_width;

            if (IsKeyPressed(KEY_SPACE)) {
                gameStarted = true;
            }

            if (gameStarted) {
                ball_x += ball_speed_x;
                ball_y += ball_speed_y;

                if (ball_x - ball_radius <= 0) {
                    ball_x = ball_radius;
                    ball_speed_x = abs(ball_speed_x);
                } else if (ball_x + ball_radius >= SCREEN_WIDTH) {
                    ball_x = SCREEN_WIDTH - ball_radius;
                    ball_speed_x = -abs(ball_speed_x);
                }

                if (ball_y - ball_radius <= 0) {
                    ball_y = ball_radius;
                    ball_speed_y = abs(ball_speed_y);
                }

                bool paddleHit = false;
                if (ball_speed_y > 0) {
                    float ballBottom = ball_y + ball_radius;
                    if (ballBottom >= paddle_y &&
                        ball_y - ball_radius < paddle_y + paddle_height &&
                        ball_x + ball_radius >= paddle_x &&
                        ball_x - ball_radius <= paddle_x + paddle_width) {
                        paddleHit = true;
                    }
                }

                if (paddleHit) {
                    ball_speed_y = -abs(ball_speed_y);
                    ball_y = paddle_y - ball_radius - 1;
                }

                if (ball_y + ball_radius >= SCREEN_HEIGHT) {
                    gameOver = true;
                }
            }
        }

        BeginDrawing();
            ClearBackground(RAYWHITE);

            DrawText("Prevent the ball from touching the walls!", 20, 20, 20, DARKGRAY);
            DrawText("Move with LEFT/RIGHT. Press SPACE to start.", 20, 50, 20, DARKGRAY);

            DrawRectangle(paddle_x, paddle_y, paddle_width, paddle_height, MAROON);
            DrawCircle(ball_x, ball_y, ball_radius, BLACK);

            if (!gameStarted && !gameOver) {
                DrawText("PRESS SPACE TO START", SCREEN_WIDTH / 2 - 140, SCREEN_HEIGHT / 2 - 20, 20, RED);
            }

            if (gameOver) {
                DrawText("GAME OVER", SCREEN_WIDTH / 2 - 120, SCREEN_HEIGHT / 2 - 20, 40, RED);
                DrawText("Press R to restart", SCREEN_WIDTH / 2 - 120, SCREEN_HEIGHT / 2 + 40, 20, DARKGRAY);
                if (IsKeyPressed(KEY_R)) {
                    gameOver = false;
                    gameStarted = false;
                    ball_x = SCREEN_WIDTH / 2;
                    ball_y = SCREEN_HEIGHT / 3;
                    ball_speed_x = 5;
                    ball_speed_y = 5;
                    paddle_x = SCREEN_WIDTH / 2 - paddle_width / 2;
                }
            }
        EndDrawing();
    }

    CloseWindow();
    return 0;
}


## 🚀 Future Improvements

- Add score tracking
- Increase ball speed over time for difficulty scaling
- Add sound effects
- Add multiple lives instead of instant game over

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## Resources

🎥 [Video Tutorial on YouTube](https://www.youtube.com/watch?v=PaAcVk5jUd8)
&nbsp;&nbsp;|&nbsp;&nbsp;