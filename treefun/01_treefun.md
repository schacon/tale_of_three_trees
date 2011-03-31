!SLIDE subsec

# ACT THREE #

## Fun With Your Trees ##

!SLIDE bullets listcode incremental

# Patchy Work #

* git add --patch [file]
* git reset --patch (commit) [file]
* git checkout --patch (commit) [file]

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
