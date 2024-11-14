# Linux-File-IO-Systems-locking
Ex07-Linux File-IO Systems-locking
# AIM:
To Write a C program that illustrates files copying and locking

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux IO Systems locking

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## 1.To Write a C program that illustrates files copying 
```
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>
int main()
{
char block[1024];
int in, out;
int nread;
in = open("filecopy.c", O_RDONLY);
out = open("file.out", O_WRONLY|O_CREAT, S_IRUSR|S_IWUSR);
while((nread = read(in,block,sizeof(block))) > 0)
write(out,block,nread);
exit(0);}
```

## 2.To Write a C program that illustrates files locking
```
#include <fcntl.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/file.h>
int main (int argc, char* argv[])
{ char* file = argv[1];
 int fd;
 struct flock lock;
 printf ("opening %s\n", file);
 /* Open a file descriptor to the file. */
 fd = open (file, O_WRONLY);
// acquire shared lock
if (flock(fd, LOCK_SH) == -1) {
    printf("error");
}else
{printf("Acquiring shared lock using flock");
}
getchar();
// non-atomically upgrade to exclusive lock
// do it in non-blocking mode, i.e. fail if can't upgrade immediately
if (flock(fd, LOCK_EX | LOCK_NB) == -1) {
    printf("error");
}else
{printf("Acquiring exclusive lock using flock");}
getchar();
// release lock
// lock is also released automatically when close() is called or process exits
if (flock(fd, LOCK_UN) == -1) {
    printf("error");
}else{
printf("unlocking");
}
getchar();
close (fd);
return 0;
}
```

## OUTPUT
### File copying
```
gnanendran@ubuntu:~$ vi filecopy.c
gnanendran@ubuntu:~$ gcc -o filecopy.o filecopy.c
gnanendran@ubuntu:~$ ./filecopy.o
gnanendran@ubuntu:~$ ls -l filecopy.o
-rwxrwxr-x 1 gnanendran gnanendran 16088 Oct 24 10:52 filecopy.o
```

### File Locking
```
gnanendran@ubuntu:~$ vi filelock.c
gnanndran@ubuntu:~$ gcc -o filelock.o filelock.c
gnanendran@ubuntu:~$ ./filelock.o tricky.txt 
opening tricky.txt
Acquiring shared lock using flock
Acquiring exclusive lock using flock
unlocking
gnanendran@ubuntu:~$ lslocks
COMMAND           PID  TYPE SIZE MODE  M      START        END PATH
VBoxService      1133 POSIX      WRITE 0          0          0 /run/snapd/ns...
tracker-miner-f  1687 POSIX 2.7M READ  0 1073741826 1073742335 /home/ganesh/.cac
tracker-miner-f  1687 POSIX  32K READ  0        128        128 /home/ganesh/.cac
tracker-miner-f  1687 POSIX 1.3M READ  0 1073741826 1073742335 /home/ganesh/.cac
tracker-miner-f  1687 POSIX  32K READ  0        128        128 /home/ganesh/.cac
tracker-miner-f  1687 POSIX 1.4M READ  0 1073741826 1073742335 /home/ganesh/.cac
tracker-miner-f  1687 POSIX  32K READ  0        128        128 /home/ganesh/.cac
tracker-miner-f  1687 POSIX 1.2M READ  0 1073741826 1073742335 /home/ganesh/.cac
tracker-miner-f  1687 POSIX  32K READ  0        128        128 /home/ganesh/.cac
tracker-miner-f  1687 POSIX 1.2M READ  0 1073741826 1073742335 /home/ganesh/.cac
tracker-miner-f  1687 POSIX  32K READ  0        128        128 /home/ganesh/.cac
tracker-miner-f  1687 POSIX 1.4M READ  0 1073741826 1073742335 /home/ganesh/.cac
tracker-miner-f  1687 POSIX  32K READ  0        128        128 /home/ganesh/.cac
tracker-miner-f  1687 POSIX 1.2M READ  0 1073741826 1073742335 /home/ganesh/.cac
tracker-miner-f  1687 POSIX  32K READ  0        128        128 /home/ganesh/.cac
VBoxClient       2259 POSIX   5B WRITE 0          0          0 /home/ganesh/.vbo
VBoxClient       2260 POSIX   5B WRITE 0          0          0 /home/ganesh/.vbo
VBoxClient       2267 POSIX   5B WRITE 0          0          0 /home/ganesh/.vbo
VBoxClient       2272 POSIX   5B WRITE 0          0          0 /home/ganesh/.vbo
VBoxClient       2277 POSIX   5B WRITE 0          0          0 /home/ganesh/.vbo
update-notifier  2399 FLOCK      WRITE 0          0          0 /run/user/1000/up
firefox          2524 POSIX      WRITE 0          0          0 /home/ganesh/snap
firefox          2524 POSIX  96K WRITE 0 1073741826 1073742335 /home/ganesh/snap
firefox          2524 POSIX 512K WRITE 0 1073741826 1073742335 /home/ganesh/snap
firefox          2524 POSIX   5M WRITE 0 1073741826 1073742335 /home/ganesh/snap
firefox          2524 POSIX   5M WRITE 0 1073741826 1073742335 /home/ganesh/snap
cron              642 FLOCK      WRITE 0          0          0 /run/snapd/ns...
snapd             669 FLOCK      WRITE 0          0          0 /var/snap/firefox
VBoxDRMClient    1131 POSIX      WRITE 0          0          0 /run/snapd/ns...
pipewire         1588 FLOCK      WRITE 0          0          0 /run/user/1000/pi
gnome-shell      1741 FLOCK      WRITE 0          0          0 /run/user/1000/wa
VBoxClient       2095 POSIX   5B WRITE 0          0          0 /home/ganesh/.vbo
VBoxClient       2096 POSIX   5B WRITE 0          0          0 /home/ganesh/.vbo
VBoxClient       2268 POSIX   5B WRITE 0          0          0 /home/ganesh/.vbo
firefox          2524 POSIX   5K WRITE 0 1073741826 1073742335 /home/ganesh/snap
firefox          2524 POSIX 224K WRITE 0 1073741826 1073742335 /home/ganesh/snap
firefox          2524 POSIX 256K WRITE 0 1073741826 1073742335 /home/ganesh/snap
firefox          2524 POSIX  64K WRITE 0 1073741826 1073742335 /home/ganesh/snap
firefox          2524 POSIX  14K WRITE 0 1073741826 1073742335 /home/ganesh/snap
```


# RESULT:
The programs are executed successfully.
