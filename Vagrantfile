# -*- mode: ruby -*-
# vi: set ft=ruby :
VV1=''  # VV1='VAULT_VERSION='+'1.15.4+ent'  # to Install specific version

aCLUSTERA_FILES =  # // Cluster A files to copy to instances
[
	"vault_files/."  # "vault_files/vault_seal.hcl", "vault_files/vault_license.txt"  ## // for individual files
];

sVUSER='vagrant'  # // vagrant user
sHOME="/home/#{sVUSER}"  # // home path for vagrant user
sPTH='cc.os.user-input'  # // path where scripts are expected
sCA_CERT='cacert.crt'  # // Root CA certificate.

## Patch UI to hide certian detailed messages making Vagrant outputs more concise
class Vagrant::UI::Colored
	def say(type, message, opts={})
	aMSG_SKIP = [ 'Verifying vmnet devices', 'Preparing network adapters', 'Forwarding ports', 'Running provisioner: file...', 'Running provisioner: shell...' ]
	if aMSG_SKIP.any? { |s| message.include? s } ; return ; end
		aMSG_EXCLUDE = [ 'Verifying vmnet devices', 'Preparing network adapters', 'Forwarding ports', '-- 22 => 2222', 'SSH address:', 'SSH username', 'SSH auth method:', 'Vagrant insecure key detected.', 'this with a newly generated keypair', 'Inserting generated public', 'Removing insecure key from', 'Key inserted!']
		# puts type," --- ",message
		if aMSG_EXCLUDE.any? { |s| message.include? s } ## puts type, " --- ", message
			super(type, message, opts.merge(hide_detail: true))
		else
			super(type, message, opts.merge(hide_detail: false))
		end
	end
end

Vagrant.configure("2") do |config|
	#config.ssh.insert_key = false
	config.vm.post_up_message = ""
	config.vm.box = "aphorise/debian12-arm64"
	config.vm.box_check_update = false  # // disabled to reduce verbosity - better enabled
	config.vm.box_version = "0.0.3"  # // Debian tested version.

	config.vm.provider "vmware_desktop" do |v|
		v.vmx["memsize"] = "2048"
		v.vmx["numvcpus"] = "2"
		v.vmx["cpuid.coresPerSocket"] = "1"
		v.vmx["ethernet0.pcislotnumber"] = "160"
		v.allowlist_verified = true
	end

	# // VAULT Server Nodes & Consul Clients.
	config.vm.define vm_name="vault1" do |vault_node|
		vault_node.vm.hostname = vm_name
		# vault_node.vm.network "forwarded_port", guest: 80, host: "48080", id: "#{vm_name}"

		# // ORDERED: setup certs.
		vault_node.vm.provision "file", source: "#{sPTH}/1.install_tls_ca_certs.sh", destination: "#{sHOME}/install_tls_ca_certs.sh"
		$script = <<-SCRIPT
chmod +x #{sHOME}/install_tls_ca_certs.sh
/bin/bash -c 'FQDN_VAULT1=vault1 #{sHOME}/install_tls_ca_certs.sh 1'
SCRIPT
		vault_node.vm.provision "shell", inline: $script

		# // where additional Vault related files exist copy them across (eg License & seal configuration)
		for sFILE in aCLUSTERA_FILES
			if(File.file?("#{sFILE}") || File.directory?("#{sFILE}"))
				vault_node.vm.provision "file", source: "#{sFILE}", destination: "#{sHOME}"
			end
		end

		# // ORDERED: setup vault
		vault_node.vm.provision "file", source: "#{sPTH}/2.install_vault.sh", destination: "#{sHOME}/install_vault.sh"
		$script = <<-SCRIPT
chmod +x #{sHOME}/install_vault.sh
/bin/bash -c '#{VV1} USER='#{sVUSER}' #{sHOME}/install_vault.sh'

SCRIPT
		vault_node.vm.provision "shell", inline: $script
	end
end
