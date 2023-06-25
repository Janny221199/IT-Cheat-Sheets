# Hands on local Tutorial
```cmd
	vimtutor
```

# Modes

## Normal Mode
- move around an open file and edit some parts

### Access normal mode

| Shortcut | Description |
| -------- | ----------- |
| ESC        | access normal mode        |

### Move Shortcuts

| Shortcut    | Description                                                                                |
| ----------- | ------------------------------------------------------------------------------------------ |
| h           | left                                                                                       |
| j           | down                                                                                       |
| k           | up                                                                                         |
| l           | right                                                                                      |
| 4j          | 4 rows down                                                                                |
| 6k          | 6 rows up                                                                                  |
| w           | begin of next word (but also stops at points and so on `Object.function()`)                |
| W           | begin of next word (but only looking for spaces)                                           |
| b           | begin of current or previous word (but also stops at points and so on `Object.function()`) |
| B           | begin of current or previous word (but only looking for spaces)                            |
| e           | end of current word (but also stops at points and so on `Object.function()`)               |
| E           | end of current word (but only looking for spaces)                                          |
| 0           | begin of line                                                                              |
| $           | end of line                                                                                |
| G           | end of file                                                                                |
| 3G          | Zeile 3                                                                                    |
| gg          | top of file                                                                                |
| CTRL+g      | see current line number, total lines, + filename                                           |
| {           | block up                                                                                   |
| }           | block down                                                                                 |
| %           | moves cursor to corresponding opening / closing parenthesis                                |
| /SEARCHTERM | search; use `n` for next search and `N` for backwards search and `CTRL+o` for getting back to start before searched; to ignore case: `:set ic`, to turn case off: `:set noic`; to (un)highlight searches: `:set hls` `:set nohls`; to show (no) partial matches: `:set is` `:set nois`                                                                                         |
| t+CHAR      | moves cursor before next occurring char of that type                                       |
| f+CHAR      | moves cursor on next occurring char of that type                                           |
| *           | moves cursor to next same word (nice for moving between function calls)                    |
| ;           | moves cursor to next same character                                                        |

### Edit Shortcuts

| Shortcut  | Description                                                                                                                                                                                                                                                                                                                                                                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| u         | undo                                                                                                                                                                                                                                                                                                                                                                           |
| U          | undo all line changes                                                                                                                                                                                                                                                                                                                                                                               |
| CTRL+r    | redo                                                                                                                                                                                                                                                                                                                                                                           |
| x         | delete character                                                                                                                                                                                                                                                                                                                                                               |
| D         | deletes rest of line (and copies)                                                                                                                                                                                                                                                                                                                                              |
| dd        | delete line (and copies)                                                                                                                                                                                                                                                                                                                                                       |
| dw        | delete word (and copies)                                                                                                                                                                                                                                                                                                                                                       |
| 3dd       | deletes 3 lines (and copies)                                                                                                                                                                                                                                                                                                                                                   |
| d}        | deletes whole block (and copies)                                                                                                                                                                                                                                                                                                                                               |
| dt+CHAR   | deletes all until next occurring char of that type (good for programming delete condition in if statement or parameters of a function)                                                                                                                                                                                                                                         |
| %d        | deletes content between selected parenthesis                                                                                                                                                                                                                                                                                                                                   |
| yy        | copy line                                                                                                                                                                                                                                                                                                                                                                      |
| p         | paste VIM clipboard inline or on whole line copied: paste VIM clipboard line below (and create line)                                                                                                                                                                                                                                                                                                                                    |
| P         | paste above VIM clipboard (and create line)                                                                                                                                                                                                                                                                                                                                    |
| COMMAND+v | paste system clipboard                                                                                                                                                                                                                                                                                                                                                         |
| c         | changing, deletes word and goes in insert mode                                                                                                                                                                                                                                                                                                                                 |
| C         | changing, deletes rest of line and goes in insert mode                                                                                                                                                                                                                                                                                                                         |
| ct+CHAR   | changing, deletes all until next occurring char of that type and goes in insert mode (good for programming delete condition in if statement or parameters of a function)                                                                                                                                                                                                       |
| r         | replace 1 character                                                                                                                                                                                                                                                                                                                                                            |
| ~         | swap case                                                                                                                                                                                                                                                                                                                                                                      |
| .         | re-run previous command (extremely good for change and replacing a specific area at multiple positions e.g. you wanna replace all between 2 parenthesis with another text and you are ate the opening brace: `ct)` -> write new text -> leave insert mode `ESC` -> go to next opening parenthesis and hit `.` and it will replace the same text again between the parenthesis) |
| >>        | indent code                                                                                                                                                                                                                                                                                                                                                                    |
| <<        | unindent code                                                                                                                                                                                                                                                                                                                                                                  |




## Insert Mode
- writing / typing and editing file

### access insert mode

| Shortcut | Description                                 |
| -------- | ------------------------------------------- |
| i        | access insert mode at cursor                |
| I        | access insert mode at begin of line         |
| a        | access insert mode behind current character |
| A        | access insert mode at end of line (using a lot) |
| o        | access insert mode with new line below      |
| O        | access insert mode with new line above                                            |


## Visual Mode
- make selections of text

### access visual mode

| Shortcut | Description                                     |
| -------- | ----------------------------------------------- |
| v        | access visual mode and select single characters |
| V        | access visual mode and select whole lines       |
| CTRL+v   | access visual mode and select whole blocks      | 

| Shortcut             | Description         |
| -------------------- | ------------------- |
| Normal Move Commands | select while moving |
| y                     | copies selection (and leaves visual mode)                    |
| d                    | delete selection (and leave visual mode)   |
| u                    | undo                |
| CTRL+r               | redo                |

## Command Mode
- execute commands 

### access command mode

| Shortcut | Description                                     |
| -------- | ----------------------------------------------- |
| :        | access command mode and type commands |


### useful commands

| Shortcut          | Description                                                                                                                                       |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| :h                | help                                                                                                                                              |
| :10               | go to line 10                                                                                                                                     |
| :%s/foo/bar/g     | replace `foo` with `bar` in whole file (% all lines, /foo regex for foo, /bar regex for bar, /g global whole line (all occurrences in that line)) |
| :%s/foo/bar/gc    | `c`with a prompt for each replacement if it should be replaces or not                                                                             |
| :3,10s/foo/bar/g  | all occurrences in lines between 3 and 10                                                                                                         |
| :!COMMAND         | run external shell commands e.g.: `:!ls`                                                                                                          |
| :w FILENAME       | write current vim content in new file (also possible with just selected text from visual mode)                                                    |
| :r FILENAME       | write content from file at current position                                                                                                       |
| :r !COMMAND       | write content of command output at current position                                                                                               |
| :CHAR(s) + CTRL+d | show list of commands starting with that char(s); use TAB to select between commands and SPACE to select one and write parameters for it                                                                                                                                                 |

## Replace Mode
- overwriting text

### access replace mode

| Shortcut | Description                                     |
| -------- | ----------------------------------------------- |
| R        | access visual mode typing will replace current characters |


# Macros
- record a macro with multiple commands and save it for a shortcut to reuse it 
- really good for code refactoring where you have to to the same thing too often
- in macros you should move with shortcuts like moving to the end of the word instead navigating single steps to make it dynamically for every word regardless of the size
- macros are stored at a key and can be accessed via `@KEY` (the saved key for the macro) -> also with a number to repeat it n times `10@KEY`
- record a macro with `q` and provide the key where is should bind to

Example:
- we wanna put `'` around every list item, swap all first characters to upper case and end the line with a `,`. So the provided data is:

```
[
	apple
	cherry
	banana
	pineapple
	strwaberry
	peach
]
```

1. go with cursor ON the fist char of the first word (here the a of apple)
2. `q` start recording macro
3. `n` to bin macro to `@n`
4. `~` to make it upper case
5. `b` go one before the begin of the word
6. `i` enter insert mode
7. write a `'` before the apple
8. `ESC` to leave insert mode
9. `A` to move to the end of the line and enter insert mode
11. write `',` behind the apple
12. `ESC` to leave insert mode
13. `j0w` go line down, got to the start of the line and to the beginning of the first word (just that the macro ends at the next line and we can run it again without moving there before (good when running it multiple times))
14. `q` end recording
15. got to the c of cherry and press `@n`

```
[
	'Apple',
	'Cherry',
	'Banana',
	'Pineapple',
	'Strwaberry',
	'Peach',
]
```



