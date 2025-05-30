rights:
	r - right on read
	w - right on write
	x - right on doing something
users:
	u - current user
	g - group
	o - another users
	a - all users

## create user or add
su - superusers straight
su - 'ular'                  # sign to ular
adduser имя_пользователя - create user
useradd -h - check all options
passwd ular - create password
vim /etc/passwd - check all users
usermod -aG sudo username - give sudo rights 
sudo whoami - check has you got sudo rights
sudo usermod -g 'www-data' -G sudo,www-data,ular    # add new group

- ls -l   - check all directories with their rights
- chown -R ==ular==:==www-data== core   - add user: 'ular' directory 'core' and new rights on directory: 'core'
- chmod u+x 'name_file' - add to current user right 'x'- doing something
- chmod go-x 'name_file' - delete right on doing something group and other users
- chmod a=rwx - give all rights everubody
