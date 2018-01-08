L’installation d’un package se fait de trois façons :

| Méthode | Description | Référence |
| --- | --- | --- |
| Interface CLI de nuget.exe : `nuget install <package_name>` | Télécharge le package identifié par \<package_name\> et développe son contenu dans un dossier du répertoire actif. Si aucun package n’est spécifié, installe tous les packages répertoriés dans le fichier `packages.config` du projet. Aucune modification n’est apportée aux fichiers projet. Les dépendances sont également téléchargées et développées. | [Informations de référence sur l’interface de ligne de commande](../tools/nuget-exe-CLI-Reference.md) |
| Console du Gestionnaire de package (Visual Studio) : `Install-Package <package_name>` | Télécharge et installe le package dans le projet actuel, puis met à jour le fichier projet pour répertorier le package en tant que dépendance. | [Guide de la console du Gestionnaire de package](../tools/Package-Manager-Console.md) |
| Interface utilisateur du Gestionnaire de package (Visual Studio) | Fournit une interface utilisateur dans laquelle vous pouvez parcourir, sélectionner et installer des packages dans un projet. Met à jour le fichier projet pour répertorier le package en tant que dépendance. | [Informations de référence sur l’interface utilisateur du Gestionnaire de package](../tools/Package-Manager-UI.md) |