# My Bash scripts
Contains Bash scripts I've written to help with small tasks.

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

---

### gitstat
`gitstat` finds all git repositories in your current directory, or under a specified directory. It prints the output of the `git status` command for each repository found.  

--- usage  
```
Usage: gitstat [-d <top level directory>] [-e <excluded directory>]
Options:
  -h, --help    Show this help menu
  -d <dir>      Specify the top level directory to search from
  -e <dir>      Specify a directory to exclude from results
```

--- examples  
`gitstat` or `gitstat .`  
run the command in your pwd.  

`gitstat -d /opt`  
run the command in "/opt".  

`gitstat -e .emacs.d/`  
run the command in your pwd, and exclude ".emacs.d/".  

`gitstat -d ~ -e ~/.emacs.d -e ~/tmp `  
run the command in your home directory, excluding `~/.emacs.d` and `~/tmp`.
