require 'rake'
require 'fileutils'
require 'open3'

SCRIPT_PATH = File.split(File.expand_path(__FILE__))
SCRIPT_NAME = SCRIPT_PATH.last
CONFIG_DIR_PATH = SCRIPT_PATH.first
EXCLUDES = [
  SCRIPT_NAME,
  '.DS_Store',
  '.bzr',
  '.git',
  '.gitignore',
  '.gitmodules',
  '.hg',
  '.hgignore',
  '.hgmodules',
  '.hgtags',
  '.svn',
  '_MTN',
  '_darcs',
  'README.md',
  /.*~$/,
  /^\#.*\#$/,
  /backup\/.*$/,
  /vim\/(backup|swap|undo|view)\/.*$/
]

# Some general purpose methods
def header(text)
  puts "\e[1m\n#{text}\e[0m"
end

def notice(text)
  puts text
end

def error(text)
  puts "\e[31m#{text}\e[0m"
end

def warn(text)
  puts "\e[33m#{text}\e[0m"
end

def info(text)
  puts "\e[32m#{text}\e[0m"
end

# Returns whether a path is excluded from linking into the home directory.
#
# @param [String] path the path a to file or directory.
# @return [true, false] if true, the path is excluded; otherwise, it is not.
def excluded?(path)
  strings = EXCLUDES.select { |item| item.class == String }
  regexps = EXCLUDES.select { |item| item.class == Regexp }
  excluded = strings.include? path
  regexps.each do |pattern|
    excluded = true if path =~ pattern
  end
  return excluded
end


def exec_exists?(command)
  ENV['PATH'].split(':').any? do |directory|
    File.exists?(File.join(directory, command))
  end
end

# Everything related to fonts
module Fonts
  SOURCE_PATH = File.join(CONFIG_DIR_PATH, "fonts")
  DESTINATION_PATH = File.join(ENV['HOME'], 'Library', 'Fonts')

  def self.installed?(fontpath)
    font = File.split(fontpath).last
    if File.exists?(File.join(DESTINATION_PATH, font))
      return true
    else
      return false
    end
  end

  def self.install
    header("Installing fonts...")
    Dir["#{SOURCE_PATH}/*"].each do |fontpath|
      if !installed?(fontpath)
        FileUtils.cp(fontpath, DESTINATION_PATH)
        if installed?(fontpath)
          info("The font \"#{File.split(fontpath).last}\" was installed correctly.")
        else
          error("The font \"#{File.split(fontpath).last}\" was not installed correctly.")
        end

      else
        warn("The font \"#{File.split(fontpath).last}\" was already installed, skipping.")
      end
    end
  end

  def self.uninstall
    header("Uninstalling fonts...")
    Dir["#{SOURCE_PATH}/*"].each do |fontpath|
      if installed?(fontpath)
        FileUtils.rm(File.join(DESTINATION_PATH, File.split(fontpath).last))
        if !installed?(fontpath)
          info ("The font \"#{File.split(fontpath).last}\" was succesfully removed.")
        else
          error("The font \"#{File.split(fontpath).last}\" was not succesfully removed.")
        end

      else
        warn("The font \"#{File.split(fontpath).last}\" was not installed, skipping.")
      end
    end
  end
end


module Git
  SOURCE_PATH = File.join(CONFIG_DIR_PATH, "git")
  DESTINATION_PATH = ENV['HOME']

  def self.installed?(filepath)
    file = ".#{File.split(filepath).last}"
    if File.exists?(File.join(DESTINATION_PATH, file))
      return true
    else
      return false
    end
  end

  def self.install
    header("Configuring git...")
    unless exec_exists?('git')
      error("Git was not found in your path and will not be configured.")
      return
    end
    Dir["#{SOURCE_PATH}/*"].each do |filepath|
      if !installed?(filepath)
        FileUtils.ln_s(filepath, File.join(DESTINATION_PATH, ".#{File.split(filepath).last}"))
        if installed?(filepath)
          info("The file \".#{File.split(filepath).last}\" was installed correctly.")
        else
          error("The file \".#{File.split(filepath).last}\" was not installed removed.")
        end
      else
        warn("The file \".#{File.split(filepath).last}\" already exists in \"#{ENV['HOME']}\".")
      end
    end
  end

  def self.uninstall
    header("Removing git configuration...")
    Dir["#{SOURCE_PATH}/*"].each do |filepath|
      if installed?(filepath)
        FileUtils.rm(File.join(DESTINATION_PATH,".#{File.split(filepath).last}"))
        if !installed?(filepath)
          info("The file \".#{File.split(filepath).last}\" was removed correctly.")
        else
          error("The file \".#{File.split(filepath).last}\" was not succesfully removed.")
        end
      else
        warn("The file \".#{File.split(filepath).last}\" doesn't exist in \"#{ENV['HOME']}\".")
      end
    end
  end
end

module Zsh
  SOURCE_PATH_PREZTO_RUNCOMS = File.join(CONFIG_DIR_PATH, "zsh", "prezto", "runcoms")
  SOURCE_PATH_CUSTOM_RUNCOMS = File.join(CONFIG_DIR_PATH, "zsh", "runcoms")
  DESTINATION_PATH = ENV['HOME']

  def self.installed?(runcom)
    file = ".#{File.split(runcom).last}"
    if File.exists?(File.join(DESTINATION_PATH, file))
      return true
    else
      return false
    end
  end

  def self.install
    # First, we symlink all the files in the prezto/runcoms folder
    Dir["#{SOURCE_PATH_PREZTO_RUNCOMS}/*"].each do |runcom|
      next if excluded? File.split(runcom).last
      FileUtils.ln_sf(runcom, File.join(DESTINATION_PATH, ".#{File.split(runcom).last}"))
      if installed?(runcom)
        info("The file \".#{File.split(runcom).last}\" was installed correctly.")
      else
        error("The file \".#{File.split(runcom).last}\" was not installed.")
      end
    end


    # Next, we'll overwrite any files with the ones found in zsh/runcoms
    Dir["#{SOURCE_PATH_CUSTOM_RUNCOMS}/*"].each do |runcom|
      FileUtils.ln_sf(runcom, File.join(DESTINATION_PATH, ".#{File.split(runcom).last}"))
      if installed?(runcom)
        info("The file \".#{File.split(runcom).last}\" was installed correctly.")
      else
        error("The file \".#{File.split(runcom).last}\" was not installed.")
      end
    end
    # Symlink to the prezto folder
    FileUtils.ln_sf(File.join(CONFIG_DIR_PATH, "zsh", "prezto"), File.join(DESTINATION_PATH, ".zprezto"))
  end

  def self.uninstall
    Dir["#{SOURCE_PATH_PREZTO_RUNCOMS}/*"].each do |runcom|
      if installed?(runcom)
        FileUtils.rm(File.join(DESTINATION_PATH,".#{File.split(runcom).last}"))
        if !installed?(runcom)
          info("The file \".#{File.split(runcom).last}\" was removed succesfully.")
        else
          error("The file \".#{File.split(runcom).last}\" was not removed.")
        end
      end
    end

    Dir["#{SOURCE_PATH_CUSTOM_RUNCOMS}/*"].each do |runcom|
      if installed?(runcom)
        FileUtils.rm(File.join(DESTINATION_PATH,".#{File.split(runcom).last}"))
        if !installed?(runcom)
          info("The file \".#{File.split(runcom).last}\" was removed succesfully.")
        else
          error("The file \".#{File.split(runcom).last}\" was not removed.")
        end
      end
    end

    FileUtils.rm(File.join(DESTINATION_PATH, ".zprezto"))
  end
end

namespace :module do
  desc 'Initialize submodules'
  task :init do
    header("Initializing submodules...")
    if File.exists? '.gitmodules'
      unless exec_exists?('git')
        error "Could not initialize submodules, Git is not found"
        next
      end
      # Popen3 does not return the exit status code.
      # Echo it onto the last line of stderr.
      Open3.popen3(
        "git submodule update --init --recursive; echo $? 1>&2"
      ) do |stdin, stdout, stderr|
        stdios = [stdin, stdout, stderr]
        threads = []
        threads << Thread.new do
          Thread.current.abort_on_exception = true
          stdout.each do |line|
            next if line !~ /^Cloning into .*\.{3}$/
            info line.gsub(/^Cloning into (.*)\.{3}$/, "Initializing: \\1")
          end
        end
        threads << Thread.new do
          Thread.current.abort_on_exception = true
          stderr.each do |line|
            if line =~ /Unable to checkout '[^']+' in submodule path '([^']+)'/
              error "Could not initialize submodule '#{$1}'"
            end
            if stderr.eof? and line.to_i != 0
              error "Could not initialize submodules"
            end
          end
        end
        begin
          threads.each(&:join)
          stdios.each(&:close)
        rescue Exception => e
          error e.message if e.class == IOError
        end
      end
    end
  end

  desc 'Update submodules'
  task :update do
    header("Updating submodules...")
    if File.exists? '.gitmodules'
      unless exec_exists?('git')
        error "Could not update submodules, Git is not found"
        next
      end
      # Popen3 does not return the exit status code.
      # Echo it onto the last line of stderr.
      Open3.popen3(
        "git submodule foreach git pull origin master; echo $? 1>&2"
      ) do |stdin, stdout, stderr|
        stdios = [stdin, stdout, stderr]
        threads = []
        threads << Thread.new do
          stdout.each do |line|
            if line =~ /Entering '([^']+)'/
              notice "Updating: #{$1}"
            end
          end
        end
        threads << Thread.new do
          stderr.each do |line|
            if line =~ /Stopping at '([^']+)'/
              error "Could not update submodule '#{$1}'"
            end
            if stderr.eof? and line.to_i != 0
              error "Could not update submodules"
            end
          end
        end
        begin
          threads.each(&:join)
          stdios.each(&:close)
          Rake::Task[:make].invoke
        rescue Exception => e
          error e.message if e.class == IOError
        end
      end
    end
  end
end


task :install => ['module:init', 'module:update'] do
  Fonts.install
  Git.install
  Zsh.install
end

task :uninstall do
  Fonts.uninstall
  Git.uninstall
  Zsh.uninstall
end