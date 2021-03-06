require 'rubygems'
require 'jammit'

desc "Generate content"
namespace :content do
  desc "Delete generated content"
  task :clean do
    puts "Deleting content in _site"
    system('rm -r _site/*')
  end

  desc "Generate content for local testing"
  task :development => :clean do
    puts "Generating content for local testing"
    system('jekyll build')
  end

  desc "Generate production content"
  task :production => :clean do
    puts "Generating content for production"
    system('jekyll build')
  end
end

desc "Generate styles"
namespace :styles do
  task :clean do
    puts "Deleting generated styles"
    system('rm styles/main.css styles/screen.css')
  end

  task :compile do
    puts "Compiling Sass"
    system('compass compile --sass-dir styles --css-dir styles')
  end

  task :concatenate do
    puts "Concatenating CSS"
    Jammit.package!({:config_path => '_assets.yml', :output_folder => 'styles'})
  end

  task :build => [:clean, :compile, :concatenate]
end

desc "Complete build"
namespace :build do
  task :development => [:"styles:build", :"content:development"]
  task :production => [:"styles:build", :"content:production"]
end

desc "Start development server"
task :server do
  system('jekyll serve --drafts --watch')
end

desc "Builds and deploys to remote production server"
task :deploy => [:"build:production"] do
  system('rsync -avrz --delete -e "ssh -i /Users/svoisen/.ssh/id_rsa" _site/ svoisen@voisen.org:photosatori.co')
  puts "Production site deployed!"
end

task :default => :"build:development"
