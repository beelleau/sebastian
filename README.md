# My Bash scripts
Contains Bash (and sed) scripts I've written to help with small tasks.

---

### classip
`classip` is a IPv4 class information tool. It can print out information about class, public/private, and special IP ranges. Additionally, it can tell you more class information about a given IP address.  

--- usage  
```
Usage: classip [-c -r -s] [<ip address>] ...
Options:
  -h, --help    Show this help menu
  -c            Show all IP class ranges
  -r            Show all private IP ranges
  -s            Show all special IP ranges
```

--- examples  
`classip -c`  
print all classful IP ranges.  

`classip -r 192.168.86.4`  
print all classful private IP ranges, and describe the IP address 192.168.86.4.  
  
`classip -rcs 10.10.10.3 1.2.3.4 169.254.0.24`  
print all classful information, and describe all IP address entered as arguments.

---

### gitstat
`gitstat` finds all git repositories in your current directory, or under a specified directory. It prints the output of the `git status` command for each repository found.  

--- usage  
```
Usage: gitstat [-d <top-level directory>] [-i <directory to ignore>]
Options:
  -h, --help    Show this help menu
  -d <dir>      Specify the top level directory to search from
  -i <dir>      Ignore a specific directory from results (repeatable)
  -l            Only list git repos, and skip default action
```

--- examples  
`gitstat` or `gitstat .`  
run the command in your pwd.  

`gitstat -d /opt`  
run the command in "/opt".  

`gitstat -i .emacs.d/`  
run the command in your pwd and ignore ".emacs.d/".  

`gitstat -d ~ -i ~/.emacs.d -i ~/tmp `  
run the command in your home directory, ignoring `~/.emacs.d` and `~/tmp`.  

`gitstat -l -i tmp/`  
run the command to just list git directories, ignoring any directories under `./tmp/`

---

### ncs
`ncs` is a sed script that converts line-breaks ('\n') into commas. It can be used to convert file of line-break'd entries into a comma-separated line.  

--- example  
```sh
 $ cat fruits
banana
strawberry
cherry
blueberry

$ ncs fruits 
banana,strawberry,cherry,blueberry
```

---

### oneshot
`oneshot` allows you to run a command against a list of remote hosts.  

! Exercise caution when using this script. In general, it should only be used to gather basic information from a list of remote servers.  

A "hostlist" is required to use `oneshot`. It must be line-break separated. It will read every line that is not blank, and does not start with a `#` or `[`.  

Functionally, this script assumes that you have key-based authentication for ssh.  

--- usage  
```
Usage: oneshot [-a|-c] [-u <username>] <hostfile> "<command>"
Options:
  -h, --help    Show this help menu
  -a            Show output in 'view' format, with line-breaks
  -c            Show output in CSV format {hostname},{output}
  -t            Conduct a ssh connection test against <hostfile>
  -u <user>     Specify SSH username
```

--- examples  
`oneshot ~/hostlist.txt "cat .bashrc"`  
ssh to each hosts in "~/hostlist.txt" and run the command `cat .bashrc`.  

`oneshot -a /var/tmp/hostlist.txt "ls -ltr /var/log"`  
ssh to each hosts in "/var/tmp/hostlist.txt" and run the command `ls -ltr /var/log`. Output in a pretty format.  

`oneshot -c -u admin hostlist "uname -r"`  
ssh to each host in "./hostlist" as the user "admin" and run the command `uname -r`. Output in a CSV format.  

`oneshot -c -u larry /tmp/hostlist "grep -i 'pretty' /etc/os-release | cut -d '=' -f2 | tr -d '\"'" | tee output.csv`  
ssh to each host in "/tmp/hostlist" as the user "larry" and run the command `grep -i 'pretty' /etc/os-release | cut -d '=' -f2 | tr -d '"'` while using `tee` to write the output to the file "output.csv" and to your terminal. Notice the escape character before the double-quote within the command.  

`oneshot -t ../hostlist`  
run the ssh "test" against `../hostlist`. You will automatically add new hosts' public keys to your `~/.ssh/known_hosts` file, and you will get a warning if a key has changed. Use this option to verify connectivity and/or conduct ssh related troubleshooting.
