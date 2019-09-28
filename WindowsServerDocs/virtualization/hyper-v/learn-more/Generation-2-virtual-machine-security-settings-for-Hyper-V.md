---
title: Configuración de seguridad de máquina virtual de generación 2 para Hyper-V
description: Describe la configuración de seguridad disponible en el administrador de Hyper-V para máquinas virtuales de generación 2.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 82544a58a8d46b3063605557be3c63cfa799e4fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364242"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Configuración de seguridad de máquina virtual de generación 2 para Hyper-V

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Use la configuración de seguridad de la máquina virtual en el administrador de Hyper-V para ayudar a proteger los datos y el estado de una máquina virtual. Puede proteger las máquinas virtuales de la inspección, el robo y la alteración del malware que se puede ejecutar en el host y de los administradores del centro de recursos. El nivel de seguridad que se obtiene depende del hardware del host que se ejecuta, la generación de la máquina virtual y de si se configura el servicio, denominado servicio de protección de host, que autoriza a los hosts a iniciar máquinas virtuales blindadas.  

El servicio de protección de host es un nuevo rol en Windows Server 2016. Identifica los hosts de Hyper-V legítimos y les permite ejecutar una máquina virtual determinada. Lo más habitual es configurar el servicio de protección de host para un centro de seguridad. No obstante, puede crear una máquina virtual blindada para ejecutarla localmente sin configurar un servicio de protección de host. Posteriormente, puede distribuir la máquina virtual blindada a un tejido de protección de host.  

Si no ha configurado el servicio de protección de host o lo está ejecutando en modo local en el host de Hyper-V y el host tiene la clave de protección del propietario de la máquina virtual, puede cambiar la configuración que se describe en este tema.   Un propietario de una clave de protección es una organización que crea y comparte una clave pública o privada para poseer todas las máquinas virtuales creadas con esa clave.  

Para obtener información sobre cómo hacer que las máquinas virtuales sean más seguras con el servicio de protección de host, consulte los siguientes recursos.  

- [Protección del tejido: Protección de secretos de inquilino en Hyper-V (encendido de vídeo) ](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Tejido protegido y máquinas virtuales blindadas](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Configuración de arranque seguro en el administrador de Hyper-V  

Arranque seguro es una característica disponible con máquinas virtuales de generación 2 que ayuda a evitar que los controladores de firmware, sistemas operativos o Unified Extensible Firmware Interface (UEFI) no autorizados (también conocidos como ROM de opción) se ejecuten durante el arranque. El arranque seguro está habilitado de forma predeterminada. Puede usar el arranque seguro con máquinas virtuales de generación 2 que ejecutan sistemas operativos de distribución de Windows o Linux.  

Las plantillas descritas en la tabla siguiente hacen referencia a los certificados necesarios para comprobar la integridad del proceso de arranque.  

|Nombre de la plantilla|Descripción|  
|-----------------|---------------|  
|Microsoft Windows|Seleccione para proteger el arranque de la máquina virtual para un sistema operativo Windows.|  
|Entidad de certificación UEFI de Microsoft|Seleccione para proteger el arranque de la máquina virtual para un sistema operativo de distribución de Linux.|  
|Máquina virtual blindada de código abierto|Esta plantilla se usa para el arranque seguro para [máquinas virtuales blindadas basadas en Linux](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Para obtener más información, vea los temas siguientes:  

- [Información general de seguridad de Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Virtual Machines de Linux y FreeBSD en Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configuración de compatibilidad de cifrado en el administrador de Hyper-V

Puede ayudar a proteger los datos y el estado de la máquina virtual seleccionando las siguientes opciones de compatibilidad con el cifrado.  

- **Habilitar módulo de plataforma segura** : esta opción hace que un chip de módulo de plataforma segura virtualizado (TPM) esté disponible para la máquina virtual. Esto permite que el invitado Cifre el disco de la máquina virtual mediante BitLocker.
  - Si el host de Hyper-V ejecuta Windows 10 1511, tendrá que habilitar el modo de usuario aislado. 
- **Cifrado del estado y migración de máquinas** virtuales: cifra el estado guardado de la máquina virtual y el tráfico de migración en vivo.

### <a name="enable-isolated-user-mode"></a>Habilitar modo de usuario aislado

Si selecciona **habilitar módulo de plataforma segura** en hosts de Hyper-V que ejecutan versiones de Windows anteriores a la actualización de aniversario de Windows 10, debe habilitar el modo de usuario aislado. No es necesario hacer esto para los hosts de Hyper-V que ejecutan Windows Server 2016 o la actualización de aniversario de Windows 10 o posterior.

El modo de usuario aislado es el entorno de tiempo de ejecución que hospeda las aplicaciones de seguridad dentro del modo seguro virtual en el host de Hyper-V. El modo seguro virtual se usa para proteger y proteger el estado del chip TPM virtual.  

Para habilitar el modo de usuario aislado en el host de Hyper-V que ejecuta versiones anteriores de Windows 10,  

1.  Abra Windows PowerShell como administrador.  

2.  Ejecute los siguientes comandos:  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

Puede migrar una máquina virtual con un TPM virtual habilitado a cualquier host que ejecute Windows Server 2016, Windows 10 Build 10586 o versiones posteriores. Pero si lo migra a otro host, es posible que no pueda iniciarlo. Debe actualizar el protector de clave para que la máquina virtual autorice al nuevo host a ejecutar la máquina virtual. Para obtener más información, consulte [tejido protegido y máquinas virtuales blindadas](https://go.microsoft.com/fwlink/?LinkId=746381) y [requisitos del sistema para Hyper-V en Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Directiva de seguridad en el administrador de Hyper-V  
Para más seguridad de las máquinas virtuales, use la opción **Habilitar blindaje** para deshabilitar las características de administración, como la conexión de la consola, PowerShell Direct y algunos componentes de integración. Si selecciona esta opción, las opciones de **arranque seguro**, **Habilitar módulo de plataforma segura**y **cifrar estado y migración de VM** se seleccionan y se aplican.   

Puede ejecutar la máquina virtual blindada localmente sin configurar un servicio de protección de host. Pero si lo migra a otro host, es posible que no pueda iniciarlo. Debe actualizar el protector de clave para que la máquina virtual autorice al nuevo host a ejecutar la máquina virtual. Para obtener más información, consulta [VM de tejido protegido y blindadas](https://go.microsoft.com/fwlink/?LinkId=746381).  

Para obtener más información acerca de la seguridad en Windows Server, consulte [seguridad y garantía](../../../security/Security-and-Assurance.md).  
