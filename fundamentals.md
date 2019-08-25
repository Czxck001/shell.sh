# Shell Fundamentals
Syntax, commands, etc.

### Broadcast Command on Multiple Terms
```sh
cat filename | xargs -I {} cp {} new_dir
ls | xargs -n1 -I {} cp {} new_dir
```

[xargs manual](http://man7.org/linux/man-pages/man1/xargs.1.html)

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

## Search Given Pattern in Files and Print Context Lines
`grep -C 3 -rI v_light .`

## Suppress output from a command
Use `scriptname >/dev/null 2>&1`.

See [SOF-discussion](http://stackoverflow.com/questions/617182/with-bash-scripting-how-can-i-suppress-all-output-from-a-command)

### Remove the files created just now
```
ll | grep "Feb 27 23:" | awk "{print \$9}" | xargs rm -rf
```
