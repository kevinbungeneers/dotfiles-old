require 'rake'
require 'fileutils'


SCRIPT_PATH = File.split(File.expand_path(__FILE__))
SCRIPT_NAME = SCRIPT_PATH.last
CONFIG_DIR_PATH = SCRIPT_PATH.first


# Some general purpose methods
def header(text)
	puts "\e[1m#{text}\e[0m"
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
		Dir["#{SOURCE_PATH}/*"].each do |fontpath|
			if installed?(fontpath)
				FileUtils.rm(File.join(DESTINATION_PATH, File.split(fontpath).last))
				if !installed?(fontpath)
					info ("The font \"#{File.split(fontpath).last}\" was succesfully removed.")
				else
					erro("The font \"#{File.split(fontpath).last}\" was not succesfully removed.")
				end
				
			else
				warn("The font \"#{File.split(fontpath).last}\" was not installed, skipping.")
			end
		end
	end
end

