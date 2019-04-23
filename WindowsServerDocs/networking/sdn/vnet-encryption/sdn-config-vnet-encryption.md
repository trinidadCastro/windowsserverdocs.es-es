---
title: Configurar el cifrado de una red Virtual
description: Cifrado de red virtual permite el cifrado de tráfico de red virtual entre las máquinas virtuales que se comunican entre sí dentro de las subredes marcadas como 'El cifrado habilitado'.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 90fb33eb4c4b63fdd5c84bf3ffc2447fd52a809b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845496"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurar el cifrado de una subred Virtual

>Se aplica a: Windows Server

Permite el cifrado de red virtual para el cifrado de tráfico de red virtual entre las máquinas virtuales que se comunican entre sí dentro de las subredes marcadas como 'El cifrado habilitado'. También utiliza la Seguridad de la capa de transporte de datagrama (DTLS) en la subred virtual para cifrar los paquetes. DTLS protege frente a las interceptaciones, alteraciones y falsificaciones realizadas por cualquier persona con acceso a la red física.

Se requiere el cifrado de red virtual:
- Certificados de cifrado en cada uno de los hosts de Hyper-V habilitado SDN.
- Un objeto de credencial en la controladora de red que hacen referencia a la huella digital del certificado.
- La configuración en cada una de las redes virtuales contiene subredes que requieren cifrado.

Una vez que habilite el cifrado en una subred, todo el tráfico de red dentro de esa subred se cifra automáticamente, además de cualquier cifrado de nivel de aplicación que también puede tener lugar.  El tráfico que cruza entre subredes, incluso si marcada como cifrada, se envía sin cifrar automáticamente. Todo el tráfico que cruza el límite de red virtual también se envía sin cifrar.

>[!NOTE]
>Al comunicarse con otra máquina virtual en la misma subred, si su actualmente conectado o conectado en un momento posterior, el tráfico se cifra automáticamente.

>[!TIP]
>Si debe restringir las aplicaciones se comuniquen solo en la subred cifrada, puede usar listas de Control de acceso (ACL) sólo para permitir la comunicación dentro de la subred actual. Para obtener más información, consulte [usar Access Control Lists (ACL) para el flujo de tráfico de red de centro de datos de administrar](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).


## <a name="step-1-create-the-encryption-certificate"></a>Paso 1. Crear el certificado de cifrado
Cada host debe tener instalado un certificado de cifrado. Puede usar el mismo certificado para todos los inquilinos o generar un único para cada inquilino. 

1.  Generar el certificado  

```
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
```

Después de ejecutar el script, un nuevo certificado aparece en el almacén My:

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2.  Exporte el certificado a un archivo.<p>Necesita dos copias de los certificados, uno con la clave privada y otro sin.

    $subjectName = "EncryptedVirtualNetworks" $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"} [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret")) Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert

3.  Instalar los certificados en cada uno de los hosts de hyper-v 

    PS C:\> dir c:\$subjectname.*


        Directory: C:\


    Modo escritura longitud nombre
    ----                -------------         ------ ----
    -a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer -a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx

4.  Instalar en un host de Hyper-V

    $server = "Server01"

    $subjectname = "EncryptedVirtualNetworks" copia c:\$SubjectName.* \\$server\c$ invoke-command - computername $server - ArgumentList $subjectname, "secreto" {param ([string] $SubjectName, [string] $Secret) $certFullPath = "c: \$SubjectName.cer "

        # create a representation of the certificate file
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath)

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

        # Important: Remove the certificate files when finished
        remove-item C:\$SubjectName.cer
        remove-item C:\$SubjectName.pfx
    }    

5.  Repita para cada servidor en su entorno.<p>Después de repetir para cada servidor, debe tener un certificado instalado en Mi almacén de cada host de Hyper-V y la raíz. 

6.  Comprobar la instalación del certificado.<p>Comprobar los certificados, examine el contenido de la mi y almacenes de certificados de raíz:

    PS C:\> Server1 escriba-pssession.

    [Server1] ¿PS C:\> cert://localmachine/my get-childitem, cert://localmachine/root |? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Asunto de huella digital
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6 CN = EncryptedVirtualNetworks


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

    Asunto de huella digital
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6 CN = EncryptedVirtualNetworks

7.  Anote la huella digital.<p>Debe realizar una nota de la huella digital porque la necesitará para crear el objeto de credencial de certificado en la controladora de red.

## <a name="step-2-create-the-certificate-credential"></a>Paso 2. Crear la credencial de certificado

Después de instalar el certificado en cada uno de los hosts de Hyper-V conectados a la controladora de red, ahora debe configurar la controladora de red para que lo utilicen.  Para ello, debe crear un objeto de credencial que contiene la huella digital del certificado de la máquina con los módulos de PowerShell de controladora de red instalados. 


    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  
    
    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller
    
    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

>[!TIP]
>Puede volver a usar esta credencial para cada red virtual cifrada, o puede implementar y usar un certificado único para cada inquilino.


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>Paso 3. Configurar una red Virtual para el cifrado

Este paso se presupone ya ha creado un nombre de red virtual "Mi red" y contiene al menos una subred virtual.  Para obtener información sobre cómo crear redes virtuales, consulte [Create, Delete o Update las redes virtuales de inquilino](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

>[!NOTE]
>Al comunicarse con otra máquina virtual en la misma subred, si su actualmente conectado o conectado en un momento posterior, el tráfico se cifra automáticamente.

1.  Recupere los objetos de red Virtual y las credenciales de la controladora de red

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork" $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

2.  Agregue una referencia a las credenciales del certificado y habilitar el cifrado en subredes individuales

    $vnet.properties.EncryptionCredential = $certcred

    # <a name="replace-the-subnets-index-with-the-value-corresponding-to-the-subnet-you-want-encrypted"></a>Reemplazar el índice de las subredes con el valor correspondiente a la subred que desee cifrados.  
    # <a name="repeat-for-each-subnet-where-encryption-is-needed"></a>Repetir para cada subred que se necesita cifrado
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

3.  Colocar el objeto de red Virtual actualizado en la controladora de red

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force


_**¡Enhorabuena!**_ Ya ha terminado una vez completados estos pasos. 


## <a name="next-steps"></a>Pasos siguientes



