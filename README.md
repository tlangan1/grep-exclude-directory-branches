# Exclude paths using grep

Unfortunately, the pattern that "--exclude-dir" accepts cannot contain a "/".
It can only match a directory name so there is no way to include
a file in a directory of a given name and also exclude a file that is
in a directory of the same name but somewhere else in the directory structure.

For example,
```Bash
grep -r -E --include "*.js" "null|tom" . --exclude-dir="node_modules" --exclude-dir="Chakra*" --exclude-dir="Google Civic*" --exclude-dir="dist"
```

To overcome this shortcomming of "grep" I used "find" to exclude a specific subdirectory structure and then fed the resultant files to grep.  Note,
https://www.man7.org/linux/man-pages/man1/find.1.html appears to be the official documentation for the GNU "find" command.

Here is a step by setp evolution of the development of that solution.

THIS IS ALL MESSED UP...I SHOULD SHOW THE EVOLUTION OF THE FIND FIRST, THEN BOLT ON THE GREP.

1. This "find" includes every occurence in the current directory tree: `find . -name "*.js"`
1. This "find" is enhanced with a branch exclusion: "./node_modules/.cache/*": `find . -not -path "./node_modules/.cache/*" -name "*.js"`
1. This "find" is enhanced to exclude two branches: `find . -name *.js -not -path "./node_modules/.cache/*" -not -path  "*/*/dist/*"
1. Here we tack on the grep using the 'exec' operation??? `find . -name *.js -not -path "./node_modules/.cache/*" -not -path  "*/*/dist/*" ! -type d -exec grep -n 'stringOrStrings' {} +`

## I think the "! -type d" portion of this command exlucdes directories but I need to confirm that

## Receate these scripts with a mock directory tree and search item to simpify the example

## Rename this grep-excluding-branches