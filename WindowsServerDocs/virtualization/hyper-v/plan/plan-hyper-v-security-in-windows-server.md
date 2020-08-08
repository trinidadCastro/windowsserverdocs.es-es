---
title: Planeación de la seguridad de Hyper-V en Windows Server
description: Proporciona listas de consideraciones de seguridad para hosts de Hyper-v y máquinas virtuales
ms.topic: article
ms.assetid: 115db481-b57e-41c3-8354-504f4bc6113a
manager: dongill
author: larsiwer
ms.author: kathydav
ms.date: 08/03/2018
ms.openlocfilehash: af974edfb94ccf1a0a4844df43885198ab68d416
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996009"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Planeación de la seguridad de Hyper-V en Windows Server

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Proteja el sistema operativo del host de Hyper-V, las máquinas virtuales, los archivos de configuración y los datos de la máquina virtual. Use la siguiente lista de procedimientos recomendados como una lista de comprobación para ayudarle a proteger el entorno de Hyper-V.

## <a name="secure-the-hyper-v-host"></a>Proteger el host de Hyper-V
- **Mantenga el sistema operativo del host seguro.**
    - Minimice la superficie expuesta a ataques mediante la opción de instalación mínima de Windows Server que necesita para el sistema operativo de administración. Para obtener más información, consulte la [sección Opciones de instalación](../../../get-started-19/install-upgrade-migrate-19.md) de la biblioteca de contenido técnico de Windows Server. No se recomienda ejecutar cargas de trabajo de producción en Hyper-V en Windows 10.
    - Mantenga actualizados el sistema operativo del host de Hyper-V, el firmware y los controladores de dispositivos con las actualizaciones de seguridad más recientes. Consulte las recomendaciones del proveedor para actualizar el firmware y los controladores.
    - No use el host de Hyper-V como estación de trabajo ni instale ningún software innecesario.
    - Administrar de forma remota el host de Hyper-V. Si debe administrar el host de Hyper-V localmente, utilice Credential Guard. Para obtener más información, consulta [Proteger las credenciales de dominio derivadas con Credential Guard](/windows/access-protection/credential-guard/credential-guard).
    - Habilitar directivas de integridad de código. Usar servicios de integridad de código protegidos por la seguridad basada en la virtualización. Para obtener más información, consulte la [Guía de implementación de Device Guard](/windows/device-security/device-guard/device-guard-deployment-guide).
- **Usar una red segura.**
    - Use una red independiente con un adaptador de red dedicado para el equipo físico de Hyper-V.
    - Use una red privada o segura para tener acceso a las configuraciones de máquinas virtuales y los archivos de disco duro virtual.
    - Use una red privada o dedicada para el tráfico de migración en vivo. Considere la posibilidad de habilitar IPSec en esta red para usar el cifrado y proteger los datos de la máquina virtual que pasan por la red durante la migración. Para obtener más información, vea [configurar hosts para la migración en vivo sin clústeres de conmutación por error](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).
- **Tráfico de migración de almacenamiento seguro.**

    Use SMB 3,0 para el cifrado de un extremo a otro de datos SMB y la manipulación o interceptación de la protección de datos en redes que no son de confianza. Use una red privada para tener acceso al contenido del recurso compartido de SMB para evitar ataques de tipo "Man in the Middle". Para obtener más información, consulte [mejoras de seguridad de SMB](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn551363(v=ws.11)).
- **Configure los hosts para que formen parte de un tejido protegido.**

    Para obtener más información, consulte [tejido protegido](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
- **Proteger los dispositivos.**

    Proteger los dispositivos de almacenamiento donde se conservan los archivos de recursos de la máquina virtual.

- **Proteja el disco duro.**

    Use Cifrado de unidad BitLocker para proteger los recursos.

- **Proteja el sistema operativo del host de Hyper-V.**

    Use las recomendaciones de configuración de seguridad de línea de base descritas en la [línea de base de seguridad de Windows Server](/windows/device-security/windows-security-baselines).

- **Conceda los permisos adecuados.**
    - Agregue los usuarios que necesitan administrar el host de Hyper-V al grupo de administradores de Hyper-V.
    - No conceda permisos de administrador de máquina virtual en el sistema operativo del host de Hyper-V.

- **Configurar las exclusiones y las opciones de antivirus para Hyper-V.**

    Windows Defender ya tiene configuradas las [exclusiones automáticas](/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) . Para obtener más información acerca de las exclusiones, consulte [exclusiones de antivirus recomendadas para hosts de Hyper-V](https://support.microsoft.com/kb/3105657).

- **No monte VHD desconocidos.** Esto puede exponer el host a los ataques a nivel de sistema de archivos.

- **No habilite el anidamiento en el entorno de producción a menos que sea necesario.**

    Si habilita el anidamiento, no ejecute hipervisores no admitidos en una máquina virtual.

Para entornos más seguros:

- **Use hardware con un chip Módulo de plataforma segura (TPM) 2,0 para configurar un tejido protegido.**

    Para obtener más información, vea [requisitos del sistema para Hyper-V en Windows Server 2016](../system-requirements-for-hyper-v-on-windows.md).

## <a name="secure-virtual-machines"></a>Protección de máquinas virtuales
- **Cree máquinas virtuales de generación 2 para los sistemas operativos invitados admitidos.**

    Para obtener más información, vea [configuración de seguridad de generación 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).

- **Habilite el arranque seguro.**

    Para obtener más información, vea [configuración de seguridad de generación 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).

- **Mantenga el SO invitado seguro.**

    - Instale las actualizaciones de seguridad más recientes antes de activar una máquina virtual en un entorno de producción.
    - Instale Integration Services para los sistemas operativos invitados admitidos que lo necesiten y manténgalo actualizado. Las actualizaciones del servicio de integración para invitados que ejecutan versiones compatibles de Windows están disponibles a través de Windows Update.
    - Proteja el sistema operativo que se ejecuta en cada máquina virtual en función del rol que realiza. Use las recomendaciones de configuración de seguridad de línea de base que se describen en la [línea de base de seguridad de Windows](/windows/device-security/windows-security-baselines).

- **Usar una red segura.**

    Asegúrese de que los adaptadores de red virtual se conectan al conmutador virtual correcto y que se aplican los límites y la configuración de seguridad adecuados.

- **Almacene los discos duros virtuales y los archivos de instantáneas en una ubicación segura.**

- **Proteger los dispositivos.**

    Configurar solo los dispositivos necesarios para una máquina virtual. No habilite la asignación discreta de dispositivos en el entorno de producción a menos que lo necesite para un escenario específico. Si lo habilita, asegúrese de exponer solo los dispositivos de proveedores de confianza.

- **Configure el software antivirus, el firewall y la detección de intrusiones** en las máquinas virtuales según corresponda según el rol de máquina virtual.

- **Habilitar la seguridad basada en la virtualización para los invitados que ejecutan Windows 10 o Windows Server 2016 o posterior.**

    Para obtener más información, consulte la [Guía de implementación de Device Guard](/windows/device-security/device-guard/device-guard-deployment-guide).

- **Habilite solo la asignación discreta de dispositivos si es necesario para una carga de trabajo específica**.

    Debido a la naturaleza de pasar a través de un dispositivo físico, trabaje con el fabricante del dispositivo para saber si debe usarse en un entorno seguro.

Para entornos más seguros:

- **Implemente máquinas virtuales con blindaje habilitado e impleméntela en un tejido protegido.**

    Para obtener más información, consulte [configuración de seguridad de generación 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) y [tejido protegido](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).