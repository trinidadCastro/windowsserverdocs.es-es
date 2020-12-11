---
description: Más información acerca de cómo crear una clave de host y agregarla a HGS
title: Crear una clave de host y agregarla a HGS
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 0e6f7374c47204d86674b75ad453ee1067d09e82
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049803"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>Crear una clave de host y agregarla a HGS

>Se aplica a: Windows Server 2019

En este tema se describe cómo preparar hosts de Hyper-V para que se conviertan en hosts protegidos mediante la atestación de clave de host (modo de clave). Creará un par de claves de host (o usará un certificado existente) y agregará la mitad pública de la clave a HGS.

## <a name="create-a-host-key"></a>Crear una clave de host

1. Instale Windows Server 2019 en el equipo host de Hyper-V.
2. Instale las características de compatibilidad con Hyper-V y protección de host de Hyper-V:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

3. Generar una clave de host automáticamente o seleccionar un certificado existente. Si usa un certificado personalizado, debe tener al menos una clave RSA de 2048 bits, un EKU de autenticación de cliente y el uso de la clave de firma digital.

    ```powershell
    Set-HgsClientHostKey
    ```

    También puede especificar una huella digital si desea utilizar su propio certificado.
    Esto puede ser útil si desea compartir un certificado entre varios equipos o usar un certificado enlazado a un TPM o a un HSM. A continuación se muestra un ejemplo de creación de un certificado enlazado a TPM (que impide que se robe y se use la clave privada en otra máquina y que solo requiera un TPM 1,2):

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject "Host Key Attestation ($env:computername)" -Provider "Microsoft Platform Crypto Provider"
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4. Obtenga la mitad pública de la clave que se va a proporcionar al servidor HGS. Puede usar el siguiente cmdlet de o, si tiene el certificado almacenado en otro lugar, proporcione un. cer que contenga la mitad pública de la clave. Tenga en cuenta que solo almacenamos y validamos la clave pública en HGS; no se conserva ninguna información de certificado ni se valida la cadena de certificados o la fecha de expiración.

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5. Copie el archivo. cer en el servidor de HGS.

## <a name="add-the-host-key-to-the-attestation-service"></a>Agregar la clave de host al servicio de atestación

Este paso se realiza en el servidor HGS y permite al host ejecutar máquinas virtuales blindadas. Se recomienda establecer el nombre en el FQDN o el identificador de recursos del equipo host, de modo que pueda hacer referencia fácilmente al host en el que está instalada la clave.

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
```

## <a name="next-step"></a>Paso siguiente

- [Confirmación de que los host pueden atestiguar correctamente](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="additional-references"></a>Referencias adicionales

- [Implementación del Servicio de protección de host para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-deploying-hgs-overview.md)
