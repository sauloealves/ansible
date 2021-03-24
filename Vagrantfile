# para geral o par de chave publica/private usar o comando ssh-keygen -t  rsa


Vagrant.configure("2") do |config|

	config.vm.box = "ubuntu/trusty64"

    config.vm.provider "virtualbox" do |v|		
        v.memory = 1024
    end

    config.vm.define "wordpress" do |m|
		
		  m.vm.provider "virtualbox" do |x| 
			  x.name = "wordpress"
		  end

		  m.vm.network "public_network", ip: "192.168.1.112"
      m.vm.provision "shell", inline: "cat /vagrant/ansible_chave_privada.pub  >> .ssh/authorized_keys"
  end

	#cria uma maquina virtual somente para rodar o ansible
  config.vm.define "ansible" do |ansible|
	
	  ansible.vm.box = "ubuntu/bionic64"
    
	  ansible.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 1
      vb.name = "trusty_ansible"
    end
	
    ansible.vm.network "public_network", ip: "192.168.1.113"
    	
    ansible.vm.provision "shell",
      inline: "apt-get update && \
        apt-get install -y software-properties-common && \
        apt-add-repository --yes --update ppa:ansible/ansible && \
        apt-get install -y ansible"        

    ansible.vm.provision "shell", inline: "cp /vagrant/ansible_chave_privada ."

    # Depois é necessário mudar a permissão para rw do arquivo
    # E finalmente troca o usuário de root para o vagrant para poder executar
    ansible.vm.provision "shell", inline: "chmod 600 ansible_chave_privada && \ 
                                          chown vagrant:vagrant ansible_chave_privada" #trocando o usuário de root para vagrant

  end


  config.vm.define "mysql" do |m|
		
    m.vm.provider "virtualbox" do |x| 
      x.name = "mysql"
    end

    m.vm.network "public_network", ip: "192.168.1.114"
    m.vm.provision "shell", inline: "cat /vagrant/ansible_chave_privada.pub  >> .ssh/authorized_keys"

    
  end

end