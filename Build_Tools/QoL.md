## Emmet

| **CSS**                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-prop` - suggest vendors<br>`bg+` - shorthand autofill<br>`bxsh` : box-shadow autofill<br>`tac` - text-align:center;<br>`pos:s` - position: static;<br>`bdtc:t` - border-top-color: transparent;<br>`prop:no` - prop: none;<br>`@f+` - @font-face { src...}<br>`anim-` - shorthand animation | `m20` : px<br>`m20p` : 20%<br>`m20p-40` : 20% 40px<br>`m0-a` : 0 auto;<br>`fz2e` : font-size: 2em;<br>`fz.8r`: 0.8rem;<br>`cr` : color: rgb(0, 0, 0) [_tabs_ to jump edit ]<br>`animdur:30ms`<br>`p2r+fz16` : multiple styles |
| **HTML**                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                               |
| Number<br>`$` replaced by no.  \|  `$@-` reverse no.  <br>`$$$` 001 002...     \|  `$@5`  start from 5<br><br>`c` : comment<br>`select+` `pic+` : add + to autofill required children                                                                                                         | ^ climb up<br>text {hello $}                                                                                                                                                                                                  |

## Linux Commands

### Arguments and Operators

read from file : `cmd < ios.md`
read from terminal : `cmd << ok`  (keep reading until user enters ok)
write to file : `> (overwrite)  >> (append)`
supress output : `cmd > dev/null`
pipe output --> input : `cmd1 | cmd2`

show success/failure for operation : `cmd -v`
create variable : `var='hello'`
show it : `echo $var`
run cmd in bgmode : `cmd &`

_Wildcards_
hello*.jsx :  substr
test?.jsx : 1 char
{c,m,p}at : 'cat' 'mat' 'pat'
{1..9} : 1 2 3 ...9

### SysInfo 

`whoami` : current user
`who` : all users
`man cmd` : detail manual (notes style)
`uname -a/-r` : show OS /detailed / kernel 
`uptime` : time since last boot
`su <user>` : get sudo access
`passwd <user>` : change pwd
`last reboot` : reboot history
`shutdown or reboot` : reboot
`df` : disks/partitions
`du -sh` : folder size
`du -ah` : each file's size
`date > log.txt` : log date
`cmd | wc -l` : line count in output of cmd
`ping <url>` : test internet
`traceroute <url>` : show packet route

### Process Management

`halt` : stop machine
`ps` : display active processes
`top` : dynamic feed
`kill <pid>`
`killall <proc>` : kill all proc*
`bg/fg <job>` : toggle job to foreground/background
`sleep 30` : wait for 30 sec

### Archiving files (-vf recommended)

`tar -c <archive> <files>` : create archive. 
`tar -x <a>` : extract
`tar -a <a1> <a2>` : append archives
`tar -r <a> <f>` : append files to archive

For .gzip : add `-z` flag
For .bz2, : add `-j` flag

### Terminal functions

`clear -x` : clear current view only
`history | less` : show history of cmds. Use `!num` to execute 
`alias bobo='echo $var'` : create cmd alias ; for permanent store in .bashrc
`cmd | less OR less <file>` : show output cleanly.
  - `:n` `:p` `:g` (while viewing to goto next, prev, EoF)
`cmd1 | xargs cmd2` : pass output as args for cmd2
`ln <file/path> <file2 or alias>` : create hard-link (or soft `-s` , better)

`ls /path/` : list files in dir.
  `-l` : show permissions
  `-a` : include hiddens
  `-t` : sort by last modified

`cd /home or cd ~` : goto home

### File Management (require args)

`sort` : outputs lines alphabetically
  - `-n` (by number) , `-r` (reverse) `-u` (1 line for several identical lines)

`rmdir` : remove dir if only empty
`rm -v` : show succ/fail for each deleted
`rm -r` : directory
`rm -ri` : interactive delete (no args)

`mkdir -p Me/You` : create me, inside it you
`cp -v from to` : use `-r` to copy dir 
`diff file1 file2` : show diff lines in files
`cmp file1 file2` : show only first diff line (and no.)
`head/tail -n 3` : show first/last 3 lines
`tail -f` : current last 10 lines in dynamic file

`chmod g+x file` : add execute permission for group
`chmod 754 file` : set file permissions. 
  - `rwx` for owner, `rx` for group and `r` for world
  - `4` – read `2` – write `1` – execute

`chown <owner>:<group> file` : change owner or/and group.

### Search and Filter

`grep "regex" <files>` : show lines that match 
  `-ri` : recursively, case-insensitive, 
  `-v` : show lines NOT matching 
  `-l` : show only filenames

`find . -name "*.txt"` : all text files from current folder onwards 
  `-empty` : empty F/s 
  `-type d/f` : find dir/file. Use before -name 
  `-exec CMD {} \;` : apply cmd on Each result. Used at last.
  `-size +2M` : larger than 2MB 
  `-size +3KB -size -100KB` : range
