# Exclude subdirectory branchs using grep

Unfortunately, the pattern that "--exclude-dir" accepts cannot contain a "/".
It can only match a directory name so there is no way to INCLUDE
a file in a directory of a given name and also EXCLUDE a file that is
in a directory of the same name but somewhere else in the directory structure. Here is an example of a grep that exlcudes some 'singular' directories.
```Bash
grep -r -E --include "*.js" "null|tom" . --exclude-dir="node_modules" --exclude-dir="Chakra*" --exclude-dir="Google Civic*" --exclude-dir="dist"
```

To overcome this shortcomming of "grep" I used "find" to exclude some branches of the subdirectory structure and then fed the resultant files to grep.  Note,
https://www.man7.org/linux/man-pages/man1/find.1.html appears to be the official documentation for the GNU "find" command.

Here is a step by setp evolution of the development of that solution.

1. This "find" includes every occurence of JavaScript files in the current directory tree: `find . -name "*.js"`
1. This "find" is enhanced with a branch exclusion of "./subdir-level-1a/subdir-level-1ab/*": `find . -not -path "./subdir-level-1a/subdir-level-1ab/*" -name "*.js"`
1. This "find" is enhanced to exclude two branches: `find . -not -path "./subdir-level-1a/subdir-level-1ab/*" -not -path  "./subdir-level-1b/subdir-level-1bb/*" -name "*.js"`
1. Here we tack on the grep using the 'exec' action: `find . -not -path "./subdir-level-1a/subdir-level-1ab/*" -not -path  "./subdir-level-1b/subdir-level-1bb/*" -name "*.js" -exec grep -n 'langan' {} +`
