---
title: Microsoft Hyper-V Server 2016
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: KBDAzure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: 9a1b6ea7b9abc94f63a1390b6fa18e4c8d4a1822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839376"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-V Server 2016 es una postura\-producto independiente que contiene solo el hipervisor de Windows, un modelo de controlador de Windows Server y componentes de virtualización. Proporciona una solución de virtualización confiable y simple para ayudarle a mejorar la utilización del servidor y reducir los costos.

La tecnología de hipervisor de Windows en Microsoft Hyper-V Server 2016 es igual que lo que está en el Hyper\-rol V en Windows Server 2016. Por lo tanto, gran parte del contenido disponible para Hyper\-rol V en Windows Server 2016 también se aplica a Microsoft Hyper-V Server 2016.

## <a name="hyper-v-server-resources-for-it-pros"></a>Hyper\-recursos de servidor V para los profesionales de TI

|Tareas|Recursos|
|-|-|
|![Símbolo de cumple los requisitos](media/All_Symbols_MeetsRequirements.png)|**Evalúe Hyper-V**<br /><br />-   [Introducción a la tecnología Hyper-V](hyper-v-technology-overview.md)<br />- [Novedades de Hyper-V en Windows Server 2016](what-s-new-in-hyper-v-on-windows.md)<br />-   [Requisitos del sistema para Hyper-V en Windows Server 2016](system-requirements-for-hyper-v-on-windows.md)<br />-   [Windows invitado sistemas operativos compatibles con Hyper-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [Las máquinas virtuales de FreeBSD y Linux compatible](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   [Compatibilidad de características por generación e invitado](hyper-v-feature-compatibility-by-generation-and-guest.md)<br /><br />**Plan para Hyper-V**<br /><br />-Decida que [generación de máquina virtual](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) satisfaga sus necesidades. <br/>-Si está moviendo o importar las máquinas virtuales, decidir cuándo [actualizar la versión](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md). <br />- [Escalabilidad](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [Funciones de red](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [Seguridad](plan/plan-hyper-v-security-in-windows-server.md)|
|![Obtener símbolos iniciada](media/All_Symbols_GetStarted.png)|**Introducción a Hyper-V Server**<br /><br />[Descargue e instale Microsoft Hyper\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016). Esto instala el hipervisor de Windows, un modelo de controlador de Windows Server y componentes de virtualización. Es similar a la ejecución de la opción de instalación Server Core de Windows Server 2016 y el Hyper\-rol V.|
|![Administrar símbolos](media/All_Symbols_Administrator.png)|**Configurar y administrar el servidor de Hyper-V**<br /><br />Hyper\-V Server no tiene una interfaz gráfica de usuario \(GUI\). Puede usar las herramientas siguientes para configurar y administrar Hyper\-V Server.<br /><br />-   [Configurar una instalación Server Core de Windows Server 2016 con Sconfig.cmd](../../get-started/sconfig-on-ws2016.md) para actualizar la configuración de dominio o grupo de trabajo, cambiar la actualización de Windows, habilitar la administración remota y mucho más.<br />-Use un normal [símbolo](../../administration/windows-commands/windows-commands.md) para los comandos no está disponibles en Sconfig.<br />-Use [Hyper\-administrador V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management) o [de Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm) administrar de forma remota Hyper\-V Server. Al utilizar Hyper\-V Manager, [instalar Hyper\-rol V en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) o [Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md).<br />-Consulte [instalación de Server Core](../../get-started/getting-started-with-server-core.md) para las opciones de administración adicional para las funciones de servidor básico que no son específicas de Hyper\-V. La mayoría de los métodos de administración que se documentan también funcionan con Hyper\-V Server.<br /><br />**Configurar y administrar Hyper\-V virtual machines**<br /><br />-   [Crear un conmutador virtual para las máquinas virtuales de Hyper-V](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [Crear una máquina virtual de Hyper-V](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [Elija entre los puntos de control estándares o de producción](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [Habilitar o deshabilitar los puntos de control](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [Administrar las máquinas virtuales de Windows con PowerShell Direct](manage/manage-windows-virtual-machines-with-powershell-direct.md) <br /><br />**Implementación**<br /><br />-   [Configuración de hosts para migración en vivo sin clústeres de conmutación por error](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [Actualizar los nodos de clúster de Windows Server](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [Actualizar la versión de la máquina virtual](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
