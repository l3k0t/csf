1. Instalação do CSF Firewall
A instalação é bem simples. Execute os comandos abaixo no terminal do seu sistema Linux com o usuário root:

cd /root;
rm -fv csf.tgz; (use este comando apenas se já tver feito o download do arquivo csf.tgz)
wget https://github.com/l3k0t/csf.git
tar -xzf csf.tgz
cd csf
sh install.sh

Não execute qualquer outro script de configuração do iptables, como o APF. Caso o tenha instalado, rode o comando abaixo para remover do seu sistema:

sh /usr/local/csf/bin/remove_apf_bfd.sh

2- Verificação de módulos do sistema
Logo após a instalação, execute o próximo comando para verificar se possui todos os módulos requeridos pelo iptables:

perl /usr/local/csf/bin/csftest.pl

Não se preocupe se não puder rodar todos os módulos, desde que o script não reporte um erro fatal. Se o resultado for como o demonstrado abaixo, o CSF funcionará normalmente:

Testing ip_tables/iptable_filter…OK
Testing ipt_LOG…OK
Testing ipt_multiport/xt_multiport…OK
Testing ipt_REJECT…OK
Testing ipt_state/xt_state…OK
Testing ipt_limit/xt_limit…OK
Testing ipt_recent…OK
Testing xt_connlimit…OK
Testing ipt_owner/xt_owner…OK
Testing iptable_nat/ipt_REDIRECT…OK
Testing iptable_nat/ipt_DNAT…OK

RESULT: csf should function on this server

2- Configurando
Os arquivos de configuração do CSF ficam na pasta /etc/csf:
csf.allow:

Coloque neste arquivo os IPs/CIDRs (https://pt.wikipedia.org/wiki/CIDR#Nota.C3.A7.C3.A3o_standard) que serão permitidos passar pelo iptables. Abaixo estão algumas sugestões de IPs, caso utilize os serviços demonstrados (os ips podem variar com o tempo, então sempre visita o site do fabricante para confirmar):

#Cloudflare Ips
199.27.128.0/21
173.245.48.0/20
103.21.244.0/22
103.22.200.0/22
103.31.4.0/22
141.101.64.0/18
108.162.192.0/18
190.93.240.0/20
188.114.96.0/20
197.234.240.0/22
198.41.128.0/17
162.158.0.0/15
104.16.0.0/12
172.64.0.0/13

#IP PagSeguro
186.234.16.8
186.234.16.9
186.234.48.8
186.234.48.9
186.234.144.17
186.234.144.18
200.147.112.136
200.147.112.137

# IPs do GerenciaNet
177.66.7.0/24

#IPs do Uptime Robot
74.86.158.106
74.86.158.107
74.86.158.109
74.86.158.110
74.86.158.108
46.137.190.13
122.248.234.23
188.226.183.141
178.62.52.237
54.79.28.129
54.94.142.218
104.131.107.63
54.67.10.127
54.64.67.106

69.162.124.226 # engine5.uptimerobot.com
69.162.124.227 # engine6.uptimerobot.com
69.162.124.228 # engine7.uptimerobot.com
69.162.124.229 # engine8.uptimerobot.com
69.162.124.230 # engine9.uptimerobot.com
69.162.124.231 # engine10.uptimerobot.com
69.162.124.232 # engine11.uptimerobot.com
69.162.124.233 # engine12.uptimerobot.com
69.162.124.234 # for future use
69.162.124.235 # for future use
69.162.124.236 # for future use
69.162.124.237 # for future use
69.162.124.238 # for future use

#New Relic Agents
50.31.164.0/24
104.16.0.0/12
162.247.240.0/22
198.41.128.0/17

#New Relic Availability monitoring
50.31.164.139 #Chicago Illinois
50.112.95.211 #US West 2 Oregon
54.247.188.179 #EU West 1 Ireland
54.248.250.232 #AP Northeast 1 Japan
54.251.34.67 #AP Southeast 1 Singapore
184.73.237.85 #US East 1 Virginia

50.16.189.130 #US East 1 Virgina
50.18.57.7 #US West 1 California
54.214.255.205 #US West 2 Oregon
54.228.244.177 #EU West 1 Ireland
54.232.123.139 #SA East 1 Brazil
54.241.22.142 #US West 1 California
54.248.225.67 #AP Northeast 1 Tokyo
54.251.109.246 #AP Southeast 1 Singapore
54.252.114.169 #AP Southeast 2 Sydney
54.252.114.170 #AP Southeast 2 Sydney
177.71.245.207 #SA East 1 Brazil

#New Relic WebHooks
50.31.164.0/24
162.247.240.0/22

#Pingdom IPs
95.141.32.46
95.211.217.68
83.170.113.210
188.138.118.144
174.34.224.167
72.46.140.106
76.72.172.208
184.75.210.226
78.40.124.16
67.205.67.76
188.138.118.184
188.138.124.110
85.17.156.99
85.17.156.11
85.17.156.76
72.46.153.26
208.64.28.194
76.164.194.74
184.75.210.90
184.75.208.210
184.75.209.18
46.165.195.139
199.87.228.66
76.72.167.90
94.247.174.83
69.64.56.47
184.75.210.186
108.62.115.226
46.20.45.18
50.23.94.74
64.141.100.136
69.59.28.19
178.255.154.2
178.255.153.2
178.255.155.2
64.237.55.3
178.255.152.2
212.84.74.156
173.204.85.217
173.248.147.18
72.46.130.42
208.43.68.59
67.228.213.178
96.31.66.245
82.103.128.63
174.34.156.130
70.32.40.2
174.34.162.242
85.25.176.167
204.152.200.42
95.211.87.85
5.178.78.77
207.244.80.239
159.8.146.132
50.22.90.227
69.64.56.153
188.138.40.20
64.120.6.122
158.58.173.160
76.72.171.180
72.46.140.186
78.31.69.179
95.211.198.87

#IP para update do CSF
85.10.199.177 # Manually allowed: 85.10.199.177 (DE/Germany/-)

csf.blocklists:

Este arquivo possui definições para listas de bloqueio de IP classificados como fonte de Spam. Cuidado ao selecionar uma lista, pois pode causar problemas de performance/rede devido a quantidade de IPs. Não esqueça de definir o parâmetro MAX (o valor 0 equivale a usar todos os IPs), pois é a quantidade máxima de endereços IP a serem usados de uma lista.

Ordem dos parâmetros:
NAME|INTERVAL|MAX|URL

Lista:
HONEYPOT|86400|0|http://www.projecthoneypot.org/list_of_ips.php?t=d&rss=1

csf.ignore

O CSF oferece a capacidade de excluir um ou mais endereços IP das regras do firewall. Endereços IP no arquivo csf.ignore não serão verificados pelas regras do firewall e só podem ser bloqueados se estiverem no arquivo csf.deny.
csf.deny

Neste arquivo ficam os IPs/CIDRs (https://pt.wikipedia.org/wiki/CIDR#Nota.C3.A7.C3.A3o_standard) que serão bloqueados de forma temporária ou permanente, sendo que um nome de domínio não é aceito e será ignorado. O padrão é usar um IP por linha mas também pode colocar uma subrede. Ex.: 192.168.254.0/24.

OBS.: Se você adicionar o texto “do not delete” aos comentários de uma entrada, o parâmetro DENY_IP_LIMIT a ignorará e ela não será removida deste arquivo de bloqueio.

Ex.:
123.456.123.456
123.456.123.0/24

-> Filtros avançados

Este tipo de filtro pode ser colocado nos arquivos csf.allow ou csf.deny.

-> Sintaxe para as regras avançadas:

tcp/udp/icmp|in/out|s/d=port|s/d=ip|u=uid

TCP/UDP: Selecione uma opção de protocolo: TCP, UDP ou ICMP;
in/out: conexões de entrada ou saída;
s/d=port: número da porta de origem (s) ou destino (d). Se desejar definir um intervalo de portas, use um _ (ex.: 2000_3000). Pode ser também um código de ICMP (http://www.dcc.fc.up.pt/~pbrandao/aulas/0506/arq_r/exercicios/CodICMP.html);
s/d=ip: endereço IP de origem ou destino;
u/g=UID: UID ou GID do pacote de origem (apenas nas conexões de saída). O valor de s/d=ip é ignorado
Nota: a filtragem de ICMP usa a “porta” do parâmetro s/d=porta para definir o tipo ICMP. Se você usa s ou d não é relevante pois é como se fosse usar o comando iptables –icmp-type. Use “iptables -p icmp -h” para ver uma lista dos tipos válidos de ICMP. Apenas um tipo ICMP é suportado por filtro.

Exemplos:

Permitir conexões através da porta 443 para uma faixa de endereços IP:

tcp|out|d=443|d=12.34.56.78/27 # csf.allow

Permitir acesso via SSH de um IP:

tcp|in|d=22|s=11.22.33.44 # csf.allow

Permitir conexões TCP de entrada para a porta 3306 vindas do IP 11.22.33.44:

tcp|in|d=3306|s=11.22.33.44

Permitir conexões TCP de saída para a porta 22 no IP 11.22.33.44

tcp|out|d=22|d=11.22.33.44

Permitir conexões TCP de saída para a porta 80 com o UID 99

tcp|out|d=80||u=99

Permitir conexões ICMP de entrada para o tipo ping vindas do IP 111.222.333.444

icmp|in|d=ping|s=111.222.333.444

csf.dirwatch

Neste arquivo você pode ter uma lista de diretórios e arquivos que você quer ser alertado quando forem alterados. Você deve especificar caminhos completos para cada entrada.
Ex.:

/etc/passwd
csf.logfiles
Arquivo aonde estão os caminhos dos arquivos de log que o CSF e o LFD irão monitorar/verificar.
Ex.:

# All:
/var/log/messages
/var/log/lfd.log
csf.conf

Este é o arquivo principal de configuração do CSF e do LFD. Como ele é muito extenso, vou comentar apenas sobre alguns parâmetros aqui:

TESTING = “0”
Este parâmetro cria uma tarefa no CRON que limpa todas as regras do iptables a cada 5 minutos (pode ser alterado no arquivo /etc/crontab para outro valor). Muito útil quando está testando novas configurações e não quer correr o risco de ficar sem acesso via SSH ao seu sistema. Altere para 0 (zero) apenas quando tiver certeza, e depois reinicie o CSF. Parar a execução do CSF também removerá a linha do /etc/crontab.

OBS.: O LFD não irá rodar enquanto estiver usando este parâmetro

AUTO_UPDATES = “1”
Habilitar este parâmetro cria uma tarefa no CRON chamado /etc/cron.d/csf_update que rodará uma vez por dia para verificar se tem uma atualização do CSF_LFD. Em caso positivo, fará upgrade e reiniciará o CSF e o LFD automaticamente

#Você também pode verificar anúncios de novas versões em http://blog.configserver.com

# Relação de portas TCP liberadas para recebimento de dados
TCP_IN = “20,21,25,53,80,110,143,443,465,587,993,995,3306,7171,7172”

# Relação de portas liberadas para envio de dados
TCP_OUT = “20,21,25,28,37,43,53,80,110,113,443,465,587,993,995,3306,7171,7172”

# Relação de portas UDP liberadas para recebimento de dados
UDP_IN = “20,21,53”

# Relação de portas UDP liberadas para envio de dados (para permitir o uso do traceroute, adicione 33434:33523 a relação abaixo)
UDP_OUT = “20,21,53,113,123,873,6277”

# Permite que o sistema receba PING
ICMP_IN = “1”

# Limita a taxa de entrada de dados ICMP por IP (para desabilitar coloque 0)
ICMP_IN_RATE = “1/s”

# Permite que o sistema envie PING
ICMP_OUT = “1”

# Limita a taxa de saída de dados ICMP (para desabilitar coloque 0)
ICMP_OUT_RATE = “1/s”
# Cuidado ao usar esta opção, pois pode causar problemas de performance/rede.
# Limite de IPs/CIDRs a serem bloqueados. Se o limite for atingido, as entradas serão removidas começando pelas mais antigas. O valor 0 remove o limite.
DENY_IP_LIMIT = “150”

OBS.: Se quiser usar uma relação realmente grande, é recomendado usar a opção IPSET.

# Limite de números IPs/CIDRs mantidos na lista temporária de bloqueio. Se o limite for atingido, as entradas serão removidas começando pelas mais antigas, independentemente do tempo que faltar para terminar o bloqueio. O valor 0 remove o limite.
DENY_TEMP_IP_LIMIT = “150”

# Ativa o LFD (login failure detection daemon). O valor 0 desabilita o LFD.
LF_DAEMON = “1”

# Verifica se o CSF está parado e o reinicia se for necessário. A verificação é feita a cada 300 segundos.
LF_CSF = “1”

# Bloqueia pacotes fora de ordem, indesejados e com o status de INVALID no iptables
PACKET_FILTER = “1”

# Faz DNS lookup reverso em endereços IP. Dependendo da quantidade de IPs pode causar problemas de performance.
LF_LOOKUPS = “0”

SYNFLOOD = “0”
SYNFLOOD_RATE = “50/s”
SYNFLOOD_BURST = “100”

# Ativa a proteção contra SYN Flood. Esta opção configura o iptables para oferecer alguma proteção contra tentativas de DOS usando pacotes TCP SYN. Você deve definir a taxa de modo que a manter no mínimo os falso-positivos, senão os seus visitantes poderão ter problemas de conexão (verifique no arquivo /var/log/messages por *SYNFLOOD Blocked*). Veja a manpage do iptables para verificar a correta sintaxe da taxa do parâmetro –limit.

# Nota: Esta opção só deve ser ativada se você estiver sob ataque SYN Flood, pois irá causar lentidão em todas as novas conexões de qualquer endereço IP até o servidor. O valor zero desativa a proteção.

SYNFLOOD = “1”

#Este parâmetro é para monitorar quando houver mais de 50 conexões por segundo.
SYNFLOOD_RATE = “50/s”

#Este parâmetro define um valor para bloquear um IP que o atingir
SYNFLOOD_BURST = “100”
# Esta opção configura o iptables para limitar o número de conexões ativas e simultâneas por endereço IP que podem ser feitas a portas específicas
CONNLIMIT = “0”
Ex.:

Sintaxe:
porta1|quantidade_conexao1|porta2|quantidade_conexao2

22;5;443;20
Permitirá 5 conexões simultâneas na porta 22 e 20 conexões simultâneas na porta 443
# Esta opção limita o número de novas conexões por intervalo de tempo que podem ser feitas a portas específicas
PORTFLOOD = “3306;tcp;10;100”

Sintaxe:
porta;protocolo;quantidade_conexao;tempo

Ex.:
22;tcp;5;250

Bloqueia o endereço IP se mais de 5 conexões são estabelecidas na porta 22 usando o protocolo TCP dentro de 250 segundos. O bloqueio é removido apenas depois de 250 segundos terem passado após o último pacote ter sido enviado pelo cliente para essa porta. Você pode adicionar mais portas separando-as por vírgulas, como descrito abaixo:

porta1;protocolo1;quantidade_conexao1;tempo1,porta2;protocolo2;quantidade_conexao2;tempo2

# Proteção contra Flood UDP. Esta opção protege a saída de dados usando UDP. Deve ser tomado muito cuidado em servidores que possuem alto tráfego de pacotes UDP, como servidores SNMP. (recomendado usar a opção User ID Tracking (UID_INTERVAL) com este recurso)

UDPFLOOD = “0”
UDPFLOOD_LIMIT = “100/s”
UDPFLOOD_BURST = “200”

# Esta é uma lista com nomes de usuário que não serão limitados pela proteção UDPFLOOD, como o serviço “named”
# Nota: O usuário root (UID:0) é sempre permitido
UDPFLOOD_ALLOWUSER = “named”
# O seguinte recurso habilita o bloqueio permanente de endereços IP que foram temporariamente bloqueados mais do que o valor definido em LF_PERMBLOCK_COUNT nos últimos LF_PERMBLOCK_INTERVAL segundos. Defina LF_PERMBLOCK para “1” para habilitar e “0” para desabilitar este recurso.
LF_PERMBLOCK = “1”
LF_PERMBLOCK_INTERVAL = “86400”
LF_PERMBLOCK_COUNT = “4”
LF_PERMBLOCK_ALERT = “1”

# Para bloquear conexões baseadas no país de origem, verifique o código corresponde no site https://countrycode.org/ e adicione (separando por vírgula) na lista abaixo:
CC_DENY = “VN,CN”

# Para permitir conexões baseadas no país de origem, verifique o código corresponde no site https://countrycode.org/ e adicione (separando por vírgula) na lista abaixo. Tome cuidado com esta opção, pois ela permite o acesso através de todas as portas no firewall. Por esta razão é recomendado que utilize a opção CC__ALLOW_FILTER.
CC_ALLOW = “BR”

# Uma alternativa ao CC_ALLOW é de apenas permitir o acesso pelos seguintes países mas ainda filtrando baseado nas regras do firewall para portas e pacotes
CC_ALLOW_FILTER = “BR”
# Os emails são enviados para o root por padrão. Altere a linha abaixo com o e-mail desejado para receber os alertas:
LF_ALERT_TO = “user@example.org”

# Enviar um alerta por e-mail quando um usuário fizer login via SSH:
LF_SSH_EMAIL_ALERT = “1”

Comandos no terminal para o CSF:

Bloquear um IP no servidor:

csf -d 192.168.1.2
csf -d 192.168.0/24

Remover um IP da lista de bloqueio:

csf -dr 192.168.1.2

Permitir um IP:

csf -a 172.16.20.1

Note que ao permitir um IP, ele ainda poderá ser bloqueado pelo LFD se iniciar um ataque de força bruta. Use o recurso IGNORE_ALLOW para não bloquear um IP que esteja na lista de IPs permitidos.

Procurar por um IP:

csf -g 192.168.1.2

Ver as portas que estão ouvindo por conexões externas e os executáveis rodando atrás deles:

csf -p

Desabilita o CSF e o LFD:

csf -x

Habilitar o CSF e o LFD:

csf -e

Enviar por e-mail uma verificação de segurança do servidor ao usuário passado como parâmetro :

csf -m user@example.com

Aplicando as alterações

Após realizar qualquer mudança na configuração do CSF, é necessário reiniciar para que ele comece a utilizar as alterações feitas. Use o comando abaixo para reiniciar o CSF:

csf -r

 

Bom, é isso pessoal. 


