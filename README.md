# system_diagnostics
A collection of commands that can be used to give an overview / monitor resources used and the health of your computer/server.

- storage commands (these may vary depending what you are doing - eg docker volumes may be inaccessible to "du" command?)
- cpu/gpu/memory commands  
- filtering using `| grep` or other lower level subprocess cpu or sub folder disk use (eg application level/user level/airflow worker level) 
- outputs file/clipboard copypaste
- ssh file transfer to local / s3
- automation of process - cronjob? (as super low resource requirement + low complexity but operates close to the base machne (vs docker/airflow etc which may "go down")

### 1. Storage commands
[du shell command](https://explainshell.com/explain?cmd=du+-m+--max-depth%3D1+--exclude+media+%7C+sort+-n)  
(`du` = "Disk Usage" command, `-h` "human readable size values", `-d/--max-depth` "number of folders below current dir to get size of, `.` = optional!, if you want to specify the path to the folder)

**Mac OS**  

`du -hd1 . | sort -hro results_from_du.txt`  
- r=reversed (/descending size order here)
- h=human-readable numbers (e.g. put "1.6G" above "2.5M", otherwise it treats them as strings. or see solution in pandas (here)[https://stackoverflow.com/questions/39684548/convert-the-string-2-90k-to-2900-or-5-2m-to-5200000-in-pandas-dataframe])
- o= output (must be followed by filename)


**Linux (Ubuntu/Debian etc)**  

`du . -h -d=1 | sort -hro results_from_du.txt` 

**_~~Windows - (not yet covered)~~_**

### 2. CPU/GPU/Memory commands
Note: if your computer has more than one core, the total %CPU possible is 100% * number of cores. (e.g. quad-core = 400%).  

#### (i) Broad view
`ps u -A`  (but `ps ux -A` shows more detail)  
- u gives user level usage stats eg cpu and mem 
- -A gives process status for all users, not just the current one

#### (ii) Tracking activity  
`top` / `top -U George` (user) / `top -u` (ordered by %cpu)  
- Available fields/"keys": `-o <and key>` pid (default), command, cpu, cpu_me, cpu_others, csw, time, threads, ports, mregion, mem, rprvt, purg, vsize, vprvt, kprvt, kshrd, pgrp, ppid, state, uid, wq, faults, cow, user, msgsent, msgrecv, sysbsd, sysmach, pageins, boosts, instrs, cycles.
- Can use `-l` and `-i` to choose the samples and interval (eg 6 samples over 1 minute would take a reading every 10 seconds for a minute?)
- Other tags: 
 - [-a | -d | -e | -c <mode>] 
 - [-F | -f] 
 - [-ncols <columns>][-R | -r]
 - [-S]
 - [-s <delay>]
 - [-n <nprocs>]
 - [-stats <key(s)>]
 - [-pid <processid>]
 - [-user <username>]
 - [-U <username>]
 - [-u]

### 3. filtering with `| grep`
`ls [optional_dir_path]` / `la` / `ll` / `ls -al`  (list files and folders in a directory).  
combine with pipe and grep to filter the output:  
`la | grep .py` (to get all python files)



### 4. Automated file naming
You ideally want the name of the output file to reflect the location you checked, otherwise it will be listed as "." (relative to where the command ran), which is not helpful if you run the `du` command in multiple different locations and consolidate these files later.  

Use combination of `pwd` get get full path, then `tr` / `sed` / `awk` commands to extract the current folder location from the pwd string. 

#### pwd
 
`pwd` = print working directory (full path to current folder)  
surround `pwd` with backticks/backquotes ("\`"s) to use pwd in another command   
eg echo \`pwd\` will give the output of pwd command (as `echo pwd` just prints "pwd")

#### tr/sed/awk ([stack exchange overview](https://unix.stackexchange.com/questions/427940/main-difference-between-tr-translate-to-sed-and-awk))  

> To characterize the three tools crudely:
>
> * tr works on characters (changes or deletes them).
> * sed works on lines (modifies words or other parts of lines, or inserts or deletes lines).
> * awk work on records with fields (by default whitespace separated fields on a line, but this may be changed by setting FS and RS).

So sed could be used here. Also awk might be used on a table output. eg `ls -l` / `ll` to sort or filter on a certain field in the table?? eg date??  

### 4. Docker 
if not using systemctl `service docker status`
if using systemcl `systemctl is-active docker`

 
 
## TO DO:
- Single run stuff/basic functionality
  - add CPU/Memory function
  - add copy paste stuff https://stackoverflow.com/questions/5130968/how-can-i-copy-the-output-of-a-command-directly-into-my-clipboard
    - MacOS - pbcopy pbpaste
    - Linux - xclip BUT NOT INSTALLED ON A LOT OF SYSTEMS, SO BEST TO AVOID.
  - add `sed` command stuff to extract folder from end of pwd command

  - add ssh file transfer and s3 bucket push etc (or a git repo/google sheet?)
- Monitoring
  - add jupyter notebook to analyse outputs
  - add google sheet results
  - Connect to google data studio
- Automated/multi-run
  - add cronjob to run this frequently
- Advanced stuff
  - add support for docker containers/processes inside/ etc
  - add support for airflow containers/workers/processes/dagruns inside /db connection/port monitoring etc
  - automate email daily from gsheets
  - set up alert for strange/unusual activity
  - Identify/call-out problem processes that are causing the issues (eg specific dags)

 
# Author
George Goldberg (ggoldberg.inbox@gmail.com) 2021
