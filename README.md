Rapport d'Audit de Sécurité Statique : Analyse Automatisée avec MobSF
Ce rapport détaille les étapes d'une analyse de sécurité automatisée effectuée sur l'application pizza.apk à l'aide du framework MobSF (Mobile Security Framework). L'objectif est d'identifier les vulnérabilités structurelles et logicielles sans exécution dynamique de l'application.

1. Mise en place de l'environnement de test
Le déploiement de l'outil a été réalisé via Docker pour garantir un environnement isolé et reproductible au sein de la VM.

Récupération de l'image : Téléchargement de la dernière version stable de MobSF depuis le registre Docker Hub.

Initialisation du conteneur : Lancement de l'instance avec mappage des ports pour accéder à l'interface web locale (port 8000).

Configuration initiale : Lors du premier lancement, le framework télécharge automatiquement les dépendances nécessaires, notamment le décompilateur JADX.

2. Identification de la cible (Reconnaissance)
Une fois l'APK téléchargé et soumis à l'interface, MobSF génère une fiche d'identité technique permettant d'assurer la traçabilité de l'audit.

Nom du package : pizza.lachgar.ma.pizzarecipes

Main Activity : SplashActivity

Empreintes cryptographiques (Hashes) : Les signatures MD5, SHA1 et SHA256 ont été calculées pour garantir l'intégrité du fichier analysé.

Compatibilité : L'application cible le SDK 26 (Android 8.0) avec une compatibilité minimale remontant au SDK 17.

3. Analyse du Score de Sécurité
Le rapport affiche un score de sécurité de 27/100. Ce résultat est jugé très faible et indique la présence de multiples vulnérabilités critiques ou d'un manque flagrant de mesures de durcissement (hardening).

4. Analyse du Manifeste et des Composants
L'analyse automatisée du fichier AndroidManifest.xml a permis d'isoler des vecteurs d'attaque potentiels.

Activités Exportées
MobSF a identifié 3 activités exportées.

Risque : Une activité exportée peut être lancée par n'importe quelle autre application présente sur l'appareil. Si ces activités ne disposent pas de contrôles d'accès rigoureux, elles peuvent permettre de contourner les écrans de connexion ou d'accéder à des données sensibles.

Permissions et Sauvegarde
Le rapport examine les permissions demandées au système. Une attention particulière est portée à l'attribut android:allowBackup="true", qui permet l'extraction des données locales de l'application via une simple connexion ADB.

5. Synthèse des Vulnérabilités (Normes OWASP)
L'analyse statique permet de mapper les défauts du code aux standards de l'industrie :

Plateforme (MASVS-PLATFORM) : Exposition de composants via des activités exportées sans protection par permissions personnalisées.

Stockage (MASVS-STORAGE) : Risque lié à la persistance des données lors d'une sauvegarde ADB.

Cryptographie (MASVS-CRYPTO) : Recherche automatisée de clés API ou de secrets codés en dur dans les fichiers de ressources.

6. Conclusion et Recommandations
L'utilisation de MobSF a permis d'obtenir une vision globale de la posture de sécurité de l'application en un temps record. Pour améliorer le score de sécurité, les actions suivantes sont préconisées :

Réduire la surface d'attaque : Passer l'attribut android:exported à false pour toutes les activités qui ne doivent pas être appelées par des applications tierces.

Désactiver la sauvegarde : Configurer allowBackup sur false dans le manifeste.

Audit de code manuel : Compléter cette analyse par un examen manuel des classes Java pour identifier les vulnérabilités logiques que l'automatisation pourrait manquer (faux négatifs).
