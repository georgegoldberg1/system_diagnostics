> *to view markdown in Atom hit `shift` + `ctrl` + `m`*

### BASIC TERMINAL COMMANDS:

* `pwd` outputs the name of the current working directory.
* `ls` lists all files and directories in the working directory. (dir on PC)
* `cd` switches you into the directory you specify.
* `mkdir` creates a new directory in the working directory.
* `touch newfilename` creates a new file inside the working directory.
* `cd ..` goes back one level
* `ls -a` shows hidden files
* `ls -l` lists files in table
* `ls -t` orders files from last modified
* `ls -alt` does all of the above, in one

* `cp * destination` copies all files into the destination
* `*` is the wildcard
* `cp m*.txt` will copy those starting with m
* `mv` moves files from one place to another or renames them

* `rm filename` deletes a file
* `rm -r directory` deletes a directory the *-r* means recursive which means it will delete all the sub files and folders too - PERMANENTLY

* `cat filename` outputs contents of a file to the terminal
* `file1 > file2` redirects the contents of file1 to file2 OVERWRITES OLD CONTENT IN FILE2
* `file1 >> file2` appends file1 to file2
* `cat <file1` redirects file1 to output eg reads the file

-----------------------------
QUICK VIM TUTORIAL / COMMANDS
-----------------------------
#### #MODES
##### Insert mode
press `i` to enter the main editor mode

##### Normal mode
press `Esc` to go back to Normal Mode

##### Visual mode
* `v` to enter visual mode for selecting whole words- you can highlight and delete them with `d` (see below commands)
* `V` to enter visual 'line' mode - for selecting whole lines
* `Ctrl+v` to enter visual 'block' mode - for selecting blocks of text
* `Shift+v` = select entire line
* `d` to cut the selection
* `y` to copy (yank) the selection
* `p` to paste before curser or `P` to paste after

#### #NAVIGATION -- (normal mode)

##### BASIC MOTION
* `h` = left
* `j` = down
* `k` = up
* `l` = right

##### JUMPING
* `w` = jump to start of next word
* `b` = jump to start of current/ previous word
* `e` = jump to end of word
* `W B E` all do the same as above but look for CAPITALS (this is important)
* `$` = jump to end of line
* `0` = jump to beginning of line
* `(` = jump to start of block/ end of prev block
* `)` = jump to end of block/ start of next block
* `gg` = jump to start of file
* `G` = jump to end of file
* `5G` = jump to 5th line (any number)

##### SEARCHING
* `/` = search for (then type your letter/ word to find CASE is important here)
* `n` = finds next occurence of your search
* `N` = finds prev occurence of your search

##### EDITING
* `x` = deletes the current character
* `r` = replaces current character with next press of the keyboard
* `R` = -replace mode- Allows replacement of multiple characters
* `d` = delete - combine with w/e/b to delete words, combine with numbers (e.g. d3w) to delete the next 3 words
* `o` = create new line ABOVE, and enter insert mode (put a number before 'o' to paste your edit multiple times when you return to normal mode)
* `O` = create new line BELOW

* `u` = UNDO
* `Ctrl+r` = REDO
* `.` = repeat last command

##### SAVE AND QUIT
* `:x` = save modifications only
* `:q` = quit
* `:q!` quit without saving
* `:w` = save, regardless of any modifications
* `:wq` = overwrite current file and quit

##### VIEW AND SPLITSCREENS
* `:vs` = vertical split
* `:sp` = horiz split
* `Ctrl+w` then arrow keys to jump between splits
* NOTE `:sp filename` will open a specific file in the new split (default is current file)
* `Ctrl+w -` and `ctrl+w +` or `ctrl+w 10-` will control the size of the current split
* `Ctrl+w _ /` = maximises/returns to normal the size of the split
* `:q` close current split

##### HELP
`:help`

##### HANDY TIPS
* in terminal: `cd` to go to home directory
* then `touch .vimrc` to create a file
* `vi .vimrc` to open it
* paste the below lines of code into .vimrc file and then save it `:x`
```
" enable save with capital `:W`
command! W :w
" switch displays easily (using ctrl+h,j,k or l) once :sp or :vs are created
nnoremap <c-j> <c-w>j
nnoremap <c-k> <c-w>k
nnoremap <c-h> <c-w>h
nnoremap <c-l> <c-w>l
" better searches - highlights words found, ignores case unless specified
set hlsearch
set incsearch
set ignorecase
set smartcase
:set number relativenumber
nnoremap <CR> :nohlsearch<cr>
```


### ZSH AND PLUGINS
* Zsh plugins https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/vi-mode
* Enabling Plugins (zsh-autosuggestions & zsh-syntax-highlighting)
* Download zsh-autosuggestions by
`git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions`

* Download zsh-syntax-highlighting by
1. `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting`
	* nano ~/.zshrc find plugins=(git)
2. Append zsh-autosuggestions & zsh-syntax-highlighting to  plugins() like this
	* plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
3. Reopen terminal

* Change shell:
 1. `chsh` will open vim so hit `i` to allow typing (INSERT MODE)
 2. change `/bin/**bash**` to `/bin/**zsh**` or vice versa
 3. Hit `esc` and `:x` to save and quit, then restart terminal

* Create a .zshrc file
* `ls -a` in your route directory and should reveal a .bash_profile and other file
* `vi .zshrc` will open vim and then type `export PATH="/Users/George/anaconda/bin:$PATH"`
* then use `ESC`, `:x` to save the file

Now in Zsh shell you will be able to run conda commands

### Terminal screen capture on Mac:
Iterm2, run command on startup:(Preferences, profiles, tab, general, send text at start)

`Ffmpeg -f avfoundation -video_size 1280x720 -framerate 30 -I "0" -vframes 1 out.jpg`


#CODE BELOW WILL TAKE A PICTURE OF WHOEVER OPENS ITERM
```
cd Pictures
rm out.jpg
Ffmpeg -f avfoundation -video_size 1280x720 -framerate 30 -i "0" -vframes 1 out.jpg
clear
cd ..
clear
```



### GIT
To connect a cloned repo to a new bitbucket/github url
1. First create a repo online.
2. Then get the url and paste the below into command line:
`git remote set-url origin https://<username>@bitbucket.org/rest of url`

#### To rename a Local and Remote Git Branch:
* Start by switching to the local branch which you want to rename:
`git checkout <old_name>`

#### Rename the local branch by typing:
`git branch -m <new_name>`
* If you’ve already pushed the <old_name> branch to the remote repository delete the <old_name> remote branch:

`git push origin --delete <old_name>`
* Finally push the <new_name> local branch and reset the upstream branch

`git push origin -u <new_name>`

`git checkout newbranchname` changes to this branch
* git ignore file: NEW = find .git folder, then find exclude file inside.

#### GIT TAGGING

Creating an annotated tag:

`$ git tag -a v1.4 -m "my version 1.4"`

List current local tags
~~~~
$ git tag
v0.1
v1.3
v1.4
~~~~

Push local tag to origin

`$ git push origin v1.5`

Push all local tags to origin

`$ git push --tags`


Rename tag and push to origin
```
$ git tag new old
$ git tag -d old
$ git push origin :refs/tags/old
$ git push --tags
```






### DOCKER
* `docker ps` = list running containers and images
* `docker container ls` = same as above
* `docker container kill <container id>` = stop a running container

* `docker exec -it c91ef4f1ff3b /bin/bash` = CHANGE INTO A POSTGRES CONTAINER / ssh into running docker container for Postgres.

* `docker volume rm volname` see docker-compose.yml for name of DB.if you wish to remove it

* stop all running containers
`docker kill $(docker ps -q)`
* remove all containers
`docker rm $(docker ps -a -q)`

#### Bugfix- if postgres port already in use:
1. `sudo lsof -iTCP -sTCP:LISTEN` finds the PID number
2. `sudo kill <PID number>` OR `brew remove postgres`

#### Access the db outside your app:
1. `docker ps -a` TO RETRIEVE POSTGRES CONTAINER ID
2. `docker exec -it <container id> /bin/bash`
3. `psql -U postgres`
4. Type sql query with semicolon ending


### PYTHON PASSWORD HASHING LIBRARIES
* Bcrypt
* Werkzeug

### VIRTUAL ENVIRONMENTS

Py-env package?? Inside install.sh file
```
conda create -n mynewflaskenv python=3.6.6
conda activate mynewflaskenv
pip install -r requirements.txt
conda deactivate
```
if there's a problem with conda find .bash_profile and .zshrc files
put export PATH="/Users/George/anaconda/bin:$PATH" into each file and once conda is updated, remove this line and instead type from the terminal: echo

`conda update --all` will update all packages

* Sublime Text will enable you to create aliases for folders and commands
atom, sublime text, vim are all text editors

##### Final note on Flask

NO GLOBAL VARIABLES ALLOWED. They need to sit inside functions (eg render template -web pages) otherwise they wont translate from one page to the next.

Define a function to pull global variables, returning each of them (Store in app.config['CONST']), then call this function at the start of each webpage.

e.g. DO THIS:
```
# --> app.py file

def get_x():
  x = 1
  return x

def loadpage():
  return render_template('webpage.html', value_of_x = get_x())
```
NOT THIS:
```
# --> app.py file

x = 1

def loadpage():
  return render_template('webpage.html', value_of_x = x)
```

If you do not adhere to the above you will immediately have problems deploying your app, even though both of the above may work locally on your machine. This is because 'webpage.html' will ignore app.py variables that aren't defined *inside* the 'loadpage' function.
