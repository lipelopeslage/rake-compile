rake-compile
============

Raketask to compile assets pipeline without a Rails project.<br>
From ruby noob to ruby noobs.

##Setting up
**Variables:**
* ROOT - the main directory. Ex: Pathname(File.dirname(__FILE__))
* BUNDLES - the name of the target assets (does not work for compile:duplicate. <br>     Ex: %w( bootstrap.css bootstrap.js )
* BUILD_DIR - build path. Ex: ROOT.join("build")
* SOURCE_DIR - source path. Ex: ROOT.join("src")

============

##Usage

###rake compile:default
Or just **rake**. Default compilation, join all assets to a single file, but does not minify them

============
###rake compile:minify
The same as compile:default, but using compression

============
###rake compile:duplicate
As the name says, just duplicates files from source to build path.

============

##Usage
Make sure to set the BUNDLES variable with real (existing) files, otherwise it might break
