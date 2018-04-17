# Git命令使用
## 本地仓库创建decelop分支
```
git branch develop  
git push -u origin develop  
```

## 同步远程git仓库代码
```
git clone https://github.com/shaoxuefei/GrowingIo.git  
git checkout -b develop origin/develop //作用是checkout远程的dev分支，在本地起名为develop 分支，并切换到本地的dev分支  
```

## 创建功能分支：
```
git checkout -b xxx develop  
```

## 完成功能开发进行develop分支合并
```
git pull origin develop  //拉取  
git checkout develop      //切换本地develop仓库分支  
git merge xxx    //合并xxx分支代码到develop  
git push          //提交  
git branch -d xxx  //删除已经完成的功能分支xxx
```

## 创建发布分支：
```
git checkout -b release-0.1 develop  
```

## 6、整理完开始完成发布：
```
git checkout master  //切换master  
git merge release-0.1 //合并release发布分支  
git push  
git checkout develop  
git merge release-0.1  
git push  
git branch -d release-0.1  //删除已经发布成功的发布分支  
```

## 创建版本Tag
```
git tag -a 0.1 -m "Initial public release" master  
git push --tags  
```

## 发现线上Bug创建维护分支
```
git checkout -b issue-#001 master //基于master创建 
//.....疯狂修复Bug
git checkout master  
git merge issue-#001  
git push  
//合并到develop分支
git checkout develop  
git merge issue-#001  
git push  
git branch -d issue-#001  
根据Tag或者是version来pull代码
git checkout version-0.1  
git pull 
```
