---
title: Référence des Packages NuGet de signé
description: Configuration requise pour la signature du package NuGet.
author: rido-min
ms.author: rido-min
manager: unnir
ms.date: 04/24/2018
ms.topic: reference
ms.reviewer: ananguar
ms.openlocfilehash: 751a8ff14bdc3a647985da4f908ad1a0fd0def9a
ms.sourcegitcommit: 5fcd6d664749aa720359104ef7a66d38aeecadc2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="signed-packages"></a><span data-ttu-id="6e3f9-103">Packages signés</span><span class="sxs-lookup"><span data-stu-id="6e3f9-103">Signed packages</span></span>

<span data-ttu-id="6e3f9-104">*NuGet 4.6.0+ et Visual Studio 2017 15,6 et versions ultérieures*</span><span class="sxs-lookup"><span data-stu-id="6e3f9-104">*NuGet 4.6.0+ and Visual Studio 2017 version 15.6 and later*</span></span>

<span data-ttu-id="6e3f9-105">Les packages NuGet peuvent inclure une signature numérique qui assure la protection de contenu falsifié.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-105">NuGet packages can include a digital signature that provides protection against tampered content.</span></span> <span data-ttu-id="6e3f9-106">Cette signature est générée à partir d’un certificat X.509 qui ajoute également des preuves d’authenticité à l’origine réelle du package.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-106">This signature is produced from an X.509 certificate that also adds authenticity proofs to the actual origin of the package.</span></span>

<span data-ttu-id="6e3f9-107">Les packages signés fournissent la validation de bout en bout pour les plus fortes.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-107">Signed packages provide the strongest end-to-end validation.</span></span> <span data-ttu-id="6e3f9-108">Une signature de l’auteur garantit que le package n’a pas été modifié depuis l’auteur de signature du package, sans quel que soit à partir de laquelle référentiel ou que le package est remis (méthode) de transport.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-108">An author signature guarantees that the package has not been modified since the author signed the package, no matter from which repository or what transport method the package is delivered.</span></span>

<span data-ttu-id="6e3f9-109">Les consommateurs qui exigent un environnement verrouillé peuvent nécessiter des packages signés avec un certificat de l’auteur spécifique.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-109">Consumers who demand a locked-down environment can require packages signed with a specific author certificate.</span></span>

<span data-ttu-id="6e3f9-110">En outre, signées par l’auteur de packages fournissent un mécanisme d’authentification supplémentaire pour le pipeline de publication nuget.org car le certificat de signature doit être enregistré à l’avance.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-110">Additionally, author-signed packages provide an extra authentication mechanism to the nuget.org publishing pipeline because the signing certificate must be registered ahead of time.</span></span>

<span data-ttu-id="6e3f9-111">Pour plus d’informations sur la création d’un package signé, consultez [de signature de Packages](../create-packages/Sign-a-package.md) et [commande de connexion nuget](../tools/cli-ref-sign.md).</span><span class="sxs-lookup"><span data-stu-id="6e3f9-111">For details on creating a signed package, see [Signing Packages](../create-packages/Sign-a-package.md) and the [nuget sign command](../tools/cli-ref-sign.md).</span></span>

> [!Important]
> <span data-ttu-id="6e3f9-112">actuellement, NuGet.org n’accepte pas les packages signés.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-112">nuget.org does not presently accept signed packages.</span></span> <span data-ttu-id="6e3f9-113">Vous pouvez signer des packages pour les publier dans des flux personnalisés.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-113">You can sign packages for publishing to custom feeds.</span></span>

> [!Important]
> <span data-ttu-id="6e3f9-114">La signature du package est actuellement pris en charge uniquement sur Windows à l’aide de nuget.exe.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-114">Package signing is currently supported only when using nuget.exe on Windows.</span></span> <span data-ttu-id="6e3f9-115">Vérification des packages signés est actuellement pris en charge uniquement sur Windows à l’aide de nuget.exe ou Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-115">Verification of signed packages is currently supported only when using nuget.exe or Visual Studio on Windows.</span></span>

## <a name="certificate-requirements"></a><span data-ttu-id="6e3f9-116">Conditions de certificat</span><span class="sxs-lookup"><span data-stu-id="6e3f9-116">Certificate requirements</span></span>

<span data-ttu-id="6e3f9-117">La signature du package nécessite un code de signature de certificat, qui est un type particulier de certificat valide pour le `id-kp-codeSigning` objectif [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)].</span><span class="sxs-lookup"><span data-stu-id="6e3f9-117">Package signing requires a code signing certificate, which is a special type of certificate that is valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)].</span></span> <span data-ttu-id="6e3f9-118">En outre, le certificat doit avoir une publique longueur de clé RSA ou plus de 2 048 bits.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-118">Additionally, the certificate must have an RSA public key length of 2048 bits or higher.</span></span>

## <a name="get-a-code-signing-certificate"></a><span data-ttu-id="6e3f9-119">Obtenir un certificat de signature de code</span><span class="sxs-lookup"><span data-stu-id="6e3f9-119">Get a code signing certificate</span></span>

<span data-ttu-id="6e3f9-120">Les certificats valides peuvent être obtenues à partir des autorités de certification publique comme :</span><span class="sxs-lookup"><span data-stu-id="6e3f9-120">Valid certificates may be obtained from public certificate authorities like:</span></span>

- [<span data-ttu-id="6e3f9-121">Symantec</span><span class="sxs-lookup"><span data-stu-id="6e3f9-121">Symantec</span></span>](https://trustcenter.websecurity.symantec.com/process/trust/productOptions?productType=SoftwareValidationClass3)
- [<span data-ttu-id="6e3f9-122">DigiCert</span><span class="sxs-lookup"><span data-stu-id="6e3f9-122">DigiCert</span></span>](https://www.digicert.com/code-signing/)
- [<span data-ttu-id="6e3f9-123">Go Daddy</span><span class="sxs-lookup"><span data-stu-id="6e3f9-123">Go Daddy</span></span>](https://www.godaddy.com/web-security/code-signing-certificate)
- [<span data-ttu-id="6e3f9-124">Signe global</span><span class="sxs-lookup"><span data-stu-id="6e3f9-124">Global Sign</span></span>](https://www.globalsign.com/en/code-signing-certificate/)
- [<span data-ttu-id="6e3f9-125">Comodo</span><span class="sxs-lookup"><span data-stu-id="6e3f9-125">Comodo</span></span>](https://www.comodo.com/e-commerce/code-signing/code-signing-certificate.php)
- [<span data-ttu-id="6e3f9-126">Certum</span><span class="sxs-lookup"><span data-stu-id="6e3f9-126">Certum</span></span>](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml) 

<span data-ttu-id="6e3f9-127">La liste complète des autorités de certification approuvées par Windows peut être obtenue à partir de [ http://aka.ms/trustcertpartners ](http://aka.ms/trustcertpartners).</span><span class="sxs-lookup"><span data-stu-id="6e3f9-127">The complete list of certification authorities trusted by Windows can be obtained from [http://aka.ms/trustcertpartners](http://aka.ms/trustcertpartners).</span></span>

## <a name="create-a-test-certificate"></a><span data-ttu-id="6e3f9-128">Créer un certificat de test</span><span class="sxs-lookup"><span data-stu-id="6e3f9-128">Create a test certificate</span></span>

<span data-ttu-id="6e3f9-129">Vous pouvez utiliser les certificats auto-émis à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-129">You can use self-issued certificates for testing purposes.</span></span> <span data-ttu-id="6e3f9-130">Pour créer un certificat auto-émis, utilisez le [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pkiclient/new-selfsignedcertificate) commande PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-130">To create a self-issued certificate, use the [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pkiclient/new-selfsignedcertificate) PowerShell command.</span></span>

```ps
New-SelfSignedCertificate -Subject "CN=NuGet Test Developer, OU=Use for testing purposes ONLY" `
                          -FriendlyName "NuGetTestDeveloper" `
                          -Type CodeSigning `
                          -KeyUsage DigitalSignature `
                          -KeyLength 2048 `
                          -KeyAlgorithm RSA `
                          -HashAlgorithm SHA256 `
                          -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                          -CertStoreLocation "Cert:\CurrentUser\My" 
```

<span data-ttu-id="6e3f9-131">Cette commande crée un certificat de test disponible dans le magasin de certificats personnel de l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-131">This command creates a testing certificate available in the current user's personal certificate store.</span></span> <span data-ttu-id="6e3f9-132">Vous pouvez ouvrir le magasin de certificats en exécutant `certmgr.msc` pour afficher le certificat nouvellement créé.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-132">You can open the certificate store by running `certmgr.msc` to see the newly created certificate.</span></span>

## <a name="timestamp-requirements"></a><span data-ttu-id="6e3f9-133">Configuration requise d’horodatage</span><span class="sxs-lookup"><span data-stu-id="6e3f9-133">Timestamp requirements</span></span>

<span data-ttu-id="6e3f9-134">Les packages signés doivent inclure un horodatage RFC 3161 pour garantir la validité de signature au-delà de la période de validité du certificat de signature de packages.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-134">Signed packages should include an RFC 3161 timestamp to ensure signature validity beyond the package signing certificate's validity period.</span></span> <span data-ttu-id="6e3f9-135">Le certificat utilisé pour signer l’horodateur doit être valide pour le `id-kp-timeStamping` objectif [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)].</span><span class="sxs-lookup"><span data-stu-id="6e3f9-135">The certificate used to sign the timestamp must be valid for the `id-kp-timeStamping` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)].</span></span> <span data-ttu-id="6e3f9-136">En outre, le certificat doit avoir une publique longueur de clé RSA ou plus de 2 048 bits.</span><span class="sxs-lookup"><span data-stu-id="6e3f9-136">Additionally, the certificate must have an RSA public key length of 2048 bits or higher.</span></span>

<span data-ttu-id="6e3f9-137">Vous trouverez des informations techniques dans le [les spécifications techniques de signature du package](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) (GitHub).</span><span class="sxs-lookup"><span data-stu-id="6e3f9-137">Additional technical details can be found in the [package signature technical specs](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) (GitHub).</span></span>