!SLIDE subsec

# ACT THREE #

## Fun With Your Trees ##

!SLIDE bullets listcode incremental

# Patchy Work #

* git add --patch [file]
* git reset --patch (commit) [file]
* git checkout --patch (commit) [file]

!SLIDE code

# git read-tree #

!SLIDE code

# git write-tree #

!SLIDE commandline incremental

    $ ls
    README          Rakefile        lib

    $ git init
    Initialized empty Git repository in /private/tmp/gt/.git/
    nothing added to commit but untracked files present (use "git add" to track)

    $ git log
    fatal: bad default revision 'HEAD'

    $ git add --all

    $ git write-tree
    4b8ad0172510761cb0e07d2c4220932bf41bbd07

    $ git ls-tree 4b8ad0172510761cb0e07d2c4220932bf41bbd07
    100644 blob 45dc653de6860...    README
    100644 blob ea3fe2ac46e92...    Rakefile
    040000 tree 99f1a6d12cb4b...    lib

!SLIDE code

# git commit-tree #

!SLIDE

# Environment Variables #

!SLIDE 

## moving around your trees ##

!SLIDE code

# GIT_DIR #

!SLIDE commandline incremental

	$ mv .git /opt/repo.git

	$ git --git-dir=/opt/repo.git log

	$ export GIT_DIR=/opt/repo.git
	$ git log

!SLIDE code

# GIT_INDEX_FILE #

!SLIDE commandline incremental

	$ git status -s
	 M README
	 M kidgloves.rb
	
	$ git add kidgloves.rb 
	
	$ git status -s
	 M README
	S  kidgloves.rb

!SLIDE commandline incremental 

	$ export GIT_INDEX_FILE=/tmp/index
	$ git read-tree HEAD
	$ git add README 
	
	$ git status -s
	S  README
	 M kidgloves.rb

!SLIDE commandline incremental 

	$ unset GIT_INDEX_FILE
	
	$ git status -s
	 M README
	S  kidgloves.rb
	
	$ export GIT_INDEX_FILE=/tmp/index
	
	$ git status -s
	S  README
	 M kidgloves.rb

!SLIDE code

# GIT_WORK_TREE #

!SLIDE commandline incremental

	$ git status -s
	 M README
	S  kidgloves.rb

	$ export GIT_DIR=$(pwd)/.git
	$ export GIT_WORK_TREE=$(pwd)

	$ cd /tmp
	$ git status -s
	 M README
	S  kidgloves.rb

!SLIDE

# WTFWIEWTUT #
### whythefuckwouldieverwanttousethis ###

!SLIDE smaller

# Publishing Docs to <br/> Another Branch #

    @@@ ruby
    task :publish_docs do
      `rocco libgit.rb` # creates libgit.html
      ENV['GIT_INDEX_FILE'] = '/tmp/i'
      `git add -f libgit.html`
      tsha = `git write-tree`
      csha = `echo 'boom' | git commit-tree #{tsha}`
      `git update-ref refs/heads/gh-pages #{csha}`
      `git push -f origin gh-pages`
    end

!SLIDE

# Making Tarballs of <br/> Project Subsets #

!SLIDE smaller

    @@@ ruby
    `rm /tmp/in`
    ENV['GIT_INDEX_FILE'] = '/tmp/in'
    `git read-tree --prefix lib   master:lib`
    `git read-tree --prefix ext-m extras:ext`
    tsha = `git write-tree`
    `git archive --format=zip -o out.zip #{tsha}`

!SLIDE commandline incremental

    $ unzip out.zip | head
    Archive:  out.zip
       creating: ext-m/
       creating: ext-m/java/
       creating: ext-m/java/nokogiri/
      inflating: ext-m/java/nokogiri/EncodingHandler.java  
      inflating: ext-m/java/nokogiri/HtmlDocument.java  
      inflating: ext-m/java/nokogiri/HtmlElementDescription.java  
      inflating: ext-m/java/nokogiri/HtmlEntityLookup.java  
      inflating: ext-m/java/nokogiri/HtmlSaxParserContext.java  
      inflating: ext-m/java/nokogiri/NokogiriService.java  

!SLIDE backup

# Auto-Backup Script #

    @@@ ruby
    back_branch = 'refs/heads/backup'
    
    `rm /tmp/backup_index`
    ENV['GIT_INDEX_FILE'] = '/tmp/backup_index'
    
    last_commit = `git rev-parse #{back_branch}`.strip
    last_tree   = `git rev-parse #{back_branch}^{tree}`.strip
    
    `git add --all`
    next_tree = `git write-tree`.strip
    
    if last_tree != next_tree
      extra = last_commit.size == 40 ? "-p #{last_commit}" : ''
      csha = `echo 'back' | git commit-tree #{next_tree} #{extra}`
      `git update-ref #{back_branch} #{csha}`
    end

!SLIDE commandline incremental

    $ git log backup
    commit e3219f9d18ac485f563995a39c139736abd75420
    Author: Scott Chacon <schacon@gmail.com>
    Date:   Thu Mar 31 13:51:05 2011 -0700

        back

    commit cff888a65f56572358bdd233fe6af46c48f1d36d
    Author: Scott Chacon <schacon@gmail.com>
    Date:   Thu Mar 31 13:50:54 2011 -0700

        back

    commit 15f3b561b351187bf712037f267036b90438c987
    Author: Scott Chacon <schacon@gmail.com>
    Date:   Thu Mar 31 13:45:56 2011 -0700

        back

!SLIDE

# Freeze Submodules Before Push #

!SLIDE tiny

    @@@ ruby
    current_commit = `git rev-parse HEAD`
    current_tree = `git rev-parse HEAD^{tree}`

    # get a list of submodules
    status = `git submodule status`.chomp
    subdata = status.split("\n")
    subdata.each do |subline|
      sharaw, path = subline.split(" ")
      sha = sharaw[1, sharaw.size - 1]
      remote = path.gsub('/', '-')
      `git remote add #{remote} #{path} 2>/dev/null` # fetch each submodule into a remote
      `git fetch #{remote}`
      `git read-tree --prefix=#{path} #{sha}` # for each submodule/sha, read-tree the sha into the path
    end

    # find heroku parent
    prev_commit = `git rev-parse heroku 2>/dev/null`.chomp  
    pcommit = (prev_commit != "heroku") ? "-p #{prev_commit}" : ''

    # write-tree/commit-tree with message of what commit sha it's based on
    tree_sha = `git write-tree`.chomp
    commit_sha = `echo "deploy at #{current_commit}" | git commit-tree #{tree_sha} #{pcommit}`

    # update-ref
    `git update-ref refs/heads/heroku #{commit_sha}`

    # reset
    `git reset HEAD`
