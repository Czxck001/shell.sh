# Shell Fundamentals
Syntax, commands, etc.

### Broadcast Command on Multiple Terms
```sh
cat filename | xargs -I {} cp {} new_dir
ls | xargs -n1 -I {} cp {} new_dir
```

[xargs manual](http://man7.org/linux/man-pages/man1/xargs.1.html)

### grep hidden files
```
grep -r search * .*
```
[How can I grep hidden files?](https://stackoverflow.com/questions/10375689/how-can-i-grep-hidden-files)

### Find non-utf8 charactors
```
grep -raxv '.*' [file-or-directory-path]
```

## Search Given Pattern in Files and Print Context Lines
`grep -C 3 -rI v_light .`

### Diff two folders
```
diff -rq folder1 folder2
```
[Compare two folders' contents in Terminal](https://www.macworld.com/article/1132219/termfoldercomp.html)

## Diff two binaries using process substitution
```sh
vimdiff <(hexdump -v demo.axf) <(hexdump -v demo_output.axf)
```
[process substitution](http://tldp.org/LDP/abs/html/process-sub.html)

## Recursively Replace Text in Files
Find `MIS` contained in files in current directory and replace them by `nat`
```bash
sed -i 's/MIS/nat/g' `grep MIS -rl .`
```

Substitute with matched group(s). Example: substitute `yaml.load(whatever)` with `yaml_safe_load(whatever)`.
```
sed -i -E 's/yaml.load\((.*)\)/yaml_safe_load\(\1\)/g' `grep 'yaml.load(' -rl .`
```

On MacOS

[A “normal” sed on Mac](https://daoyuan.li/a-normal-sed-on-mac/)

The meaning of back quotes:

[Wildcards, Quotes, Back Quotes and Apostrophes in shell commands](https://www.codecoffee.com/tipsforlinux/articles/26-2.html).

## Suppress output from a command
Use `scriptname >/dev/null 2>&1`.

See [SOF-discussion](http://stackoverflow.com/questions/617182/with-bash-scripting-how-can-i-suppress-all-output-from-a-command)

### Remove the files created just now
```
ll | grep "Feb 27 23:" | awk "{print \$9}" | xargs rm -rf
```

### Simple profiling (i.e. Maximum Memory Usage)
For Mac:
```bash
/usr/bin/time -l ls / >/dev/null
```

For Linux:
```bash
/usr/bin/time -v ls / >/dev/null
```

### Recursively change mode
```bash
find . -type f -exec chmod 644 {} \;
```


### Change directory ownership

```
# You should change the ownership of these directories to your user.
sudo chown -R $(whoami) /usr/local/sbin
  
# And make sure that your user has write permission.
chmod u+w /usr/local/sbin
```
