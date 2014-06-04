require 'rubygems'
require 'bundler'
require 'pathname'
require 'logger'
require 'fileutils'
require 'uglifier'

Bundler.require

ROOT        = Pathname(File.dirname(__FILE__))
LOGGER      = Logger.new(STDOUT)
BUNDLES     = %w( all.css all.js )
BUILD_DIR   = ROOT.join("build")
SOURCE_DIR  = ROOT.join("src")

namespace :compile do
  sprockets = Sprockets::Environment.new(ROOT) do |env|
    env.logger = LOGGER
  end


  sprockets.context_class.class_eval do
    def asset_path(path, options = {})
      "/src/#{path}"
    end
  end


  # SUBTASKS
  desc "Compress all .scss .js - SUBTASK"
  task :compress do
    sprockets.js_compressor  = :uglify # require 'uglifier'
    sprockets.css_compressor  = :scss
  end

  desc "Write assets together - SUBTASK"
  task :write do
    sprockets.append_path(SOURCE_DIR.join('javascripts').to_s)
    sprockets.append_path(SOURCE_DIR.join('stylesheets').to_s)
    BUNDLES.each do |bundle|
      assets = sprockets.find_asset(bundle)
      prefix, basename = assets.pathname.to_s.split('/')[-2..-1]
      FileUtils.mkpath BUILD_DIR.join(prefix)

      assets.write_to(BUILD_DIR.join(prefix, basename))    
    end
  end

  desc "Copy assets separate - SUBTASK"
  task :copy do
    sprockets.append_path(SOURCE_DIR.join('javascripts').to_s)
    sprockets.append_path(SOURCE_DIR.join('stylesheets').to_s)
    BUNDLES.each do |bundle|
      assets = sprockets.find_asset(bundle)
      prefix, basename = assets.pathname.to_s.split('/')[-2..-1]
      FileUtils.mkpath BUILD_DIR.join(prefix)

      assets.write_to(BUILD_DIR.join(prefix, basename))    

      # This code bellow makes the copy
      assets.to_a.each do |asset|
        realname = asset.pathname.basename.to_s.split(".")[0..1].join(".") # strip filename.css.foo.bar.css multiple extensions
        asset.write_to(BUILD_DIR.join(prefix, realname))
      end
    end
  end

  
  # TASKS
  desc "Compile assets without minification"
  task :default do    
    Rake::Task["compile:write"].reenable
    Rake::Task["compile:write"].invoke    
  end

  desc "Compile assets without minification"
  task :duplicate do    
    Rake::Task["compile:copy"].reenable
    Rake::Task["compile:copy"].invoke    
  end

  desc "Compile with minification"
  task :minify do
    Rake::Task["compile:compress"].reenable
    Rake::Task["compile:compress"].invoke
    Rake::Task["compile:default"].reenable
    Rake::Task["compile:default"].invoke
  end

end

task :default => "compile:default"
