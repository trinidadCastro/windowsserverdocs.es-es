---
title: Debe ejecutar el servicio de administración de máquinas virtuales de Hyper-V
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: 58886b68ca30ddeb064fc12c6cb4c00183399715
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826116"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>Debe ejecutar el servicio de administración de máquinas virtuales de Hyper-V

>Se aplica a: Windows Server 2016
  
Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Requisitos previos|  

En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.

## <a name="issue"></a>Problema  
  
*No se está ejecutando el servicio de administración de máquinas virtuales.*  
  
## <a name="impact"></a>Impacto  
  
*No se puede realizar ninguna operación de administración de la máquina virtual.*  
  
Las máquinas virtuales que ejecutan se seguirán ejecutando. Sin embargo, no podrá administrar máquinas virtuales, o crear o eliminarlas hasta que el servicio se está ejecutando.  
  
## <a name="resolution"></a>Resolución  
  
*Use el complemento Servicios o la herramienta de línea de comandos Sc config para volver a configurar el servicio se inicie automáticamente.*  
  
> [!TIP]  
> Si no se encuentra el servicio en la aplicación de escritorio o la herramienta de línea de comandos informa de que el servicio no existe, las herramientas de administración de Hyper-V probablemente no se instalan. Y si no es capaz de ver la consola MMC de Hyper-V desde el menú Inicio, debe instalar las herramientas de administración de Hyper-V.

Para instalar las herramientas de administración de Hyper-V:  
>   
> - En Windows Server, abra el administrador del servidor y use el Asistente para agregar Roles y características. Para obtener más información, consulte [instalar el rol de Hyper-V en Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  También puede usar PowerShell para instalar las herramientas (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - En Windows, desde el escritorio, comience a escribir **programas**, haga clic en **programas y características** (panel de Control) > **o desactivar las características de Windows Active**  >   **Hyper-V** > **las herramientas de administración de Hyper-V**. A continuación, haga clic en **Aceptar**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Para volver a configurar el servicio para empezar a usar automáticamente la aplicación de escritorio de servicios  
  
1.  Abra la aplicación de escritorio de servicios. (Haga clic en **iniciar**, haga clic en el **Iniciar búsqueda** , escriba **services.msc**, y, a continuación, presione ENTRAR.)  
  
2.  En el panel de detalles, haga clic en **administración de máquinas virtuales de Hyper-V**y, a continuación, haga clic en **propiedades**.  
  
3.  En el **General** ficha **inicio** escriba, haga clic en **automática**.  
  
4.  Para iniciar el servicio, haga clic en **iniciar**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>Para volver a configurar el servicio para empezar a usar automáticamente SC Config  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **iniciar** y comience a escribir **Windows PowerShell**.)  
  
2.  Haga clic en **Windows PowerShell** y haga clic en **ejecutar como administrador**.  
  
3.  Para volver a configurar el servicio, escriba:  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  Para iniciar el servicio, escriba:  
  
    ```  
    sc start vmms  
    ```  
  
Si el servicio ya está configurado para iniciarse automáticamente y deberá reiniciar el servicio, puede hacerlo desde el Administrador de Hyper-V, o desde el comando "sc start vmms" mostrado anteriormente.  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>Para reiniciar el servicio de administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de navegación, haga clic en el nombre del servidor si aún no está seleccionada.  
  
3.  En el **acciones** panel, haga clic en **iniciar servicio**.  
  


