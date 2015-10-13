# Window
Each activity is given a window in which to draw its user interface.

A window is basically like you think of a window on the desktop. It has a single Surface in which the contents of the window is rendered. An application interacts with the Window Manager to create windows; the Window Manager creates a Surface for each window and gives it to the application for drawing. The application can draw whatever it wants in the Surface; to the Window Manager it is just an opaque rectangle.

참고:
<http://stackoverflow.com/questions/9451755/what-is-an-android-window>