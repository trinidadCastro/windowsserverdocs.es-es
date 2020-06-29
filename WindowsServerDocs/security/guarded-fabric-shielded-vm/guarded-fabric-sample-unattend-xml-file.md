---
title: Crear el archivo de respuesta de especialización del sistema operativo
ms.prod: windows-server
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 526ded03c877613766b8a0b762f1db1a693d2019
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475002"
---
# <a name="create-os-specialization-answer-file"></a>Crear el archivo de respuesta de especialización del sistema operativo

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Como preparación para la implementación de máquinas virtuales blindadas, puede que tenga que crear un archivo de respuesta de especialización del sistema operativo. En Windows, esto se conoce comúnmente como el archivo "unattend.xml". La función **New-ShieldingDataAnswerFile de** Windows PowerShell le ayuda a hacerlo. Después, puede usar el archivo de respuesta al crear máquinas virtuales blindadas a partir de una plantilla mediante System Center Virtual Machine Manager (o cualquier otro controlador de tejido).

Para obtener instrucciones generales sobre los archivos de instalación desatendida para las máquinas virtuales blindadas, consulte [crear un archivo de respuesta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file).

## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>Descarga de la función New-ShieldingDataAnswerFile

Puede obtener la función **New-ShieldingDataAnswerFile** de la [Galería de PowerShell](https://aka.ms/gftools). Si el equipo tiene conectividad a Internet, puede instalarlo desde PowerShell con el siguiente comando:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

La `unattend.xml` salida se puede empaquetar en los datos de blindaje, junto con artefactos adicionales, para que se pueda usar para crear máquinas virtuales blindadas a partir de plantillas.

En las secciones siguientes se muestra cómo puede usar los parámetros de función para un `unattend.xml` archivo que contiene varias opciones:

- [Archivo de respuesta básico de Windows](#basic-windows-answer-file)
- [Archivo de respuesta de Windows con Unión a un dominio](#windows-answer-file-with-domain-join)
- [Archivo de respuesta de Windows con direcciones IPv4 estáticas](#windows-answer-file-with-static-ipv4-addresses)
- [Archivo de respuesta de Windows con una configuración regional personalizada](#windows-answer-file-with-a-custom-locale)
- [Archivo de respuesta básico de Linux](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>Archivo de respuesta básico de Windows

Los siguientes comandos crean un archivo de respuesta de Windows que simplemente establece la contraseña y el nombre de host de la cuenta de administrador.
Los adaptadores de red de la máquina virtual usarán DHCP para obtener las direcciones IP y la máquina virtual no se unirá a un dominio de Active Directory.
Cuando se le pida que escriba una credencial de administrador, especifique el nombre de usuario y la contraseña que desee.
Si desea configurar la cuenta predefinida de administrador, use "administrador" para el nombre de usuario.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>Archivo de respuesta de Windows con Unión a un dominio

Los siguientes comandos crean un archivo de respuesta de Windows que une la máquina virtual blindada a un dominio de Active Directory.
Los adaptadores de red de la máquina virtual usarán DHCP para obtener direcciones IP.

La primera solicitud de credencial solicitará la información de la cuenta de administrador local.
Si desea configurar la cuenta predefinida de administrador, use "administrador" para el nombre de usuario.

La segunda solicitud de credenciales solicitará las credenciales que tengan derecho a unir el equipo al dominio de Active Directory.

Asegúrese de cambiar el valor del parámetro "-DomainName" por el FQDN de su dominio de Active Directory.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>Archivo de respuesta de Windows con direcciones IPv4 estáticas

Los siguientes comandos crean un archivo de respuesta de Windows que usa las direcciones IP estáticas proporcionadas en el momento de la implementación por el administrador de tejido, como System Center Virtual Machine Manager.

Virtual Machine Manager proporciona tres componentes a la dirección IP estática mediante un grupo de direcciones IP: dirección IPv4, dirección IPv6, dirección de puerta de enlace y dirección DNS. Si desea incluir campos adicionales o requerir una configuración de red personalizada, deberá modificar manualmente el archivo de respuesta generado por el script.

Las capturas de pantallas siguientes muestran los grupos de direcciones IP que se pueden configurar en Virtual Machine Manager. Estos grupos son necesarios si desea utilizar una dirección IP estática.

Actualmente, la función solo admite un servidor DNS. Este es el aspecto de la configuración de DNS:

![Configuración del servidor DNS con grupo de direcciones IP estáticas](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

Este es el aspecto que tendría el resumen para crear el grupo de direcciones IP estáticas. En Resumen, solo debe tener una ruta de red, una puerta de enlace y un servidor DNS, y debe especificar la dirección IP.

![Resumen de la creación de grupos de direcciones IP estáticas](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

Debe configurar el adaptador de red para la máquina virtual. En la captura de pantalla siguiente se muestra dónde establecer esa configuración y cómo cambiarla a una dirección IP estática.

![Configurar el hardware para usar la dirección IP estática](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

Después, puede usar el `-StaticIPPool` parámetro para incluir los elementos IP estáticos en el archivo de respuesta. Los parámetros `@IPAddr-1@` , `@NextHop-1-1@` y `@DNSAddr-1-1@` en el archivo de respuesta se reemplazarán por los valores reales especificados en Virtual Machine Manager en el momento de la implementación.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>Archivo de respuesta de Windows con una configuración regional personalizada

Los siguientes comandos crean un archivo de respuesta de Windows con una configuración regional personalizada.

Cuando se le pida que escriba una credencial de administrador, especifique el nombre de usuario y la contraseña que desee.
Si desea configurar la cuenta predefinida de administrador, use "administrador" para el nombre de usuario.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>Archivo de respuesta básico de Linux

A partir de la versión 1709 de Windows Server, puede ejecutar algunos sistemas operativos invitados Linux en máquinas virtuales blindadas.
Si usa el agente de System Center Virtual Machine Manager Linux para especializar esas máquinas virtuales, el cmdlet New-ShieldingDataAnswerFile puede crear archivos de respuesta compatibles para ella.

En un archivo de respuesta de Linux, normalmente incluirá la contraseña raíz, la clave SSH raíz y, opcionalmente, la información del grupo de direcciones IP estáticas.
Reemplace la ruta de acceso a la mitad pública de la clave SSH antes de ejecutar el script siguiente.

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="additional-references"></a>Referencias adicionales

- [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
