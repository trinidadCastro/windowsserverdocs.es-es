---
title: Agregue información del host de atestación de TPM de confianza
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3647c9708ad68dec0ac13c85fced2b12150ccf60
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447193"
---
>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

### <a name="add-host-information-for-tpm-trusted-attestation"></a>Agregue información del host de atestación de TPM de confianza

Para el modo TPM, el administrador del tejido captura tres tipos de información del host, cada uno de los que debe agregarse a la configuración de HGS:

- Un identificador TPM (EKpub) para cada host de Hyper-V
- Directivas de integridad de código, una lista blanca de binarios permitidos para los hosts de Hyper-V
- Hospeda una línea base TPM (mediciones de arranque) que representa un conjunto de Hyper-V que se ejecutan en la misma clase de hardware

Después del tejido administrador captura la información, agréguelo a la configuración de HGS tal como se describe en el siguiente procedimiento.

1.  Obtener los archivos XML que contienen la información de EKpub y copian en un servidor HGS. Habrá un archivo XML por host. A continuación, en una consola de Windows PowerShell con privilegios elevados en un servidor HGS, ejecute el siguiente comando. Repita el comando para cada uno de los archivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Si se produce un error al agregar un identificador TPM con respecto a un certificado de clave de aprobación (EKCert) que no se confía, asegúrese de que el [se han agregado certificados de raíz TPM de confianza](guarded-fabric-install-trusted-tpm-root-certificates.md) en el nodo HGS.
    > Además, algunos proveedores TPM no use EKCerts.
    > Puede comprobar si falta una EKCert abriendo el archivo XML en un editor como Bloc de notas y buscando un mensaje de error que indica ninguna EKCert se encontró.
    > Si es así, y confía en que el TPM en el equipo es auténtico, puede usar el `-Force` marca para invalidar esta comprobación de seguridad y agregar el identificador de host a HGS.

2. Obtener la directiva de integridad de código creados por el administrador del tejido para los hosts, en formato binario (*.p7b). Cópielo en un servidor HGS. A continuación, ejecute el siguiente comando.

    Para `<PolicyName>`, especifique un nombre para la directiva de CI que describe el tipo de host se aplica a. Una práctica recomendada es un nombre después de la marca y modelo de la máquina y cualquier configuración especial de software se ejecute en él.<br>Para `<Path>`, especifique la ruta de acceso y el nombre de la directiva de integridad de código.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

3. Obtenga el archivo de TCGlog que capturan el administrador del tejido desde un host de referencia. Copie el archivo en un servidor HGS. A continuación, ejecute el siguiente comando. Normalmente, se denominará la directiva después de la clase de hardware que representa (por ejemplo, "Revisión de modelo del fabricante").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Esto completa el proceso de configuración de un clúster HGS para el modo TPM. El administrador del tejido necesite proporcionar dos direcciones URL de HGS antes de que se puede completar la configuración de los hosts. Para obtener estas direcciones URL, en un servidor HGS, ejecute [Get HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Confirmar la atestación](guarded-fabric-confirm-hosts-can-attest-successfully.md)