# frozen_string_literal: true

require "bundler/gem_tasks"

# By default, run tests and linter.
task default: [:test, :rubocop]


require "rake/testtask"

Rake::TestTask.new do |t|
  t.libs = %w[lib test]
  t.pattern = "test/**/*_test.rb"
  t.verbose = !ENV["VERBOSE"].nil?
  t.warning = !ENV["WARNING"].nil?
end

require "rubocop/rake_task"

RuboCop::RakeTask.new


require "yard"
require "yard/rake/yardoc_task"

YARD::Rake::YardocTask.new(:yardoc) do |y|
  y.options = ["--output-dir", "yardoc"]
end

CLOBBER.include "yardoc"


desc "Run all benchmarks in the benchmarks directory"
task :benchmarks do
  Dir["benchmarks/bm_*.rb"].each do |file|
    sh "ruby #{file}"
  end
end


desc "Downloads the Public Suffix List file from the repository and stores it locally."
task :"update-list" do
  require "net/http"

  definition_url = "https://raw.githubusercontent.com/publicsuffix/list/master/public_suffix_list.dat"

  File.open("data/list.txt", "w+") do |f|
    response = Net::HTTP.get_response(URI.parse(definition_url))
    response.body
    f.write(response.body)
  end
end
