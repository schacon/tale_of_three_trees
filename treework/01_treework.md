!SLIDE subsec center

# ACT TWO #

## Working With Trees ##

!SLIDE subsec bold

# git status #

!SLIDE example

<pre>
<b>$ git status</b>
# On branch master
# Your branch is behind 'origin/master' by 2 commits,
#  and can be fast-forwarded.
#
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       <span class="green">modified:   jobs/email_reply.rb</span>
#
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes 
#     in working directory)
#
#       <span class="red">modified:   app/helpers/users_helper.rb</span>
#       <span class="red">modified:   test/unit/email_reply_job_test.rb</span>
#
</pre>

!SLIDE example

<pre>
<b>$ git status</b>
# On branch master
# Your branch is behind 'origin/master' by 2 commits,
#  and can be fast-forwarded.
#
# Changes to be committed:
#   <strong>HEAD and index differ</strong>
#
#       <span class="green">modified:   jobs/email_reply.rb</span>
#
# Changed but not updated:
#   <strong>index and working directory differ</strong>
#
#
#
#       <span class="red">modified:   app/helpers/users_helper.rb</span>
#       <span class="red">modified:   test/unit/email_reply_job_test.rb</span>
#
</pre>

!SLIDE center

![only a file](ex1.png)

!SLIDE center

![git init](ex2.png)

!SLIDE center

![git add](ex3.png)

!SLIDE center

![git commit](ex4.png)

!SLIDE center

![modify](ex5.png)

!SLIDE

<pre>
<b>$ git status</b>
# On branch master
# Your branch is behind 'origin/master' by 2 commits,
#  and can be fast-forwarded.
#
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes 
#     in working directory)
#
#       <span class="red">modified:   file.txt</span>
#
</pre>

!SLIDE center

![modify](ex5.png)

!SLIDE center

![git add](ex6.png)

!SLIDE

<pre>
<b>$ git status</b>
# On branch master
# Your branch is behind 'origin/master' by 2 commits,
#  and can be fast-forwarded.
#
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       <span class="green">modified:   file.txt</span>
#
</pre>

!SLIDE center

![git add](ex6.png)

!SLIDE center

![git commit](ex7.png)

!SLIDE subsec bold

# git reset #

!SLIDE subsec

# 2 forms #

!SLIDE code smaller

# git reset [commit] [path] #

# git reset [commit] #

!SLIDE code

# 1. Path Form #

## git reset [commit] [path] ##

!SLIDE

## `git reset [file]` ##

# is the opposite of #

## `git add [file]` ##

!SLIDE

![](reset-path1.png)

!SLIDE

![](reset-path2.png)

!SLIDE

# Reset to <br/> an older file #

!SLIDE

![](reset1.png)

!SLIDE

![](reset-path3.png)

!SLIDE

<pre>
<b>$ git status</b>
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       <span class="green">modified:   file.txt</span>
#
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       <span class="red">modified:   file.txt</span>
#
</pre>

!SLIDE code

# 2. Commit Form #

## git reset [commit] ##

!SLIDE bullets list incremental

# Reset Options #

* **--soft** move HEAD to target
* **[--mixed]** then copy to index
* **--hard** then copy to work dir

!SLIDE title

# --soft #

## move HEAD to another commit ##

!SLIDE center

![git commit](reset1.png)

!SLIDE center

![reset soft](reset-soft.png)

!SLIDE title

# --mixed #

## move HEAD to another commit, then copy into index ##

!SLIDE center

![git commit](reset1.png)

!SLIDE center

![reset soft](reset-mixed.png)

!SLIDE title

# --hard #

## move HEAD, copy to index, copy to working directory ##

!SLIDE center

![git commit](reset1.png)

!SLIDE center

![reset soft](reset-hard.png)

!SLIDE

# WTFWIEWTUT #
### whythefuckwouldieverwanttousethis ###


!SLIDE

# unstaging changes #

!SLIDE code

## git reset HEAD -- file ##

!SLIDE

![](reset-path1.png)


!SLIDE

# undo last commit #

!SLIDE smaller

# `git reset [--mixed] HEAD~` #

## moves HEAD back and moves index back ##

!SLIDE

![](reset1.png)

!SLIDE

![](reset-mixed.png)

!SLIDE

# squash the last 2 commits into one #

!SLIDE small

# git reset --soft HEAD~2 #
# git commit #

## moves HEAD back, keeps index ##

!SLIDE

![](reset1.png)

!SLIDE

![](squash1.png)

!SLIDE

![](squash2.png)



!SLIDE

# &lt;/reset>

!SLIDE center

![cert](cert.jpg)

!SLIDE subsec bold

# git checkout #

!SLIDE subsec

# just a bit outside #
### tried the corner and missed ###

!SLIDE subsec

# 2 forms #

!SLIDE code smaller

# git checkout [commit] [path] #

# git checkout [commit] #

!SLIDE center

![](reset-checkout.png)

!SLIDE center

# Reset v. Checkout #

<table class="rdata">
  <tr>
    <th></th>
    <th>HEAD</th>
    <th>Index</th>
    <th>Work Dir</th>
    <th>WD Safe</th>
  </tr>
  <tr class="level">
    <th>Commit Level</th>
    <td colspan="4">&nbsp;</th>
  </tr>
  <tr class="even">
    <th class="cmd">reset --soft [commit]</th>
    <td class="yes">REF</td>
    <td class="no">NO</td>
    <td class="no">NO</td>
    <td class="yes-wd">YES</td>
  </tr>
  <tr class="odd">
    <th class="cmd">reset [commit]</th>
    <td class="yes">REF</td>
    <td class="yes">YES</td>
    <td class="no">NO</td>
    <td class="yes-wd">YES</td>
  </tr>
  <tr class="even">
    <th class="cmd">reset --hard [commit]</th>
    <td class="yes">REF</td>
    <td class="yes">YES</td>
    <td class="yes">YES</td>
    <td class="no-wd">NO</td>
  </tr>
  <tr class="odd">
    <th class="cmd">checkout [commit]</th>
    <td class="yes">HEAD</td>
    <td class="yes">YES</td>
    <td class="yes">YES</td>
    <td class="yes-wd">YES</td>
  </tr>
  <tr class="level">
    <th>File Level</th>
    <td colspan="4">&nbsp;</th>
  </tr>
  <tr class="even">
    <th class="cmd">reset (commit) [file]</th>
    <td class="no">NO</td>
    <td class="yes">YES</td>
    <td class="no">NO</td>
    <td class="yes-wd">YES</td>
  </tr>
  <tr class="odd">
    <th class="cmd">checkout (commit) [file]</th>
    <td class="no">NO</td>
    <td class="yes">YES</td>
    <td class="yes">YES</td>
    <td class="no-wd">NO</td>
  </tr>
</table>

