---
title: Configuración de seguridad de máquina virtual de generación 2 de Hyper-V
description: Describe la configuración de seguridad disponible en el Administrador de Hyper-V para máquinas virtuales de generación 2
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 90a2b7234ee55d8469b6e02ba3de3a0efc080a3e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889506"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Configuración de seguridad de máquina virtual de generación 2 de Hyper-V

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server, Microsoft Hyper-V Server 2019 de 2019

Use la configuración de seguridad de máquina virtual en el Administrador de Hyper-V para ayudar a proteger los datos y el estado de una máquina virtual. Puede proteger las máquinas virtuales de inspección, el robo y la alteración de frente al malware que se puede ejecutar en el host y los administradores de centros de datos. El nivel de seguridad que obtendrá depende el hardware del host que se ejecuta, la generación de máquina virtual, y si desea configurar el servicio, llamado el servicio guardián de Host, que autoriza a los hosts para iniciar las máquinas virtuales blindadas.  

El servicio de protección de Host es un rol nuevo en Windows Server 2016. Identifica los hosts de Hyper-V legítimos y les permite ejecutarse una máquina virtual dada. Con más frecuencia, podría configurar el servicio de protección de Host para un centro de datos. Pero puede crear una máquina virtual blindada para ejecutarlo localmente sin necesidad de configurar un servicio de protección de Host. Más adelante, puede distribuir la máquina virtual blindada a un tejido de guardián de Host.  

Si no ha configurado el servicio de protección de Host o se ejecuta en modo local en el host de Hyper-V y el host tiene la clave de guardián del propietario de la máquina virtual, puede cambiar la configuración descrita en este tema.   Un propietario de una clave de protección es una organización que crea y los recursos compartidos que se crea una clave pública o privada para el propietario de todas las máquinas virtuales con esa clave.  

Para obtener información sobre cómo puede realizar las máquinas virtuales más segura con el servicio de protección de Host, consulte los siguientes recursos.  

- [Protección del tejido: Proteger los secretos de inquilino en Hyper-V (vídeo de Ignite)](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Las máquinas virtuales blindadas y tejido protegido](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Proteger la configuración de arranque en el Administrador de Hyper-V  

Arranque seguro es una característica disponible con máquinas virtuales de generación 2 que ayuda a impida la ejecución en tiempo de arranque de firmware no autorizado, los sistemas operativos o controladores de Unified Extensible Firmware Interface (UEFI) (también conocidos como ROM). Arranque seguro está habilitado de forma predeterminada. Puede usar el arranque seguro con máquinas virtuales de generación 2 que se ejecutan los sistemas operativos de distribución de Windows o Linux.  

Las plantillas descritas en la siguiente tabla hacen referencia a los certificados que necesita para comprobar la integridad del proceso de arranque.  

|Nombre de la plantilla|Descripción|  
|-----------------|---------------|  
|Microsoft Windows|Seleccione la máquina virtual para un sistema operativo de Windows para el arranque seguro.|  
|Entidad emisora de certificados de Microsoft UEFI|Seleccione la máquina virtual para un sistema operativo de distribución de Linux para el arranque seguro.|  
|Abrir origen de máquina virtual blindada|Esta plantilla se aprovecha para arranque seguro para [basado en Linux, máquinas virtuales blindadas](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Para obtener más información, vea los temas siguientes:  

- [Información general sobre la seguridad de Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Linux y FreeBSD máquinas virtuales Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configuración de compatibilidad de cifrado en el Administrador de Hyper-V

Puede ayudar a proteger los datos y el estado de la máquina virtual mediante la selección de las siguientes opciones de soporte técnico de cifrado.  

- **Habilitar el módulo de plataforma segura** -esta configuración hace que un chip de módulo de plataforma segura (TPM) virtualizados disponible a la máquina virtual. Esto permite que el invitado cifrar el disco de máquina virtual mediante el uso de BitLocker.
  - Si el host de Hyper-V se está ejecutando Windows 10 1511, deberá habilitar el modo aislado de usuario. 
- **Cifrar el tráfico de migración de estado y la máquina virtual** : cifra el estado de la máquina virtual que guardó y tráfico de migración en vivo.

### <a name="enable-isolated-user-mode"></a>Habilitar el modo de usuario aislado

Si selecciona **habilitar el módulo de plataforma segura** en hosts de Hyper-V que ejecutan versiones de Windows anteriores a Windows 10 Anniversary Update, debe habilitar el modo aislado de usuario. No es necesario hacer esto para los hosts de Hyper-V que ejecute Windows Server 2016 o Windows 10 Anniversary Update o posterior.

El modo aislado de usuario es el entorno de tiempo de ejecución que hospeda las aplicaciones de seguridad en el modo seguro Virtual en el host de Hyper-V. El modo seguro virtual se usa para proteger y el estado del chip TPM virtual.  

Para habilitar el modo de usuario aislado en el host de Hyper-V que ejecutan versiones anteriores de Windows 10,  

1.  Abra Windows PowerShell como administrador.  

2.  Ejecute los siguientes comandos:  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

Puede migrar una máquina virtual con TPM virtual habilitado para cualquier host que ejecuta Windows Server 2016, Windows 10 compilar versiones 10586 o posterior. Pero si migra a otro host, es posible que no pueda iniciarlo. Debe actualizar el Protector de clave para esa máquina virtual para autorizar el nuevo host para ejecutar la máquina virtual. Para obtener más información, consulte [máquinas virtuales blindadas y tejido protegido](https://go.microsoft.com/fwlink/?LinkId=746381) y [requisitos del sistema para Hyper-V en Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Directiva de seguridad en el Administrador de Hyper-V  
Para obtener más seguridad de máquina virtual, use el **habilitar el blindaje** opción para deshabilitar las características de administración como la conexión de la consola, PowerShell Direct y algunos componentes de integración. Si selecciona esta opción, **arranque seguro**, **habilitar el módulo de plataforma segura**, y **tráfico de migración de estado de cifrado y la máquina virtual** opciones están seleccionadas y aplican.   

Puede ejecutar la máquina virtual blindada localmente sin necesidad de configurar un servicio de protección de Host. Pero si migra a otro host, es posible que no pueda iniciarlo. Debe actualizar el Protector de clave para esa máquina virtual para autorizar el nuevo host para ejecutar la máquina virtual. Para obtener más información, consulta [VM de tejido protegido y blindadas](https://go.microsoft.com/fwlink/?LinkId=746381).  

Para obtener más información sobre la seguridad en Windows Server, vea [seguridad y control](../../../security/Security-and-Assurance.md).  
