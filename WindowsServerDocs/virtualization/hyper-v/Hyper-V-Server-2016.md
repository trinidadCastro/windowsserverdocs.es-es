---
title: Microsoft Hyper-V Server 2016
ms.prod: windows-server
ms.technology: compute-hyper-v
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: kbdazure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: f5c68f260ff90a07a17a39fdbb881ddcbdb857ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853268"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-V Server 2016 es un stand\-producto independiente que contiene solo el hipervisor de Windows, un modelo de controlador de Windows Server y componentes de virtualización. Proporciona una solución de virtualización sencilla y confiable para ayudarle a mejorar el uso del servidor y reducir los costos.

La tecnología de hipervisor de Windows en Microsoft Hyper-V Server 2016 es la misma que la del rol Hyper\-V en Windows Server 2016. Por lo tanto, gran parte del contenido disponible para el rol Hyper\-V en Windows Server 2016 también se aplica a Microsoft Hyper-V Server 2016.

## <a name="hyper-v-server-resources-for-it-pros"></a>Recursos de Hyper\-V Server para profesionales de ti

|Tareas|Recursos|
|-|-|
|![Cumple el símbolo de requisitos](media/All_Symbols_MeetsRequirements.png)|**Evaluar Hyper-V**<p>[información general sobre la tecnología de Hyper-V](hyper-v-technology-overview.md) -   <br />- las novedades [de Hyper-V en Windows Server 2016](what-s-new-in-hyper-v-on-windows.md)<br />-   [requisitos del sistema para Hyper-V en Windows Server 2016](system-requirements-for-hyper-v-on-windows.md)<br />-   [sistemas operativos invitados de Windows admitidos para Hyper-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [compatible con Linux y FreeBSD virtual machines](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />[compatibilidad de características de -   por generación e invitado](hyper-v-feature-compatibility-by-generation-and-guest.md)<p>**Planeación de Hyper-V**<p>-Decida qué [generación de máquina virtual](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) satisface sus necesidades. <br/>-Si va a mover o importar máquinas virtuales, decida cuándo desea [actualizar la versión](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md). <br />[escalabilidad](plan/plan-hyper-v-scalability-in-windows-server.md) -  <br />[redes](plan/plan-hyper-v-networking-in-windows-server.md) de -  <br />[seguridad](plan/plan-hyper-v-security-in-windows-server.md) de - |
|![Símbolo de introducción](media/All_Symbols_GetStarted.png)|**Introducción a Hyper-V Server**<p>[Descargue e instale Microsoft Hyper\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016). Esto instala el hipervisor de Windows, un modelo de controlador de Windows Server y componentes de virtualización. Es similar a ejecutar la opción de instalación Server Core de Windows Server 2016 y el rol de Hyper\-V.|
|![Administrar símbolo](media/All_Symbols_Administrator.png)|**Configurar y administrar el servidor de Hyper-V**<p>Hyper\-V Server no tiene una interfaz gráfica de usuario \(\)GUI. Puede usar las siguientes herramientas para configurar y administrar el servidor de Hyper\-V.<p>-   [configurar una instalación Server Core de Windows server 2016 con Sconfig. cmd](../../get-started/sconfig-on-ws2016.md) para actualizar la configuración de dominio o grupo de trabajo, cambiar Windows Update configuración, habilitar la administración remota, etc.<br />-Use un [símbolo del sistema](../../administration/windows-commands/windows-commands.md) normal para los comandos que no están disponibles en Sconfig.<br />-Usar el [Administrador de hyper\-v](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management) o [Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm) para administrar de forma remota Hyper\-v Server. Para usar el administrador de Hyper\-V, [Instale el rol de hyper\-v en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) o [Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md).<br />-Consulte [instalación de Server Core](../../get-started/getting-started-with-server-core.md) para obtener opciones de administración adicionales para las funciones básicas de servidor que no son específicas de Hyper\-V. La mayoría de los métodos de administración documentados también funcionan con Hyper\-V Server.<p>**Configuración y administración de máquinas virtuales de Hyper\-V**<p>-   [crear un conmutador virtual para máquinas virtuales de Hyper-V](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [crear una máquina virtual en Hyper-V](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [elegir entre los puntos de control estándar o de producción](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [habilitar o deshabilitar puntos de control](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [administrar máquinas virtuales Windows con PowerShell Direct](manage/manage-windows-virtual-machines-with-powershell-direct.md) <p>**Implementación**<p>-   [configurar hosts para la migración en vivo sin clústeres de conmutación por error](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [actualizar nodos de clúster de Windows Server](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [actualizar la versión de la máquina virtual](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
