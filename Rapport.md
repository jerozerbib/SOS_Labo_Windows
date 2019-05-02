# Password theft & persistence

## Authors . Filipe Fortunato, Jeremy Zerbib


### P1. Expliquer l'utilité de l'argument -Pn 

L'utilité de l'argument *-Pn* est de pouvoir sauter la découverte d'hôtes en supposant que d'autres sont disponibles.

### P2. Quel est le contrôleur de domaine ? Comment pouvez vous le déterminer (2 façon distinctes)?

Une manière de déterminer est de se baser sur le fait qu'il s'agit de la seule machine avec un port *DNS* ouvert et un serveur *LDAP* et *Kerberos-sec*.  
Une autre méthode utilise un scan de *SMB version* et nous pouvons donc trouver le nom *WAD-DC-SRV2* comme nom de hôte (DC = domain controller).
Son adresse est donc : 172.22.4.2

172.22.4.2, seule machine avec le port DNS ouvert et serveur LDAP et Kerberos-sec.
Deuxième méthode . scan SMB version, WAD-DC-SRV2 comme nom de hôtes (DC = domain controller) 

### P3. Quels sont les droits d’exécution que vous obtenez ?

Les droits trouvées sont NT Authority/System, ce qui nous des privilèges complets sur la machine.  

### P4. Comment expliquer que vous disposez d’autant de privilège ?

Exploitation du process *SMB* qui a tous les privileges systemes.

### P5. Quel processus exécute votre meterpreter sur la machine victime (identifiant + nom) ?

Le processus exécuté est Powershell avec l'identifiant 23566

### P6. Quelle est la différence entre la version reverse_tcp et bind_tcp de meterpreter ?

*bind_tcp* sert à ouvrir un port TCP sur la machine victime.  
reverse_tcp demande à la machine victime de se connecter sur un port TCP local a l'attaquant.

### P7. Dans quelle situation est-il recommandé d’utiliser la version reverse_tcp ?

Dans le cas où il y a un firewall entre la machine attaquée et nous, ce dernierpourrait filtrer les comms entrantes sur un port. 

### P8. Dans la sortie de l’exécution, la notion de stage apparait, de quoi s’agit-il ?

Second payload de code à exec sur la machine distante

### P9. Décrivez la structure des entrées récupérées depuis la base SAM ?

<user> : <user-id> : <password-LM-hash> : <password-NTLM-hash> :::

### P10. Comment expliquer que plusieurs comptes partagent les mêmes hashs ?

Les mots de passe *LM* sont désactivés/non utilisés.

### P11. Est-ce que plusieurs machines utilisent les mêmes mots de passe locaux (utilisez smb_login) ?

Oui, car le smb_login arrive à se connecter à toutes les machines.

### P12. Quel est le format de hash utilisé pour stocker les hash MS-CACHE ? A quoi correspondent les différentes parties ?

```bash
jdoe:$DCC2$10240#jdoe#d635a431fe20bf670139e7724f55de88::
<username> : $<DCC2 magic>$ <iterations> #<username># <DCC1 hash> :: 
```

### P13. À quoi correspond le compte qui se termine par un $ retrouvé dans la mémoire de LSASS ?

Il s'agit du compte du domaine. 

### P14. Quel type de compte est nécessaire afin d’accéder au GPO sur le partage SYSVOL ?

Il faut avoir un compte ayant les permissions "*ListObject*".

### P15. Quel est l’identifiant de la GPO qui contient le mot de passe ?

\\WAD-DC-SRV2.WAD.LOCAL\SYSVOL\wad.local\Policies\{8B5F6D08-8F6B-49FF-8401-B4BDB860BD54}\USER\Preferences\ScheduledTasks\ScheduledTasks.xml

### P16. Quelle est la valeur chiffrée en CPassword qui correspond au mot de passe trouvé dans la GPP ?

La valeur chiffrée en CPassword qui correspond au mot de passe trouvl est : `F7mL0Gt49wv64Y8HxukelyarUAwwd2BfPagryCKMRP8`

### P17. Est-ce que ce compte est utilisé sur l’une des machines (smb_login) ?

Oui, il est utilisé par toutes les machines locales.


### P18. Expliquer pourquoi une seule entrée est retournée par le module ?

### P19. Quel est le SPN complet vulnérable ?

### P20. Quel est le compte du domaine associé à ce SPN ?

### P21. Est-ce que ce compte est utilisé sur l’une des machines (utilisez smb_login) ?

### P22. Analyser les logs au moment de l’attaque et décrivez comment psexec fonctionne ?

### P23. Quels sont les privilèges requis pour l’utilisation de psexec ?

### P24. Quelle vulnérabilité exploitez-vous pour rebondir sur le second serveur ?

### P25. Comment avez-vous pu récupérer un compte du domaine sur le second serveur ?

### P26. Quelles sont les actions qui justifient l’utilisation d’un compte « Domain Admins » ?

### P27. Comment éviter qu’un de ces comptes puisse être volé ?

### P28. Qu’est-ce qui se passe quand vous essayez de monter le partage la première fois ?

### P29. Qu’est-ce qui se passe quand vous essayez de monter le partage la seconde fois ? Comment expliquer cette différence ?

### P30. Localiser l’événement généré au moment de l’authentification avec le Golden Ticket dans les logs du DC

### P31. Combien de temps est valable le golden ticket que vous avez généré ?

### P32. Qu’est ce que l’administrateur du domaine doit faire s’il détecte qu’un attaquant a compromis le hash du compte krbtgt ?



