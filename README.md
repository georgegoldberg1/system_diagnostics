# system_diagnostics
A collection of commands that can be used to give an overview / monitor resources used and the health of your computer/server.

- storage commands (these may vary depending what you are doing - eg docker volumes may be inaccessible to "du" command?)
- cpu/gpu/memory commands  
- filtering using `| grep` or other lower level subprocess cpu or sub folder disk use (eg application level/user level/airflow worker level) 
- outputs file/clipboard copypaste
- ssh file transfer to local / s3
- automation of process - cronjob? (as super low resource requirement + low complexity but operates close to the base machne (vs docker/airflow etc which may "go down")  

Some commands depend on your operating system. Currently no commands are here for windows. To find out your system details on linux or mac, see below:  
get mac os version:  
`sw_vers` and processor details `sysctl -a | grep brand` / `system_profiler | grep Processor`  
get linux version:  
`cat /etc/os-release | grep 'HOME_URL\|VERSION=\|ID_LIKE=\|VERSION_CODENAME'`  

### 1. Get Disk Usage Stats
[du shell command](https://explainshell.com/explain?cmd=du+-m+--max-depth%3D1+--exclude+media+%7C+sort+-n)  
(`du` = "Disk Usage" command, `-h` "human readable size values", `-d/--max-depth` "number of folders below current dir to get size of, `.` = optional!, if you want to specify the path to the folder)

**Mac OS**  
get disk usage:  
`du -hd1 . | sort -hro results_from_du.txt`  
- r=reversed (/descending size order here)
- h=human-readable numbers (e.g. put "1.6G" above "2.5M", otherwise it treats them as strings. or see solution in pandas [here](https://stackoverflow.com/questions/39684548/convert-the-string-2-90k-to-2900-or-5-2m-to-5200000-in-pandas-dataframe))
- o= output (must be followed by filename)  

**Linux (Ubuntu/Debian etc)**  
get disk usage:  
`du . -h -d=1 | sort -hro results_from_du.txt`  

**_~~Windows - (not yet covered)~~_**

### 2. Get CPU/GPU/Memory Stats (overall machine)
Note: if your computer has more than one core, the total %CPU possible is 100% * number of cores. (e.g. quad-core = 400%).  

#### (i) Snapshot of processes (PS command)
`ps u -A`  (but `ps ux -A` shows more detail)  
- u gives user level usage stats eg cpu and mem 
- -A gives process status for all users, not just the current one

#### (ii) Ongoing / more accurate tracking processes + activity (TOP command)
**MAC OS TOP**  
`top` / `top -U George` (user) / `top -u` (ordered by %cpu)  
- Available fields/"keys": `-o <key>` pid (default), command, cpu, cpu_me, cpu_others, csw, time, threads, ports, mregion, mem, rprvt, purg, vsize, vprvt, kprvt, kshrd, pgrp, ppid, state, uid, wq, faults, cow, user, msgsent, msgrecv, sysbsd, sysmach, pageins, boosts, instrs, cycles.
- Intervals (refreshes): `-i <num>`
- 
- Can use `-l` (logging mode num samples) and `-i` to choose the samples and interval (eg 6 samples over 1 minute would take a reading every 10 seconds for a minute?)
- Other tags: 
 - [-a (accumulative over period) | -d (delta/change vs prev sample) | -e (absolute at start time)| -c <mode>] 
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
 
 ```cpu_percent=$(top -l  2 | grep -E "^CPU" | tail -1 | awk '{ print $3 + $5"%" }')```
 
 **LINUX TOP** ([see guide](https://manpages.ubuntu.com/manpages/xenial/man1/top.1.html))  
 ```
  top -hv | -bcEHiOSs1 -d secs -n max -u|U user -p pid(s) -o field -w [cols]
 ```  
(delay -d, samples -n, )
- `-o +'%CPU'` (sort dec on cpu use. swap + with - to change sort direction)
- 
 
> [source](https://unix.stackexchange.com/questions/13968/show-top-five-cpu-consuming-processes-with-ps)  Why use ps when you can do it easily with the top command?
> If you must use ps, try this:
> `ps aux | sort -nrk 3,3 | head -n 5`
> If you want something that's truly 'top'esq with constant updates, use watch
> `watch "ps aux | sort -nrk 3,3 | head -n 5"`
>
>The correct answer is: `ps --sort=-pcpu | head -n 6`, So you can specify columns without interfering with sorting.
>(eg `ps -Ao user,uid,comm,pid,pcpu,tty --sort=-pcpu | head -n 6` )
> Note In Mac OS X, ps doesn't recognize --sort, but offers -r to sort by current CPU usage. Thus, for Mac OS X you can use: `ps -Ao user,uid,comm,pid,pcpu,tty -r | head -n 6`  
 
**OR USE DOCKER**  
`docker top <container name/id>`  

### 3. Docker  (containerised processes on the machine)
**Check Docker status**  
- not using systemctl: `service docker status`  
- using systemctl: `systemctl is-active docker`  
 
**Check Docker Setup**  
- INFO: `docker info`  
 - (eg. `docker info | grep 'Total Memory'` to show memory allocated (for airflow it should be at least 8GB)  
 - (eg2: `docker info | grep 'Containers:\|Running:\|Paused:\|Stopped:\|Images:'`)  
- UPDATE CONFIG on one or more containers `docker update ????`   
 
**Check container ports**  
- `docker port <container name/id>`  
- INSPECT: `docker inspect <container name/id> | grep Port`  
 
**Check container history**  
- LOGS: `docker logs <container name/id>`  
 
**Monitor docker overall**  
- REALTIME events: `docker events`  
 
**Monitor containers**
- cpu/mem use, STATS: `docker stats <container name/id>`  
- External process monitor: `docker top <container name/id>`  
- Internal process monitor (from inside the container): `ps -elf`  / `top`
to start and go into a container `docker run -it --rm ubuntu:latest /bin/bash`  

### 4. Storing output files + automated file naming
You ideally want the name of the output file to reflect the location you checked, otherwise it will be listed as "." (relative to where the command ran), which is not helpful if you run the `du` command in multiple different locations and consolidate these files later.  

Use combination of `pwd` (print working directory) to get get full path, then `tr` / `sed` / `awk` / `grep` commands to extract the current folder location from the pwd string. [stack exchange seg/awk/tr overview](https://unix.stackexchange.com/questions/427940/main-difference-between-tr-translate-to-sed-and-awk)) Note: surround `pwd` with backticks/backquotes ("\`"s) to use pwd in another command (eg echo \`pwd\` will give the output of pwd command, as `echo pwd` just prints "pwd")
 
## TO DO:
- Single run stuff/basic functionality
  - ~~add CPU/Memory function~~
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
  - ~~add support for docker containers/processes inside/ etc~~
  - add support for airflow containers/workers/processes/dagruns inside /db connection/port monitoring etc
  - Database monitoring/ DB health
  - AWS monitoring/health/capacity/utilisation / efficiency + cost 
  - automate email daily from gsheets
  - set up alert for strange/unusual activity
  - Identify/call-out problem processes that are causing the issues (eg specific dags)
 
# Author
George Goldberg (ggoldberg.inbox@gmail.com) 2021
