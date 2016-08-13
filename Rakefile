GH_PAGES_REF = "_site/.git/refs/remotes/origin/gh-pages"

directory "_site"

file GH_PAGES_REF => "_site" do
  repo_url = `git config --get remote.origin.url`.strip

  cd "_site" do
      sh "git init"
      sh "git remote add origin #{repo_url}"
      sh "git fetch origin"

      if `git branch -r` =~ /gh-pages/
        sh "git checkout gh-pages"
      else
        sh "git checkout --orphan gh-pages"
      end
  end
end


desc "Prepare for build"
task :prepare => GH_PAGES_REF

desc "Build static files"
task :build => [:prepare] do
  sh "bundle exec jekyll build"
end

desc "Deploy static files to gh-pages branch"
task :deploy => [:build] do
  message = nil

  head = `git log --pretty="%h" -n1`.strip
  message = "Site updated to #{head}"

  cd "_site" do
    sh 'git add --all'
    if /nothing to commit/ =~ `git status`
      puts "No changes to commit."
    else
      sh "git commit -m \"#{message}\""
    end
    sh "git push origin gh-pages"
  end
end
