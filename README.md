# Linux tips

## cron

* A common error is not to set PATH correctly because cron runs at default with a very limited PATH and env.

```
# first view your env in terminal
$ env

# now create a env check in cron
$ crontab -e
* * * * * env >> env.txt

# By comparing the two above you will see a big difference in the two environments
# A solution is e.g. to place a PATH within each shell script e.g.

    #!/bin/bash
    PATH=/bin:/usr/bin:/usr/local/bin/

```

# Some Linux commands

* They have been used on Linux Debian or Ubuntu, various versions.

* WARNING: be careful with some of the commands, best read about them elsewhere first!

```

# use > to write to a file and use >> to append to a file
echo "srcfile.txt" >  "dstfile.txt"
echo "srcfile.txt" >> "dstfile.txt"

# Ubuntu apt-get command
# see also: https://help.ubuntu.com/community/AptGet/Howto 

# List file placements in couchdb (which is an open source apache database)
dpkg -L couchdb

# Check package version installed
dpkg -s openssl

# Upgrade a specific package
apt-get install openssl

# Check Linux version:
lsb_release -a

# Upgrade to new Linux release:
do-release-upgrade

# Check which services start at boot time
systemctl list-units --type service

# How to kill an application by name:
sudo killall -9 scp

# 10 oldest processes
ps -elf | sort -r -k12 | head -n 10

# Kill a process by <pid>
kill <pid>

# Reboot Linux:
sudo reboot

#Power off:
poweroff, halt or shutdown -h now

# Ports:
netstat -ntlp | grep LISTEN

# DNS
nslookup -query=ns google.com
nslookup google.com
nslookup -query=mx google.com

# More at: http://www.thegeekstuff.com/2012/07/nslookup-examples/
internet listen
netstat -l

# Internet connections
netstat -plant

# My terminals IP address
wget -qO - http://ipecho.net/plain; echo

# Disk size
df -H

# Sum of .csv files sizes (in current directory)
du -hc *.csv | tail -1

# Count # of lines in file
wc -l <filename>

# Count # of lines in *.csv files in current directory
wc -l *.csv

# Count # of files in directory
ls -1 | wc -l

# bash script: Loop over files (*.csv) in current directory (.) and
# do something with each file (file -i) e.g. list the encoding of each file
    #!/bin/bash
    for xx in ./*.csv; do
        file -i "$xx"
    done

# Compress files and folders in directory to named file backup.tgz’
tar -cvzf backup.tgz *

# Uncompress the above tar file. Flag f must be the last flag
tar -xvzf backup.tgz

# check IP addresses
nslookup riskbutler.com

# Memory check
cat /proc/meminfo 

# Services
service --status-all 

# Linux node/system limitations
# soft limits are simply the currently enforced limits
# hard limits mark the maximum value which cannot be exceeded by setting a soft limit
ulimit -a

# List all users
cut -d: -f1 /etc/passwd

# delete a user
userdel user_name

# --- make a user with restricted access ---
ln -s /bin/bash /bin/rbash

useradd -d /home/user_name -s /bin/rbash -m user_name
passwd user_name

mkdir /home/user_name/bin

chown root. /home/user_name/.profile
chmod 755 /home/user_name/.profile

## set PATH
nano /home/user_name/.profile

--- .bash_profile ---
PATH=$HOME/bin
---

## allow above user: "ls", "mkdir", "ping" commands
ln -s /bin/ls /home/user_name/bin/ls
ln -s /bin/mkdir /home/user_name/bin/mkdir
ln -s /bin/ping /home/user_name/bin/ping

--- end of: make a user with restricted access ... ---

# Users of processes
ps aux

# Find folder named “radar”
find / -name "radar"

# Find file named textfile.txt
find / -name textfile.txt

#Find files not named jquery-ui*.css
find . -type f -not -name 'jquery-ui*.css'

# Delete files not named logo.png
find . -type f ! -name 'logo.png' -delete
delete a lot of files
find . -name "*.pdf" -delete

# Rename all files with ending .txt to .csv
# [optionally] To find placement of perl command rename: $ which rename => /usr/bin/rename
rename 's/txt/csv/' *.txt

# Add text in more.txt to the end of a text file orig.txt
cat more.txt >> orig.txt

# Find and replace
# http://unix.stackexchange.com/questions/112023/how-can-i-replace-a-string-in-a-files 
# http://www.brunolinux.com/02-The_Terminal/Find_and%20Replace_with_Sed.html 
cat filename | sed  's/oldstring/newstring/g'  >  newfilename  
… >> filename
echo ‘oldstring’ | sed ...

# Look for particular string in a group of files
# ... this finds all cases of the string “echo” in the .php files in all folders from “.”
# note: w is to match whole word (not only part of text), and l is to only return the file name
grep -rn --color 'echo' *.php
grep -rnw '/path/to/somewhere/' -e "pattern"

# Copy directory but exclude a directory within
rsync -av --progress sourcefolder /destinationfolder --exclude thefoldertoexclude    

# Check encoding of file
file -i YourFile.txt

# Change encoding of text file f: from t: to o: output
iconv -f ISO_8859-1 -t UTF-8 -o YourNewFile.txt YourOldFile.txt 

# ... list all iconv known encodings
iconv -l

# ... combine with curl
curl URL | iconv -f ISO_8859-1 -t UTF-8 > YourCurlDownloadedFile.txt

# Check date of ssl certificate validity
# returns eg:
# notBefore=Jan  8 09:57:51 2014 GMT
# notAfter=Jan  8 09:57:51 2015 GMT
# Note: I personally now use Let's Encrypt and its auto cron job that fully automates updates
# read more at: https://letsencrypt.org/
echo | openssl s_client -connect site:port 2>/dev/null | openssl x509 -noout -dates

# Start graphical environment
startx

# Install desktop on Ubuntu
sudo apt-get install ubuntu-desktop

# Time
# https://help.ubuntu.com/community/UbuntuTime 
echo "UTC" | sudo tee /etc/timezone
sudo dpkg-reconfigure --frontend noninteractive tzdata

# is UTC time in seconds since 1970
date +%s

# Download file(s)
# http://www.thegeekstuff.com/2012/07/wget-curl/
# Upload file index.html to ftp server
curl --user username:password --ftp-ssl --ftp-pasv -T "index.html" ftp://ftp_server_address/

# tar and compress, and uncompress: j (bzip2) and z (gzip)
tar -cvjf ../yahoo.tar.bz2 yahoo*
tar cvzf MyImages.tgz /home/img/ 
tar -xvf thumbnails.tar.gz

# tar and compress many files
find . -name 'my_prefix*' > filelist.txt
tar czvf archive.tar.gz --files-from filelist.txt

# Working with strings and conversions
# Note the arrow and following text is not part of the command but shows expected output
YY="20160201"

echo ${YY:2:2}  => “16”

echo ${YY:0:4}"-"${YY:4:2}"-"${YY:6:2}  => “2016-02-01”

echo "YYYYMMDD" | sed -n -e "s_\(....\)\(..\)\(..\)_\2/\3/\1_p" => "MM/DD/YYYY"

echo "YYYYMMDD" | sed -n -e "s_\(....\)\(..\)\(..\)_\1-\2-\3_p" => YYYY-MM-DD

echo "YYYYMMDDHHMMSS" | sed -n -e "s_\(....\)\(..\)\(..\)\(..\)\(..\)\(..\)_\1-\2-\3 \4:\5:\6_p" => YYYY-MM-DD HH:MM:SS

echo "YYYYMMDD,HHMMSS" | sed -n -e "s_\(....\)\(..\)\(..\)\(.\)\(..\)\(..\)\(..\)_\1-\2-\3 \5-\6-\7_p" => YYYY-MM-DD HH-MM-SS

echo "20160201,235432" | sed -n -e "s_\(....\)\(..\)\(..\)\(.\)\(..\)\(..\)\(..\)_\1-\2-\3 \5:\6:\7_p" => 2016-02-01 23:54:32

```
