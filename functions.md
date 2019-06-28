# Functions

## dirt
Launch a docker container into interactive mode. Mount current directory.

```sh
dirt() {
  if [ "$#" -gt 1 ]
  then
    wd=$2
  else
    wd='.'
  fi
  docker run -v $(cd $wd ; pwd):/tmp -w /tmp -it $1
}
```
e.g.
```sh
dirt ubuntu:latest  # mount .
dirt ubuntu:latest path/to/your/directory
```

## prune-merged-on-remote

Prune all merged branches on all remotes.

**NOTE**: For safety, this function is a dry-run.

**NOTE**: Run this function in a clean repository (without extra local branches).

**NOTE**: It will also try prune the default branch (will fail, for sure).

```sh
prune-merged-on-remote() {
  for branch in `git branch -r --merged | grep -v HEAD`
  do
    remote=`echo ${branch} | cut -d"/" -s -f1`
    branch_name=`echo ${branch} | cut -d"/" -s -f2`
    echo "git push ${remote} :${branch_name}"
  done
}
```

e.g.

```sh
prune-merged-on-remote  # dry-run
zsh <(prune-merged-on-remote)  # do the thing
```

## archive-stale-remote-branch

Archive all stale branches on remotes into tags starting with `archive/`.

**NOTE**: For safety, this function is a dry-run.

**NOTE**: Run this function in a clean repository (without extra local branches).

```sh
archive-stale-remote-branch() {
  for branch in `git branch -r --no-merged | grep -v HEAD`
  do
    remote=`echo ${branch} | cut -d"/" -s -f1`
    branch_name=`echo ${branch} | cut -d"/" -s -f2`
    date=`git log --no-merges -n 1 --format="%ci" $branch | head -n 1 | cut -d" " -f1`
    if [[ $date < $1 ]];
    then
      echo "git tag archive/${branch_name} ${branch}"
      echo "git push ${remote} :${branch_name}"
    fi
  done
  echo "git lfs fetch --all"  # Otherwise 'git push --tags' may fail.
  echo "git push --tags"
}
```
e.g.
```sh
zsh <(archive-stale-remote-branch 2019-04)  # Archive all branches inactive since April 2019.
zsh <(archive-stale-remote-branch 2019-04-29)  # Archive all branches inactive since April 29th, 2019.
```
