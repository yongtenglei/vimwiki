# Git

#### Diff

```git
# show diff of was unstaged but was changed
git diff

# show diff of was staged
git diff --staged
# or 
git diff --cached
```

#### Commit
```git
git commit

# commit command and commit information in the same line
git commit -m "information balabala"

# skip add(stage) process
git commit -am "information balabala"
```

#### Remove file

```git
# do not using `rm` remove any file directely

# use
git rm filename

# remove changed or staged file before using -f
git rm -f filename

# remove file in Git repository but keep it in local workspace
git rm --cached filename
```

#### Move file

```git
git mv README.md README

# the same as
$ mv README.md README
$ git rm README.md
$ git add README
```

#### History

```git
# show information like patch
git log -p

# show information with statistics
git log --stat

# show information more pretty
git log --pretty=oneline

git log --pretty=format:"%h - %an, %ar : %s"

# limit output entry number
git log -2

git log --since=2.weeks

git log --until=1.day
```

#### Undo

```git
1. git commit --amend

$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend

# undo stage (using git status to get some hint)
2. git reset HEAD filename

# undo any change of a file (using git status to get some hint)
3. git checkout --filename
! dangers, it will discard all changes but use last commit(version) to cover it
```

#### Remote repository

```git
# show repository
git remote -v

# add remote repository
git remote add shortname url

# fetch data what you don't have but don't merge automatically
git fetch remote

# fetch and merge
git pull remote

# push
# git push remote brach
git push origin main

# git push remote local brach:remote brach
git push origin main:main
# the same as
git push origin main

# check repository
git remote show remote

# rename and remove
git remote rename old_name new_name

git remote remove remote
```

# Tags

```git
# list tages
git tag (-l --l to pair)

git tag -l "v1.8.*"

# Tag
# lightweight 轻量标签 and annotated with information 附注标签

# create a annotated tag
git tag -a v1.8 -m "version 1.8"
# you can use "$ git show v1.8" get some information attached this tag

# create a lightweight tag (just given a name)
git tag v1.8

# tagging later
git tag -a v1.7 9fceb02
# 9fceb02 is the first 7 digits of commit checksum (you can use all of checksum or part)

# sharing tag
# git push main(origin) tagname
git push main(origin) v1.5

# muti-tags
git push main(origin) --tags

# delete a tag
git tag -d tagname
# remote
git push main(origin) --delete tagname

# checkout tag
# a better behaviour
# new a new branch at the same time if you want to do some modification
git checkout -b verion1.7 v1.7
```

# Aliases

```git
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

### biaoti
ntiarsntestnrsen

**粗体** nnsetnsrinnietnern
```python
print
```

nsrtienrsitn







