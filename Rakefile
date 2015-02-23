require 'bundler/gem_tasks'

task :default => :test

# ==========================================================
# Packaging
# ==========================================================

GEMSPEC = eval(File.read('semian.gemspec'))

require 'rubygems/package_task'
Gem::PackageTask.new(GEMSPEC) do |pkg|
end

# ==========================================================
# Ruby Extension
# ==========================================================

$:.unshift File.expand_path("../lib", __FILE__)
require 'semian/platform'
if Semian.sysv_semaphores_supported?
  require 'rake/extensiontask'
  Rake::ExtensionTask.new('semian', GEMSPEC) do |ext|
    ext.ext_dir = 'ext/semian'
    ext.lib_dir = 'lib/semian'
  end
  task :build => :compile
else
  task :build do; end
end

# ==========================================================
# Testing
# ==========================================================

require 'rake/testtask'
Rake::TestTask.new 'test' do |t|
  t.libs = ['lib', 'test']
  t.pattern = "test/*_test.rb"
end
task :test => :build

# ==========================================================
# Documentation
# ==========================================================
require 'rdoc/task'
RDoc::Task.new do |rdoc|
  rdoc.rdoc_files.include("lib/*.rb", "ext/semian/*.c")
end
