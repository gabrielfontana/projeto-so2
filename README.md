# Projeto de Sistemas Operacionais II

<details open="open">
  <summary><h2>Tabela de Conteúdos</h2></summary>
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


![collectd Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/6/67/Logo_der_Software_collectd.svg/220px-Logo_der_Software_collectd.svg.png)

Lançado em 8 de julho de 2005, collectd é um _daemon_ Unix (processo executado em segundo plano) que coleta métricas de desempenho do sistema periodicamente e fornece mecanismos para armazenar ou transferir essas informações, que podem ser utilizadas para criar visualizações valiosas com o objetivo de identificar os problemas de um sistema específico. A aquisição e armazenamento de dados são tratados por _plugins_ na forma de objetos compartilhados.

Vantagens importantes em trabalhar com o collectd:
 - Projeto de código aberto: por ser gratuito e estar sempre em desenvolvimento ativo, possui uma presença sólida no mercado de coleta de métricas do sistema.
 - Alto nível de desempenho e portabilidade: é escrito em linguagem C visando consumir o mínimo de recursos possíveis.
 - Extensível: por padrão, o daemon vem com mais de 100 plugins que variam de usos comuns aos especializados e avançados, sendo possível ainda  a criação de novos plugins.

## InfluxDB

![InfluxDB Logo](https://www.mundodocker.com.br/wp-content/uploads/2017/05/Influxdb.png)

InfluxDB é um banco de dados de série temporal de código aberto (TSDB) desenvolvido pela InfluxData. Ele é escrito em Go e otimizado para armazenamento rápido e de alta disponibilidade e recuperação de dados de séries temporais em campos como monitoramento de operações, métricas de aplicativos, dados de sensores da Internet das Coisas e análises em tempo real.

>  Um banco de dados de série temporal (TSDB) é aquele otimizado para  armazenar dados com registro de data e hora ou série temporal.

>  Os dados de série temporal são uma coleção de observações obtidas por meio de medições repetidas ao longo do tempo.

O principal uso do InfluxDB é para aplicações de monitoramento em tempo real, já que estas demandam uma quantidade enorme de leitura e escrita no banco. Dessa forma, o uso de uma base de dados que comporte esse volume é essencial para não prejudicar o sistema.

## Grafana
![InfluxDB Logo](https://cdn.icon-icons.com/icons2/2699/PNG/512/grafana_logo_icon_171049.png)

Desenvolvido pela Grafana Labs e lançado em 19 de janeiro de 2014, Grafana é um _software_ de código aberto multiplataforma voltado para visualização e análise. Ele permite que você consulte, visualize, alerte e explore suas métricas, independentemente de onde estejam armazenadas. Em outras palavras, ele fornece ferramentas para transformar seus dados em belos gráficos e visualizações.

As empresas usam o Grafana para monitorar sua infraestrutura e análise de log, principalmente para melhorar sua eficiência operacional. Os painéis facilitam o rastreamento de usuários e eventos, pois automatizam a coleta, o gerenciamento e a visualização de dados. Com o tempo, esse _software_ ganhou muita popularidade na indústria e hoje é utilizado por grandes empresas como Intel, PayPal, eBay, Tinder, Wix e muitas outras.

# 2. Monitoramento de um sistema Arch Linux

Versões utilizadas neste tutorial:

 - Distribuição: Arch Linux
 - Kernel: Linux 5.12.3-arch-1
 - collectd: 5.9.2-1
 - InfluxDB: 1.8.3-1
 - Grafana: 7.4.3-1

Com uma máquina Arch já preparada, instale uma interface gráfica de sua preferência antes de iniciar.

- Interface Gráfica (Xfce):
	```sh
	sudo pacman -Syu
	sudo pacman -S xorg xfce4 xfce4-goodies lightdm lightdm-gtk-greeter
	sudo systemctl enable lightdm
	reboot
	```

- Interface Gráfica (KDE):
	```sh
	sudo pacman -Syu
	sudo pacman -S xorg plasma-meta kde-applications
	sudo systemctl enable sddm
	sudo systemctl enable NetworkManager
	reboot
	```

Baixe os pacotes manualmente no site [Arch archive](https://archive.archlinux.org/):

 - [collectd](https://archive.archlinux.org/packages/c/collectd/collectd-5.9.2-1-x86_64.pkg.tar.xz)
 - [InfluxDB](https://archive.archlinux.org/packages/i/influxdb/influxdb-1.8.3-1-x86_64.pkg.tar.zst)
 - [Grafana](https://archive.archlinux.org/packages/g/grafana/grafana-7.4.3-1-x86_64.pkg.tar.zst)

## collectd

Instalando o collectd:
```sh
sudo pacman -U /root/Downloads/collectd-5.9.2-1-x86_64.pkg.tar.xz
``` 
Configurado o collectd:

- Acesse o arquivo (`/etc/collectd.conf`)
	- Ative o (`FQDNLookup`):
		```sh
		FQDNLookup true
		```
	-  Ative os seguintes plugins:	
		```sh			
		LoadPlugin syslog
		LoadPlugin cpu
		LoadPlugin df
		LoadPlugin disk
		LoadPlugin entropy
		LoadPlugin interface
		LoadPlugin irq
		LoadPlugin load
		LoadPlugin memory
		LoadPlugin network
		LoadPlugin processes
		LoadPlugin rrdtool
		LoadPlugin swap
		LoadPlugin uptime 
		LoadPlugin users
		```
		> **Observações:** 
		 > - Para ativá-los, basta apagar o símbolo "#" localizado antes de LoadPlugin). 
		 > - Talvez seja necessário configurar o plugin disk manualmente. Para isso, vá até a seção  (`<Plugin disk>`) e especifique o disco a ser utilizado na opção (`Disk`).
		 > - Talvez seja necessário configurar o plugin interface manualmente. Para isso, vá até a seção (`<Plugin interface>`) e especifique a interface de rede a ser utilizada na opção (`Interface`).
		 > -  Ao desabilitar plugins, certifique-se de também desabilitar os blocos de código associados (se houver).
		 
	-  Ative o bloco (`<Plugin syslog>`):	
		```sh
		<Plugin syslog>
			LogLevel info
		</Plugin>
		```		

	- Ao final do arquivo de configuração, digite o seguinte bloco:
		```sh
		<Plugin "network">
			Server "127.0.0.1" "25826"
		</Plugin>
		```

Iniciando o collectd:
```sh
sudo systemctl start collectd
sudo systemctl enable collectd
``` 
		
## InfluxDB

Instalando o InfluxDB:
```sh
sudo pacman -U /root/Downloads/influxdb-1.8.3-1-x86_64.pkg.tar.zst
``` 
Baixando o arquivo types.db:
```sh
sudo  mkdir /usr/local/share/collectd
sudo  wget -P /usr/local/share/collectd https://raw.githubusercontent.com/collectd/collectd/master/src/types.db
```
Iniciando o InfluxDB:
```sh
sudo systemctl start influxdb
sudo systemctl enable influxdb
``` 
Criando o banco de dados:
```sh
influx
CREATE DATABASE collectd
SHOW DATABASES
```
Configurando o InfluxDB:
- No arquivo (`/etc/influxdb/influxdb.conf`), encontre a seção (`[[collectd]]`) e a configure desse modo:
	```sh
	[[collectd]]
		enabled = true
		bind-address = ":25826"
		database = "collectd"
		retention-policy = ""
		typesdb = "/usr/local/share/collectd"
		batch-size = 5000
		batch-pending = 10
		batch-timeout = "10s"
		read-buffer = 0
	```
Reiniciando o serviço para ele possa reler o arquivo de configuração:
```sh
sudo systemctl restart influxdb
```

Conferindo se as métricas do collectd já estão sendo enviadas ao InfluxDB:
```sh
influx
USE collectd
SHOW MEASUREMENTS
```
```sh
# Resultado

> SHOW MEASUREMENTS
name: measurements
name
----
cpu_value
df_value
disk_io_time
disk_read
disk_value
disk_weighted_io_time
disk_write
entropy_value
interface_rx
interface_tx
irq_value
load_longterm
load_midterm
load_shortterm
memory_value
processes_value
swap_value
uptime_value
users_value
>
```

## Grafana

Instalando o Grafana:
```sh
sudo pacman -U /root/Downloads/grafana-7.4.3-1-x86_64.pkg.tar.zst
```

Iniciando o collectd:
```sh
sudo systemctl start grafana
``` 

Clique [aqui](#4-criar-sua-primeira-dashboard-utilizando-o-grafana) para aprender a acessar o Grafana, configurar o InfluxDB como base de dados e criar sua primeira dashboard.
	
# 3. Monitoramento de um sistema Ubuntu
	
## collectd

## InfluxDB

## Grafana

# 4. Criar sua primeira dashboard utilizando o Grafana

