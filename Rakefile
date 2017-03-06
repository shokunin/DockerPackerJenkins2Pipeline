require 'yaml'
require 'popen4'
STDOUT.sync = true

task :default => :build_full_container
##########################################################################
def run_command(cmd)
  cmdrun = IO.popen(cmd)
  output = cmdrun.read
  cmdrun.close
  if $?.to_i > 0
    puts "count not run #{cmd}, it returned an error #{output}"
    exit 2
  end
  puts "OK: ran command #{cmd}"
end
##########################################################################
hf = Dir.glob('ansible/playbook.yml')
errors = []
desc 'Check Playbook Syntax'
task :lint_playbook do
  hf.each do |playbook_file|
    begin
      YAML.load_file(playbook_file)
    rescue Exception => e
      errors << e.message
    end
  end
  if errors.empty?
    puts "Rake: #{hf.length} playbook files all checkout!"
  else
    errors.each do |err|
      puts "ERROR: YAML parse errors"
      puts err
    end
    exit 1
  end
end
##########################################################################
desc 'Update the Galaxy Modules'
task :update_galaxy do
  puts 'Rake: updating galaxy modules'
  run_command('cd ansible && rm -rf roles/* && ansible-galaxy install --roles-path roles -r requirements.yml')
end

task :validate_packer do
  puts 'Rake: validating packer.json'
  run_command('packer validate packer.json')
end

task :build_container do
  puts 'Rake: Building container'
  POpen4::popen4( "packer build packer.json" ) do |stdout, stderr, stdin|  
    stdout.each do |line|  
      puts line  
    end
  end  
end

desc 'Ensure environment is setup properly'
task :check_env do
  ['BASE_CONTAINER', 'DOCKER_REGISTRY', 'CONTAINER_NAME'].each do |st|
    unless ENV.has_key? st
      puts "ENV var #{st} is not set - see the Readme"
      exit 1
    end
  end
end

desc 'Build the container'
task :build_full_container => [:check_env, :validate_packer, :update_galaxy, :lint_playbook, :build_container] do
  puts 'Rake: Building the container '
end

