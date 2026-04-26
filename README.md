Rapport d'Audit de Sécurité Statique : Application Pizza Recipes
Ce rapport détaille les résultats de l'analyse automatisée effectuée avec MobSF (Mobile Security Framework) sur l'application pizza.apk. L'objectif est d'identifier les faiblesses structurelles et les vulnérabilités logicielles avant toute mise en production.

1. Méthodologie et Environnement
L'audit a été réalisé dans un environnement isolé en utilisant la technologie de conteneurisation Docker. Cette approche garantit la répétabilité des tests et la sécurité de l'hôte d'analyse.

Processus technique :

Récupération de l'image officielle MobSF via Docker Hub.

Déploiement du conteneur avec exposition du port 8000.

Analyse statique automatisée (Static Analysis) sans exécution du code.

2. Informations sur la Cible
L'analyse porte sur les métadonnées techniques suivantes :

Nom du fichier : pizza.apk

Package : pizza.lachgar.ma.pizzarecipes

Activité principale : SplashActivity

SDK Cible : 26 (Android 8.0)

SDK Minimum : 17 (Android 4.2)

Empreinte SHA256 : 7def8c514f65bcb1d92403945d498618e0567dbd51b40796da4acf0f1aeb7cd6

3. Score de Sécurité et État de Santé
Le score de sécurité global attribué par MobSF est de 27/100.

Ce score extrêmement faible indique une posture de sécurité critique. Une telle note est généralement le résultat de multiples vulnérabilités de haute sévérité, telles que l'utilisation de protocoles de communication non sécurisés, la présence de secrets en clair dans le code ou des configurations de manifeste permissives.

4. Analyse du Manifeste (Composants Android)
Le manifeste est la première ligne de défense d'une application. L'analyse révèle des points de vigilance majeurs :

Activités Exportées (Exported Activities)
L'application expose 3 activités.
Lorsqu'une activité est marquée comme "exportée", elle devient accessible aux autres applications installées sur l'appareil. Cela peut permettre à une application malveillante de forcer l'ouverture de certains écrans, de contourner les étapes d'authentification ou d'intercepter des flux de données internes. Ce risque est classé sous la catégorie OWASP MASVS-PLATFORM.

Gestion des Backups
La configuration permet la sauvegarde des données utilisateur. Si le terminal est compromis physiquement, un attaquant peut extraire les données privées de l'application via une simple commande ADB.

5. Analyse des Risques et Standards (MASVS)
Les vulnérabilités détectées par MobSF s'alignent sur les standards de l'industrie :

MASVS-STORAGE : Risque de fuite de données via les sauvegardes automatiques et le stockage local non chiffré.

MASVS-PLATFORM : Exposition indue des composants (Activities) permettant des attaques par "Activity Hijacking".

MASVS-RESILIENCE : Score de 27/100 suggérant une absence totale de mécanismes d'obfuscation ou de protection contre l'ingénierie inverse.

6. Recommandations de Sécurité
Pour améliorer la sécurité de l'application, les mesures suivantes doivent être implémentées :

Restriction des composants : Désactiver l'attribut android:exported pour toutes les activités qui n'ont pas besoin de communiquer avec des applications tierces.

Protection des données : Désactiver android:allowBackup dans le manifeste ou implémenter un agent de sauvegarde chiffré.

Mise à jour du SDK : Augmenter le targetSdkVersion pour bénéficier des dernières protections du noyau Android (ex: Scoped Storage).

Audit de code : Revoir les classes Java pour éliminer les logs de débogage et les clés API potentiellement codées en dur.

Conclusion
L'application Pizza Recipes présente un niveau de risque élevé pour ses utilisateurs. En l'état, elle ne répond pas aux critères minimaux de sécurité pour une diffusion publique. Une correction immédiate des composants exportés et une révision de la politique de stockage sont impératives.
