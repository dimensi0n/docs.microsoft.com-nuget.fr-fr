---
title: Guide de la Console Gestionnaire de Package NuGet | Documents Microsoft
author: kraigb
hms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 2b92b119-6861-406c-82af-9d739af230e4
description: "Instructions pour à l’aide de la Console du Gestionnaire de Package NuGet dans Visual Studio pour l’utilisation de packages."
keywords: "Console de gestionnaire de package NuGet, powershell NuGet, gérer des packages NuGet"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: cc11963a9b9bfe9aa456d8cd4c8397e1084f660b
ms.sourcegitcommit: a40c1c1cc05a46410f317a72f695ad1d80f39fa2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2018
---
# <a name="package-manager-console"></a>Console du Gestionnaire de package

La Console du Gestionnaire de Package NuGet est intégrée à Visual Studio sur Windows 2012 et versions ultérieures. (Il n’est pas inclus dans Visual Studio pour Mac ou Visual Studio Code.)

La console vous permet d’utiliser [commandes NuGet PowerShell](../tools/powershell-reference.md) pour rechercher, installer, désinstaller et mettre à jour les packages NuGet. L’utilisation de la console est nécessaire dans les cas où le Gestionnaire de Package UI ne fournit pas d’une façon d’effectuer une opération.

Par exemple, rechercher et installer un package s’effectue en trois étapes simples :

1. Ouvrez le projet ou la solution dans Visual Studio, puis ouvrez la console à l’aide de la **Outils > Gestionnaire de Package NuGet > Package Manager Console** commande.

1. Recherchez le package que vous souhaitez installer. Si vous connaissez déjà cela, passez à l’étape 3.

    ```ps
    # Find packages containing the keyword "elmah"
    Find-Package elmah
    ```

1. Exécutez la commande d’installation :

    ```ps
    # Install the Elmah package to the project named MyProject.
    Install-Package Elmah -ProjectName MyProject
    ```

Dans cette rubrique :

- [Ouverture de la console](#opening-the-console-and-console-controls)
- [Installation d’un package](#installing-a-package)
- [Désinstallation d’un package](#uninstalling-a-package)
- [Recherche d’un package](#finding-a-package)
- [Un package de mise à jour](#updating-a-package)
- [Disponibilité de la console](#availability-of-the-console)
- [Extension de la Console du Gestionnaire de Package](#extending-the-package-manager-console)
- [Configuration d’un profil de NuGet PowerShell](#setting-up-a-nuget-powershell-profile)

> [!Important]
> Toutes les opérations qui sont disponibles dans la console peuvent également être effectuées avec la [NuGet CLI](../tools/nuget-exe-cli-reference.md). Toutefois, les commandes de la console fonctionnent dans le contexte de Visual Studio et une projet/solution enregistrée et souvent accomplir plus de leurs commandes CLI équivalentes. Par exemple, l’installation d’un package via la console ajoute une référence au projet n’est pas le cas de la commande CLI. Pour cette raison, les développeurs qui travaillent dans Visual Studio en général, préfèrent à l’aide de la console pour l’interface CLI.

> [!Tip]
> Nombre d’opérations console dépend de disposer d’une solution ouverte dans Visual Studio avec un nom de chemin d’accès connu. Si vous avez une solution non enregistrée, ou aucune solution, vous pouvez voir l’erreur, « Solution est pas ouvert ou non enregistrée. Vérifiez que vous avez une solution ouverte et enregistrée. » Cela indique que la console ne peut pas déterminer le dossier de solution. L’enregistrement d’une solution non enregistrée, ou en créant et en enregistrant une solution si vous n’en avez pas ouvert, devrait corriger l’erreur.

## <a name="opening-the-console-and-console-controls"></a>Ouverture de la console et les contrôles de la console

1. Ouvrez la console dans Visual Studio en utilisant le **Outils > Gestionnaire de Package NuGet > Package Manager Console** commande. La console est une fenêtre de Visual Studio qui peut être organisée et positionnée comme vous le souhaitez (voir [personnaliser des dispositions de fenêtres dans Visual Studio](/visualstudio/ide/customizing-window-layouts-in-visual-studio)).

1. Par défaut, les commandes de la console fonctionnent par rapport à un projet et la source du package spécifique défini dans le contrôle en haut de la fenêtre :

    ![Contrôles de la Console du Gestionnaire de package pour le projet et de la source du package](media/PackageManagerConsoleControls1.png)

1. Sélection d’une source de package différent et/ou d’un projet modifie les valeurs par défaut pour les commandes suivantes. Moyen d’ignorer ces paramètres sans modifier les valeurs par défaut, la plupart des commandes prend en charge `-Source` et `-ProjectName` options.

1. Pour gérer les sources de package, sélectionnez l’icône d’engrenage. Il s’agit d’un raccourci vers le **Outils > Options > Gestionnaire de Package NuGet > Sources de Package** boîte de dialogue, comme décrit dans la [Package Manager UI](Package-Manager-UI.md#package-sources) page. En outre, le contrôle situé à droite du sélecteur de projet efface contenu de la console :

    ![Paramètres de la Console du Gestionnaire de package et effacer les contrôles](media/PackageManagerConsoleControls2.png)

1. Le bouton plus à droite interrompt une commande longue. Par exemple, en cours d’exécution `Get-Package -ListAvailable -PageSize 500` répertorie les packages top 500 sur la source par défaut (par exemple nuget.org), ce qui peut prendre plusieurs minutes pour s’exécuter.

    ![Contrôle d’arrêt de Console du Gestionnaire de package](media/PackageManagerConsoleControls3.png)

## <a name="installing-a-package"></a>Installation d’un package

```ps
# Add the Elmah package to the default project as specified in the console's project selector
Install-Package Elmah

# Add the Elmah package to a project named UtilitiesLib that is not the default
Install-Package Elmah -ProjectName UtilitiesLib
```

Consultez [Install-Package](../tools/ps-ref-install-package.md).

Installation d’un package effectue les actions suivantes :

- Affiche les termes du contrat de licence applicable dans la fenêtre de console avec accord implicite. Si vous n’acceptez pas les termes du contrat, vous devez désinstaller le package immédiatement.
- Ajoute une référence au projet dans le format de la référence est en cours d’utilisation. Références apparaissent par la suite dans l’Explorateur de solutions et le fichier de format de référence applicable. Notez, toutefois, avec PackageReference, vous devez enregistrer le projet pour voir les modifications dans le fichier projet directement.
- Met en cache le package :
    - PackageReference : package est mis en cache dans `%USERPROFILE%\.nuget\packages` et le verrouillage de fichier par exemple, `project.assets.json` est mis à jour.
    - `packages.config`: crée un `packages` dossier à la racine de la solution et copie les fichiers de package dans un sous-dossier. Le `package.config` fichier est mis à jour.
- Les mises à jour `app.config` et/ou `web.config` si le package utilise [source et configuration des transformations de fichiers](../create-packages/source-and-config-file-transformations.md).
- Installe toutes les dépendances, si elle est déjà présent dans le projet. Cela peut mettre à jour les versions de package dans le processus, comme décrit dans [résolution de dépendance](../consume-packages/dependency-resolution.md).
- Affiche le fichier Lisez-moi du package si elle est disponible, dans une fenêtre de Visual Studio.

> [!Tip]
> Un des principaux avantages de l’installation des packages avec le `Install-Package` commande dans la console est qui ajoute une référence au projet comme si vous avez utilisé le Gestionnaire de Package UI. En revanche, le `nuget install` commande CLI télécharge le package uniquement et n’ajoute pas automatiquement une référence.

## <a name="uninstalling-a-package"></a>Désinstallation d’un package

```ps
# Uninstalls the Elmah package from the default project
Uninstall-Package Elmah

# Uninstalls the Elmah package and all its unused dependencies
Uninstall-Package Elmah -RemoveDependencies 

# Uninstalls the Elmah package even if another package depends on it
Uninstall-Package Elmah -Force
```

Consultez [Uninstall-Package](../tools/ps-ref-uninstall-package.md). Utilisez [Get-Package](../tools/ps-ref-get-package.md) pour voir tous les packages actuellement installés dans le projet par défaut si vous avez besoin rechercher un identificateur.

Désinstallation d’un package effectue les actions suivantes :

- Supprime les références au package du projet (et le format de la référence est en cours d’utilisation). Les références n’apparaissent plus dans l’Explorateur de solutions. (Vous devrez peut-être régénérer le projet pour voir qu’il est supprimé de la **Bin** dossier.)
- Annule les modifications apportées à `app.config` ou `web.config` lorsque le package a été installé.
- Dépendances supprime précédemment installés si aucun package restantes n’utilisent ces dépendances.

> [!Tip]
> Comme `Install-Package`, le `Uninstall-Package` commande présente l’avantage de la gestion des références dans le projet, contrairement à la `nuget uninstall` commande CLI.

## <a name="updating-a-package"></a>Un package de mise à jour

```ps
# Checks if there are newer versions available for any installed packages
Get-Package -updates

# Updates a specific package using its identifier, in this case jQuery
Update-Package jQuery

# Update all packages in the project named MyProject (as it appears in Solution Explorer)
Update-Package -ProjectName MyProject

# Update all packages in the solution
Update-Package
```

Consultez [Get-Package](../tools/ps-ref-get-package.md) et [Package de mise à jour](../tools/ps-ref-update-package.md)

## <a name="finding-a-package"></a>Recherche d’un package

```ps
# Find packages containing keywords
Find-Package elmah
Find-Package logging

# List packages whose ID begins with Elmah
Find-Package Elmah -StartWith

# By default, Get-Package returns a list of 20 packages; use -First to show more
Find-Package logging -First 100

# List all versions of the package with the ID of "jquery"
Find-Package jquery -AllVersions -ExactMatch
```

Consultez [Find-Package](../tools/ps-ref-find-package.md). Dans Visual Studio 2013 et versions antérieures, utilisez [Get-Package](../tools/ps-ref-get-package.md) à la place.

## <a name="availability-of-the-console"></a>Disponibilité de la console

Dans Visual Studio 2017, NuGet et le Gestionnaire de Package NuGet sont installés automatiquement lorsque vous sélectionnez. Charges de travail liées NET ; Vous pouvez également l’installer séparément en vérifiant la **des composants individuels > Code Outils > Gestionnaire de package NuGet** option dans le programme d’installation de Visual Studio 2017.

En outre, si vous êtes pas le Gestionnaire de Package NuGet dans Visual Studio 2015 et versions antérieures, vérifiez **Outils > Extensions et mises à jour...**  et recherchez l’extension du Gestionnaire de Package NuGet. Si vous ne parvenez pas à utiliser le programme d’installation des extensions dans Visual Studio, vous pouvez télécharger l’extension directement à partir de [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html).

La Console du Gestionnaire de Package n’est pas actuellement disponible dans Visual Studio pour Mac. Toutefois, les commandes équivalentes, sont disponibles via le [NuGet CLI](nuget-exe-CLI-reference.md). Visual Studio pour Mac possède une interface utilisateur pour la gestion des packages NuGet. Consultez [package, y compris un NuGet dans votre projet](/visualstudio/mac/nuget-walkthrough).

La Console du Gestionnaire de Package n’est pas incluse avec Visual Studio Code.

## <a name="extending-the-package-manager-console"></a>Extension de la Console du Gestionnaire de Package

Certains packages installent de nouvelles commandes de la console. Par exemple, `MvcScaffolding` crée des commandes telles que `Scaffold` illustré ci-dessous, qui génère les contrôleurs ASP.NET MVC et les vues :

![Installation et l’utilisation de MvcScaffold](media/PackageManagerConsoleInstall.png)

## <a name="setting-up-a-nuget-powershell-profile"></a>Configuration d’un profil de PowerShell NuGet

Un profil de PowerShell vous permet de rendre les commandes couramment utilisées disponible partout où vous utilisez PowerShell. NuGet prend en charge un profil spécifique NuGet que se trouve généralement à l’emplacement suivant :

```
%UserProfile%\Documents\WindowsPowerShell\NuGet_profile.ps1
```

Pour trouver le profil, tapez `$profile` dans la console :

```ps
$profile
C:\Users\<user>\Documents\WindowsPowerShell\NuGet_profile.ps1
```

Pour plus d’informations, reportez-vous à [les profils Windows PowerShell](https://technet.microsoft.com/library/bb613488.aspx).