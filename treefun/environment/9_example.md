!SLIDE

# pulling it all together #

!SLIDE

# auto-backup system #

!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
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
    end
    
!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    end

!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
      `rm /tmp/backup_index`
      ENV['GIT_INDEX_FILE'] = '/tmp/backup_index'
    
    
    
    
    
    
    
    
    
    
    
    
    end


!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
      `rm /tmp/backup_index`
      ENV['GIT_INDEX_FILE'] = '/tmp/backup_index'
    
      last_commit = `git rev-parse #{back_branch}`.strip
      last_tree   = `git rev-parse #{back_branch}^{tree}`.strip
    
    
    
    
    
    
    
    
    
    end

!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
      `rm /tmp/backup_index`
      ENV['GIT_INDEX_FILE'] = '/tmp/backup_index'
    
      last_commit = `git rev-parse #{back_branch}`.strip
      last_tree   = `git rev-parse #{back_branch}^{tree}`.strip
    
      `git add --all`
      next_tree = `git write-tree`.strip
    
    
    
    
    
    
    end

!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
      `rm /tmp/backup_index`
      ENV['GIT_INDEX_FILE'] = '/tmp/backup_index'
    
      last_commit = `git rev-parse #{back_branch}`.strip
      last_tree   = `git rev-parse #{back_branch}^{tree}`.strip
    
      `git add --all`
      next_tree = `git write-tree`.strip
    
      if last_tree != next_tree
    
    
    
      end
    end

!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
      `rm /tmp/backup_index`
      ENV['GIT_INDEX_FILE'] = '/tmp/backup_index'
    
      last_commit = `git rev-parse #{back_branch}`.strip
      last_tree   = `git rev-parse #{back_branch}^{tree}`.strip
    
      `git add --all`
      next_tree = `git write-tree`.strip
    
      if last_tree != next_tree
         extra = last_commit.size == 40 ? "-p #{last_commit}" : ''
         
         
      end
    end

!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
      `rm /tmp/backup_index`
      ENV['GIT_INDEX_FILE'] = '/tmp/backup_index'
    
      last_commit = `git rev-parse #{back_branch}`.strip
      last_tree   = `git rev-parse #{back_branch}^{tree}`.strip
    
      `git add --all`
      next_tree = `git write-tree`.strip
    
      if last_tree != next_tree
         extra = last_commit.size == 40 ? "-p #{last_commit}" : ''
         csha = `echo 'back' | git commit-tree #{next_tree} #{extra}`
         
      end
    end

!SLIDE tiny

    @@@ ruby
    back_branch = 'refs/heads/backup'

    project = "/Users/schacon/projects/git/simplegit"

    Dir.chdir project do
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
    end

!SLIDE

# Let's Try It #

!SLIDE

# Another Example #


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

