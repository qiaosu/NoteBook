Git
=======

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
