Vagrant.configure("2") do |config|
  config.vm.define "azure_devops_agent" do |azure_devops_agent|
    azure_devops_agent.vm.box = "bento/ubuntu-16.04"
    azure_devops_agent.vm.provision "ansible_local" do |ansible|
      ansible.become = true
      ansible.playbook = "playbook.yml"
      ansible.galaxy_role_file = "requirements.yml"
      ansible.galaxy_roles_path = "/etc/ansible/roles"
      ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
    end
  end
end