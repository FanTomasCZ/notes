Try adding the following to `~/.inputrc` file:
```
# Use [Tab] and [Shift]+[Tab] to cycle through all the possible completions:
"\t": menu-complete
"\e[Z": menu-complete-backward
set completion-ignore-case
```
(breaks Home/End buttons and does not enter into directores)