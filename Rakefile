# encoding: UTF-8

require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'
require File.join(File.dirname(__FILE__), 'lib', 'mm-devise', 'version')

desc 'Default: run tests for all MongoMapper ORM setups.'
task :default => :pre_commit

desc 'Run Devise tests for all MongoMapper ORM setups.'
task :pre_commit do
  Dir[File.join(File.dirname(__FILE__), 'test', 'orm', '*.rb')].each do |file|
    orm = File.basename(file).split(".").first
    ENV['DEVISE_PATH'] ||= File.expand_path('../devise')
    system "rake test DEVISE_ORM=#{orm} DEVISE_PATH=#{ENV['DEVISE_PATH']}"
  end
end

desc 'Run Devise tests using MongoMapper. Specify path to devise with DEVISE_PATH'
Rake::TestTask.new(:test) do |test|
  ENV['DEVISE_ORM'] ||= 'mongo_mapper'
  ENV['DEVISE_PATH'] ||= File.expand_path('../devise')
  unless File.exist?(ENV['DEVISE_PATH'])
    puts "Specify the path to devise (e.g. rake DEVISE_PATH=/path/to/devise). Not found at #{ENV['DEVISE_PATH']}"
    exit
  end
  test.libs << 'lib' << 'test'
  test.libs << "#{ENV['DEVISE_PATH']}/lib"
  test.libs << "#{ENV['DEVISE_PATH']}/test"
  test.test_files = FileList["#{ENV['DEVISE_PATH']}/test/**/*_test.rb"] + FileList['test/**/*_test.rb']
  test.verbose = true
end

task :test => :check_dependencies

desc 'Generate documentation for dm-devise.'
Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_dir = 'rdoc'
  rdoc.title    = 'mm-devise #{version}'
  rdoc.options << '--line-numbers' << '--inline-source'
  rdoc.rdoc_files.include('README.rdoc')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

begin
  require 'jeweler'
  Jeweler::Tasks.new do |gem|
    gem.name = "mm-devise"
    gem.version = MongoMapper::Devise::VERSION.dup
    gem.summary = %Q{Support for using MongoMapper ORM with devise}
    gem.description = %Q{dm-devise adds MongoMapper support to devise (http://github.com/plataformatec/devise) for authentication support for Rails}
    gem.email = "jmorgan@morgancreative.net"
    gem.homepage = "http://github.com/kristianmandrup/mm-devise"
    gem.authors = ["Kristian Mandrup"]
    gem.add_dependency 'mongo_mapper',   '>= 0.8.2'
    # gem.add_dependency 'bson_ext',       '>= 1.0.4'
    gem.add_dependency 'devise'
    gem.add_dependency 'warden',         '~> 0.10.7'
    gem.add_dependency 'bcrypt-ruby',    '~> 2.1.2'
    # gem is a Gem::Specification... see http://www.rubygems.org/read/chapter/20 for additional settings
  end
  Jeweler::GemcutterTasks.new
rescue LoadError
  puts "Jeweler (or a dependency) not available. Install it with: gem install jeweler"
end
