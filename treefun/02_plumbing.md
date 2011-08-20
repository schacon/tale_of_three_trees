!SLIDE center subsection

# Plumbing #

!SLIDE center

![](plumbing.jpg)

!SLIDE code
# rev-parse #

!SLIDE commandline incremental

    $ git rev-parse origin/master
    d7799fadf93647b7c2ae745881882efbc849982b

    $ git rev-parse master~163^2~3^2
    860d7c566bd3041069c5763559341a1b6a2a1305

    $ git rev-parse origin/master ^origin/3-0-stable
    d7799fadf93647b7c2ae745881882efbc849982b
    ^adea1467f6e7ecc7eef03072816a94447db9ff28

    $ git rev-parse origin/3-0-stable..origin/master
    d7799fadf93647b7c2ae745881882efbc849982b
    ^adea1467f6e7ecc7eef03072816a94447db9ff28

!SLIDE code
# hash-object #

!SLIDE commandline incremental small

    $ git hash-object -w ~/.ssh/id_rsa.pub
    b162e87ce3cbdc33a0dbc0752d7950d75176bd77

    $ echo 'my awesome value' | git hash-object -w --stdin
    c0eabb05329c6eded8e3f73f3aca40c4b3d6d974

    $ git show c0eabb05329c6eded8e3f73f3aca40c4b3d6d974
    my awesome value


!SLIDE code
# ls-tree #

!SLIDE commandline incremental smaller

    $ git ls-tree master
    100644 blob a906cb2a4a904a152e80877d4088654daad0c859	README
    100644 blob 8f94139338f9404f26296befa88755fc2598c289	Rakefile
    040000 tree 99f1a6d12cb4b6f19c8655fca46c3ecf317074e0	lib


!SLIDE code
# ls-files #

!SLIDE commandline incremental smaller

    $ git ls-files -s
    100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README
    100644 8f94139338f9404f26296befa88755fc2598c289 0	Rakefile
    100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0	lib/simplegit.rb

    $ echo 'test' >> Rakefile 
    $ git add Rakefile

    $ git ls-files -s
    100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README
    100644 b4a42ae873f6623b25df7b36d0b7233d4d9188af 0	Rakefile
    100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0	lib/simplegit.rb


!SLIDE code
# read-tree #

!SLIDE commandline incremental smaller

    $ git ls-files -s  # show index
    100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README
    100644 8f94139338f9404f26296befa88755fc2598c289 0	Rakefile
    100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0	lib/simplegit.rb

    $ git ls-tree -r HEAD  # show HEAD
    100644 blob a906cb2a4a904a152e80877d4088654daad0c859	README
    100644 blob 8f94139338f9404f26296befa88755fc2598c289	Rakefile
    100644 blob 47c6340d6459e05787f644c2447d2595f5d3a54b	lib/simplegit.rb

    $ git status -s
    $ git read-tree HEAD~2

    $ git ls-files -s  # show index
    100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README
    100644 a874b732e12a5c04b5a73d7f1123c249997b0b2d 0	Rakefile
    100644 a0a60ae62dd2244a68d78151331067c5fb5d6b3e 0	lib/simplegit.rb

    $ git status -s
    MM Rakefile
    MM lib/simplegit.rb

    $ git read-tree HEAD
    $ git status
    # On branch master
    nothing to commit (working directory clean)

!SLIDE code
# update-index #

!SLIDE commandline incremental smaller

    $ git ls-files -s
    100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README
    100644 8f94139338f9404f26296befa88755fc2598c289 0	Rakefile
    100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0	lib/simplegit.rb

    $ echo 'my awesome value' | git hash-object -w --stdin
    c0eabb05329c6eded8e3f73f3aca40c4b3d6d974

    $ git update-index --add --cacheinfo 100644 c0ea...3d6d974 test.txt

    $ git ls-files -s
    100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README
    100644 8f94139338f9404f26296befa88755fc2598c289 0	Rakefile
    100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0	lib/simplegit.rb
    100644 c0eabb05329c6eded8e3f73f3aca40c4b3d6d974 0	test.txt

    $ git status -s
    AD test.txt
    

!SLIDE code
# write-tree #

!SLIDE commandline incremental smaller

    $ git status -s

    $ echo 'test' >> Rakefile 
    $ git add Rakefile

    $ git ls-files -s 
    100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README
    100644 8f94139338f9404f26296befa88755fc2598c289 0	Rakefile
    100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0	lib/simplegit.rb

    $ git write-tree
    cfda3bf379e4f8dba8717dee55aab78aef7f4daf

    $ git ls-tree cfda3bf379e4f8dba8717dee55aab78aef7f4daf
    100644 blob a906cb2a4a904a152e80877d4088654daad0c859	README
    100644 blob 8f94139338f9404f26296befa88755fc2598c289	Rakefile
    040000 tree 99f1a6d12cb4b6f19c8655fca46c3ecf317074e0	lib

    $ git ls-tree -r -t cfda3bf379e4f8dba8717dee55aab78aef7f4daf
    100644 blob a906cb2a4a904a152e80877d4088654daad0c859	README
    100644 blob 8f94139338f9404f26296befa88755fc2598c289	Rakefile
    040000 tree 99f1a6d12cb4b6f19c8655fca46c3ecf317074e0	lib
    100644 blob 47c6340d6459e05787f644c2447d2595f5d3a54b	lib/simplegit.rb

!SLIDE code
# commit-tree #

!SLIDE commandline incremental smaller

    $ git ls-tree -r cfda3bf379e4f8dba8717dee55aab78aef7f4daf
    100644 blob a906cb2a4a904a152e80877d4088654daad0c859	README
    100644 blob 8f94139338f9404f26296befa88755fc2598c289	Rakefile
    100644 blob 47c6340d6459e05787f644c2447d2595f5d3a54b	lib/simplegit.rb

    $ echo 'my commit message' | git commit-tree cfda3bf379e4f8dba8717dee55aab78aef7f4daf
    c0cbf5ec5cac782e09679cbfe7deb109c1e9e6f6

    $ git log c0cbf5ec5cac782e09679cbfe7deb109c1e9e6f6
    commit c0cbf5ec5cac782e09679cbfe7deb109c1e9e6f6
    Author: Scott Chacon <schacon@gmail.com>
    Date:   Wed Aug 17 23:23:36 2011 -0500

        my commit message

!SLIDE code
# update-ref #

!SLIDE commandline incremental smaller

    $ git update-ref refs/heads/newbranch c0cbf5ec5cac782e09679cbfe7deb109c1e9e6f6

    $ git branch
    * master
      newbranch

    $ git log newbranch
    commit c0cbf5ec5cac782e09679cbfe7deb109c1e9e6f6
    Author: Scott Chacon <schacon@gmail.com>
    Date:   Wed Aug 17 23:23:36 2011 -0500

        my commit message


!SLIDE code
# symbolic-ref #

!SLIDE commandline incremental smaller

    $ git symbolic-ref HEAD refs/heads/newbranch

    $ git branch
      master
    * newbranch

    $ git log
    commit c0cbf5ec5cac782e09679cbfe7deb109c1e9e6f6
    Author: Scott Chacon <schacon@gmail.com>
    Date:   Wed Aug 17 23:23:36 2011 -0500

        my commit message


!SLIDE bullets incremental smaller

# the plumbing commands #

* rev-parse
* hash-object
* ls-tree
* ls-files -s
* read-tree

!SLIDE bullets incremental smaller

# the plumbing commands #

* update-index
* write-tree
* commit-tree
* update-ref
* symbolic-ref

!SLIDE smaller

# the plumbing commands #

## `http://bit.ly/plumbing-cheat` ##
