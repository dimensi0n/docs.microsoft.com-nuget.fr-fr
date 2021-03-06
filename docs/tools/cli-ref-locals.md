---
title: Commande de variables locales de CLI de NuGet
description: Référence de la commande de variables locales de nuget.exe
author: karann-msft
ms.author: karann
ms.date: 03/19/2018
ms.topic: reference
ms.openlocfilehash: ea216c4651ba9619bc3b6a7ab52546fd299b0bb6
ms.sourcegitcommit: 1d1406764c6af5fb7801d462e0c4afc9092fa569
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43545026"
---
# <a name="locals-command-nuget-cli"></a>locals (commande, NuGet CLI)

**S’applique à :** la consommation de package &bullet; **versions prises en charge :** 3.3 +

Efface ou répertorie les ressources NuGet locales telles que la *http-cache*, *global-packages* dossier et le dossier temporaire. Le `locals` commande peut également être utilisée pour afficher une liste de ces emplacements. Pour plus d’informations, consultez [gérer les packages globaux et les dossiers de cache](../consume-packages/managing-the-global-packages-and-cache-folders.md).

## <a name="usage"></a>Utilisation

```cli
nuget locals <folder> [options]
```

où `<folder>` est un des `all`, `http-cache`, `packages-cache` *(3.5 et versions antérieures)*, `global-packages`, `temp` *(3.4 +)*, et `plugins-cache` *(4.8 +)*.

## <a name="options"></a>Options

| Option | Description |
| --- | --- |
| Effacer | Efface le dossier spécifié. |
| ConfigFile | Le fichier de configuration de NuGet à appliquer. Si non spécifié, `%AppData%\NuGet\NuGet.Config` (Windows) ou `~/.nuget/NuGet/NuGet.Config` (Mac/Linux) est utilisé.|
| ForceEnglishOutput | *(3.5 +)* Force nuget.exe pour exécuter à l’aide d’une culture dite indifférente, en anglais. |
| Help | Affiche l’aide de la commande. |
| Liste | Répertorie l’emplacement du dossier spécifié, ou les emplacements de tous les dossiers lorsqu’il est utilisé avec *tous les*. |
| NonInteractive | Supprime les invites pour l’entrée de l’utilisateur ou de confirmations. |
| Verbosity | Spécifie la quantité de détails affichés dans la sortie : *normal*, *silencieux*, *détaillées*. |

Consultez également [variables d’environnement](cli-ref-environment-variables.md)

## <a name="examples"></a>Exemples

```cli
nuget locals all -list
nuget locals http-cache -clear
```

Pour obtenir des exemples supplémentaires, consultez [gérer les packages globaux et les dossiers de cache](../consume-packages/managing-the-global-packages-and-cache-folders.md).
