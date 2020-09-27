# 8. Vim Tutor

## Lesson 1

### Lesson 1.1: Moving the Cursor

To move the cursor, press the h, j, k, l keys as follows:
- `j` moves down
- `k` moves up
- `h` moves left
- `l` moves right

You can hold down the keys until it repeats.
You can also use the normal arrow keys but "HJKL" is much faster.

If you ever type a command incorrectly, press `<ESC>` to go into Normal mode.

### Lesson 1.2: Exiting Vim

Press the `<esc>` key to make sure you are in Normal mode. 
Type `:q!` to exit and **discard** any changes that have been made. 

### Lesson 1.3: Deleting Text

Press `x` to delete the character under the cursor.

### Lesson 1.3: Inserting Text

Press `i` to insert text.

### Lesson 1.4: Appending Text (adding text to the end of the line)

Press `<shift> a` or `A` to append text.

### Lesson 1.5: Editing a File

Use `:wq` to save a file and exit.

## Lesson 2

### Lesson 2.1-2: Deletion Commands

Type `dw` to delete a word.

Type `d$` to delete to the end of the line. 

### Lesson 2.2: On Operators and Motions

Many commands that change text are made from an operator and a motion.

The `d` operator has this short list of (non comprehensive) motions:
- `w` until the start of the next word, EXCLUDING its first character.
- `e` to the end of the current word, INCLUDING the last character.
- `$` to the end of the line, INCLUDING the last character.

### Lesson 2.4: Using a Count for a Motion

Typing a number before a motion repeats it that many times.

So, `2e` goes to the end of two words forward. `2w` goes to the beginning of two words forward.
`0` goes to the start of the line.

### Lesson 2.5 Using a Count to Delete More

Typing a number with an operator repeats it that many times.

We can do something like `d2w` to delete two words.

### Lesson 2.6: Operating on Lines

Type `dd` to delete an entire line.
We can also do something like `2dd` to delete two lines.

### Lesson 2.7: The Undo Command

Type `u` to undo the last commands, `<shift>U` to fix a whole line.

Type `CTRL-R` to redo commands.

## Lesson 3

### Lesson 3.1: The Put Command

Type `p` to put previously deleted text after the cursor.

### Lesson 3.2: The Replace Command

Type `r[x`] to replace the character at the cursor with [`x]`.

### Lesson 3.3: The Change Command

To change until the end of a word, type `c[e]`

### Lesson 3.4: More Changes Using `c`

The change operator is used with the same motions as delete.
-`w` word
-`$` end of line

## Lesson 4

### Lesson 4.1: Cursor Location and File Status

Type `CTRL-G` to show location in the file and the file status. Type `G` to move to a line in the file.

`<shift> g` or `G` will go to the bottom of the file.
`gg` will go to the top of the file.
`[line number]<shift>g` will go to the specified line number.

### Lesson 4.2: Search Command

Type `/` followed by a phrase to search for the phrase. Use `?` to search in the backwards direction.

Type `n` to search in the forward direction.
Type `<shift>n` to search in the backwards direction.

To go back to where you came from, press `CTRL-O`. `CTRL-I` goes forward.

### Lesson 4.3: Matching Parenthesis Search

Place cursor on a (, [, { and type `%` to find the matching closing character.

### Lesson 4.4: The Substitute Command

Type `:s/old/new/g` to substitute "new" for "old". The `g` flag at the end means that it is global for that line. It may be omitted.

`:#,#s/old/new/g` where $,$ are the line numbers of the range of lines where the substitution is to be done.
`:%s/old/new/g` to change every occurence in the whole file.
`:%s/oild/new/gc` to find every occurence in the whole file with a prompt whether to substitute or not.

## Lesson 5

### Lesson 5.1: How to Execute an External Command

Type `:![command]` to execute that command.
The commands are finished by hitting `<ENTER>`.

### Lesson 5.2: More on Writing Files

Type `:w [FILENAME]` to save the changes made to the text.

### Lesson 5.3: Selecting Text to Write

To save part of the file type `v [motion]` `:w [FILENAME]`

For example, `:'<,'>w TEST` is what you might see.

### Lesson 5.4 Retrieving and Merging Files

To insert the contents of a file, type `:r [FILENAME]`

You can also read the output of an externam command. For example `:r !ls` reads the output of the `ls` command and puts it below the cursor.

## Lesson 6

### Lesson 6.1: The Open Command

Type `o` to open a new line below the cursor and be put in Insert mode. Do `<SHIFT>O` to open up a line above the cursor.

### Lesson 6.2: The Append Command

Type `a` to insert text after the cursor.

Remember, `<shift>A` is used to append at the end of a line.

### Lesson 6.3: Another Way to Replace

Type `<shift>R` to replace more than one character.

### Lesson 6.4: Copy and Paste Text

Use the `y` (yank) operator to copy text and use `p` to paste it. 

`y` can be used as an operator, for example, we can `yw` to yank one word.

### Lesson 6.5: Set Option

Set an option so a search or substitute ignores case.

`:set ic` will ignore case.
`:set hls is` sets the hlsearch and incsearch options.
`:set noic` will disable ignoring case

If you want to ignore case for just one search command, use `\c` in the phrase.
- `/ignore\c <ENTER>`

## Lesson 7

### Lesson 7.1: Getting Help

Vim has a comprehensive online hel system. Try one of these three:
- press `<HELP>` key
- press `<F1>` key
- type `:help <ENTER>`

Type `CTRL-W CTRL-W` to jump from one window to another.
Type `:q` to quit from the help window.

You can get help on a specific subject:
-`:help w`
-`:help c_CTRL-D`
-`:help insert-index`
-`:help user-manual`

### Lesson 7.2: Create a Startup Script

To use more Vim features, you have to create a "vimrc" file.

`:e ~/.vimrc` to edit the vimrc file.
Read example contents using `:r $VIMRUNTIME/vimrc_example.vim`
Write the file with `:w`

### Lesson 7.3: Completion

Command line completion with `CTRL-D` and `<TAB>`

Make sure Vim is not in compatible mode: `:set nocp`
Look what files exist in the directory `:!ls`
Type the start of a command `:e`
Press `CTRL-D` and Vim will show a list of commands that start with "e".
Type `d,TAB>` and Vim will complete the command name to `:edit`.
Now add a space and the start of an existing file name: `:edit FIL`
Press `<TAB>` Vim will complete the name if it is unique.

*edited T9/27*
