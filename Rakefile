require 'sunspot'
require 'topic'

desc 'Start a development server'
task :server do
	if which('shotgun')
		exec 'shotgun -O docs.rb'
	else
		warn 'warn: shotgun not installed; reloading is disabled.'
		exec 'ruby -rubygems docs.rb -p 9393'
	end
end

desc 'Index documentation'
task :index do
  puts "indexing now:"
  Sunspot.config.solr.url = ENV["WEBSOLR_URL"]
  docs = FileList['docs/*.txt']
  docs.each { |d| puts "...indexing #{d}"; Sunspot.index(topic_for(d).text_only) }
  Sunspot.commit
  puts "finished indexing"
end

task :start => :server

def which(command)
	ENV['PATH'].
		split(':').
		map  { |p| "#{p}/#{command}" }.
		find { |p| File.executable?(p) }
end

def topic_for(doc)
  Topic.load(name_for(doc), File.read(doc))
end

def name_for(doc)
  File.basename(doc, '.txt')
end

