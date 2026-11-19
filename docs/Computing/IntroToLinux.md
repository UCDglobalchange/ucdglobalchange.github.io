# Introduction to Linux and the Command Line

FARM runs Linux, and you'll interact with it through a text **shell** (bash). A few dozen commands
cover most daily work.

## Navigating

```bash
pwd                   # where am I?
ls -lh                # list files with sizes
cd /quobyte/emoniergrp   # change directory
cd ~                  # go home
cd ..                 # up one level
```

## Files and directories

```bash
mkdir -p results/figs        # make directories
cp file.nc backup/           # copy (-r for directories)
mv old.txt new.txt           # move / rename
rm file.txt                  # delete — there is no trash can!
less file.txt                # page through a file (q to quit)
head -n 20 file.csv          # first 20 lines
tail -f logs/job-123.out     # follow a growing log file
```

## Finding things

```bash
grep -rn "precip" scripts/          # search text in files
find . -maxdepth 2 -name "*.nc"     # find files by name (keep it shallow on the login node)
du -sh *                            # size of each item here
qinfo quota /quobyte/emoniergrp     # space used on the group share
```

## Permissions

```bash
chmod +x run.sh             # make a script executable
chmod -R g+rX shared_dir    # let lab mates read a directory
```

## Editing files

- `nano` is the easiest terminal editor.
- `vim` is powerful but has a learning curve (`vimtutor` teaches the basics).
- Or edit remotely from VS Code with the *Remote – SSH* extension (editing only; run heavy work in
  jobs).

## Staying connected: tmux

`tmux` keeps sessions alive if your connection drops:

```bash
tmux new -s work      # start a session
# Ctrl-b d            # detach
tmux attach -t work   # reattach later
```

## Learn more

- Software Carpentry, *The Unix Shell*: <https://swcarpentry.github.io/shell-novice/>
- UC Davis DataLab workshops: <https://datalab.ucdavis.edu>
