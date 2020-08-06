---
title: Configuración de nodos de HGS adicionales
ms.prod: windows-server
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: fb744d2be9cc0002158deb0d9665a354ef23851a
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769363"
---
# <a name="configure-additional-hgs-nodes"></a>Configuración de nodos de HGS adicionales

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En entornos de producción, HGS debe configurarse en un clúster de alta disponibilidad para asegurarse de que las máquinas virtuales blindadas se puedan encender incluso si un nodo HGS deja de funcionar. En entornos de prueba, no se necesitan nodos de HGS secundarios.

Use uno de estos métodos para agregar nodos HGS, como mejor se adapte a su entorno.

| Entorno | Opción 1 | Opción 2 |
|--|--|--|
| Nuevo bosque de HGS | [Uso de archivos PFX](#dedicated-hgs-forest-with-pfx-certificates) | [Usar huellas digitales de certificado](#dedicated-hgs-forest-with-certificate-thumbprints) |
| Bosque bastión existente | [Uso de archivos PFX](#existing-bastion-forest-with-pfx-certificates) | [Usar huellas digitales de certificado](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>Prerrequisitos

Asegúrese de que cada nodo adicional:
- Tiene la misma configuración de hardware y software que el nodo principal
- Está conectado a la misma red que los demás servidores HGS
- Puede resolver los demás servidores HGS por sus nombres DNS.

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>Bosque de HGS dedicado con certificados PFX

1. Promoción del nodo HGS a un controlador de dominio
2. Inicialización del servidor HGS

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promoción del nodo HGS a un controlador de dominio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)]

### <a name="initialize-the-hgs-server"></a>Inicialización del servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)]

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>Bosque de HGS dedicado con huellas digitales de certificado

1. Promoción del nodo HGS a un controlador de dominio
2. Inicialización del servidor HGS
3. Instalar las claves privadas de los certificados

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promoción del nodo HGS a un controlador de dominio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)]

### <a name="initialize-the-hgs-server"></a>Inicialización del servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)]

### <a name="install-the-private-keys-for-the-certificates"></a>Instalar las claves privadas de los certificados

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>Bosque bastión existente con certificados PFX

1. Unir el nodo al dominio existente
2. Conceder derechos de equipo para recuperar la contraseña de gMSA y ejecutar install-ADServiceAccount
3. Inicialización del servidor HGS

### <a name="join-the-node-to-the-existing-domain"></a>Unir el nodo al dominio existente

1. Asegúrese de que al menos una NIC del nodo está configurada para usar el servidor DNS en el primer servidor HGS.
2. Una el nuevo nodo HGS al mismo dominio que el primer nodo HGS.

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Conceder derechos de equipo para recuperar la contraseña de gMSA y ejecutar install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)]

### <a name="initialize-the-hgs-server"></a>Inicialización del servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)]

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>Bosque bastión existente con huellas digitales de certificado

1. Unir el nodo al dominio existente
2. Conceder derechos de equipo para recuperar la contraseña de gMSA y ejecutar install-ADServiceAccount
3. Inicialización del servidor HGS
4. Instalar las claves privadas de los certificados

### <a name="join-the-node-to-the-existing-domain"></a>Unir el nodo al dominio existente

1. Asegúrese de que al menos una NIC del nodo está configurada para usar el servidor DNS en el primer servidor HGS.
2. Una el nuevo nodo HGS al mismo dominio que el primer nodo HGS.

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Conceder derechos de equipo para recuperar la contraseña de gMSA y ejecutar install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)]

### <a name="initialize-the-hgs-server"></a>Inicialización del servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)]

Los certificados de firma y cifrado del primer servidor de HGS tardarán hasta 10 minutos en replicarse en este nodo.

### <a name="install-the-private-keys-for-the-certificates"></a>Instalar las claves privadas de los certificados

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>Configuración de HGS para comunicaciones HTTPS

Si desea proteger los puntos de conexión de HGS con un certificado SSL, debe configurar el certificado SSL en este nodo, así como todos los demás nodos del clúster de HGS.
HGS *no* replica los certificados SSL y no es necesario usar las mismas claves para cada nodo (es decir, puede tener distintos certificados SSL para cada nodo).

Al solicitar un certificado SSL, asegúrese de que el nombre de dominio completo del clúster (como se muestra en la salida de `Get-HgsServer` ) sea el nombre común del sujeto del certificado o que se incluya como nombre DNS alternativo del sujeto.
Cuando haya obtenido un certificado de la entidad de certificación, puede configurar HGS para usarlo con [set-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver).

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Si ya ha instalado el certificado en el almacén de certificados local y desea hacer referencia a él mediante la huella digital, ejecute el siguiente comando en su lugar:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS siempre expondrá los puertos HTTP y HTTPS para la comunicación.
No se admite la eliminación del enlace HTTP en IIS; sin embargo, puede usar el Firewall de Windows u otras tecnologías de Firewall de red para bloquear las comunicaciones a través del puerto 80.

## <a name="decommission-an-hgs-node"></a>Retirada de un nodo HGS

Para retirar un nodo HGS:

1. [Borre la configuración de HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration).

   Esto quita el nodo del clúster y desinstala los servicios de atestación y protección de claves.
   Si es el último nodo del clúster, se necesita-Force para indicar que desea quitar el último nodo y destruir el clúster en Active Directory.

   Si HGS está implementado en un bosque bastión (valor predeterminado), este es el único paso.
   Opcionalmente, puede separar el equipo del dominio y quitar la cuenta de gMSA de Active Directory.

2. Si HGS creó su propio dominio, también debe [desinstalar HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) para separar el dominio y degradar el controlador de dominio.
