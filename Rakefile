require 'rubygems'
require 'rake'

# rake gems:reinstall

namespace :gems do
  task :reinstall do

    ENV['prompt'] ||= 'yes'


    # Make a list of originally installed gems just in case...
    gem_list_file = File.join(File.dirname(__FILE__), 'gems.txt')    
    system("gem list > #{gem_list_file}") unless File.exists? gem_list_file
    
    puts 'Warning: Gems are installed for all users, with sudo'

    # Get list of installed gems which have 'extensions'
    Gem.source_index.search(nil).map.reject { |s| s.extensions.empty? }.each do |spec|
      next if spec.name.include? 'mysql'

      puts "**** #{spec.name} #{spec.version} ****"

      # Prompt y/n
      unless ENV['prompt'] == 'no'
        puts 'Continue? y/n/q'
        result = STDIN.gets.chomp.downcase                 
        next if result == 'n'
        exit if result == 'q'
      end

      options = "#{spec.name} --version='#{spec.version}'"
      puts '**** UNINSTALL ****'
      system("sudo gem uninstall #{options} -I -x -q")
      puts '**** INSTALL ****'
      system("sudo gem install #{options}") 
    end
  end
end