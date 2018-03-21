LXC before 1.0 mapped the user and group ids from inside the container to the same ids on the host. This can be considered a security risk. Since 1.0 you have the option to use a range outside from the host.

### map a range of uids and gids to a user.

This can be done in two ways:
* using system-tools  
usermod --add-subuids 100000-165536 $USER  
usermod --add-subgids 100000-165536 $USER

* manually by editing /etc/subuid and /etc/subgid and adding  
$USER:100000:65536  
in both files


### edit the container config and add its id range:

lxc.id_map = u 0 100000 65536  
lxc.id_map = g 0 100000 65536

### move every file inside the containers root to the id range.

**This is what this script is for.** It adds or subtracts a given number to every files user/group-id. In addition it changes the container directory accordingly and sets its permissions so that only the owner can access it.

### usage

**lxc-namespace -c** $CONTAINER_NAME (**-m** mode **-n** uid/gid **-p** $PATH)
