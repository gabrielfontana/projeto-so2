# Projeto de SO
Projeto desenvolvido em Linux para a disciplina de Sistemas Operacionais II

# Monitoramento de um sistema Arch Linux utilizando collect, InfluxDB e Grafana

Versões utilizadas neste tutorial:

 - Distribuição: Arch Linux
 - Kernel: Linux 5.12.3-arch-1
 - collectd: 5.9.2-1
 - InfluxDB: 1.8.3-1
 - Grafana: 7.4.3-1

Antes de iniciar, instale uma interface gráfica de sua preferência.

Interface Gráfica (Xfce):
```sh
sudo pacman -Syu
sudo pacman -S xorg xfce4 xfce4-goodies lightdm lightdm-gtk-greeter
sudo systemctl enable lightdm
reboot
```

Interface Gráfica (KDE):
```sh
sudo pacman -Syu
sudo pacman -S xorg plasma-meta kde-applications
sudo systemctl enable sddm
sudo systemctl enable NetworkManager
reboot
```

Baixe os pacotes manualmente no site Arch archive:

 - [collectd](https://archive.archlinux.org/packages/c/collectd/collectd-5.9.2-1-x86_64.pkg.tar.xz)
 - [InfluxDB](https://archive.archlinux.org/packages/i/influxdb/influxdb-1.8.3-1-x86_64.pkg.tar.zst)
 - [Grafana](https://archive.archlinux.org/packages/g/grafana/grafana-7.4.3-1-x86_64.pkg.tar.zst)

## collectd

Instalação:
```sh
pacman -U /root/Downloads/collectd-5.9.2-1-x86_64.pkg.tar.xz
```

Baixar o arquivo types.db:
```sh
mkdir /usr/local/share/collectd
wget -P /usr/local/share/collectd https://raw.githubusercontent.com/collectd/collectd/master/src/types.db
```

Configuração:
```sh
nano /etc/collectd.conf
```

Após acessar /etc/collect.conf, ative os seguintes plugins (para ativá-los, basta apagar o símbolo "#" na frente de LoadPlugin):
| Plugins   | Documentação |
| ------    | ------------ |
| syslog    |              | 
| cpu       |              | 
| df        |              | 
| disk      |              | 
| entropy   |              | 
| interface |              | 
| irq       |              | 
| load      |              | 
| memory    |              | 
| network   |              | 
| processes |              | 
| rrdtool   |              | 
| swap      |              | 
| uptime    |              | 
| users     |              | 


	
