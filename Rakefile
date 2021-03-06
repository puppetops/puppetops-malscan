require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'
require 'metadata-json-lint/rake_task'
require 'puppet-syntax/tasks/puppet-syntax'

require 'rubocop/rake_task'

PuppetLint.configuration.fail_on_warnings = true
PuppetLint.configuration.relative = true
PuppetLint.configuration.ignore_paths = ['spec/**/*.pp', 'pkg/**/*.pp']

task :validate do
  Dir['manifests/**/*.pp'].each do |manifest|
    sh "puppet parser validate --noop #{manifest}"
  end
  Dir['spec/**/*.rb', 'lib/**/*.rb'].each do |ruby_file|
    sh "ruby -c #{ruby_file}" unless ruby_file =~ %r{spec/fixtures}
  end
  Dir['templates/**/*.epp'].each do |template|
    sh "puppet epp validate #{template}"
  end
end

desc 'Run metadata_lint, lint, validate, doc tests, and spec tests'
task :test do
  [:metadata_lint, :syntax, :lint, :validate, :spec, :rubocop].each do |test|
    Rake::Task[test].invoke
  end
end
