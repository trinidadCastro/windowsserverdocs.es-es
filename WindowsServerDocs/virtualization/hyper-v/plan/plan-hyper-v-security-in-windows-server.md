---
title: Planear la seguridad de Hyper-V en Windows Server
description: Proporciona una lista de consideraciones de seguridad para hosts de Hyper-v y máquinas virtuales
ms.prod: windows-server-threshold
ms.service: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 115db481-b57e-41c3-8354-504f4bc6113a
manager: dongill
author: larsiwer
ms.author: kathyDav
ms.date: 08/03/2018
ms.openlocfilehash: 462124e1907ef03aa7746dd050be3e8c84c7abda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877416"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Planear la seguridad de Hyper-V en Windows Server

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server, Microsoft Hyper-V Server 2019 de 2019

Proteger el sistema operativo de host de Hyper-V, las máquinas virtuales, archivos de configuración y datos de máquinas virtuales. Utilice la siguiente lista de las prácticas recomendadas como una lista de comprobación para ayudarle a proteger su entorno de Hyper-V.

## <a name="secure-the-hyper-v-host"></a>Proteger el host de Hyper-V
- **Mantenga el host de proteger el sistema operativo.**
    - Minimizar la superficie de ataque mediante la opción de instalación mínima de Windows Server que necesite para el sistema operativo de administración. Para obtener más información, consulte el [sección de opciones de instalación](/windows-server/windows-server#installation-options) de la biblioteca de contenido técnica de Windows Server. No se recomienda que ejecute las cargas de trabajo de producción en Hyper-V en Windows 10.
    - Mantener el sistema operativo de host de Hyper-V, el firmware y los controladores de dispositivo actualizados con las últimas actualizaciones de seguridad. Compruebe las recomendaciones de su proveedor para actualizar el firmware y los controladores.
    - No utilice el host de Hyper-V como estación de trabajo ni instalar ningún software innecesario.
    - Administrar el host de Hyper-V de forma remota. Si debe administrar el host de Hyper-V localmente, usar a Credential Guard. Para obtener más información, consulta [Proteger las credenciales de dominio derivadas con Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).
    - Habilitar las directivas de integridad de código. Usar la seguridad basada en virtualización proteger servicios de integridad de código. Para obtener más información, consulte [Guía de implementación de Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
- **Usar una red segura.**
    - Usar una red independiente con un adaptador de red dedicada para el equipo de Hyper-V físico.
    - Usar una red privada o segura a las configuraciones de máquina virtual de acceso y los archivos de disco duro virtual.
    - Usar una red privada y dedicada para el tráfico de migración en vivo. Considere la posibilidad de habilitar IPSec en esta red para usar el cifrado y proteger los datos de la máquina virtual a través de la red durante la migración. Para obtener más información, consulte [configurar hosts para migración en vivo sin clústeres de conmutación por error](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).
- **Proteger el tráfico de migración de almacenamiento.** 

    Usar SMB 3.0 para el cifrado de extremo a otro de los datos SMB y la protección de datos, manipulación o interceptación en redes no confiables. Usar una red privada para tener acceso al contenido del recurso compartido de SMB para evitar ataques man-in-the-middle. Para obtener más información, consulte [mejoras de seguridad SMB](https://technet.microsoft.com/library/dn551363.aspx). 
- **Configure los hosts para formar parte de un tejido protegido.** 

    Para obtener más información, consulte [protegido fabric](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
- **Proteger los dispositivos.** 

    Proteger los dispositivos de almacenamiento donde se guardan los archivos de recursos de máquina virtual.
    
- **Proteger la unidad de disco dura.** 

    Use el cifrado de unidad BitLocker para proteger los recursos.
    
- **El sistema operativo de host de Hyper-V de protección.** 

    Use las recomendaciones de configuración de seguridad de línea de base se describe en el [línea de base de seguridad de Windows Server](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Conceda los permisos adecuados.**
    - Agregue los usuarios que necesitan para administrar el host de Hyper-V al grupo de administradores de Hyper-V.
    - No conceda a los administradores de la máquina virtual permisos en Hyper-V de host del sistema operativo.

- **Configurar opciones para Hyper-V y las exclusiones del antivirus.**  

    Ya tiene Windows Defender [exclusiones automáticas](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) configurado. Para obtener más información acerca de las exclusiones, consulte [recomienda las exclusiones del antivirus para hosts de Hyper-V](https://support.microsoft.com/kb/3105657). 

- **No Monte el VHD desconocidos.** Esto puede exponer el host a los ataques de nivel de sistema de archivos.

- **No habilite el anidamiento en el entorno de producción a menos que sea necesario.**

    Si habilita el anidamiento, hipervisores no admitidos no se ejecutan en una máquina virtual.  

Para entornos más seguros:

- **Usar hardware con un chip de módulo de plataforma segura (TPM) 2.0 para configurar un tejido protegido.** 

    Para obtener más información, consulte [requisitos del sistema para Hyper-V en Windows Server 2016](../system-requirements-for-hyper-v-on-windows.md).

## <a name="secure-virtual-machines"></a>Proteger las máquinas virtuales
- **Creación de generación 2 máquinas virtuales para sistemas operativos invitados admitidos.** 

    Para obtener más información, consulte [configuración de seguridad de la generación 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Habilitar el arranque seguro.** 

    Para obtener más información, consulte [configuración de seguridad de la generación 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Mantenga seguros de sistema operativo invitado.**

    - Instalar las últimas actualizaciones de seguridad antes de activar una máquina virtual en un entorno de producción.
    - Instalar integration services para los sistemas operativos invitados compatibles que necesita y mantenerlo actualizado. Las actualizaciones del servicio de integración para huéspedes que ejecutan versiones compatibles de Windows están disponibles a través de Windows Update.
    - Reforzar el sistema operativo que se ejecuta en cada máquina virtual según el rol que desempeña. Use las recomendaciones de configuración de seguridad de línea base que se describen en la [Windows Security Baseline](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Usar una red segura.** 

    Asegúrese de que los adaptadores de red virtual conectan al conmutador virtual correcto y han aplicado los límites y configuración de seguridad apropiada.
    
- **Store discos duros virtuales y los archivos de instantáneas en una ubicación segura.**

- **Proteger los dispositivos.** 

    Configurar solo los dispositivos necesarios para una máquina virtual. No habilite la asignación discreta de dispositivos en su entorno de producción a menos que necesite para un escenario concreto. Si habilita esta opción, asegúrese de que exponer solo los dispositivos de los proveedores de confianza. 
    
- **Configurar antivirus, firewall y software de detección de intrusiones** dentro de las máquinas virtuales según corresponda según el rol de máquina virtual.

- **Habilitar la seguridad de virtualización de acuerdo para los invitados que ejecutan Windows 10 o Windows Server 2016 o posterior.** 

    Para obtener más información, consulte el [Guía de implementación de Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
    
- **Habilitar solo discretos asignación del dispositivo si es necesario para una carga de trabajo específica**. 

    Dada la naturaleza de pasar a través de un dispositivo físico, trabajar con el fabricante del dispositivo para comprender si debe usarse en un entorno seguro.

Para entornos más seguros:

- **Implementar máquinas virtuales con el blindaje habilitado e implementarlas en un tejido protegido.** 

    Para obtener más información, consulte [configuración de seguridad de la generación 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) y [protegido fabric](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
