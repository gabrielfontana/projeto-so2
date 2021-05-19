# Projeto de Sistemas Operacionais II

<details open="open">
  <summary>Sumário</summary>
  <ol>
  <li>
      <a href="#1-introdução">Introdução</a>
      <ul>
        <li><a href="#collectd">collectd</a></li>
        <li><a href="#influxdb">InfluxDB</a></li>
        <li><a href="#grafana">Grafana</a></li>
      </ul>
    </li>
    <li>
      <a href="#2-monitoramento-de-um-sistema-arch-linux">Monitoramento de um sistema Arch Linux</a>
    </li>
    <li>
      <a href="#3-monitoramento-de-um-sistema-ubuntu">Monitoramento de um sistema Ubuntu</a>
    </li>
    <li><a href="#4-criar-sua-primeira-dashboard-utilizando-o-grafana">Criar sua primeira dashboard utilizando o Grafana</a></li>
  </ol>
</details>

# 1. Introdução

## collectd

Lançado em 8 de julho de 2005, collectd é um daemon Unix (processo executado em segundo plano) que coleta métricas de desempenho do sistema periodicamente e fornece mecanismos para armazenar ou transferir essas informações, que podem ser utilizadas para criar visualizações valiosas com o objetivo de identificar os problemas de um sistema específico. A aquisição e armazenamento de dados são tratados por plugins na forma de objetos compartilhados.

## InfluxDB

InfluxDB é um banco de dados de série temporal de código aberto (TSDB) desenvolvido pela InfluxData. Ele é escrito em Go e otimizado para armazenamento rápido e de alta disponibilidade e recuperação de dados de séries temporais em campos como monitoramento de operações, métricas de aplicativos, dados de sensores da Internet das Coisas e análises em tempo real.

## Grafana

Desenvolvido pela Grafana Labs e lançado em 19 de janeiro de 2014, Grafana é um software de código aberto multiplataforma voltado para visualização e análise. Ele permite que você consulte, visualize, alerte e explore suas métricas, independentemente de onde estejam armazenadas. Em outras palavras, ele fornece ferramentas para transformar seus dados em belos gráficos e visualizações.

# 2. Monitoramento de um sistema Arch Linux

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
sudo pacman -U /root/Downloads/collectd-5.9.2-1-x86_64.pkg.tar.xz
```

Baixe o arquivo types.db:
```sh
sudo  mkdir /usr/local/share/collectd
sudo  wget -P /usr/local/share/collectd https://raw.githubusercontent.com/collectd/collectd/master/src/types.db
```
Arquivo de configuração:
```sh
sudo nano /etc/collectd.conf
```
 Após acessar /etc/collect.conf, ative os seguintes plugins (para ativá-los, basta apagar o símbolo "#" na frente de LoadPlugin):
|    Plugin            | Documentação |
| -------------------- | ------------ |
| LoadPlugin syslog    |              | 
| LoadPlugin cpu       |              | 
| LoadPlugin df        |              | 
| LoadPlugin disk      |              | 
| LoadPlugin entropy   |              | 
| LoadPlugin interface |              | 
| LoadPlugin irq       |              | 
| LoadPlugin load      |              | 
| LoadPlugin memory    |              | 
| LoadPlugin network   |              | 
| LoadPlugin processes |              | 
| LoadPlugin rrdtool   |              | 
| LoadPlugin swap      |              | 
| LoadPlugin uptime    |              | 
| LoadPlugin users     |              | 

## InfluxDB

## Grafana

# 3. Monitoramento de um sistema Ubuntu
	
## collectd

## InfluxDB

## Grafana

# 4. Criar sua primeira dashboard utilizando o Grafana

