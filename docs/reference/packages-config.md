---
title: Référence du fichier packages.config NuGet
description: Dans certains types de projets, packages.config gère la liste des packages NuGet utilisés dans le projet.
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 12/07/2017
ms.topic: reference
ms.openlocfilehash: 73234f79cb9eb30327c4e206a5bc51c5bc1c6f1d
ms.sourcegitcommit: 3eab9c4dd41ea7ccd2c28bb5ab16f6fbbec13708
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="packagesconfig-reference"></a><span data-ttu-id="95ff1-103">Informations de référence sur packages.config</span><span class="sxs-lookup"><span data-stu-id="95ff1-103">packages.config reference</span></span>

<span data-ttu-id="95ff1-104">Le fichier `packages.config` est utilisé dans certains types de projets pour gérer la liste des packages référencés par le projet.</span><span class="sxs-lookup"><span data-stu-id="95ff1-104">The `packages.config` file is used in some project types to maintain the list of packages referenced by the project.</span></span> <span data-ttu-id="95ff1-105">Cela permet à NuGet de restaurer facilement les dépendances du projet quand le projet doit être acheminé vers un autre ordinateur, tel qu’un serveur de builds, sans tous ces packages.</span><span class="sxs-lookup"><span data-stu-id="95ff1-105">This allows NuGet to easily restore the project's dependencies when the project to be transported to a different machine, such as a build server, without all those packages.</span></span>

## <a name="schema"></a><span data-ttu-id="95ff1-106">Schéma</span><span class="sxs-lookup"><span data-stu-id="95ff1-106">Schema</span></span>

<span data-ttu-id="95ff1-107">Le schéma est simple : l’en-tête XML standard est suivi d’un seul nœud `<packages>` qui contient un ou plusieurs éléments `<package>`, un pour chaque référence.</span><span class="sxs-lookup"><span data-stu-id="95ff1-107">The schema is simple: following the standard XML header is a single `<packages>` node that contains one or more `<package>` elements, one for each reference.</span></span> <span data-ttu-id="95ff1-108">Chaque élément `<package>` peut avoir les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="95ff1-108">Each `<package>` element can have the following attributes:</span></span>

| <span data-ttu-id="95ff1-109">Attribut</span><span class="sxs-lookup"><span data-stu-id="95ff1-109">Attribute</span></span> | <span data-ttu-id="95ff1-110">Obligatoire</span><span class="sxs-lookup"><span data-stu-id="95ff1-110">Required</span></span> | <span data-ttu-id="95ff1-111">Description</span><span class="sxs-lookup"><span data-stu-id="95ff1-111">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="95ff1-112">ID</span><span class="sxs-lookup"><span data-stu-id="95ff1-112">id</span></span> | <span data-ttu-id="95ff1-113">Oui</span><span class="sxs-lookup"><span data-stu-id="95ff1-113">Yes</span></span> | <span data-ttu-id="95ff1-114">Identificateur du package, tel que Newtonsoft.json ou Microsoft.AspNet.Mvc.</span><span class="sxs-lookup"><span data-stu-id="95ff1-114">The identifier of the package, such as Newtonsoft.json or Microsoft.AspNet.Mvc.</span></span> | 
| <span data-ttu-id="95ff1-115">version</span><span class="sxs-lookup"><span data-stu-id="95ff1-115">version</span></span> | <span data-ttu-id="95ff1-116">Oui</span><span class="sxs-lookup"><span data-stu-id="95ff1-116">Yes</span></span> | <span data-ttu-id="95ff1-117">Version exacte du package à installer, comme 3.1.1 ou la version bêta 4.2.5.11.</span><span class="sxs-lookup"><span data-stu-id="95ff1-117">The exact version of the package to install, such as 3.1.1 or 4.2.5.11-beta.</span></span> <span data-ttu-id="95ff1-118">Une chaîne de version doit avoir au moins trois chiffres ; un quatrième est facultatif, comme un suffixe de préversion.</span><span class="sxs-lookup"><span data-stu-id="95ff1-118">A version string must have at least three numbers; a fourth is optional, as is a pre-release suffix.</span></span> <span data-ttu-id="95ff1-119">Les plages ne sont pas autorisées.</span><span class="sxs-lookup"><span data-stu-id="95ff1-119">Ranges are not allowed.</span></span> | 
| <span data-ttu-id="95ff1-120">targetFramework</span><span class="sxs-lookup"><span data-stu-id="95ff1-120">targetFramework</span></span> | <span data-ttu-id="95ff1-121">Non</span><span class="sxs-lookup"><span data-stu-id="95ff1-121">No</span></span> | <span data-ttu-id="95ff1-122">[Moniker du Framework cible (TFM)](target-frameworks.md) à appliquer lors de l’installation du package.</span><span class="sxs-lookup"><span data-stu-id="95ff1-122">The [target framework moniker (TFM)](target-frameworks.md) to apply when installing the package.</span></span> <span data-ttu-id="95ff1-123">Cet attribut est initialement défini sur la cible du projet quand un package est installé.</span><span class="sxs-lookup"><span data-stu-id="95ff1-123">This is initially set to the project's target when a package is installed.</span></span> <span data-ttu-id="95ff1-124">Par conséquent, différents éléments `<package>` peuvent avoir différents monikers TFM.</span><span class="sxs-lookup"><span data-stu-id="95ff1-124">As a result, different `<package>` elements can have different TFMs.</span></span> <span data-ttu-id="95ff1-125">Par exemple, si vous créez un projet ciblant .NET 4.5.2, les packages installés à ce stade utilisent le moniker TFM net452.</span><span class="sxs-lookup"><span data-stu-id="95ff1-125">For example, if you create a project targeting .NET 4.5.2, packages installed at that point will use the TFM of net452.</span></span> <span data-ttu-id="95ff1-126">Si vous reciblez ultérieurement le projet vers .NET 4.6 et ajoutez d’autres packages, ceux-ci utilisent le moniker TFM net46.</span><span class="sxs-lookup"><span data-stu-id="95ff1-126">If you ;later retarget the project to .NET 4.6 and add more packages, those will use TFM of net46.</span></span> <span data-ttu-id="95ff1-127">Une incompatibilité entre la cible du projet et les attributs `targetFramework` génère des avertissements, auquel cas vous pouvez réinstaller les packages concernés.</span><span class="sxs-lookup"><span data-stu-id="95ff1-127">A mismatch between the project's target and `targetFramework` attributes will generate warnings, in which case you can reinstall the affected packages.</span></span> | 
| <span data-ttu-id="95ff1-128">allowedVersions</span><span class="sxs-lookup"><span data-stu-id="95ff1-128">allowedVersions</span></span> | <span data-ttu-id="95ff1-129">Non</span><span class="sxs-lookup"><span data-stu-id="95ff1-129">No</span></span> | <span data-ttu-id="95ff1-130">Plage de versions autorisées pour ce package appliquée au cours de la mise à jour du package (consultez [Limitation des versions de mise à niveau](../consume-packages/reinstalling-and-updating-packages.md#constraining-upgrade-versions)).</span><span class="sxs-lookup"><span data-stu-id="95ff1-130">A range of allowed versions for this package applied during package update (see [Constraining upgrade versions](../consume-packages/reinstalling-and-updating-packages.md#constraining-upgrade-versions).</span></span> <span data-ttu-id="95ff1-131">Cet attribut n’affecte *pas* le package installé pendant une opération d’installation ou de restauration.</span><span class="sxs-lookup"><span data-stu-id="95ff1-131">It does *not* affect what package is installed during an install or restore operation.</span></span> <span data-ttu-id="95ff1-132">Consultez [Gestion de versions des packages](../reference/package-versioning.md#version-ranges-and-wildcards) pour connaître la syntaxe.</span><span class="sxs-lookup"><span data-stu-id="95ff1-132">See [Package versioning](../reference/package-versioning.md#version-ranges-and-wildcards) for syntax.</span></span> <span data-ttu-id="95ff1-133">L’interface utilisateur du Gestionnaire de package désactive également toutes les versions en dehors de la plage autorisée.</span><span class="sxs-lookup"><span data-stu-id="95ff1-133">The PackageManager UI also disables all versions outside the allowed range.</span></span> | 
| <span data-ttu-id="95ff1-134">developmentDependency</span><span class="sxs-lookup"><span data-stu-id="95ff1-134">developmentDependency</span></span> | <span data-ttu-id="95ff1-135">Non</span><span class="sxs-lookup"><span data-stu-id="95ff1-135">No</span></span> | <span data-ttu-id="95ff1-136">Si le projet de consommation lui-même crée un package NuGet, l’affectation de la valeur `true` à cet attribut pour une dépendance empêche l’ajout de ce package quand le package de consommation est créé.</span><span class="sxs-lookup"><span data-stu-id="95ff1-136">If the consuming project itself creates a NuGet package, setting this to `true` for a dependency prevents that package from being included when the consuming package is created.</span></span> <span data-ttu-id="95ff1-137">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="95ff1-137">The default is `false`.</span></span> | 

## <a name="examples"></a><span data-ttu-id="95ff1-138">Exemples</span><span class="sxs-lookup"><span data-stu-id="95ff1-138">Examples</span></span>

<span data-ttu-id="95ff1-139">L’élément `packages.config` suivant fait référence à deux dépendances :</span><span class="sxs-lookup"><span data-stu-id="95ff1-139">The following `packages.config` refers to two dependencies:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="jQuery" version="3.1.1" targetFramework="net46" />
  <package id="NLog" version="4.3.10" targetFramework="net46" />
</packages>
```

<span data-ttu-id="95ff1-140">L’élément`packages.config` suivant fait référence à neuf packages, mais `Microsoft.Net.Compilers` n’est pas inclus lors de la création du package de consommation en raison de l’attribut `developmentDependency`.</span><span class="sxs-lookup"><span data-stu-id="95ff1-140">The following `packages.config` refers to nine packages, but `Microsoft.Net.Compilers` will not be included when building the consuming package because of the `developmentDependency` attribute.</span></span> <span data-ttu-id="95ff1-141">La référence à Newtonsoft.Json restreint également les mises à jour des versions 8.x et 9.x uniquement.</span><span class="sxs-lookup"><span data-stu-id="95ff1-141">The reference to Newtonsoft.Json also restricts updates to 8.x and 9.x versions only.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.CodeDom.Providers.DotNetCompilerPlatform" version="1.0.0" targetFramework="net46" />
  <package id="Microsoft.Net.Compilers" version="1.0.0" targetFramework="net46" developmentDependency="true" />
  <package id="Microsoft.Web.Infrastructure" version="1.0.0.0" targetFramework="net46" />
  <package id="Microsoft.Web.Xdt" version="2.1.1" targetFramework="net46" />
  <package id="Newtonsoft.Json" version="8.0.3" allowedVersions="[8,10)" targetFramework="net46" />
  <package id="NuGet.Core" version="2.11.1" targetFramework="net46" />
  <package id="NuGet.Server" version="2.11.2" targetFramework="net46" />
  <package id="RouteMagic" version="1.3" targetFramework="net46" />
  <package id="WebActivatorEx" version="2.1.0" targetFramework="net46" />
</packages>
```