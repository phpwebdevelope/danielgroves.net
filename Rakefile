task default: %w[build]
$linebreak = "\n\n =========================\n"

task :version do
    jekyll "--version"
end

task :watch do
    jekyll "serve --watch --future --drafts"
end

task :build => :version do
    clean
    puts $linebreak
    puts "Building for production"
    jekyll "build"
end

task :build_all => :version do
    clean
    puts $linebreak
    puts "Building with all future and draft posts"
    jekyll "build --future --drafts"
end

task :deploy => :build do
    if "#{ENV['CI_BUILD_REF_NAME']}" == "master"
        puts $linebreak
        puts "On master branch, will attempt to deploy"
        system "rsync -avz --omit-dir-times --no-perms --delete _site/ #{ENV['REMOTE']}"

        puts $linebreak
        puts "Attempting to push open Git Repo"
        system "git remote add github git@github.com:danielgroves/danielgroves.net.git"
        system "git reset HEAD --hard"
        system "git checkout master"
        system "git push github master"
    else
        puts $linebreak
        puts "Cannot deploy non-master branch"
    end
end

def jekyll(args)
    system "jekyll #{args}"
end

def clean
    puts $linebreak
    puts "Cleaning previous builds"
    system "rm -Rf _site/"
end
