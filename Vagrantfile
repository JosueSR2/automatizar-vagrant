# Configuración de Vagrant
Vagrant.configure("2") do |config|
  # Configuración para las 3 máquinas
  config.vm.box = "ubuntu/bionic64" # Base Ubuntu 18.04

  # Servidor Apache 1
  config.vm.define "apache1" do |apache1|
    apache1.vm.hostname = "apache1"
    apache1.vm.network "private_network", ip: "192.168.56.101"
    apache1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    apache1.vm.synced_folder "./www/apache1", "/var/www/html"
    apache1.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      systemctl enable apache2
      echo "<h1>Servidor Apache 1</h1>" > /var/www/html/index.html
      systemctl restart apache2
    SHELL
  end

  # Servidor Apache 2
  config.vm.define "apache2" do |apache2|
    apache2.vm.hostname = "apache2"
    apache2.vm.network "private_network", ip: "192.168.56.102"
    apache2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    apache2.vm.synced_folder "./www/apache2", "/var/www/html"
    apache2.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      systemctl enable apache2
      echo "<h1>Servidor Apache 2</h1>" > /var/www/html/index.html
      systemctl restart apache2
    SHELL
  end

  # Servidor Nginx (balanceador de carga)
  config.vm.define "nginx" do |nginx|
    nginx.vm.hostname = "nginx"
    nginx.vm.network "private_network", ip: "192.168.56.103"
    nginx.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    nginx.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx
      systemctl enable nginx
      cat <<EOF > /etc/nginx/conf.d/load_balancer.conf
upstream backend {
    server 192.168.56.101;
    server 192.168.56.102;
}

server {
    listen 80;

    location / {
        proxy_pass http://backend;
    }
}
EOF
      systemctl restart nginx
    SHELL
  end
end
