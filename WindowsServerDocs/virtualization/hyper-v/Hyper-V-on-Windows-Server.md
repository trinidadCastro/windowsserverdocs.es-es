---
title: Hyper-V en Windows Server
description: Proporciona vínculos a artículos claves sobre probando, planeación, implementación y administración de Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: KBDAzure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: 1a658b611b68d7ecde64bdf0f8a318cc1af2804c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880266"
---
# <a name="hyper-v-on-windows-server"></a>Hyper-V en Windows Server

>Se aplica a: Windows Server 2016, Windows Server 2019

El rol de Hyper-V en Windows Server permite crear un entorno informático virtualizado donde puede crear y administrar máquinas virtuales. Puede ejecutar varios sistemas operativos en un equipo físico y aislar los sistemas operativos entre sí. Con esta tecnología, puede mejorar la eficacia de sus recursos informáticos y liberar los recursos de hardware.

Vea los temas de la tabla siguiente para obtener más información sobre Hyper-V en Windows Server.

## <a name="hyper-v-resources-for-it-pros"></a>Recursos de Hyper-V para los profesionales de TI

|Tarea |Recursos|
|---|---|
|![Marca de verificación y el icono de documento para mostrar los requisitos se cumplen](media/All_Symbols_MeetsRequirements.png)|**Evalúe Hyper-V**<br /><br />- [Introducción a la tecnología Hyper-V](Hyper-V-Technology-Overview.md)<br />- [Novedades de Hyper-V en Windows Server](What-s-new-in-Hyper-V-on-Windows.md)<br />- [Requisitos del sistema para Hyper-V en Windows Server](System-requirements-for-Hyper-V-on-Windows.md)<br />- [Windows invitado sistemas operativos compatibles con Hyper-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />- [De máquinas virtuales Linux y FreeBSD compatibles](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)<br />- [Compatibilidad de características por generación e invitado](Hyper-V-feature-compatibility-by-generation-and-guest.md) <br /><br />**Plan para Hyper-V**<br /><br />- [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />- [Planear la escalabilidad de Hyper-V en Windows Server](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [Planear funciones de red de Hyper-V en Windows Server](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [Planear la seguridad de Hyper-V en Windows Server](plan/plan-hyper-v-security-in-windows-server.md)|
|![Icono del cursor y de proyección solar](media/All_Symbols_GetStarted.png)|**Introducción a Hyper-V**<br /><br />- [Descargue e instale Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<br /><br />**Server Core o GUI opción de instalación de Windows Server 2019 como host de máquina virtual**<br /><br />- [Instalar el rol de Hyper-V en Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [Crear un conmutador virtual para las máquinas virtuales de Hyper-V](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [Crear una máquina virtual de Hyper-V](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![Icono de persona y herramientas](media/All_Symbols_Administrator.png)|**Actualizar los hosts de Hyper-V y máquinas virtuales**<br /><br />- [Actualizar los nodos de clúster de Windows Server](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />- [Actualizar la versión de la máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<br /><br />**Configurar y administrar Hyper-V**<br /><br />- [Configuración de hosts para migración en vivo sin clústeres de conmutación por error](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)<br />- [Administrar Nano Server de forma remota](../../get-started/manage-nano-server.md)<br />- [Elija entre los puntos de control estándares o de producción](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [Habilitar o deshabilitar los puntos de control](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [Administrar las máquinas virtuales de Windows con PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [Configurar réplica de Hyper-V](manage/Set-up-Hyper-V-Replica.md)|
|![Icono de burbujas de conversación](media/All_Symbols_Chat.png)|**Blogs**<br /><br />Consulte las publicaciones más recientes de los administradores de programas, los directores de producto, los desarrolladores y evaluadores en los equipos de Hyper-V y Microsoft Virtualization.<br /><br />- [Blog de virtualización](https://blogs.technet.com/b/virtualization/)<br />- [Windows Server Blog](https://blogs.technet.com/b/windowsserver/)<br />- [Blog de virtualización de Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/) (archivado)|
|![Icono de grupo de usuario](media/All_Symbols_Users_Group.png)|**Foro de y los grupos de noticias**<br /><br />¿Tiene preguntas? Hable con el equipo del producto Hyper-V, MVP y sus colegas.<br /><br />- [Windows Server Community](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)<br />- [Foro de TechNet de Windows Server Hyper-V](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverhyperv)|

## <a name="related-technologies"></a>Tecnologías relacionadas

En la tabla siguiente se enumera las tecnologías que puede desear utilizar en su entorno informático de virtualización.

|Tecnología|Descripción|
|--------------|---------------|
|[Cliente Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|Incluido con Windows 8, Windows 8.1 y Windows 10 que se pueden instalar a través de la tecnología de virtualización **programas y características** en el **Panel de Control**.|
|[Agrupación en clústeres de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|Característica de Windows Server que proporciona alta disponibilidad para hosts de Hyper-V y máquinas virtuales.|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|Componente de System Center que proporciona una solución de administración para el centro de datos virtualizado. Puede configurar y administrar los hosts de virtualización, redes y recursos de almacenamiento, por lo que puede crear e implementar máquinas virtuales y servicios en nubes privadas que haya creado.|
|[Contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/)|Usar contenedores de Hyper-V y Windows Server para ofrecer entornos estandarizados para los equipos de desarrollo, prueba y producción.|