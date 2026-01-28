Skyline Engine Game Template
=============

This is a template project for a game made with the Skyline Engine (https://github.com/melodicht/skyline-engine).
It contains the bare minimum necessary to build and run a game with the engine.

# Building and running the Project
First get the base engine up and running on your system using the instructions in it's readme.
Do not put the engine project directory inside the game project directory, or vice versa.
You build this project using the same commands as building the engine, except you run the build commands in the `build` directory of this project instead of the one in the engine directory, and you add an additional option to the cmake command: `-DSKL_ENGINE_PATH={path/to/engine/folder}`
