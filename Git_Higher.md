### rebase
```shell
# rebase experiment to master
git checkout experiment
git rebase master
```

![rebase experiment to master](https://git-scm.com/book/en/v2/images/basic-rebase-2.png)
```shell
# fast-forward
git checkout master
git merge experiment
```

![fast-forward](https://git-scm.com/book/en/v2/images/basic-rebase-4.png)


### A more interesting rebase example
![example](https://git-scm.com/book/en/v2/images/interesting-rebase-1.png)

```shell
# rebase client part to master but not in server
git rebase --onto master server clint
```

![rebase client part to master but not in server](https://git-scm.com/book/en/v2/images/interesting-rebase-2.png)


```shell
# fast-forward
git checkout master
git merge client
```

![fast-forward](https://git-scm.com/book/en/v2/images/basic-rebase-4.png)


```shell
# another method rebase server to master no need to checkout to server
git rebase master server
```

![rebase server to master](https://git-scm.com/book/en/v2/images/interesting-rebase-4.png)


```shell
# fast-forward
git checkout master
git merge server
```

![fast-forward](https://git-scm.com/book/en/v2/images/basic-rebase-4.png)

# 变基的原则
***不要为发布在远程仓库或分享你的代码，别人以此代码为基础做开发时使用变基***
