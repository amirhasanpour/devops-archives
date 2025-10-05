# Vim Setup & Basics Cheat Sheet

# **Step 1: Install Vim**
 On Ubuntu/Debian:
```bash
sudo apt update && sudo apt install vim -y
```

 On CentOS / RHEL:
```bash
sudo yum install vim -y
```

 On macOS (with Homebrew):
```bash
brew install vim
```

# **Step 2: Open a File**
```bash
vim filename.txt
```
If the file exists → it opens.
If it doesn’t → it will be created.



# **Step 3: Vim Modes**


Normal mode → default mode (for navigation/commands). <br>
Insert mode → for writing text (`i`). <br>
Visual mode → for selecting text (`v`). <br>
Command mode → for commands like save/quit (`:` in Normal mode).


# **Step 4: Start Editing Text**
1-Press i to enter Insert mode.
2-Write your text.
3-Press Esc to return to Normal mode.

# **Step 5: Save and Quit**
From Normal mode (Esc first):
-Save and quit:
```bash
:wq
```

-Save only:
```bash
:w
```

-Quit without saving:
```bash
:q!
```

# **Step 6: Navigation**
(`h`) → move left

(`l`) → move right

(`j`) → move down

(`k`) → move up

(`0`) → jump to beginning of line

(`$`) → jump to end of line

(`gg`) → jump to start of file

(`G`) → jump to end of file


# **Step 7: Editing**
(`x`) → delete character under cursor

(`dd`) → delete whole line

(`yy`) → copy (yank) whole line

(`p`) → paste after cursor

(`u`) → undo

(`Ctrl`) + (`r`) → redo


# **Step 8: Search**
(`/word`) → search forward

(`?word`) → search backward

(`n`) → next match

(`N`) → previous match

# **Step 9: Configure Vim (Optional)**
Open the config file:
```bash
vim ~/.vimrc
```
Example settings:
```bash
set number        " show line numbers
set tabstop=4     " tab size = 4 spaces
set shiftwidth=4  " indentation size = 4
set expandtab     " convert tabs to spaces
syntax on         " enable syntax highlighting

```
# **Step 10: Quick Exit**
(`ZZ`) → save and quit.<br>
(`ZQ`) → quit without saving.<br>
