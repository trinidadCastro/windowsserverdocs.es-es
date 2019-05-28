---
title: Crear el archivo de respuesta de especialización del sistema operativo
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5717fcc9e1732b6273620e633c140c6df58ec8b7
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624655"
---
# <a name="create-os-specialization-answer-file"></a>Crear el archivo de respuesta de especialización del sistema operativo

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En preparación para implementar máquinas virtuales blindadas, necesita crear un archivo de respuesta de especialización del sistema operativo. En Windows, esto se conoce comúnmente como el archivo "unattend.xml". El **New-ShieldingDataAnswerFile** función de Windows PowerShell le ayuda a hacerlo. Puede, a continuación, use el archivo de respuesta al crear máquinas virtuales blindadas desde una plantilla mediante el uso de System Center Virtual Machine Manager (o cualquier otro controlador de tejido).

Para obtener directrices generales para los archivos de instalación desatendida para las máquinas virtuales blindadas, consulte [crear un archivo de respuesta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file).
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>Descarga de la función New-ShieldingDataAnswerFile

Puede obtener el **New-ShieldingDataAnswerFile** funcionar desde el [Galería de PowerShell](https://aka.ms/gftools). Si el equipo tiene conectividad a Internet, puede instalarlo desde PowerShell con el siguiente comando:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

El `unattend.xml` salida puede empaquetarse en los datos de blindaje, junto con artefactos adicionales, por lo que se puede usar para crear máquinas virtuales blindadas a partir de plantillas.

Las secciones siguientes muestran cómo puede utilizar los parámetros de función para un `unattend.xml` archivo que contiene varias opciones:

- [Archivo de respuesta básico de Windows](#basic-windows-answer-file)
- [Archivo con la unión a un dominio de la respuesta de Windows](#windows-answer-file-with-domain-join)
- [Archivo de respuesta de Windows con direcciones IPv4 estáticas](#windows-answer-file-with-static-ipv4-addresses)
- [Archivo de respuesta de Windows con una configuración regional personalizada](#windows-answer-file-with-a-custom-locale)
- [Archivo de respuesta básico de Linux](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>Archivo de respuesta básico de Windows

Los siguientes comandos crean un archivo de respuesta de Windows que simplemente establece la contraseña de la cuenta de administrador y el nombre de host.
Los adaptadores de red de máquina virtual usará DHCP para obtener las direcciones IP y la máquina virtual no se unirá a un dominio de Active Directory.
Cuando se le pida que escriba una credencial de administrador, especifique el nombre de usuario deseado y la contraseña.
Si desea configurar la cuenta de administrador integrada, utilice "Administrador" para el nombre de usuario.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>Archivo con la unión a un dominio de la respuesta de Windows

Los siguientes comandos crean un archivo de respuesta de Windows que une la máquina virtual blindada a un dominio de Active Directory.
Los adaptadores de red de máquina virtual usará DHCP para obtener las direcciones IP.

La primera solicitud de credencial solicitará la información de cuenta de administrador local.
Si desea configurar la cuenta de administrador integrada, utilice "Administrador" para el nombre de usuario.

La segunda petición de credenciales le pedirá las credenciales que tengan el derecho de unir la máquina al dominio de Active Directory.

No olvide cambiar el valor de la "-DomainName" parámetro para el FQDN del dominio de Active Directory.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>Archivo de respuesta de Windows con direcciones IPv4 estáticas

Los siguientes comandos crean un archivo de respuesta de Windows que utiliza direcciones IP estáticas proporcionadas por el Administrador de tejido, tales como System Center Virtual Machine Manager en el momento de la implementación.

Virtual Machine Manager proporciona tres componentes a la dirección IP estática mediante el uso de un grupo de IP: Dirección IPv4, dirección IPv6, direcciones de puerta de enlace y dirección DNS. Si desea que los campos adicionales se incluirán o requerir una configuración de red personalizada, deberá editar manualmente el archivo de respuesta generado por la secuencia de comandos.

Las capturas de pantalla siguientes se muestran los grupos de IP que se pueden configurar en Virtual Machine Manager. Estos grupos son necesarios si desea usar la dirección IP estática.

Actualmente, la función admite sólo un servidor DNS. Aquí es cuál sería el aspecto de la configuración de DNS:

![Configurar el servidor DNS con el grupo de IP estáticas](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

Aquí es cuál sería el resumen para crear el grupo de direcciones IP estáticas. En resumen, debe tener una sola red route, una puerta de enlace y servidor DNS, y debe especificar la dirección IP.

![Resumen de creación del grupo IP estática](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

Deberá configurar el adaptador de red para la máquina virtual. Captura de pantalla siguiente muestra dónde establecer que la configuración y cómo cambiar a la dirección IP estática.

![Configurar el hardware para usar la dirección IP estática](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

A continuación, puede usar el `-StaticIPPool` parámetro debe incluir los elementos IP estáticos en el archivo de respuesta. Los parámetros `@IPAddr-1@`, `@NextHop-1-1@`, y `@DNSAddr-1-1@` en la respuesta de archivo, a continuación, se reemplazará con los valores reales que especificó en tiempo de implementación de Virtual Machine Manager.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>Archivo de respuesta de Windows con una configuración regional personalizada

Los siguientes comandos crean un archivo de respuesta de Windows con una configuración regional personalizada.

Cuando se le pida que escriba una credencial de administrador, especifique el nombre de usuario deseado y la contraseña.
Si desea configurar la cuenta de administrador integrada, utilice "Administrador" para el nombre de usuario.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>Archivo de respuesta básico de Linux

A partir de Windows Server versión 1709, puede ejecutar a determinados sistemas operativos invitados de Linux en máquinas virtuales blindadas.
Si está utilizando al agente de System Center Virtual Machine Manager Linux especializar esas máquinas virtuales, el cmdlet New-ShieldingDataAnswerFile puede crear archivos de respuesta compatible para él.

En un archivo de respuesta de Linux, normalmente incluirá la contraseña raíz y clave SSH raíz información de grupo IP estática si lo desea.
Reemplace la ruta de acceso a la mitad pública de la clave SSH antes de ejecutar el script siguiente.

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>Vea también

- [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
