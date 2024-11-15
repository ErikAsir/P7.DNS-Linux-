Voy a utilizar la practica anterior como base ya que se parecen mucho y asi puedo proceder con el paso 5 directamente.

Crear Zona propia:
- Para esto me dirijo al archivo de zona db.asircastelao.int para modificar los registros DNS relacionados con este dominio. El archivo quedaria asi:

$TTL        	604800
@           	IN  	SOA   	ns.asircastelao.int. root.asircastelao.int. (
                              	2      	; Serial
                              	604800 	; Refresh
                              	86400  	; Retry
                              	2419200	; Expire
                              	604800 )   ; Negative Cache TTL

;Registros NS
@           	IN  	NS            	ns.asircastelao.int.
@           	IN  	AAAA          	::1
@           	IN  	A             	172.28.5.1
ns          	IN  	A             	172.28.5.1
test        	IN  	A             	172.28.5.4
www         	IN  	A             	172.28.5.7
alias       	IN  	CNAME 	test
texto       	IN  	TXT   	mensaje

Instalacion de dig:
- Accedemos al contenedor
$ docker exec -it nombre_contenedor bash

- Actualizamos e instalamos
$ apt update
$ apt install -y dnsuit

- Consulta al servidor DNS 1:
$ dig @172.28.5.1 ns.asircastelao.int

- Resultado 1:

 root@8ea354a2656b:/# dig @172.28.5.1 ns.asircastelao.int

; <<>> DiG 9.18.28-0ubuntu0.24.04.1-Ubuntu <<>> @172.28.5.1 ns.asircastelao.int
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 5639
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: c23e1d089c3c879401000000673663fa0972b43cb4d1df69 (good)
;; QUESTION SECTION:
;ns.asircastelao.int.    	IN	A

;; ANSWER SECTION:
ns.asircastelao.int.	604800	IN	A	172.28.5.1

;; Query time: 0 msec
;; SERVER: 172.28.5.1#53(172.28.5.1) (UDP)
;; WHEN: Thu Nov 14 20:56:26 UTC 2024
;; MSG SIZE  rcvd: 92

- Resultado 2:

root@8ea354a2656b:/# dig @172.28.5.1 www.google.com

; <<>> DiG 9.18.28-0ubuntu0.24.04.1-Ubuntu <<>> @172.28.5.1 www.google.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 399
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: e9e3a801ebf1a948010000006736642afe64d0b1ced6f29c (good)
;; QUESTION SECTION:
;www.google.com.        	IN	A

;; Query time: 38 msec
;; SERVER: 172.28.5.1#53(172.28.5.1) (UDP)
;; WHEN: Thu Nov 14 20:57:14 UTC 2024
;; MSG SIZE  rcvd: 71

SERVER: 172.28.5.1#53 --> Indica que dig encontro un servidor DNS y esta usando ese servidor para resolver el dominio consultado



