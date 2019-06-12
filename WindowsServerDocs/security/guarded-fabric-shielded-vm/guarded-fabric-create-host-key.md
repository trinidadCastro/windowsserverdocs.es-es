---
title: Crear una clave de host y su incorporación a HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0526831fb0648e7f8f6fb1a081180f2e2aa9f09f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447489"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>Crear una clave de host y su incorporación a HGS

>Se aplica a: Windows Server 2019


En este tema se explica cómo preparar los hosts de Hyper-V para convertirse en hosts protegidos con la atestación de clave de host (modo de clave). Deberá crear un par de claves de host (o usar un certificado existente) y agregue la mitad de la clave pública para HGS.

## <a name="create-a-host-key"></a>Crear una clave de host

1.  En el equipo host de Hyper-V, instale Windows Server 2019.
2.  Instalar las características de Hyper-V y compatibilidad de Hyper-V de guardián de Host:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  Generar automáticamente una clave de host, o seleccione un certificado existente. Si utiliza un certificado personalizado, debe tener al menos una clave RSA de 2048 bits, el EKU de autenticación de cliente y uso de claves de firma Digital.

    ```powershell
    Set-HgsClientHostKey
    ```

    Como alternativa, puede especificar una huella digital si desea usar su propio certificado. 
    Esto puede ser útil si desea compartir un certificado en varias máquinas, o use un certificado enlazado a un HSM o de un TPM. Este es un ejemplo de cómo crear un certificado enlazado a TPM (que impide tener la clave privada robado y usarse en otra máquina y requiere un TPM 1.2):

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  Obtener la mitad pública de la clave para proporcionar a un servidor HGS. Puede usar el siguiente cmdlet o, si tiene el certificado almacenado en otra parte, proporcione un archivo .cer que contiene el público en la mitad de la clave. Tenga en cuenta que solo estamos almacenar y validar la clave pública en HGS; no mantenemos ninguna información del certificado ni verificamos la fecha de cadena o la expiración del certificado.

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  Copie el archivo .cer en el servidor HGS.

## <a name="add-the-host-key-to-the-attestation-service"></a>Agregue la clave de host para el servicio de atestación

Este paso se realiza en el servidor HGS y permite que el host ejecutar máquinas virtuales blindadas. Se recomienda que establezca el nombre para el FQDN o identificador de recurso de la máquina host, por lo que puede hacer referencia fácilmente al host que la clave se instala en.

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Confirmar hosts atestigua correctamente](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>Vea también

- [Implementar el servicio de protección de Host para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-deploying-hgs-overview.md)
