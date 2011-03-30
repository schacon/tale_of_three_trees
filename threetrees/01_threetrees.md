!SLIDE subsec center

# ACT ONE #

## The Three Trees ##

![](trees.png)

!SLIDE title center

### first tree ###
# the HEAD #

![](tree-head.png)

!SLIDE commandline incremental

    $ cat .git/HEAD 
    ref: refs/heads/master

    $ cat .git/refs/heads/master 
    e9a570524b63d2a2b3a7c3325acf5b89bbeb131e

    $ git cat-file -p e9a570524b63d2a2b3a7c3325acf5b89bbeb131e
    tree cfda3bf379e4f8dba8717dee55aab78aef7f4daf
    author Scott Chacon <schacon@gmail.com> 1301511835 -0700
    committer Scott Chacon <schacon@gmail.com> 1301511835 -0700

    initial commit

    $ git ls-tree -r cfda3bf379e4f8dba8717dee55aab78aef7f4daf
    100644 blob a906cb2a4a904a152...   README
    100644 blob 8f94139338f9404f2...   Rakefile
    040000 tree 99f1a6d12cb4b6f19...   lib



!SLIDE title center

### second tree ###
# the index #

![](tree-index.png)

!SLIDE

# The Staging Area #

!SLIDE code smaller

    @@@ ruby
    require 'rugged'

    index = Rugged::Index.new("/opt/repo/.git/index");
    index.refresh

    index.each do |entry|
      puts "File Name: " + entry.path
      puts " Blob SHA: " + entry.sha
      puts "File Size: " + entry.file_size.to_s
      puts "File Mode: " + entry.mode.to_s
      puts "    mtime: " + entry.mtime.to_i.to_s
      puts "    ctime: " + entry.ctime.to_i.to_s
      puts "    Inode: " + entry.ino.to_s
      puts "      UID: " + entry.uid.to_s
      puts "      GID: " + entry.gid.to_s
      puts
    end

!SLIDE stagingdata

    File Name: README
     Blob SHA: 45dc653de6860faeb30581cd7654f9a51fc2c443
    File Size: 135
    File Mode: 33188
        mtime: 1301512685
        ctime: 1301512685
        Inode: 15472643
          UID: 501
          GID: 0

    File Name: Rakefile
     Blob SHA: ea3fe2ac46e92bf38dc824128e3eddd397f537e3
    File Size: 604
    File Mode: 33188
        mtime: 1301512703
        ctime: 1301512703
        Inode: 15472659
          UID: 501
          GID: 0

    File Name: lib/simplegit.rb
     Blob SHA: 47c6340d6459e05787f644c2447d2595f5d3a54b
    File Size: 355
    File Mode: 33188
        mtime: 1301507599
        ctime: 1301507599
        Inode: 15445705
          UID: 501
          GID: 0

!SLIDE title center

### third tree ###
# the working directory #

![](tree-workdir.png)

!SLIDE bigcode

    $ tree
    .
    ├── .git
    │   ├── HEAD
    │   ├── [snip]
    │   └── index
    ├── README
    ├── Rakefile
    └── lib
        └── git.rb

!SLIDE subsec center

# Three Trees #
### HEAD, Index and Working Directory ###


![](trees.png)

!SLIDE center

![](workflow.png)

!SLIDE bullets list incremental

# Tree Roles #

* **HEAD** last commit, next parent
* **Index** proposed next commit
* **Work Dir** sandbox
