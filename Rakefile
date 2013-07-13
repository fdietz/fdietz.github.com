require "rubygems"
require "bundler/setup"
require "shellwords"

Bundler.require

def prompt(message)
  print message
  STDIN.gets.chomp
end

def slug(str)
  str.downcase.gsub(/[^a-z0-9]+/, '-').gsub(/^-|-$/, '');
end

desc 'create a new draft post'
task :post do
  title     = prompt('Title: ')
  slug      = "#{Time.now.strftime('%Y-%m-%d')}-#{slug(title)}"

  file = File.join(File.dirname(__FILE__), '_posts', slug + '.markdown')

  if File.exist?(file)
    puts "Can't create new post: \e[33m#{file}\e[0m"
    puts "  \e[31m- Path already exists.\e[0m"
    exit 1
  end

  File.open(file, "w") do |f|
    f << <<-EOS.gsub(/^    /, '')
    ---
    layout: post
    title: #{title}
    published: false
    ---

    EOS
  end

  system ("#{ENV['EDITOR']} #{file}")
end

desc "Publish blog to gh-pages"
task :publish do
  Dir.mktmpdir do |tmp|
    cp_r "_site/.", tmp
    Dir.chdir tmp
    system "git init"
    system "git add ."
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m #{message.shellescape}"
    system "git remote add origin git@github.com:fdietz/fdietz.github.com.git"
    system "git push origin master --force"
  end
end

desc 'List all draft posts'
task :drafts do
  puts `find ./_posts -type f -exec grep -H 'published: false' {} \\;`
end