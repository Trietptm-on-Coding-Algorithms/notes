- http://www.gnu.org/software/sed/manual/sed.html

Command line limits (bash, zsh) ??

# Basic `sed` usage
```
sed -i 's/old/new/g' file                 # basic case
sed -i '.bak' 's/old/new/g' file          # basic case with backup
sed -i 's/old/new/g' file1 file2          # tedious of file list is long
sed -i 's/old/new/g' <shell-glob-pattern> # may hit command line limit
sed -i 's/old/new/g' $(find . -type f -name <pattern>) # may hit command line limit
```

# In the shell script
```
for i in <shell-glob-pattern>; do
    sed -i 's/old/new/g' "$i"             # "$i" is wrapped in double quotes to protect filenames with spaces from splitting into separate sed arguments.
done                                      # launches multiple sed processes
```


# I'm surprised nobody has mentioned the -exec argument to find, which is probably the right way of doing this:

find -type f -name 'xa*' -exec sed -i 's/asd/dsg/g' {} \;

# Alternatively, one could use xargs, which will invoke fewer processes:

find -type f -name 'xa*' | xargs sed -i 's/asd/dsg/g'


grep -Rn '**xxxx**' /path | awk -F: '{print $1}' | xargs sed -i 's/**xxxx**/**yyyy**/'
```
