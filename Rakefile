begin
  require 'bundler/setup'
rescue LoadError
  puts 'You must `gem install bundler` and `bundle install` to run rake tasks'
end

Bundler::GemHelper.install_tasks

require 'yard'
YARD::Rake::YardocTask.new do |t|
  t.files = ['lib/**/*.rb', 'README.md']
end

require "rspec/core/rake_task"
RSpec::Core::RakeTask.new(:spec) do |task|
  task.pattern = 'spec/*_spec.rb'
end

task :default => :spec

namespace :spec do
  desc 'Run all spec variants'
  task :all do
    rake = 'bundle exec rake'
    fail unless system("#{rake} spec:generators")
    [['', ''], ['db_prefix_', ''], ['', '_db_suffix'], ['abc_', '_123']].each do |prefix, suffix|
      env = "DB_PREFIX=#{prefix} DB_SUFFIX=#{suffix}"
      fail unless system("#{rake} spec #{env}")
    end
    require 'active_record/version'
    if ActiveRecord::VERSION::MAJOR == 3
      fail unless system("#{rake} spec ATTR_ACCESSIBLE=1")
    end
  end

  desc 'Run generator specs'
  RSpec::Core::RakeTask.new(:generators) do |task|
    task.pattern = 'spec/generators/*_spec.rb'
  end
end
