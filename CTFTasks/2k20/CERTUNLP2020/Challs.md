# CSIRT
## 0xB16B00B5 part 1
![Description](https://github.com/srbleu/Writeups-en-MarkDown/blob/master/CTFTasks/2k20/CERTUNLP2020/0xB16B00B51.png)

Bien jugando con la información que tenemos la búsqueda fue 0xB16B00B5 malware chile
Encontramos un [artículo](https://blog.segu-info.com.ar/2020/11/nuevo-ransomware-egregor-cencosud-jumbo.html) muy útil que nos cuenta que afecto a Cencosud

Flag **Cencosud**

## 0xB16B00B5 part 2
![Description](https://github.com/srbleu/Writeups-en-MarkDown/blob/master/CTFTasks/2k20/CERTUNLP2020/0xB16B00B52.png)

Si seguimos leyendo el articulo anterior veremos una lista de IoC
```
+ C&C
    185.82.126.8
+ IPs
    185.238.0[.]233
    45.153.242[.]129
    217.8.117[.]147
    91.199. 212[.]52 (FALSO POSITIVO, corresponde a un certificado de crt.sectigo.com)
+ Dominios principales
    www.egregor[eliminado].*
    egregor[eliminado]*.onion
    *egregor.top
    sekhmet*.*
```
Así que la respuesta sería **185.82.126.8**
## 0xB16B00B5 part 3
![Description](https://github.com/srbleu/Writeups-en-MarkDown/blob/master/CTFTasks/2k20/CERTUNLP2020/0xB16B00B53.png)
Buscamos el Abuse Contact para ese bloque de IPS y encontramos que el contacto es **abuse@nano.lv** haciendo una query whois 
## Phishing
![Description](https://github.com/srbleu/Writeups-en-MarkDown/blob/master/CTFTasks/2k20/CERTUNLP2020/Phising.png)
Bien lo primero fue checkear los registros DNS para este dominio
```
dig TXT bankination.ctf.cert.unlp.edu.ar

; <<>> DiG 9.16.1-Ubuntu <<>> TXT bankination.ctf.cert.unlp.edu.ar
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32166
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;bankination.ctf.cert.unlp.edu.ar. IN	TXT

;; ANSWER SECTION:
bankination.ctf.cert.unlp.edu.ar. 10799	IN TXT	"v=spf1 ip4:172.217.192.27 flag{this-could_be"

;; Query time: 952 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
;; WHEN: sáb dic 05 00:04:33 CET 2020
;; MSG SIZE  rcvd: 118
```
BIen tenemos la primera mitad de la flag y una referencia a spf , spf esta muy relacionado con MX y con DMARC . si hacemos un [check de DMARC online](https://mxtoolbox.com/SuperTool.aspx?action=dmarc%3a+bankination.ctf.cert.unlp.edu.ar&run=toolpage)
Vemos el resto de la flag:
```
v=DMARC1; p=quarantine;_0n3_part_0f_eL_PRoBLeMA}
```
Flag **flag{this-could_be_0n3_part_0f_eL_PRoBLeMA}**
