Elim - a script to replace the 'rm' command in Linux.
Creates recycle bins in sepcified disks and unwanted files are sent to these.

1. Copy script into a folder that is in your path (typing echo $PATH will tell you this).
    For CERN's lxplus, it is /afs/cern.ch/user/r/rvallanc/bin/ for me. Put it in an equivalent folder for yourself - you may need to make this folder first!
    For Bham moose I put it in /home/rxv/bin/.

2. cd into this folder where you put elim and type: chmod u+x elim. That just makes it an executable.

3. Open up elim and change the fields in BINPARENTDIRS=(
    "/afs/cern.ch/user/r/rvallanc"
    "/afs/cern.ch/work/r/rvallanc"
    "/eos/user/r/rvallanc"
    )
    to equivalent ones for you.  For Bham I use BINPARENTDIRS=(
    "/home/rxv"
    "/disk/moose/atlas/rxv"
    )
   These are the folders where your recycle bins will be created and files put into.

4. If pulling code from github, this step shouldn't be necessary:
    For some reason, "^M" flags sometimes appear in the code if copying e.g. from an email, which bash complains about.
    In the folder where elim is stored type: sed -i 's/\r/""/g' elim.  That'll get rid of those flags.

5. You're all set.  Just type elim to see a list of options.  Call elim on a file or a directory or combination of things to try. You can test it by making a file called a.txt and then calling elim a.txt.

6. *OPTIONAL* but super useful.  If you're happy, you can set this up with a crontab.  That'll automatically call elim -o every few hours (even if you're logged off) to delete files that have been in the recycle bins more than 2 days.
    On Bham system (ssh into eprexa or b):
    crontab -e
    Then type in: 0 */4 * * * FULL_PATH_OF_ELIM_FILE -o > /dev/null
    (So full path for me is just /home/rxv/bin/elim) - if Mark resets eprexa or b, you need to setup the crontab again btw.
    On lxplus:
    acrontab -e
    Then type in: 0 */4 * * * lxplus.cern.ch FULL_PATH_OF_ELIM_FILE -o > /dev/null




To work the program, supply a list of files and/or directories as arguments to elim:
Type 'elim myFile.txt myDir/ mySymbolicLink'

Type 'elim -b (--bins)' to print out recycle bin locations
Type 'elim -c (--contents)' to print out in recycle bin contents
Type 'elim -r (--recover) path_1 path_2 ... path_N' to recover files/folders from path_1, 2, etc into path_N
Type 'elim -o (--obliterate)' to permanently remove files/folders older than 2 days in all bins
Type 'elim -on (--obliterateNow)' to permanently remove files/folders in all bins now

