# smtp_user_enumeration-exercise
How to exercise with SMTP-Postfix user enumeration

Obiettivo dell’esercitazione è di effettuare l’enumerazione degli utenti. Per poter sfruttare la vulnerabilità di postfix è necessario creare una cartella con i relativi files (users.txt, script) scaricabili qui: **https://github.com/m96dg/vulnerable_docker_smtp**

Per l’esercitazione è comodo utilizzare un docker vulnerabile ad hoc presente su docker hub:

Link: **https://hub.docker.com/r/m96dg/pw_smtp_postfix**

**$docker pull m96dg/pw_smtp_postfix**

![image](https://user-images.githubusercontent.com/65173648/150690619-4a9ee53b-b513-4fd9-a678-b31f4d7f9885.png)

Per poter vedere le immagini dei docker pullate:

**$docker images**

Runnare il docker sul porto 25:

**$docker run -d -p 25:25 m96dg/pw_smtp_postfix**

Per poter verificare che sia effettivamente in esecuzione:

**$docker ps**

![image](https://user-images.githubusercontent.com/65173648/150690702-1564221b-6ba1-4320-b5ba-5405620f26ce.png)

Fase di **scanning** (cerchiamo di raccogliere quante più informazioni possibili dall'host):

**$nmap -sS -p- -T4 -Pn -oA portScan localhost**

![image](https://user-images.githubusercontent.com/65173648/150690988-a2ed1f73-e03f-4356-b4b4-23a85f2bd1d8.png)

Fase di **enumeration** (cerchiamo di approndire le informazioni andando ad enumerare le porte trovare in fase di scansione):

**$nmap -sV -sC -oA versionScan localhost**

![image](https://user-images.githubusercontent.com/65173648/150691014-91a7d485-c16b-41a0-8bf3-6261a68cca09.png)

Per poter enumerare gli utenti ci sono due metodi:

1) Lanciare uno script in perl con una lista di utenti ad hoc:

**$perl smtp-user-enum.pl -M VRFY -U users.txt -t localhost**

![image](https://user-images.githubusercontent.com/65173648/150691037-059ede34-9145-402f-9cbc-bb551b392666.png)

2) Connettersi al server e provare ad enumerare gli utenti in modo manuale col comando VRFY:

**$nc localhost 25**

![image](https://user-images.githubusercontent.com/65173648/150691057-0d962fd4-d221-4c0f-bab2-dae713399aa9.png)

