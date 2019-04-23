---
title: Configure additional HGS nodes (Configurar roles HGS adicionales)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/22/2018
ms.openlocfilehash: a89337457cc71ffee78e3f73fecc2262f1fb38e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855206"
---
# <a name="configure-additional-hgs-nodes"></a>Configure additional HGS nodes (Configurar roles HGS adicionales)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En entornos de producción, HGS debe configurarse en un clúster de alta disponibilidad para asegurarse de que se pueden apagar las máquinas virtuales blindadas en incluso si un nodo HGS deja de funcionar. Para entornos de prueba, no son necesarios los nodos secundarios de HGS.

Utilice uno de estos métodos para agregar nodos HGS, lo más adecuados para su entorno.

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|Nuevo bosque de HGS  | [Usa archivos PFX](#dedicated-hgs-forest-with-pfx-certificates) | [Uso de huellas digitales de certificados](#dedicated-hgs-forest-with-certificate-thumbprints) |
|Bosque bastión existente |  [Usa archivos PFX](#existing-bastion-forest-with-pfx-certificates) | [Uso de huellas digitales de certificados](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que cada nodo adicional: 
- Tiene la misma configuración de hardware y software que el nodo principal 
- Está conectado a la misma red que los demás servidores HGS
- Puede resolver los otros servidores HGS por sus nombres DNS

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>Bosque dedicado de HGS con certificados PFX

1. Promueve el HGS del nodo a un controlador de dominio
2. Inicializar el servidor HGS

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promueve el HGS del nodo a un controlador de dominio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar el servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>Bosque dedicado de HGS con huellas digitales de certificados
 
1. Promueve el HGS del nodo a un controlador de dominio
2. Inicializar el servidor HGS
3. Instalar las claves privadas para los certificados

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promueve el HGS del nodo a un controlador de dominio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar el servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>Instalar las claves privadas para los certificados

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>Bosque bastión existente con los certificados PFX

1. Unir el nodo al dominio existente
2. Conceder los derechos de la máquina para recuperar la contraseña de gMSA y ejecutar Install-ADServiceAccount
3. Inicializar el servidor HGS

### <a name="join-the-node-to-the-existing-domain"></a>Unir el nodo al dominio existente

1. Asegúrese de que al menos una NIC en el nodo está configurada para usar el servidor DNS en el primer servidor HGS.
2. Unir el nuevo nodo HGS en el mismo dominio que el primer nodo HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Conceder los derechos de la máquina para recuperar la contraseña de gMSA y ejecutar Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar el servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>Bosque bastión existente con huellas digitales de certificados

1. Unir el nodo al dominio existente
2. Conceder los derechos de la máquina para recuperar la contraseña de gMSA y ejecutar Install-ADServiceAccount
3. Inicializar el servidor HGS
4. Instalar las claves privadas para los certificados

### <a name="join-the-node-to-the-existing-domain"></a>Unir el nodo al dominio existente

1. Asegúrese de que al menos una NIC en el nodo está configurada para usar el servidor DNS en el primer servidor HGS.
2. Unir el nuevo nodo HGS en el mismo dominio que el primer nodo HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Conceder los derechos de la máquina para recuperar la contraseña de gMSA y ejecutar Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar el servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

Se tardará hasta 10 minutos para el cifrado y certificados de firma desde el primer servidor HGS para replicar en este nodo.

### <a name="install-the-private-keys-for-the-certificates"></a>Instalar las claves privadas para los certificados

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>Configuración de HGS para comunicaciones HTTPS

Si desea proteger HGS puntos de conexión con un certificado SSL, debe configurar el certificado SSL en este nodo, así como todos los nodos del clúster de HGS.
Certificados SSL *no* replicado por HGS y no es necesario usar las mismas claves para cada nodo (es decir, puede tener diferentes certificados SSL para cada nodo).

Al solicitar un certificado SSL, asegúrese del nombre de dominio completo del clúster (como se muestra en la salida de `Get-HgsServer`) es el nombre común de sujeto del certificado o incluido como un nombre DNS alternativo del firmante.
Cuando haya obtenido un certificado de la entidad emisora de certificados, puede configurar HGS para usarlo con [conjunto HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver).

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Si ya ha instalado el certificado en el almacén de certificados local y desea hacer referencia a él mediante la huella digital, ejecute el siguiente comando:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS siempre va a exponer los puertos HTTP y HTTPS para la comunicación.
No se admite para quitar el enlace HTTP en IIS, sin embargo, puede usar el Firewall de Windows u otras tecnologías de servidor de seguridad de red para bloquear las comunicaciones a través del puerto 80.

## <a name="decommission-an-hgs-node"></a>Retirar un nodo HGS

Para retirar un nodo HGS:

1. [Borrar la configuración de HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration).

   Esto quita el nodo del clúster y desinstala los servicios de protección de claves y atestación. 
   Si es el último nodo del clúster, - Force es necesario para indicar que desea quitar el último nodo y destruir el clúster en Active Directory. 
   
   Si el HGS se implementa en un bosque bastión (valor predeterminado), es el único paso. 
   Si lo desea puede separar el equipo del dominio y quitar la cuenta de gMSA de Active Directory.

1. Si el HGS crea su propio dominio, también debería [desinstalar HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) para separar el dominio y degradar el controlador de dominio.



## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Validar la configuración de HGS](guarded-fabric-verify-hgs-configuration.md)

