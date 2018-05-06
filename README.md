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
    touch mine/rename_me
    touch mine/remove_me
    chmod o+w mine/remove_me
    touch mine/sticky_will_protect_me
    tree -p
    sudo su other -c 'mv mine/rename_me mine/cant_rename'  # Permission denied
    sudo su other -c 'touch mine/cant_create'              # Permission denied
    sudo su other -c 'rm -f mine/remove_me'                # Permission denied
    chmod o+w mine
    sudo su other -c 'mv mine/rename_me mine/can_rename'   # fine
    sudo su other -c 'touch mine/can_create'               # fine
    sudo su other -c 'rm mine/remove_me'                   # fine
    chmod +t mine  # mine dir is still 777 but renaming will fail
    tree -p
    # Note how error message changes
    sudo su other -c 'mv mine/sticky_will_protect_me mine/cant_rename_with_sticky'  # Operation not permitted
    sudo su other -c 'rm -f mine/sticky_will_protect_me'                            # Operation not permitted
    sudo su other -c 'touch mine/can_create_with_sticky'                            # fine
    # cleanup
    cd -
    sudo rm -r /learn_sticky_bit
    sudo deluser other
