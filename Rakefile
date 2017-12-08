require 'html-proofer'
require 'rubocop/rake_task'
require 'jekyll'

task default: %w[proof verify rubocop]

task :build do
  config = Jekyll.configuration(
    'source' => './',
    'destination' => './_site'
  )
  site = Jekyll::Site.new(config)
  Jekyll::Commands::Build.build site, config
end

task proof: 'build' do
  HTMLProofer.check_directory(
    './_site', \
    assume_extension: true, \
    check_html: true, \
    disable_external: true
  ).run
end

task proof_external: 'build' do
  HTMLProofer.check_directory(
    './_site', \
    assume_extension: true, \
    check_html: true, \
    cache: { timeframe: '1w' }, \
    hydra: { max_concurrency: 12 }
  ).run
end

namespace :docker do
  desc "build docker images"
  task :build do
    puts "Generating stats (HTML partial) of websites supporting Bitcoin Cash"
    puts `python ./scripts/python/bchAccepted.py`
    puts "Generating static files for nginx"
    puts `bundle exec jekyll build`
    puts "Building acceptbitcoincash docker image"
    puts `docker build -t acceptbitcoincash/acceptbitcoincash .`
  end
end

task :verify do
  ruby './verify.rb'
end

RuboCop::RakeTask.new
