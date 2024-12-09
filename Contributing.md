Project: Create a Custom Bash Prompt
PS1 customization
    PS1 supports various placeholders:
        >Username: \u 
        >Hostname:
            >upto the first".": \h 
            >The complete hostname: \H 
        Current Working Directory:
            >full path: \w 
            >last directory of the fullpath \W 
        Time in 24-hour format: \t 
        Time in 12-hour format with am/pm : \@
            Example:
                > PS1='\u@\h:\w$ '

Terminal escape sequences
eg: echo -e "\e[30;40m":
    > 30: Black foreground
    > 40: Black background
    FG      BG      Name        VGA
    30      40      Black       0,0,0
    31      41      Red         170,0,0
    32      42      Green       0,170,0
    33      43      Yellow      170,85,0
    34      44      Blue        0,0,170
    35      45      Magenta     170,0,170
    36      46      Cyan        0,170,170
    37      47      White       170,170,170
    90      100     Gray        85,85,85
    97      107     Bright White 255,255,255

for i in {0..15}; do echo "$(tput setab ${i}) color: ${i}$(tput sgr0)"; done

"new PS1"
PS1="\[$(tput setaf 5)\]-> \[$(tput setaf 6)$(tput bold)\]\W \[$(tput setaf 4)\](\t)\[$(tput sgr0)\]\\$ "

"modified PS1"
PS1="\[$(tput setaf 5)\]->\[$(tput setaf 5)$(tput bold)\]\u \[$(tput setaf 6)$(tput bold)\]\W \[$(tput setaf 4)\](\t)\[$(tput sgr0)\]\\$ "

Understanding &&, ||, and ; in Linux
    In the heart of every Linux system is the command line interface (CLI). 
    Linux commands, especially when combined into scripts, can automate tasks
    manage system resources and solve complex problems.
    To truly unlock the power of the CLI, one needs to understand how to control
    the flow of command execution. This is where operators like &&, ||, and ; come 
    into play.

    The && Operator:
        The && operator allows us to chain commands together such that the second command only executes
        if the first one succeeds. This is useful when the second command relies on the success of the first.
        example: mkdir new_dir && cd new_dir 
    The || Operator:
        The || operator on the other hand allows us to execute the second command only if the first one fails.
        This is useful for providing a fallback command or handling errors.
        For example:
            mkdir new_dir || echo "Directory already exists!"
    The ; Operator:
        The ; operator is used to execute multiple commands sequentially, regardless of the success or failure
        of any command.
        For example:
            mkdir new_dir; ls

Filename Expansion:
    ref:
        https://www.gnu.org/software/bash/manual/html_node

grep command:
    grep -o '[^,]*$'
 Explanation:
    
    -o (--only-matching) only outputs the part of the input that 
        matches the pattern (the default is to print the entire 
        line if it contains a match).
    [^,] is a character class that matches any character other 
        than a comma.
    * matches the preceding pattern zero or more time, 
        so [^,]* matches zero or more non‑comma characters.
    $ matches the end of the string.
    Putting this together, the pattern matches zero or 
        more non-comma characters at the end of the string.
    When there are multiple possible matches, grep prefers 
        the one that starts earliest. So the entire last field 
        will be matched.

Example:
    one,two,three
    foo,bar
then grep -o '[^,]*$' will output
    three
    bar
Example2:
    ls -l /usr/share | head -n 10 | grep -o '[^" "]*$' will output:
    312
    accountsservice
    aclocal
    adobe
    alsa
    alsa-card-profile
    anaconda
    appdata
    app-info
    applications

Other Commands:
    hostname --> states the hostname of your server
  You can set the hostname of your server using the following syntax:
    sudo hostnamectl set-hostname <newHostname>
Directory Management Commands:
    mkdir => create directories
    rmdir => delete an empty directory
    rm -rf => delete directory and its content recursively don't Prompt
    cd   => change directory 
    pwd  => present working directory
    ls  => list 

Brace Expansion '{..}'
    echo a{d,c,b}
        ad ac ab 
    mkdir /usr/local/src/bash/{old,new,dist,bugs}
        /usr/local/src/bash/old
        /usr/local/src/bash/new
        /usr/local/src/bash/dist
        /usr/local/src/bash/bugs 
    chown root:root /usr/{ucb/{ex,edit}, lib/{ex?.?*,how_ex}}

Copy Command: cp 
    SYNOPSIS
       cp [OPTION]... [-T] SOURCE DEST
       cp [OPTION]... SOURCE... DIRECTORY
       cp [OPTION]... -t DIRECTORY SOURCE...

        cp -t <destination dir> -rv <source dir...>
    Note: if you are to combine the options it should be before -t options
    Also; the -t options allows you to copy the source files and its source dir to the destination directory
    
        cp -rvt <destination dir> <source dir,...>
Move Command: mv 
    NAME
       mv - move (rename) files

SYNOPSIS
       mv [OPTION]... [-T] SOURCE DEST
       mv [OPTION]... SOURCE... DIRECTORY
       mv [OPTION]... -t DIRECTORY SOURCE...

sed Command: sed
    NAME
        sed - stream editor for filtering and transforming text
    SYNOPSIS
        sed [OPTION]... {script-only-if-no-other-script} [input-file]...

    example sed 's/<text-to-search>/<replace-with-text>/g' [source-file]
    NB. Above will only print to screen without changing the actual source file
        to change the source file with sed use the option [-i]

        sed -i 's/<text-to-search>/<replace-with-text>/g' [source-file]

Set hostname:
    sudo hostnamectl set-hostname demo



Linux User Mangement:
    sudo useradd -m -d /home/newuser newuser
    sudo passwd newuser

  modifying user's details:
    sudo usermod [option] username
        option: -c => change user description (full name)
                -s => change default shell
                -d => change home directory
                -m => move the existing home directory to a new location
                -l => change username
                -g => change primary group
                -G => change secondary groups
                -aG => add secondary group
    sudo userdel [option] username
        option: -r => remove user and home directory 
                -f => remove user forcefully even if user is logged in
    
    sudo groupadd [option] groupname
                -g => set custom GID
            
    sudo groupmod [option] groupname
                -n => change the name of the group
                -g => change the group Id (GID)
    
    sudo groupdel [option] groupname
                -f => forces the removal of the group even if there's some user having the group as primary 

Connecting to a Linux ec2 instance on aws:
    In this example: we know the following:
                    PublicIp : 44.201.180.221
                    SSH-Keypair: Class32.pem 
                    user: ec2-user 

    On the terminal:
                ssh -i "class32.pem" ec2-user@44.201.180.221
            or 
                ssh -i "class32.pem" ec2-user@ec2-44-201-180-221.compute-1.amazonaws.com
Git:
    Git Commands:
        -> git add <file>
        -> git status
        -> git commit
    .gitignore file
        # ignore all .a files
            *.a
        # but do track lib.a, even though you are ignoring .a files above
            !lib.a
        # only ignore the TODO file in the current directory, not subdir/TODO
            /TODO
        # ignore all files in any directory named build
            build/
        # ignore doc/notes.txt, but not doc/server/arch.txt
            doc/*.txt
        # ignore all .pdf files in doc/ directory and any of its subdirectories
            doc/**/*.pdf
                


     