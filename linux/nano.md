```
-E, --tabstospaces
Convert typed tabs to spaces.

-T <#cols>, --tabsize=<#cols>
Set the displayed tab length to #cols columns.
The value of #cols must be greater than 0. The default value is 8.
```
For four spaces, the appropriate command would therefore be `nano -ET4`.

---
```shell
# Default editor selection
# Ubuntu
select-editor
# CentOS
echo export EDITOR=/bin/nano >> ~/.bashrc
```

* List keyboard shortcuts <kbd>CTRL</kbd>+<kbd>G</kbd>  
**M** means <kbd>Meta</kbd> (<kbd>Alt</kbd> by default in Linux) - http://superuser.com/questions/581671/find-next-command-in-nano/581691#581691
---
* Select <kbd>ALT</kbd>+<kbd>M</kbd>+<kbd>A</kbd>
* Copy <kbd>ALT</kbd>+<kbd>6</kbd> (with no selection copies current line) - on Linux <kbd>ALT</kbd>+<kbd>Shift</kbd>+<kbd>6</kbd>
* Cut <kbd>Ctrl</kbd>+<kbd>K</kbd> (with no selection cuts current line)
* Paste <kbd>Ctrl</kbd>+<kbd>U</kbd>
---
* Undo <kbd>ALT</kbd>+<kbd>U</kbd>
* Redo <kbd>ALT</kbd>+<kbd>E</kbd>
* Go to the first line <kbd>ALT</kbd>+<kbd>\\</kbd>
* Go to the last line <kbd>ALT</kbd>+<kbd>/</kbd>
* Go to line number <kbd>Ctrl</kbd>-<kbd>_</kbd>
* Find next <kbd>ALT</kbd>+<kbd>W</kbd>
* Find and replace <kbd>CTRL</kbd>+<kbd>\\</kbd>
