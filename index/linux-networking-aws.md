# Linux
# Day-1[Intorduction]
- What is OS
- Linux Introudction
- Kernal and It's features
- Access Command Line
- Shell,Terminal
- Editing Command line
- # and $
- Basic Commands
  - whoami,cmd1;cmd2,date,file,cat,head,tail,wc,Tab Completion,ls,continuing long command using \,history[!26],uname
# Day-2[File System]
- Linux File System
- Absolute path and relative path
- pwd,touch,mkdir,cp,mv,rmdir,rm,cd,echo,man,less,more,grep,find,locate,df,du,free
- Inodes
- Soft Link and example
- Hard link and it's limitation and example
- file matching patterns,regular expressions
- variables defining
- command substitution
# Day-3[Redirections,editors]
- Redirections
- tee command
- vim,nano editors
- configuring programs
  EDITOR=vim
  export EDITOR
  export PATH=${PATH}:/home/user/sbin
  env
  setting default editor
- setting variables automatically
  /etc/profile, /etc/bashrc,~/.bashrc
# Day-4[UserManagemnt,FilePermissions]
- user management users and groups
- commnds
  - id,ps -au,useradd,passwd,su,su-,sudo,sudo -i,sudo visudo
- configuration files
  - /etc/passwd,/etc/group,/etc/shadow,var/log/secure,/etc/sudoers/
- file permissions
  - symbolic[rwx],numerical[777] and examples
- umask[default permissions], /etc/profile
- commnds
  - chmod,chown,chgrp
- special permissions
  - suid,sgid,sticky
# Day-5[Process Management,Services Management]
  - What is process
  - commands
    top,ps,fg,bg,sleep,jobs,kill,w,pstree,uptime,lscpu
- service management
- service file creation on /etc/
- commands
  systemctl 
# Day-5[SSH,Logs]
- What is SSH
- Commands
  ssh,ssh-keygen,ssh-agent,eval $(ssh-agent),ssh-copy-id
- Configuration files
  /etc/ssh/ssh_config,~/.ssh/,~/.ssh/id_rsa.pub,~/.ssh/id_rsa,/etc/ssh/sshd_config
- What are Logs
- commands
  - journalctl,timedatectl
- Logs configuration files
  /var/log,/etc/rsyslog.conf,/etc/rsyslog.d
- Logs prority

# Day-6[Networking]
- What is networking
- IP address,DNS just Intro
- commands
  - ping,netstat,ss,ip route,ip a,tracepath,hostname,hostnamectl
- configuration files
  /etc/resolv.conf,/etc/hosts,/etc/hostname

# Day-7[Archiving,PackageManagementScheduling]
- Commands
  - tar,scp,rsync
- Packagement
  - Commands
    - rpm,yum,dnf,apt
- schduling jobs
  - crontab,/etc/crontab

# Day-8[Bash Scripting]
- Variable Declaration
- Variable Substitution
- Loops
- Conditions
- Exit status
- Functions
- Positional Arguments
(https://devhints.io/bash)[https://devhints.io/bash]

# Day[Advance]
- ACL's,
- Selinux,
- NetworkSecurity
# Day[Advance]
- Managing File System Mounts
- LVM
- NFS
- Boot Process

# AWS

# Day-1[EC2]
