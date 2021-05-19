# projeto-so2
Projeto desenvolvido em Linux para a disciplina de Sistemas Operacionais II

# Monitoramento de um sistema Ubuntu utilizando collectd, InfluxDB e Grafana

Versões utilizadas neste tutorial
	Distribuição: Ubuntu 20.04.2 LTS
	Kernel: Linux 5.8.0-53-generic
	
	collectd: 5.9.2.g-1ubuntu5
	InfluxDB: 1.8.5-1
	Grafana: 7.4.3

collectd:
	- Instalação do collectd:
		sudo apt-get install collectd collectd-utils

	- Baixar o arquivo types.db:
		sudo mkdir /usr/local/share/collectd
		sudo wget -P /usr/local/share/collectd https://raw.githubusercontent.com/collectd/collectd/master/src/types.db
		
	- Configurar o collectd:
		sudo nano /etc/collectd/collectd.conf
		
		- Ativar os seguintes plugins (para ativá-los, basta apagar o símbolo "#" na frente de LoadPlugin):		  
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

			* Talvez seja necessário configurar o plugin disk manualmente. Para isso, vá até a seção <Plugin disk> e especifique o disco a ser utilizado na opção Disk. 

			* Talvez seja necessário configurar o plugin interface manualmente. Para isso, vá até a seção <Plugin interface> e especifique a interface de rede a ser utilizada na opção Interface. 
			
		
		- Ao final do arquivo de configuração, digite o seguinte trecho:
			<Plugin "network">
				Server "127.0.0.1" "25826"
			</Plugin>
	
	- Iniciar a unit do collectd:
		sudo service collectd start
		
InfluxDB:
	- Instalação do InfluxDB:
		wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -
		source /etc/lsb-release
		echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
		sudo apt-get update && sudo apt-get install influxdb
		
	- Iniciar a unit do InfluxDB:
		sudo service influxdb start
	
	- Criar o banco de dados:
		influx
		CREATE DATABASE collectd
		SHOW DATABASES
			
	- Configuração do InfluxDB:
		sudo nano /etc/influxdb/influxdb.conf
		
		- Alterar a seção do collectd (lembre-se de "descomentar" as linhas apagando o símbolo "#"):
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
	
	- Reiniciar a unit do InfluxDB:
		sudo service influxdb restart
		
	- Conferir se o collectd já está enviando as métricas para o InfluxDB:
		influx
		USE collectd
		SHOW MEASUREMENTS
		
Grafana:
	- Instalação do Grafana:
		sudo apt-get install -y adduser libfontconfig1
		wget https://dl.grafana.com/oss/release/grafana_7.4.3_amd64.deb
		sudo dpkg -i grafana_7.4.3_amd64.deb

	- Iniciar a unit do Grafana:
		sudo service grafana-server start
	
	- Acessar o Grafana, configurar a base de dados para InfluxDB e criar o primeiro gráfico:
		1. Acesse um navegador web e digite o seguinte endereço na barra de pesquisa: localhost:3000
		2. Informe [username: admin] [password: admin] para realizar o login
		3. Após isso, já logado no Grafana, procure pelo símbolo de uma engrenagem (Configuration) no lado esquerdo da tela
		4. Em seguida, escolha a opção "Data Sources"
		5. Clique no botão "Add Data Sources" e escolha o InfluxDB
		6. Em seguida, abaixo de "HTTP", no campo "URL", digite http://localhost:8086
		7. Ao final, no campo "Database", digite o nome do banco de dados criado no Influxdb: collectd
		8. Para finalizar e salvar, clique em "Save & Test"
		9. Para criar a dashboard, procure pelo símbolo "+" (Create) no lado esquerdo da tela e escolha a opção "Dashboard"
		10. Clique em "Add new panel"
		11. Na seção de "Query", escolha a métrica do sistema que deseja monitorar informando-a em "select_measurement"
		12. É possível personalizar o gráfico e a maneira como as informações são visualizadas alterando opções no lado direito em "Panel" e "Field"
		13. Clique em "Apply" para salvar esse painel 
		14. Clique no símbolo de disquete (Save dashboard) para salvar essa dashboard
