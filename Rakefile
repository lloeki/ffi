require 'rubygems'
require 'rake/gempackagetask'
require 'rubygems/specification'
require "spec/rake/spectask"
require 'date'

GEM = "ffi"
GEM_VERSION = "0.5.0"
AUTHOR = "Wayne Meissner"
EMAIL = "wmeissner@gmail.com"
HOMEPAGE = "http://kenai.com/projects/ruby-ffi"
SUMMARY = "A Ruby binding to libffi (compatible with Rubinius and JRuby's FFI)"

spec = Gem::Specification.new do |s|
  s.name = GEM
  s.version = GEM_VERSION
  s.platform = Gem::Platform::RUBY
  s.has_rdoc = true
  s.extra_rdoc_files = ["README", "LICENSE"]
  s.summary = SUMMARY
  s.description = s.summary
  s.author = AUTHOR
  s.email = EMAIL
  s.homepage = HOMEPAGE
  
  s.extensions = %w(ext/extconf.rb)
  
  s.require_path = 'lib'
  s.autorequire = GEM
  s.files = %w(LICENSE README Rakefile) + Dir.glob("{ext,lib,nbproject,samples,specs}/**/*")
end

desc "Run all specs"
Spec::Rake::SpecTask.new("specs") do |t|
  t.spec_opts = ["--format", "specdoc", "--colour"]
  t.spec_files = Dir["spec/**/*_spec.rb"].sort
end

if RUBY_PLATFORM == "java"
  desc "Run specs"
  task :specs do
    sh %{#{Gem.ruby} -S spec #{Dir["specs/*_spec.rb"].join(" ")} -fs --color}
  end
else
  desc "Run specs"
  task :specs do
    ENV["MRI_FFI"] = "1"
    sh %{#{Gem.ruby} -S spec #{Dir["specs/*_spec.rb"].join(" ")} -fs --color}
  end
end

Rake::GemPackageTask.new(spec) do |pkg|
  pkg.gem_spec = spec
end

desc "install the gem locally"
task :install => [:package] do
  sh %{sudo #{Gem.ruby} -S gem install pkg/#{GEM}-#{GEM_VERSION}}
end

desc "create a gemspec file"
task :make_spec do
  File.open("#{GEM}.gemspec", "w") do |file|
    file.puts spec.to_ruby
  end
end