!SLIDE subsection center

# Environment Variables #

!SLIDE 

## moving around your git pieces ##

!SLIDE bullets incremental

* git directory
* index file
* working directory

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

# Overwriting your credentials #

!SLIDE code

# GIT_AUTHOR_NAME #
# GIT_AUTHOR_EMAIL #

