---
layout: "../../layouts/BlogPost.astro"
title: "Python Tkinter MP3 Player"
description: "This tutorial will show you how to create a python mp3 player application with Tkinter"
image: "/python.png"
---

1. Start by importing the tkinter module, the filedialog module from tkinter, and the os module. These modules will be used to create the user interface, browse for files, and play the selected file.

```python
import tkinter as tk
from tkinter import filedialog
import os
```

2. Next, create the root window for the application. This is the main window that will contain all of the other widgets in the application.

```python
# create the root window
root = tk.Tk()
```

3. Add a title to the window using the `title` method. This will help to
   identify the window and make it look more professional

```python
root.title("Mp3 Player")
```

4. Add a description label to the window using the Label widget. This will provide some context for the user and explain what the application does.

```python
# add a description label
description_label = tk.Label(
    root,
    text="Use the buttons below to browse for and play mp3 files:"
)
description_label.pack()
```

5. Create a variable to store the selected filepath. This will be used to keep track of the selected file and make it available to the other functions in the application.

```python
# create a variable to store the selected filepath
selected_file = None
```

6. Create a function to allow the user to browse for mp3 files. This function will use the askopenfilename method from the filedialog module to display a file selection dialog and allow the user to choose a file. If a file is selected, the function will store the filepath in the selected_file variable.

```python
# create a function to browse for mp3 files
def browse_mp3():
    # ask the user to select a file
    filepath = filedialog.askopenfilename(
        initialdir=os.getcwd(),
        title="Select an mp3 file",
        filetypes=[("mp3 files", "*.mp3")]
    )
    # if a file was selected, store the filepath in a variable
    if filepath:
        global selected_file
        selected_file = filepath
```

7. Create a function to play the selected mp3 file. This function will check if a file has been selected, and if so, it will use the startfile method from the os module to play the file using the default application.

```python
# create a function to play the selected mp3 file
def play_mp3():
    # if a file has not been selected, show an error message
    if not selected_file:
        tk.messagebox.showerror(
            "Error",
            "No mp3 file selected"
        )
        return
    # if a file has been selected, play it using the default application
    os.startfile(selected_file)
```

8. Create a button to allow the user to browse for mp3 files. This button will use the Button widget from tkinter and call the browse_mp3 function when clicked.

```python
# create a button to allow the user to browse for mp3 files
browse_button = tk.Button(
    root,
    text="Browse for mp3",
    command=browse_mp3
)
browse_button.pack()
```

9. Create a button to play the selected mp3 file. This button will use the Button widget from tkinter and call the play_mp3 function when clicked.

```python
# create a button to play the selected mp3 file
play_button = tk.Button(
    root,
    text="Play mp3",
    command=play_mp3
)
play_button.pack()
```

10. Add some padding around the buttons to make them stand out more and look nicer. You can use a loop to iterate over all of the child widgets in the root window and call the pack_configure method with the padx and pady options set to 10.

```python
# add some padding around the buttons
for child in root.winfo_children():
    child.pack_configure(padx=10, pady=10)
```

11. Start the main event loop for the application. This will cause the application to run until the user closes the window or quits the application.

```python
# start the main loop
root.mainloop()
```

To run your app, you can click run at the top, and click run module (if you are using Python IDLE), or you can double click your .py
file. Before you run your app, you should find an mp3 file online,
or you can install this one: [MP3 File]("/music.mp3")
