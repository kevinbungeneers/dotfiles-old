require 'rake'
require 'fileutils'


SCRIPT_PATH = File.split(File.expand_path(__FILE__))
SCRIPT_NAME = SCRIPT_PATH.last
CONFIG_DIR_PATH = SCRIPT_PATH.first


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



task :install do
	Fonts.install
	Git.install
end

task :uninstall do
	Fonts.uninstall
	Git.uninstall
end