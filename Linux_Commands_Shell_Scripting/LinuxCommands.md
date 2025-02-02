# Linux Commands

## Package Management

### ```apt-get```

* ``` apt-get clean ``` - removes the downloaded .deb packages from cache in /var/cache/apt/archives/

* ``` apt-get autqoclean ``` - cleans the local repository of retrieved packages but only for the packages that you have uninstalled or those with no newer versions available.

* ``` apt-get autoremove ``` - removes the unneeded packages that were once installed as a dependency


## Process Management

### `killall`
used to kill all process by name or command.

### `pstree`
Display a tree view of processes

### `htop`
It  is similar to top, but allows you to scroll vertically and horizon‐
tally, and interact using a pointing device (mouse)

## File and Directory Management
### `lsof`
List open  files and processes which opened them


## System Information
### `uname`

### `df`

### `du`

### `free`

### `lscpu`

## User and Group Management

### `useradd`

### `userdel`

### `usermod`

### `passwd`

### `id`

### `groups`

### `groupdel`

### `groupadd`

## Network Configuration and Monitoring

### `netstat`

### `ip`

### `ss`
Used to show "Socket Statistics" . 

* `ss --summary` - To get a summary
* `ss --process` - To get list process info like PID along listening ports etc

### `traceroute`

### `ssh`

### `nc`

## Misc

### ```printenv```
* Displays all the environment variables delared


## References
* https://substack.com/@rockybhatia/note/c-79923887?r=40ln3j
* https://substack.com/@javinpaul/note/c-82066112?r=40ln3j
* Linux man pages
* https://www.geeksforgeeks.org/ss-command-in-linux/
* https://www.cyberciti.biz/faq/unix-linux-killall-command-examples-usage-syntax/