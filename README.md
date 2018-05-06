## Learn sticky bit 

Read: https://en.wikipedia.org/wiki/Sticky_bit

> In computing, the sticky bit is a user ownership access right flag that can be assigned to files and directories on Unix-like systems.
> 
> When a directory's sticky bit is set, the filesystem treats the files in such directories in a special way so only the file's owner, the directory's owner, or root user can rename or delete the file. Without the sticky bit set, any user with write and execute permissions for the directory can rename or delete contained files, regardless of the file's owner. Typically this is set on the /tmp directory to prevent ordinary users from deleting or moving other users' files.

prepare dependencies:

    apt-get install -y tree

Read, copy and paste into shell, read output.

    sudo adduser other --no-create-home --disabled-password --disabled-login --gecos ''
    sudo mkdir /learn_sticky_bit
    sudo chmod 777 /learn_sticky_bit
    cd /learn_sticky_bit
    mkdir mine
    touch mine/1
    touch mine/2
    chmod o+w mine/2
    touch mine/3
    tree -p
    sudo su other -c 'mv mine/1 mine/cant_rename'  # Permission denied
    sudo su other -c 'touch mine/cant_create'  # Permission denied
    sudo su other -c 'rm -f mine/2'  # Permission denied
    chmod o+w mine
    sudo su other -c 'mv mine/1 mine/can_rename'  # fine
    sudo su other -c 'touch mine/can_create'  # fine
    sudo su other -c 'rm mine/2'  # fine
    chmod +t mine  # mine dir is still 777 but renaming will fail
    tree -p
    sudo su other -c 'mv mine/3 mine/cant_rename_with_sticky'  # Operation not permitted
    sudo su other -c 'rm -f mine/3'  # Operation not permitted
    sudo su other -c 'touch mine/can_create_with_sticky'  # fine
    # cleanup
    cd -
    sudo rm -r /learn_sticky_bit
    sudo deluser other
