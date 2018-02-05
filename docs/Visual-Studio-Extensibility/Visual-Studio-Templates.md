---
title: "Packages NuGet dans les modèles Visual Studio | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/3/2018
ms.topic: article
ms.prod: nuget
ms.technology: 
description: "Instructions pour inclure des packages NuGet dans des modèles de projet et d’élément Visual Studio."
keywords: "NuGet dans Visual Studio, modèles de projet Visual Studio, modèles d’élément Visual Studio, packages dans des modèles de projet, packages dans des modèles d’élément"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 45a2ca2c08660be650f9cf38301f628923e1f8be
ms.sourcegitcommit: a40c1c1cc05a46410f317a72f695ad1d80f39fa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2018
---
# <a name="packages-in-visual-studio-templates"></a><span data-ttu-id="fc05a-104">Packages dans des modèles Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fc05a-104">Packages in Visual Studio templates</span></span>

<span data-ttu-id="fc05a-105">Les modèles de projet et d’élément Visual Studio doivent souvent vérifier que certains packages y sont installés lors de la création d’un projet ou d’un élément.</span><span class="sxs-lookup"><span data-stu-id="fc05a-105">Visual Studio project and item templates often need to ensure that certain packages are installed when a project or item is created.</span></span> <span data-ttu-id="fc05a-106">Par exemple, le modèle ASP.NET MVC 3 installe jQuery, Modernizr et d’autres packages.</span><span class="sxs-lookup"><span data-stu-id="fc05a-106">For example, the ASP.NET MVC 3 template installs jQuery, Modernizr, and other packages.</span></span>

<span data-ttu-id="fc05a-107">Pour la prise en charge de cette contrainte, les créateurs de modèle peuvent indiquer à NuGet d’installer les packages nécessaires, au lieu de bibliothèques individuelles.</span><span class="sxs-lookup"><span data-stu-id="fc05a-107">To support this, template authors can instruct NuGet to install the necessary packages, rather than individual libraries.</span></span> <span data-ttu-id="fc05a-108">Les développeurs peuvent ensuite facilement mettre à jour ces packages à tout moment.</span><span class="sxs-lookup"><span data-stu-id="fc05a-108">Developers can then easily update those packages at any later time.</span></span>

<span data-ttu-id="fc05a-109">Pour en savoir plus sur la création des modèles eux-mêmes, reportez-vous à [Guide pratique pour créer des modèles de projet](/visualstudio/ide/how-to-create-project-templates) ou à [Création de modèles de projet et d’élément personnalisés](/visualstudio/extensibility/creating-custom-project-and-item-templates).</span><span class="sxs-lookup"><span data-stu-id="fc05a-109">To learn more about authoring templates themselves, refer to [How to: Create Project Templates](/visualstudio/ide/how-to-create-project-templates) or [Creating Custom Project and Item Templates](/visualstudio/extensibility/creating-custom-project-and-item-templates).</span></span>

<span data-ttu-id="fc05a-110">Le reste de cette section décrit les étapes spécifiques à effectuer lors de la création d’un modèle pour inclure correctement des packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="fc05a-110">The remainder of this section describes the specific steps to take when authoring a template to properly include NuGet packages.</span></span>

- [<span data-ttu-id="fc05a-111">Ajout de packages à un modèle</span><span class="sxs-lookup"><span data-stu-id="fc05a-111">Adding packages to a template</span></span>](#adding-packages-to-a-template)
- [<span data-ttu-id="fc05a-112">Bonnes pratiques</span><span class="sxs-lookup"><span data-stu-id="fc05a-112">Best practices</span></span>](#best-practices)

<span data-ttu-id="fc05a-113">Pour obtenir un exemple, consultez [l’exemple NuGetInVsTemplates](https://bitbucket.org/marcind/nugetinvstemplates).</span><span class="sxs-lookup"><span data-stu-id="fc05a-113">For an example, see the [NuGetInVsTemplates sample](https://bitbucket.org/marcind/nugetinvstemplates).</span></span>

## <a name="adding-packages-to-a-template"></a><span data-ttu-id="fc05a-114">Ajout de packages à un modèle</span><span class="sxs-lookup"><span data-stu-id="fc05a-114">Adding packages to a template</span></span>

<span data-ttu-id="fc05a-115">Quand un modèle est instancié, un [Assistant Modèle](/visualstudio/extensibility/how-to-use-wizards-with-project-templates) est appelé pour charger la liste des packages à installer, ainsi que des informations sur l’emplacement où rechercher ces packages.</span><span class="sxs-lookup"><span data-stu-id="fc05a-115">When a template is instantiated, a [template wizard](/visualstudio/extensibility/how-to-use-wizards-with-project-templates) is invoked to load the list of packages to install along with information about where to find those packages.</span></span> <span data-ttu-id="fc05a-116">Les packages peuvent être incorporés dans l’extension VSIX, incorporés dans le modèle ou situés sur le disque dur local, auquel cas vous utilisez une clé de Registre pour référencer le chemin de fichier.</span><span class="sxs-lookup"><span data-stu-id="fc05a-116">Packages can be embedded in the VSIX, embedded in the template, or located on the local hard drive in which case you use a registry key to reference the file path.</span></span> <span data-ttu-id="fc05a-117">Vous pouvez trouver plus d’informations sur ces emplacements plus loin dans cette section.</span><span class="sxs-lookup"><span data-stu-id="fc05a-117">Details on these locations are given later in this section.</span></span>

<span data-ttu-id="fc05a-118">Les packages préinstallés fonctionnent avec des [Assistants Modèle](/visualstudio/extensibility/how-to-use-wizards-with-project-templates).</span><span class="sxs-lookup"><span data-stu-id="fc05a-118">Preinstalled packages work using [template wizards](/visualstudio/extensibility/how-to-use-wizards-with-project-templates).</span></span> <span data-ttu-id="fc05a-119">Un Assistant spécial est appelé quand le modèle est instancié.</span><span class="sxs-lookup"><span data-stu-id="fc05a-119">A special wizard gets invoked when the template gets instantiated.</span></span> <span data-ttu-id="fc05a-120">L’Assistant charge la liste des packages qui doivent être installés et passe ces informations aux API NuGet appropriées.</span><span class="sxs-lookup"><span data-stu-id="fc05a-120">The wizard loads the list of packages that need to be installed and passes that information to the appropriate NuGet APIs.</span></span>

<span data-ttu-id="fc05a-121">Étapes pour inclure des packages dans un modèle :</span><span class="sxs-lookup"><span data-stu-id="fc05a-121">Steps to include packages in a template:</span></span>

1. <span data-ttu-id="fc05a-122">Dans votre fichier `vstemplate`, ajoutez une référence à l’Assistant Modèle NuGet en ajoutant un élément [`WizardExtension`](/visualstudio/extensibility/wizardextension-element-visual-studio-templates) :</span><span class="sxs-lookup"><span data-stu-id="fc05a-122">In your `vstemplate` file, add a reference to the NuGet template wizard by adding a [`WizardExtension`](/visualstudio/extensibility/wizardextension-element-visual-studio-templates) element:</span></span>

    ```xml
    <WizardExtension>
        <Assembly>NuGet.VisualStudio.Interop, Version=1.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a</Assembly>
        <FullClassName>NuGet.VisualStudio.TemplateWizard</FullClassName>
    </WizardExtension>
    ```

    <span data-ttu-id="fc05a-123">`NuGet.VisualStudio.Interop.dll` est un assembly contenant seulement la classe `TemplateWizard`, qui est un simple wrapper qui effectue un appel dans l’implémentation actuelle de `NuGet.VisualStudio.dll`.</span><span class="sxs-lookup"><span data-stu-id="fc05a-123">`NuGet.VisualStudio.Interop.dll` is an assembly that contains only the `TemplateWizard` class, which is a simple wrapper that calls into the actual implementation in `NuGet.VisualStudio.dll`.</span></span> <span data-ttu-id="fc05a-124">La version de l’assembly ne change jamais : les modèles de projet/élément continuent donc de fonctionner avec les nouvelles versions de NuGet.</span><span class="sxs-lookup"><span data-stu-id="fc05a-124">The assembly version will never change so that project/item templates continue to work with new versions of NuGet.</span></span>

1. <span data-ttu-id="fc05a-125">Ajoutez la liste des packages à installer dans le projet :</span><span class="sxs-lookup"><span data-stu-id="fc05a-125">Add the list of packages to install in the project:</span></span>

    ```xml
    <WizardData>
        <packages>
            <package id="jQuery" version="1.6.2" />
        </packages>
    </WizardData>
    ```

    <span data-ttu-id="fc05a-126">*(NuGet 2.2.1+)*  L’Assistant prend en charge plusieurs éléments `<package>`, ce qui permet la prise en charge de plusieurs sources de packages.</span><span class="sxs-lookup"><span data-stu-id="fc05a-126">*(NuGet 2.2.1+)* The wizard supports multiple `<package>` elements to support multiple package sources.</span></span> <span data-ttu-id="fc05a-127">Les attributs `id` et `version` sont tous deux obligatoires, ce qui signifie qu’une version spécifique d’un package est installée même si une version plus récente est disponible.</span><span class="sxs-lookup"><span data-stu-id="fc05a-127">Both the `id` and `version` attributes are required, meaning that specific version of a package will be installed even if a newer version is available.</span></span> <span data-ttu-id="fc05a-128">Ceci évite que les mises à jour des packages ne compromettent le bon fonctionnement du modèle, en laissant au développeur la possibilité de mettre à jour le package avec le modèle.</span><span class="sxs-lookup"><span data-stu-id="fc05a-128">This prevents package updates from breaking the template, leaving the choice to update the package to the developer using the template.</span></span>

1. <span data-ttu-id="fc05a-129">Spécifiez le référentiel où NuGet peut trouver les packages, comme décrit dans les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="fc05a-129">Specify the repository where NuGet can find the packages as described in the following sections.</span></span>

### <a name="vsix-package-repository"></a><span data-ttu-id="fc05a-130">Référentiel de packages VSIX</span><span class="sxs-lookup"><span data-stu-id="fc05a-130">VSIX package repository</span></span>

<span data-ttu-id="fc05a-131">L’approche recommandée du déploiement pour les modèles de projet/élément Visual Studio est une [extension VSIX](/visualstudio/extensibility/shipping-visual-studio-extensions), car elle vous permet de placer ensemble dans un package plusieurs modèles de projet/élément, et permet aux développeurs de découvrir facilement vos modèles avec le Gestionnaire d’extensions Visual Studio ou avec la galerie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc05a-131">The recommended deployment approach for Visual Studio project/item templates is a [VSIX extension](/visualstudio/extensibility/shipping-visual-studio-extensions) because it allows you to package multiple project/item templates together and allows developers to easily discover your templates using the VS Extension Manager or the Visual Studio Gallery.</span></span> <span data-ttu-id="fc05a-132">Les mises à jour de l’extension sont également faciles à déployer avec le [mécanisme de mise à jour automatique du Gestionnaire d’extensions Visual Studio](/visualstudio/extensibility/how-to-update-a-visual-studio-extension).</span><span class="sxs-lookup"><span data-stu-id="fc05a-132">Updates to the extension are also easy to deploy using the [Visual Studio Extension Manager automatic update mechanism](/visualstudio/extensibility/how-to-update-a-visual-studio-extension).</span></span>

<span data-ttu-id="fc05a-133">L’extension VSIX elle-même peut être utilisée comme source pour les packages nécessaires au modèle :</span><span class="sxs-lookup"><span data-stu-id="fc05a-133">The VSIX itself can serve as the source for packages required by the template:</span></span>

1. <span data-ttu-id="fc05a-134">Modifiez l’élément `<packages>` dans le fichier `.vstemplate` comme suit :</span><span class="sxs-lookup"><span data-stu-id="fc05a-134">Modify the `<packages>` element in the `.vstemplate` file as follows:</span></span>

    ```xml
    <packages repository="extension" repositoryId="MyTemplateContainerExtensionId">
        <!-- ... -->
    </packages>
    ```

    <span data-ttu-id="fc05a-135">L’attribut `repository` spécifie le type de référentiel comme étant `extension`, tandis que `repositoryId` est l’identificateur unique de l’extension VSIX elle-même (c’est la valeur de l’attribut `ID` dans le fichier `vsixmanifest` de l’extension, voir [Informations de référence sur l’extension VSIX avec un schéma 2.0](/visualstudio/extensibility/vsix-extension-schema-2-0-reference)).</span><span class="sxs-lookup"><span data-stu-id="fc05a-135">The `repository` attribute specifies the type of repository as `extension` while `repositoryId` is the unique identifier of the VSIX itself (This is the value of the `ID` attribute in the extension’s `vsixmanifest` file, see [VSIX Extension Schema 2.0 Reference](/visualstudio/extensibility/vsix-extension-schema-2-0-reference)).</span></span>

1. <span data-ttu-id="fc05a-136">Placez vos fichiers `nupkg` dans un dossier nommé `Packages` au sein de l’extension VSIX.</span><span class="sxs-lookup"><span data-stu-id="fc05a-136">Place your `nupkg` files in a folder called `Packages` within the VSIX.</span></span>

1. <span data-ttu-id="fc05a-137">Ajoutez les fichiers de package nécessaires comme `<Asset>` dans votre fichier `vsixmanifest` (voir [Informations de référence sur l’extension VSIX avec un schéma 2.0](/visualstudio/extensibility/vsix-extension-schema-2-0-reference)) :</span><span class="sxs-lookup"><span data-stu-id="fc05a-137">Add the necessary package files as `<Asset>` in your `vsixmanifest` file (see [VSIX Extension Schema 2.0 Reference](/visualstudio/extensibility/vsix-extension-schema-2-0-reference)):</span></span>

    ```xml
    <Asset Type="Moq.4.0.10827.nupkg" d:Source="File" Path="Packages\Moq.4.0.10827.nupkg" d:VsixSubPath="Packages" />
    ```

1. <span data-ttu-id="fc05a-138">Notez que vous pouvez distribuer des packages dans la même extension VSIX que vos modèles de projet, ou les placer dans une extension VSIX distincte si c’est plus approprié pour votre scénario.</span><span class="sxs-lookup"><span data-stu-id="fc05a-138">Note that you can deliver packages in the same VSIX as your project templates or you can put them in a separate VSIX if that makes more sense for your scenario.</span></span> <span data-ttu-id="fc05a-139">Ne référencez cependant aucune extension VSIX dont vous n’avez pas le contrôle, car les modifications apportées à cette extension peuvent compromettre le bon fonctionnement de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="fc05a-139">However, do not reference any VSIX over which you do not have control, because changes to that extension could break your template.</span></span>

### <a name="template-package-repository"></a><span data-ttu-id="fc05a-140">Référentiel de packages de modèles</span><span class="sxs-lookup"><span data-stu-id="fc05a-140">Template package repository</span></span>

<span data-ttu-id="fc05a-141">Si vous distribuez un seul modèle de projet/élément et que vous n’avez pas besoin de placer plusieurs modèles dans un package, vous pouvez utiliser une approche plus simple mais plus limitée, qui inclut des packages directement dans le fichier ZIP du modèle de projet/élément :</span><span class="sxs-lookup"><span data-stu-id="fc05a-141">If you are distributing only a single project/item template and do not need to package multiple templates together, you can use a simpler but more limited approach that includes packages directly in the project/item template ZIP file:</span></span>

1. <span data-ttu-id="fc05a-142">Modifiez l’élément `<packages>` dans le fichier `.vstemplate` comme suit :</span><span class="sxs-lookup"><span data-stu-id="fc05a-142">Modify the `<packages>` element in the `.vstemplate` file as follows:</span></span>

    ```xml
    <packages repository="template"">
        <!-- ... -->
    </packages>
    ```

    <span data-ttu-id="fc05a-143">L’attribut `repository` a la valeur `template` et l’attribut `repositoryId` n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="fc05a-143">The `repository` attribute has the value `template` and the `repositoryId` attribute is not required.</span></span>

1. <span data-ttu-id="fc05a-144">Placez les packages dans le dossier racine du fichier ZIP du modèle de projet/élément.</span><span class="sxs-lookup"><span data-stu-id="fc05a-144">Place packages in the root folder of the project/item template ZIP file.</span></span>

<span data-ttu-id="fc05a-145">Notez que l’utilisation de cette approche dans une extension VSIX qui contient plusieurs modèles aboutit à un encombrement inutile quand un ou plusieurs packages sont communs aux modèles.</span><span class="sxs-lookup"><span data-stu-id="fc05a-145">Note that using this approach in a VSIX that contains multiple templates leads to unnecessary bloat when one or more packages are common to the templates.</span></span> <span data-ttu-id="fc05a-146">Dans ce cas, utilisez [l’extension VSIX comme référentiel](#vsix-package-repository), comme décrit dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="fc05a-146">In such cases, use the [VSIX as the repository](#vsix-package-repository) as described in the previous section.</span></span>

### <a name="registry-specified-folder-path"></a><span data-ttu-id="fc05a-147">Chemin de dossier spécifié dans le Registre</span><span class="sxs-lookup"><span data-stu-id="fc05a-147">Registry-specified folder path</span></span>

<span data-ttu-id="fc05a-148">Les SDK qui sont installés avec un fichier MSI peuvent installer des packages NuGet directement sur l’ordinateur du développeur.</span><span class="sxs-lookup"><span data-stu-id="fc05a-148">SDKs that are installed using an MSI can install NuGet packages directly on the developer's machine.</span></span> <span data-ttu-id="fc05a-149">Ceci les rend immédiatement disponibles quand un modèle de projet ou un élément est utilisé, au lieu d’obliger à les extraire à ce moment-là.</span><span class="sxs-lookup"><span data-stu-id="fc05a-149">This makes them immediately available when a project or item template is used, rather than having to extract them during that time.</span></span> <span data-ttu-id="fc05a-150">Les modèles ASP.NET utilisent cette approche.</span><span class="sxs-lookup"><span data-stu-id="fc05a-150">ASP.NET templates use this approach.</span></span>

1. <span data-ttu-id="fc05a-151">Faites en sorte que le fichier MSI installe les packages sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="fc05a-151">Have the MSI install packages to the machine.</span></span> <span data-ttu-id="fc05a-152">Vous pouvez installer seulement les fichiers `.nupkg`, ou vous pouvez les installer en même temps que le contenu développé, ce qui vous épargne une étape supplémentaire quand le modèle est utilisé.</span><span class="sxs-lookup"><span data-stu-id="fc05a-152">You can install only the `.nupkg` files, or you can install those along with the expanded contents, which saves an additional step when the template is used.</span></span> <span data-ttu-id="fc05a-153">Dans ce cas, suivez la structure de dossiers standard de NuGet, où les fichiers `.nupkg` se trouvent dans le dossier racine, chaque package ayant un sous-dossier dont le nom correspond à la paire ID/version.</span><span class="sxs-lookup"><span data-stu-id="fc05a-153">In this case, follow NuGet's standard folder structure wherein the `.nupkg` files are in the root folder, and then each package has a subfolder with the id/version pair as the subfolder name.</span></span>

1. <span data-ttu-id="fc05a-154">Écrivez une clé de Registre pour identifier l’emplacement du package :</span><span class="sxs-lookup"><span data-stu-id="fc05a-154">Write a registry key to identify the package location:</span></span>

    - <span data-ttu-id="fc05a-155">Emplacement de la clé : une clé `HKEY_LOCAL_MACHINE\SOFTWARE[\Wow6432Node]\NuGet\Repository` à l’échelle de l’ordinateur. S’il s’agit de modèles et de packages installés au niveau de l’utilisateur, vous pouvez également utiliser `HKEY_CURRENT_USER\SOFTWARE\NuGet\Repository`</span><span class="sxs-lookup"><span data-stu-id="fc05a-155">Key location: Either the machine-wide `HKEY_LOCAL_MACHINE\SOFTWARE[\Wow6432Node]\NuGet\Repository` or if it's per-user installed templates and packages, alternatively use `HKEY_CURRENT_USER\SOFTWARE\NuGet\Repository`</span></span>
    - <span data-ttu-id="fc05a-156">Nom de la clé : utilisez un nom qui est unique pour vous.</span><span class="sxs-lookup"><span data-stu-id="fc05a-156">Key name: use a name that's unique to you.</span></span> <span data-ttu-id="fc05a-157">Par exemple, les modèles ASP.NET MVC 4 pour Visual Studio 2012 utilisent `AspNetMvc4VS11`.</span><span class="sxs-lookup"><span data-stu-id="fc05a-157">For example, the ASP.NET MVC 4 templates for VS 2012 use `AspNetMvc4VS11`.</span></span>
    - <span data-ttu-id="fc05a-158">Valeurs : le chemin complet du dossier des packages.</span><span class="sxs-lookup"><span data-stu-id="fc05a-158">Values: the full path to the packages folder.</span></span>

1. <span data-ttu-id="fc05a-159">Dans l’élément `<packages>` du fichier `.vstemplate`, ajoutez l’attribut `repository="registry"` et spécifiez le nom de votre clé de Registre dans l’attribut `keyName`.</span><span class="sxs-lookup"><span data-stu-id="fc05a-159">In the `<packages>` element in the `.vstemplate` file, add the attribute `repository="registry"` and specify your registry key name in the `keyName` attribute.</span></span>

    - <span data-ttu-id="fc05a-160">Si vous avez déjà décompressé vos packages, utilisez l’attribut `isPreunzipped="true"`.</span><span class="sxs-lookup"><span data-stu-id="fc05a-160">If you have pre-unzipped your packages, use the `isPreunzipped="true"` attribute.</span></span>
    - <span data-ttu-id="fc05a-161">*(NuGet 3.2 +)*  Si vous voulez forcer une build au moment du design à la fin de l’installation du package, ajoutez l’attribut `forceDesignTimeBuild="true"`.</span><span class="sxs-lookup"><span data-stu-id="fc05a-161">*(NuGet 3.2+)* If you want to force a design-time build at the end of package installation, add the `forceDesignTimeBuild="true"` attribute.</span></span>
    - <span data-ttu-id="fc05a-162">À titre d’optimisation, ajoutez `skipAssemblyReferences="true"`, car le modèle lui-même inclut déjà les références nécessaires.</span><span class="sxs-lookup"><span data-stu-id="fc05a-162">As an optimization, add `skipAssemblyReferences="true"` because the template itself already includes the necessary references.</span></span>

        ```xml
        <packages repository="registry" keyName="AspNetMvc4VS11" isPreunzipped="true">
            <package id="EntityFramework" version="5.0.0" skipAssemblyReferences="true" />
            <-- ... -->
        </packages>
        ```

## <a name="best-practices"></a><span data-ttu-id="fc05a-163">Bonnes pratiques</span><span class="sxs-lookup"><span data-stu-id="fc05a-163">Best Practices</span></span>

1. <span data-ttu-id="fc05a-164">Déclarez une dépendance sur l’extension VSIX NuGet en ajoutant une référence à celle-ci dans votre manifeste VSIX :</span><span class="sxs-lookup"><span data-stu-id="fc05a-164">Declare a dependency on the NuGet VSIX by adding a reference to it in your VSIX manifest:</span></span>

    ```xml
    <Reference Id="NuPackToolsVsix.Microsoft.67e54e40-0ae3-42c5-a949-fddf5739e7a5" MinVersion="1.7.30402.9028">
        <Name>NuGet Package Manager</Name>
        <MoreInfoUrl>http://docs.microsoft.com/nuget/</MoreInfoUrl>
    </Reference>
    <!-- ... -->
    ```

1. <span data-ttu-id="fc05a-165">Forcez l’enregistrement des modèles de projet/élément lors de la création en ajoutant [`<PromptForSaveOnCreation>true</PromptForSaveOnCreation>`](/visualstudio/extensibility/promptforsaveoncreation-element-visual-studio-templates) dans le fichier `.vstemplate`.</span><span class="sxs-lookup"><span data-stu-id="fc05a-165">Require project/item templates to be saved on creation by including [`<PromptForSaveOnCreation>true</PromptForSaveOnCreation>`](/visualstudio/extensibility/promptforsaveoncreation-element-visual-studio-templates) in the `.vstemplate` file.</span></span>

1. <span data-ttu-id="fc05a-166">Les modèles n’incluent pas de fichier `packages.config` ou `project.json` ; n’incluez aucune référence ou contenu qui serait ajouté lors de l’installation des packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="fc05a-166">Templates do not include a `packages.config` or `project.json` file, and do not include or any references or content that would be added when NuGet packages are installed.</span></span>