## Learn sticky bit 

Read: https://en.wikipedia.org/wiki/Sticky_bit

prepare dependencies:
```
apt-get install -y tree
```
Read, copy and paste into shell, read output
```
sudo adduser other --no-create-home --disabled-password --disabled-login --gecos ''
sudo mkdir /learn_sticky_bit
sudo chmod 777 /learn_sticky_bit
cd /learn_sticky_bit
mkdir mine
touch mine/1
touch mine/2
tree -p
sudo su other -c 'mv mine/1 mine/cant_rename'  # Permission denied
sudo su other -c 'touch  mine/cant_create'  # Permission denied
chmod o+w mine
sudo su other -c 'mv mine/1 mine/can_rename'  # fine
sudo su other -c 'touch  mine/can_create'  # fine
chmod +t mine  # mine dir is still 777 but renaming will fail
tree -p
sudo su other -c 'mv mine/2 mine/cant_rename_with_sticky'  # Operation not permitted
sudo su other -c 'touch  mine/can_create_with_sticky'  # fine
# cleanup
cd -
sudo rm -r /learn_sticky_bit
sudo deluser other
```
