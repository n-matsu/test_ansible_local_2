Vagrant.configure(2) do |config|

	# 共通のイメージから２台構成
	config.vm.box = "bento/centos-6.7"
	# インベントリファイルに勝手に実行権限が付いてエラーになるのでオプションで明示
	config.vm.synced_folder ".", "/vagrant", :mount_options => ["dmode=777","fmode=666"]
	
	# 1: web-server
	config.vm.define "local.web" do |server|
		server.vm.network :forwarded_port, guest: 80, host: 8080
		server.vm.network :private_network, ip: "192.168.33.10"
		server.vm.hostname = "local.web"
		
		# haltしないでdestroyした時にエラーになる(provisionが走る)ので限定
		# ansible_localでインストールされるansible(1.9.4)バージョンが上がれば多分発生しなくなる？
		# vagrant(1.8.1)の方の問題かも
		if ARGV[0] == "up" || ARGV[0] == "provision" then
			server.vm.provision :ansible_local do |ansible|
				ansible.install = true
				ansible.verbose = true
				ansible.inventory_path = "./provisioning/hosts/local"
				ansible.playbook = "./provisioning/webservers.yml"
				ansible.limit = "webservers"
			end
		end
	end
	
	# 2: db-server
	config.vm.define "local.db" do |server|
		server.vm.network :private_network, ip: "192.168.33.20"
		server.vm.hostname = "local.db"
		
		# haltしないでdestroyした時にエラーになる(provisionが走る)ので限定
		# ansible_localでインストールされるansible(1.9.4)バージョンが上がれば多分発生しなくなる？
		# vagrant(1.8.1)の方の問題かも
		if ARGV[0] == "up" || ARGV[0] == "provision" then
			server.vm.provision :ansible_local do |ansible|
				ansible.install = true
				ansible.verbose = true
				ansible.inventory_path = "./provisioning/hosts/local"
				ansible.playbook = "./provisioning/dbservers.yml"
				ansible.limit = "dbservers"
			end
		end
	end
	
end
