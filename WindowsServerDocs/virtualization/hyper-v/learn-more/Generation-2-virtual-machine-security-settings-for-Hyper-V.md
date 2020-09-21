---
title: Configuración de seguridad de las máquinas virtuales de generación 2 para Hyper-V
description: Describe la configuración de seguridad disponible en el Administrador de Hyper-V para las máquinas virtuales de generación 2.
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: 27f5ce49aeef8b662a07b3fc559fae7c1402be4f
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745980"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Configuración de seguridad de las máquinas virtuales de generación 2 para Hyper-V

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Usa la configuración de seguridad de la máquina virtual en el Administrador de Hyper-V para ayudar a proteger los datos y el estado de una máquina virtual. Puedes proteger las máquinas virtuales de la inspección, el robo y la manipulación tanto del malware que se puede ejecutar en el host como de los administradores del centro de datos. El nivel de seguridad que se obtiene depende del hardware del host que se ejecuta, de la generación de las máquinas virtuales y de si se configura el servicio, denominado Servicio de protección de host, que autoriza a los hosts a iniciar máquinas virtuales blindadas.

El Servicio de protección de host es un nuevo rol de Windows Server 2016. Identifica los hosts de Hyper-V legítimos y les permite ejecutar una máquina virtual determinada. Lo más habitual es configurar el Servicio de protección de host para un centro de datos. No obstante, puedes crear una máquina virtual blindada para ejecutarla localmente sin configurar un Servicio de protección de host. Posteriormente, puedes distribuir la máquina virtual blindada a un tejido de protección de host.

Si no has configurado el Servicio de protección de host o lo ejecutas en modo local en el host de Hyper-V, y el host tiene la clave de protección del propietario de la máquina virtual, puedes cambiar la configuración que se describe en este tema.   Un propietario de una clave de protección es una organización que crea y comparte una clave pública o privada para poseer todas las máquinas virtuales creadas con esa clave.

Para obtener información sobre cómo hacer que las máquinas virtuales sean más seguras con el Servicio de protección de host, consulta los siguientes recursos.

- [Protección del tejido: protección de los secretos de inquilino en Hyper-V (vídeo de Ignite)](https://go.microsoft.com/fwlink/?LinkId=746379)
- [VM blindadas y tejido protegido](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Característica Arranque seguro del Administrador de Hyper-V

Arranque seguro es una característica disponibles en las máquinas virtuales de generación 2 que ayuda a evitar la ejecución durante el arranque de firmware, sistemas operativos o controladores Unified Extensible Firmware Interface (UEFI; también conocidos como ROM opcionales) no autorizados. La característica Arranque seguro está habilitada de forma predeterminada. Puedes usar el arranque seguro con las máquinas virtuales de generación 2 que ejecutan sistemas operativos de distribución Windows o Linux.

Las plantillas que se describen en la tabla siguiente hacen referencia a los certificados necesarios para comprobar la integridad del proceso de arranque.

|Nombre de plantilla|Descripción|
|-----------------|---------------|
|Microsoft Windows|Selecciona esta opción para proteger el arranque de la máquina virtual para un sistema operativo Windows.|
|Entidad de certificación UEFI de Microsoft|Selecciona esta opción para proteger el arranque de la máquina virtual para un sistema operativo de distribución Linux.|
|VM blindada de código abierto|Esta plantilla se utiliza para proteger el arranque de las [VM blindadas basadas en Linux](../../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template.md).|

Para obtener más información, vea los temas siguientes:

- [Información general de seguridad de Windows 10](/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)
- [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)
- [Máquinas virtuales Linux y FreeBSD en Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configuración de Compatibilidad con cifrado en el Administrador de Hyper-V

Para ayudar a proteger los datos y el estado de la máquina virtual, puedes seleccionar las siguientes opciones de compatibilidad con el cifrado.

- **Habilitar el módulo de plataforma segura**: esta opción hace que un chip de Módulo de plataforma segura virtualizado (TPM) esté disponible para la máquina virtual. Esto permite al invitado cifrar el disco de la máquina virtual mediante BitLocker.
  - Si el host de Hyper-V ejecuta Windows 10 1511, tendrás que habilitar el modo de usuario aislado.
- **Cifrar el estado y el tráfico de migración de la máquina virtual**: cifra el estado guardado de la máquina virtual y el tráfico de migración en vivo.

### <a name="enable-isolated-user-mode"></a>Habilitar el modo de usuario aislado

Si seleccionas **Habilitar el módulo de plataforma segura** en los hosts de Hyper-V que ejecutan versiones de Windows anteriores a la Actualización de aniversario de Windows 10, debes habilitar el modo de usuario aislado. No es necesario hacerlo para los hosts de Hyper-V que ejecutan Windows Server 2016 o la Actualización de aniversario de Windows 10 o posterior.

El modo de usuario aislado es el entorno de tiempo de ejecución que hospeda las aplicaciones de seguridad en el modo seguro virtual en el host de Hyper-V. El modo seguro virtual se usa para proteger el estado del chip de TPM virtual.

Para habilitar el modo de usuario aislado en el host de Hyper-V que ejecuta versiones anteriores de Windows 10:

1.  Abra Windows PowerShell como administrador.

2.  Ejecute los siguientes comandos:

    ```
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force

    ```

Puedes migrar una máquina virtual con TPM virtual habilitado a cualquier host que ejecute Windows Server 2016, Windows 10, compilación 10586 o versiones posteriores. No obstante, si se migra a otro host, es posible que no puedas iniciarla. Debes actualizar el protector de clave de esa máquina virtual para permitir que el nuevo host ejecute la máquina virtual. Para obtener más información, consulta [VM blindadas y tejido protegido](https://go.microsoft.com/fwlink/?LinkId=746381) y [Requisitos del sistema para Hyper-V en Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).

## <a name="security-policy-in-hyper-v-manager"></a>Directiva de seguridad del Administrador de Hyper-V
Para mejorar la seguridad de las máquinas virtuales, usa la opción **Habilitar blindaje** para deshabilitar características de administración como la conexión de la consola, PowerShell Direct y algunos componentes de integración. Si habilitas esta opción, se seleccionan y aplican las opciones **Arranque seguro**, **Habilitar el módulo de plataforma segura** y **Cifrar el estado y el tráfico de migración de la máquina virtual**.

Puedes ejecutar la máquina virtual blindada localmente sin configurar un Servicio de protección de host. No obstante, si se migra a otro host, es posible que no puedas iniciarla. Debes actualizar el protector de clave de esa máquina virtual para permitir que el nuevo host ejecute la máquina virtual. Para obtener más información, consulta [VM blindadas y tejido protegido](https://go.microsoft.com/fwlink/?LinkId=746381).

Para obtener información sobre la seguridad en Windows Server, consulta [Seguridad y control](../../../security/Security-and-Assurance.yml).