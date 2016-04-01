Vagrant.configure(2) do |config|
	config.vm.box = "bento/centos-6.7"
	config.vm.network :forwarded_port, guest: 80, host: 8080
	config.vm.network :private_network, ip: "192.168.33.10"
	config.vm.hostname = "local.dev01"
	# インベントリファイルに勝手に実行権限が付いてエラーになるのでオプションで明示
	config.vm.synced_folder ".", "/vagrant", :mount_options => ["dmode=777","fmode=666"]
	
	# haltしないでdestroyした時にエラーになる(provisionが走る)ので限定
	# ansible_localでインストールされるansibleバージョンが上がれば多分発生しなくなる？
	# vagrant(1.8.1)の方の問題かも
	if ARGV[0] == "up" || ARGV[0] == "provision" then
		config.vm.provision :ansible_local do |ansible|
			ansible.install = true
			ansible.verbose = true
			ansible.inventory_path = "./provisioning/hosts/test"
			ansible.playbook = "./provisioning/test.yml"
			ansible.limit = "local"
		end
	end
end
