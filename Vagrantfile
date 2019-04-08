Vagrant.configure("2") do |config|

  config.vm.define "chocodemo" do |chocodemo|
    chocodemo.vm.box = "StefanScherer/windows_10"
    chocodemo.windows.halt_timeout = 20
    chocodemo.winrm.username = "vagrant"
    chocodemo.winrm.password = "vagrant"
    chocodemo.vm.guest = :windows
    chocodemo.vm.communicator = "winrm"

    chocodemo.vm.hostname = "chocodemo"
    chocodemo.windows.set_work_network = true

    chocodemo.vm.network :forwarded_port, guest: 5985, host: 14985, id: "winrm", auto_correct: true
    chocodemo.vm.network :forwarded_port, guest: 3389, host: 14389, id: "rdp", auto_correct: true
    chocodemo.vm.network :forwarded_port, guest: 22, host: 14222, id: "ssh", auto_correct: true

    chocodemo.vm.provider :virtualbox do |v, override|
      override.vm.network :private_network, ip: "10.10.117.13"
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.gui = true
      v.customize ["modifyvm", :id, "--vram", 32]
      v.customize ["modifyvm", :id, "--memory", "4096"]
      v.customize ["modifyvm", :id, "--audio", "none"]
      v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
      v.customize ["modifyvm", :id, "--draganddrop", "hosttoguest"]
      v.customize ["modifyvm", :id, "--usb", "off"]
      # linked clones for speed and size
      v.linked_clone = true if Vagrant::VERSION >= '1.8.0'
    end

    chocodemo.vm.provision :shell, :path => "shell/PrepareWindows.ps1", privileged: false
    chocodemo.vm.provision :shell, :path => "shell/SetWindowsPreferences.ps1", privileged: false
    #chocodemo.vm.provision :shell, :path => "shell/InstallChocolatey.ps1", privileged: false
    chocodemo.vm.provision :shell, :path => "shell/NotifyGuiAppsOfEnvironmentChanges.ps1", privileged: false
  end
end