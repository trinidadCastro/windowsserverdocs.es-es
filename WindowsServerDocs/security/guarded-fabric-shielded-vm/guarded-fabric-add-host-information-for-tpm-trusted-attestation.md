---
title: Agregar información de host para la atestación de confianza de TPM
description: Información sobre cómo agregar información de host para la atestación de confianza de TPM.
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 06/21/2019
ms.openlocfilehash: 6a199792408bc9086186308758cb5d85d21d6c41
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865704"
---
# <a name="add-host-information-for-tpm-trusted-attestation"></a>Agregar información de host para la atestación de confianza de TPM

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En el modo TPM, el administrador del tejido captura tres tipos de información de host, cada una de las cuales debe agregarse a la configuración de HGS:

- Un identificador de TPM (EKpub) para cada host de Hyper-V
- Directivas de integridad de código, un permitidos de archivos binarios permitidos para los hosts de Hyper-V
- Una línea de base de TPM (medidas de arranque) que representa un conjunto de hosts de Hyper-V que se ejecutan en la misma clase de hardware

Una vez que el administrador del tejido Capture la información, agréguela a la configuración de HGS tal y como se describe en el procedimiento siguiente.

1. Obtenga los archivos XML que contienen la información de EKpub y cópielos en un servidor HGS. Habrá un archivo XML por host. Después, en una consola de Windows PowerShell con privilegios elevados en un servidor HGS, ejecute el siguiente comando. Repita el comando para cada uno de los archivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Si se produce un error al agregar un identificador de TPM con respecto a un certificado de clave de aprobación que no es de confianza (EKCert), asegúrese de que los [certificados raíz del TPM de confianza se han agregado](guarded-fabric-install-trusted-tpm-root-certificates.md) al nodo HGS.
    > Además, algunos proveedores de TPM no usan EKCerts.
    > Puede comprobar si falta un EKCert abriendo el archivo XML en un editor como el Bloc de notas y comprobando si hay un mensaje de error que indica que no se encontró EKCert.
    > Si este es el caso y confía en que el TPM del equipo es auténtico, puede usar la `-Force` marca para invalidar esta comprobación de seguridad y agregar el identificador de host a HGS.

2. Obtener la Directiva de integridad de código creada por el administrador de tejido para los hosts, en formato binario ( \* . p7b). Cópielo en un servidor HGS. Luego, ejecute el siguiente comando.

    En `<PolicyName>` , especifique un nombre para la Directiva de CI que describe el tipo de host al que se aplica. Un procedimiento recomendado consiste en asignarle un nombre después de la marca y el modelo de su equipo y cualquier configuración de software especial que se ejecute en él.<br>Para `<Path>` , especifique la ruta de acceso y el nombre de archivo de la Directiva de integridad de código.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

    > [!NOTE]
    > Si usa una directiva de integridad de código firmada, registre una copia sin firmar de la misma directiva con HGS.
    > La firma de las directivas de integridad de código se utiliza para controlar las actualizaciones de la Directiva, pero no se mide en el TPM del host y, por tanto, no se puede atestiguar en el HGS.

3. Obtenga el archivo de registro de TCG que el administrador del tejido capturó de un host de referencia. Copie el archivo en un servidor HGS. Luego, ejecute el siguiente comando. Normalmente, asignará un nombre a la Directiva después de la clase de hardware que representa (por ejemplo, "revisión del modelo del fabricante").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Esto completa el proceso de configuración de un clúster de HGS para el modo TPM. Es posible que el administrador del tejido tenga que proporcionar dos direcciones URL desde HGS antes de que se pueda completar la configuración de los hosts. Para obtener estas direcciones URL, en un servidor HGS, ejecute [Get-HgsServer](/powershell/module/hgsserver/get-hgsserver).

## <a name="next-step"></a>Paso siguiente

> [Confirmar la atestación](guarded-fabric-confirm-hosts-can-attest-successfully.md)
