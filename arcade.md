# Arcade Games

For my two projects in my Software Systems class, I worked on arcade games in C. For my first project, my group made Pong, but focused on a system where we could expand the number of games in the app if we wanted to. In the second project, we made a game we called MineSnake - a mix of Minesweeper and Snake. Please check out our [arcade games repo](https://github.com/olincollege/arcade-games) and our [MineSnake repo](https://github.com/olincollege/minesnake).

## My Work

For both projects, we used a Model-View-Controller (MVC) architecture. I worked on both the Model and the Controller in both projects. For Pong, I made a [State struct](https://github.com/olincollege/arcade-games/blob/main/src/states.h#L14-L30) and a [State Manager struct](https://github.com/olincollege/arcade-games/blob/main/src/states.h#L33-L48). The `State` struct was essentially an abstract class, storing pointers to functions for initializing, updating, drawing, and tearing down that state. `StateManager` itself is just a stack of `State`s, with the top `State` in the stack being the active one. This way, it is easy to add states on top of other states, and go back to previous states. For instance, the start menu, the Pong game, and the pause menu would each be its own `State`. If the app is in the Pong state, and the user requests a pause, the pause state can be added to the top of the stack and removed when requested, without affecting the Pong state itself. Instead of reloading the entire state when the user unpauses, the code can simply resume calling the update and draw methods of the Pong state. Finally, I also created the [`update` method for the Pong state](https://github.com/olincollege/arcade-games/blob/main/src/pong/pong.c#L76-L116), along with [controller functionality](https://github.com/olincollege/arcade-games/blob/main/src/pong/pong_controller.c) to keep track of the user holding keys (as the library we used only alerted the code of presses and releases).

For MineSnake, we focused more on the game itself and did not implement the `StateManager` feature we had for the first project. Our idea for MineSnake was to combine the real-time difficulty of snake with the slow, methodic difficulty of MineSweeper to make a new game. The snake would be moving on the Minesweeper board, and squares would be revealed as the snake "ate" them. But be warned, because if the snake eats a bomb it's game over. The goal of the game is for the snake to find all the food hidden throughout the minesweeper field and eat it, but (as with normal Snake), it gets bigger with each bit of food it eats. Again, I focused on both the [model](https://github.com/olincollege/minesnake/blob/main/src/model.h) and [controller](https://github.com/olincollege/minesnake/blob/main/src/controller.h) for this game. As this is a much more complicated game than Pong, the model needed a lot more work from bomb placement to advancing the snake properly to revealing new regions of the board. If you would like to try out the final game, download the code and follow the instructions in the [README](https://github.com/olincollege/minesnake/blob/main/README.md)