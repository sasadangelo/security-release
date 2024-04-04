require 'yaml'

# Load settings from servers.yml file.
configuration = YAML.load_file('configuration.yaml')

Vagrant.configure("2") do |config|
    configuration["servers"].each do |opts|
        config.vm.define opts["name"] do |config|
            config.vm.box = opts["box"]
            config.vm.box_version = opts["box_version"]
            config.vm.hostname = opts["name"]
            config.vm.network :private_network, ip: opts["eth1"]

            config.vm.provider "virtualbox" do |v|
                v.name = opts["name"]
            	v.customize ["modifyvm", :id, "--groups", "/Security Release"]
                v.customize ["modifyvm", :id, "--memory", opts["mem"]]
                v.customize ["modifyvm", :id, "--cpus", opts["cpu"]]

            end

            # Run some steps only if the machine is in creation state
            if ARGV[0] == "up" || ARGV[0] == "reload" || ARGV[0] == "provision"
                config.vm.synced_folder ".", "/vagrant", disabled: false
            end
        end
    end
end
