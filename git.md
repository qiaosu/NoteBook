Git
=======

####Remote

关联存在的remote

<pre><code>$ git remote add origin https://github.com/user/repo.git
# Set a new remote
git remote add heroku git@heroku.com:repo.git </code></pre>

修改remote url

<pre><code>$ git remote set-url origin https://github.com/user/repo2.git
# Change the 'origin' remote's URL</code></pre>

移除remote

<pre><code>$ git remote rm destination
# Remove remote</code></pre>

####同步fork的repo

<https://help.github.com/articles/syncing-a-fork>

**1.Setup**

<pre><code>$ git remote -v
origin  https://github.com/user/repo.git (fetch)
origin  https://github.com/user/repo.git (push)

$ git remote add upstream https://github.com/otheruser/repo.git

$ git remote -v
origin    https://github.com/user/repo.git (fetch)
origin    https://github.com/user/repo.git (push)
upstream  https://github.com/otheruser/repo.git (fetch)
upstream  https://github.com/otheruser/repo.git (fetch)</code></pre>

**2.Syncing**

2.1 Fetching

<pre><code>$ git fetch upstream</code></pre>

2.2 Merging

<pre><code>$ git checkout master

$ git merge upstream/master</code></pre>

####Submodule

<http://chrisjean.com/2009/04/20/git-submodules-adding-using-removing-and-updating/>
<http://www.kafeitu.me/git/2012/03/27/git-submodule.html>

**Add**

<pre><code>$ git submodule add git@github.com:user/repo.git path/folder</code></pre>

check:

<pre><code>$ git status

$ cat .gitmodules</code></pre>

**Remove**

1. 删除.gitmodules文件中关于目标submodule的部分, like:

<pre><code>[submodule "lib/billboard"]

path = lib/billboard

url = git@mygithost:billboard</code></pre>

2. 在.git/config中删除关于目标submodule的部分, 此步骤不是必须的.

3. 清除缓存

<pre><code>$ git rm --cached path/folder</code></pre>

**Update**

1. 在repo的根文件夹:

<pre><code>$ git submodule init #第一次或者执行过sync.

$ git submodule update</code></pre>

2. 进入子模块的目录:

<pre><code>$ git status

$ git checkout master #第一次或者执行过sync.

$ git status

$ git pull</code></pre>


**git pull 使用法**

1. 用 rebase 尽量减少多余的 merge commit

<pre><code>git pull --rebase</code></pre>

2. pull 特定分支例

<pre><code>git pull --rebase origin a-black-and-thick-branch</code></pre>

3. rebase 冲突了, 又不喜欢一步一步的 git rebase 怎么办?

<pre><code>git rebase --abort
    git reset --hard HEAD
    git pull
    git status</code></pre>
