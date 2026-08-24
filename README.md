# 2048solver

A little program that watches the 2048 game on your screen and tells you which
move to make next. It does not play for you — it just looks at the board and
draws the suggested move (LEFT / RIGHT / UP / DOWN) next to it.

Based on https://github.com/TheoKanning/2048-Python-Bot

---

## Before you start

**This only works on Windows.** It uses Windows-only features to read the
screen, so it will not run on a Mac or on Linux.

You will need three things: Python, two extra Python packages, and the 2048
game open. Steps below, in order.

---

## Step 1 — Install Python

1. Go to https://www.python.org/downloads/
2. Click the big yellow **Download Python** button.
3. Run the file you downloaded.
4. **Important:** on the very first screen of the installer, tick the box that
   says **"Add python.exe to PATH"** (it is at the bottom, and it is *not*
   ticked by default). If you miss this, the commands below will not work.
5. Click **Install Now** and wait for it to finish.

To check it worked:

1. Press the **Windows key**, type `powershell`, and press Enter. A blue or
   black window with a text prompt opens. This is the "terminal" — every
   command below gets typed into this window, followed by Enter.
2. Type this and press Enter:

   ```
   python --version
   ```

   You should see something like `Python 3.13.1`. If you instead get an error
   or the Microsoft Store opens, Python is not installed correctly — redo the
   install and make sure the "Add python.exe to PATH" box is ticked.

---

## Step 2 — Install the two packages the program needs

In the same terminal window, type this and press Enter:

```
python -m pip install pillow pywin32
```

Wait until it finishes (you will see a lot of text scroll by, ending in
something like `Successfully installed ...`). You only ever have to do this
once.

- **pillow** is what lets the program take a picture of your screen.
- **pywin32** is what lets it read your keypresses and draw the overlay.

---

## Step 3 — Download this project

If you already have the folder with `solver.py` in it, skip to Step 4.

Otherwise, on the GitHub page for this project click the green **Code** button,
then **Download ZIP**. Save it somewhere easy to find, then right-click the ZIP
file and choose **Extract All**. Remember where you put it — for example
`C:\Users\YourName\2048solver`.

---

## Step 4 — Open the game and line it up

1. Open https://play2048.co in your browser.
2. Move the browser window to the **left half of your screen** (click the
   window, then press **Windows key + Left arrow** — Windows will snap it to
   the left half).
3. Scroll so the whole 4x4 board is visible.

The program looks at a fixed rectangle of your screen, so the board has to be
roughly where the program expects it. Step 6 shows you how to check and fix
this.

---

## Step 5 — Run the program

1. Open the folder containing `solver.py` in File Explorer.
2. Click the address bar at the top (where the folder path is shown), type
   `powershell`, and press Enter. A terminal opens already pointed at that
   folder.
3. Type this and press Enter:

   ```
   python solver.py
   ```

A red rectangle and 16 green dots appear on top of your screen. That is the
overlay. **Press Esc to close it.**

---

## Step 6 — Check the alignment (do this the first time)

Look at the overlay:

- The **red rectangle** is the area the program is reading.
- The **16 green dots** should each sit inside one tile of the board.
- Next to each dot is the number the program thinks that tile is.

**If the numbers next to the dots match the numbers on the board — you're
done.** Go to Step 7.

**If you see `?` next to the dots**, or the dots are not on the board at all,
the rectangle is in the wrong place. To fix it:

1. Press **Esc** to close the overlay.
2. Open `solver.py` in Notepad (right-click the file → **Open with** →
   **Notepad**).
3. Near the top you will see these four lines:

   ```
   x_pad = 222
   y_pad = 351
   x_max = 722
   y_max = 851
   ```

   `x_pad` and `y_pad` are the top-left corner of the rectangle, counted in
   pixels from the top-left corner of your screen. `x_max` and `y_max` are the
   bottom-right corner.

4. Nudge the numbers and re-run. If the dots need to move **right**, make
   `x_pad` and `x_max` *bigger* by the same amount. To move them **down**, make
   `y_pad` and `y_max` bigger by the same amount. Keep the rectangle 500x500
   (that is, `x_max` should stay exactly 500 more than `x_pad`, and likewise
   for `y`) — the dot positions assume that size.
5. Save the file (Ctrl+S), run `python solver.py` again, and check.

Easier alternative: instead of moving the numbers, drag the **browser window**
until the board lines up with the red rectangle. That usually takes less
fiddling.

---

## Step 7 — Play

Leave the overlay running and play 2048 normally with your arrow keys. After
each move the overlay re-reads the board and updates the suggested move in red
text just left of the rectangle's top-left corner.

The overlay is click-through, so it will not get in the way of the game.

Press **Esc** when you want to stop.

---

## The other two modes

Running `python solver.py` with no extra words gives you the overlay. There are
two other modes, typed the same way in the terminal:

| Command | What it does |
| --- | --- |
| `python solver.py` | The overlay. This is the normal one. |
| `python solver.py --play` | No overlay. Suggestions are printed as text in the terminal, one second after each arrow key you press. Press Esc to quit. |
| `python solver.py --grab` | Takes one picture of the rectangle, marks the dots on it, and saves it as `overlay_debug.png` next to `solver.py`. Handy for checking alignment if the overlay is hard to read. |

---

## If something goes wrong

**`python: The term 'python' is not recognized...`**
Python is not on your PATH. Reinstall it and tick "Add python.exe to PATH".

**`ModuleNotFoundError: No module named 'PIL'`**
Pillow did not install. Run `python -m pip install pillow` again and read the
output for errors.

**`ModuleNotFoundError: No module named 'win32api'`**
pywin32 did not install. Run `python -m pip install pywin32` again.

**`ModuleNotFoundError: No module named 'tkinter'`**
Rare, but it means Python was installed without the tkinter option. Reinstall
Python and leave all the optional-feature checkboxes ticked.

**The overlay shows `?` next to every dot, with three numbers after it**
Those three numbers are the colour it found. Either the rectangle is misplaced
(see Step 6) or your browser is applying a filter — turn off dark mode, browser
extensions that change page colours, and any screen-tinting tool like Night
Light or f.lux, then try again.

**Everything is offset by a fixed amount**
This is usually Windows display scaling. In **Settings → System → Display**,
set **Scale** to **100%**, then re-run.

**The overlay freezes for a moment after each move**
That is normal — it is thinking about the next move. It comes back on its own.
