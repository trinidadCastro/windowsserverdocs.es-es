---
title: Configurar el cifrado para una red Virtual
description: Este tema proporciona información acerca del cifrado de red Virtual para Software definido a redes en Windows Server
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 7d1de535e7758793e5ddaa7eeefada0aa55d3328
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurar el cifrado para una subred Virtual

>Se aplica a: Windows Server

Este tema contiene las siguientes secciones que se describen los pasos necesarios para habilitar el cifrado en una red Virtual.

- [Crear el certificado de cifrado](#bkmk_Certificate)
- [Crear la credencial de certificado](#bkmk_credential)
- [Configurar una red Virtual para el cifrado](#bkmk_vnet)

## <a name="bkmk_Certificate"></a>Crear el certificado de cifrado
Un certificado de cifrado se requiere para instalarse en cada host donde se puede usar el cifrado.  Puedes usar el mismo certificado para todos los inquilinos o generar un único certificado por inquilino si es necesario.

Paso 1: Generar el certificado

    $subjectName = "EncryptedVirtualNetworks"
    $cryptographicProviderName = "Microsoft Base Cryptographic Provider v1.0";
    [int] $privateKeyLength = 1024;
    $sslServerOidString = "1.3.6.1.5.5.7.3.1";
    $sslClientOidString = "1.3.6.1.5.5.7.3.2";
    [int] $validityPeriodInYear = 5;

    $name = new-object -com "X509Enrollment.CX500DistinguishedName.1"
    $name.Encode("CN=" + $SubjectName, 0)

    #Generate Key
    $key = new-object -com "X509Enrollment.CX509PrivateKey.1"
    $key.ProviderName = $cryptographicProviderName
    $key.KeySpec = 1 #X509KeySpec.XCN_AT_KEYEXCHANGE
    $key.Length = $privateKeyLength
    $key.MachineContext = 1
    $key.ExportPolicy = 0x2 #X509PrivateKeyExportFlags.XCN_NCRYPT_ALLOW_EXPORT_FLAG 
    $key.Create()

    #Configure Eku
    $serverauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $serverauthoid.InitializeFromValue($sslServerOidString)
    $clientauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $clientauthoid.InitializeFromValue($sslClientOidString)
    $ekuoids = new-object -com "X509Enrollment.CObjectIds.1"
    $ekuoids.add($serverauthoid)
    $ekuoids.add($clientauthoid)
    $ekuext = new-object -com "X509Enrollment.CX509ExtensionEnhancedKeyUsage.1"
    $ekuext.InitializeEncode($ekuoids)

    # Set the hash algorithm to sha512 instead of the default sha1
    $hashAlgorithmObject = New-Object -ComObject X509Enrollment.CObjectId
    $hashAlgorithmObject.InitializeFromAlgorithmName( $ObjectIdGroupId.XCN_CRYPT_HASH_ALG_OID_GROUP_ID, $ObjectIdPublicKeyFlags.XCN_CRYPT_OID_INFO_PUBKEY_ANY, $AlgorithmFlags.AlgorithmFlagsNone, "SHA512")


    #Request Certificate
    $cert = new-object -com "X509Enrollment.CX509CertificateRequestCertificate.1"

    $cert.InitializeFromPrivateKey(2, $key, "")
    $cert.Subject = $name
    $cert.Issuer = $cert.Subject
    $cert.NotBefore = (get-date).ToUniversalTime()
    $cert.NotAfter = $cert.NotBefore.AddYears($validityPeriodInYear);
    $cert.X509Extensions.Add($ekuext)
    $cert.HashAlgorithm = $hashAlgorithmObject
    $cert.Encode()

    $enrollment = new-object -com "X509Enrollment.CX509Enrollment.1"
    $enrollment.InitializeFromRequest($cert)
    $certdata = $enrollment.CreateRequest(0)
    $enrollment.InstallResponse(2, $certdata, 0, "")

Después de ejecutar el ejemplo anterior, verás un nuevo certificado en el almacén My del equipo donde se ejecutó el script:

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

Paso 2: Exportar el certificado a un archivo sin que tendrá dos copias del certificado, uno con la clave privada y.

    $subjectName = "EncryptedVirtualNetworks"
    $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
    [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
    Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert

Después de ejecutar el ejemplo anterior ahora tiene dos archivos de certificado.  Estos deben instalarse en cada uno de los hosts de hyper-v.

    PS C:\> dir c:\$subjectname.*


        Directory: C:\


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
    -a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx

Paso 3: Instalar en un host de Hyper-V

    $server = "Server01"

    $subjectname = "EncryptedVirtualNetworks"
    copy c:\$SubjectName.* \\$server\c$
    invoke-command -computername $server -ArgumentList $subjectname,"secret" {
        param (
            [string] $SubjectName,
            [string] $Secret
        )
        $certFullPath = "c:\$SubjectName.cer"

        # create a representation of the certificate file
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath){$_}

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("Root", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        $certFullPath = "c:\$SubjectName.pfx"
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath, $Secret, "MachineKeySet,PersistKeySet")

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("My", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        # Important: Remove the certficate files when finished
        remove-item C:\$SubjectName.cer
        remove-item C:\$SubjectName.pfx
    }    

Repita para cada servidor en el entorno.  Ahora debería tener un certificado instalado en la raíz y en Mi almacén de cada host de Hyper-V

Puedes comprobar la instalación del certificado comprobando el contenido de la mi y almacenes de certificados del elemento raíz:

    PS C:\> enter-pssession Server1

    [Server1]: PS C:\> get-childitem cert://localmachine/my,cert://localmachine/root | ? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

    Thumbprint                                Subject
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

Toma nota de la huella digital que necesitarás para crear el objeto de credenciales de certificado en el controlador de red.

## <a name="bkmk_Certificate"></a>Crear la credencial de certificado

Después de que el certificado se instalará correctamente en cada uno de los hosts de Hyper-V conectados al controlador de red, puedes configurar el controlador de red para usarla.

Deberás crear un objeto de credenciales que contiene la huella digital de certificado.  Tendrás que hacerlo desde un equipo en el que tiene los módulos de powershell de controlador de red instalados.

    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  
    
    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller
    
    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

Puedes volver a usar esta credencial para cada cifrado netwokr virtual, o puede implementar y usar un único certificado para cada inquilino.

## <a name="bkmk_Certificate"></a>Configurar una red Virtual para el cifrado

Este paso se supone que ya has creado un nombre de red virtual "Mi red" contiene al menos una subred virtual.  Para obtener información sobre la creación de redes virtuales, vea [crear, eliminar o redes virtuales del inquilino de actualización](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

Paso 1: Recuperar los objetos de red Virtual y credenciales desde el controlador de red

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork"
    $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

Paso 2: Agregar una referencia a la credencial de certificado y habilitar el cifrado de subredes individuales.

    $vnet.properties.EncryptionCredential = $certcred

    # Replace the Subnets index with the value corresponding to the subnet you want encrypted.  
    # Repeat for each subnet where encryption is needed
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

Paso 3: Coloca el objeto de red Virtual actualizado en el controlador de red

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force

Una vez completado, se necesita ningún paso adicional.  Todas las máquinas virtuales que está conectada actualmente y cualquier VM que más adelante está conectado a la subred anterior tendrán el tráfico que se cifran automáticamente cuando se comunica con otra máquina virtual en la misma subred.


