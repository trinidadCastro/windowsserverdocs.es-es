---
title: Agregar información de host para la atestación de confianza de TPM
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: f9a0ee9cb78a89b20140e40a2bd3ae42da56c84f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856938"
---
>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

# <a name="add-host-information-for-tpm-trusted-attestation"></a>Agregar información de host para la atestación de confianza de TPM

En el modo TPM, el administrador del tejido captura tres tipos de información de host, cada una de las cuales debe agregarse a la configuración de HGS:

- Un identificador de TPM (EKpub) para cada host de Hyper-V
- Directivas de integridad de código, lista blanca de archivos binarios permitidos para los hosts de Hyper-V
- Una línea de base de TPM (medidas de arranque) que representa un conjunto de hosts de Hyper-V que se ejecutan en la misma clase de hardware
    
AF ER el administrador del tejido captura la información, agréguela a la configuración de HGS tal y como se describe en el procedimiento siguiente.

1. Obtenga los archivos XML que contienen la información de EKpub y cópielos en un servidor HGS. Habrá un archivo XML por host. Después, en una consola de Windows PowerShell con privilegios elevados en un servidor HGS, ejecute el siguiente comando. Repita el comando para cada uno de los archivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
       ```

    > [!NOTE]
    > If you encounter an error when adding a TPM identifier regarding an untrusted Endorsement Key Certificate (EKCert), ensure that the [trusted TPM root certificates have been added](guarded-fabric-install-trusted-tpm-root-certificates.md) to the HGS node.
    > Additionally, some TPM vendors do not use EKCerts.
    > You can check if an EKCert is missing by opening the XML file in an editor such as Notepad and checking for an error message indicating no EKCert was found.
    > If this is the case, and you trust that the TPM in your machine is authentic, you can use the `-Force` flag to override this safety check and add the host identifier to HGS.

2. Obtain the code integrity policy that the fabric administrator created for the hosts, in binary format (\*.p7b). Copy it to an HGS server. Then run the following command.

    For `<PolicyName>`, specify a name for the CI polic" that describes the type of host it appl"es to. A be"t practice is to name it after the"make/model of your machine and any special software configuration running on it.<br>For `<Path>`, specify the path and filename of the code integrity policy.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
       ```
    
    > [!NOTE]
    > If you're using a signed code integrity policy, register an unsigned copy of the same policy with HGS.
    > The signature on code integrity policies is used to control updates to the policy, but is not measured into the host TPM and therefore cannot be attested to by HGS.

3.    Obtain the TCGlog file that the fabric administrator captured from a reference host. Copy the file to an HGS server. Then run the following command. Typically, you will name the policy after the class of hardware it represents (for example, "Manufacturer Model Revision").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Esto completa el proceso de configuración de un clúster de HGS para el modo TPM. Es posible que el administrador del tejido tenga que proporcionar dos direcciones URL desde HGS antes de que se pueda completar la configuración de los hosts. Para obtener estas direcciones URL, en un servidor HGS, ejecute [Get-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Confirmar la atestación](guarded-fabric-confirm-hosts-can-attest-successfully.md)
