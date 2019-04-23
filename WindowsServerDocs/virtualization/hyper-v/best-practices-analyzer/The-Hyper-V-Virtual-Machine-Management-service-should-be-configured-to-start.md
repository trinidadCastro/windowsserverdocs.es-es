---
title: El servicio de administración de máquinas virtuales de Hyper-V debe estar configurado para iniciarse automáticamente
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c33f81678d7fdc71e81834a002fd3d7917a6f632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833256"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>El servicio de administración de máquinas virtuales de Hyper-V debe estar configurado para iniciarse automáticamente

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  

En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.

## <a name="issue"></a>Problema  
  
*El servicio de administración de máquinas virtuales de Hyper-V no está configurado para iniciarse automáticamente.*  
  
## <a name="impact"></a>Impacto  
  
*No se pueden administrar las máquinas virtuales hasta que se inicie el servicio.*  
  
Las máquinas virtuales que ejecutan se seguirán ejecutando. Sin embargo, no podrá administrar máquinas virtuales, o crear o eliminarlas hasta que el servicio se está ejecutando.  
  
## <a name="resolution"></a>Resolución  
  
*Utilice la herramienta Servicios de complemento o sc config de línea de comandos para volver a configurar el servicio se inicie automáticamente.*  
  
> [!TIP]  
> Si no se encuentra el servicio en la aplicación de escritorio o la herramienta de línea de comandos informa de que el servicio no existe, las herramientas de administración de Hyper-V probablemente no se instalan. Para instalarlos:  
>   
> - En Windows Server, abra el administrador del servidor y use el Asistente para agregar Roles y características. Para obtener más información, consulte [instalar el rol de Hyper-V en Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
> - En Windows, desde el escritorio, comience a escribir **programas**, haga clic en **programas y características** (panel de Control) > **o desactivar las características de Windows Active**  >   **Hyper-V** > **las herramientas de administración de Hyper-V**. A continuación, haga clic en **Aceptar**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Para volver a configurar el servicio para empezar a usar automáticamente la aplicación de escritorio de servicios  
  
1.  Abra la aplicación de escritorio de servicios. (Haga clic en **iniciar**, haga clic en el cuadro de búsqueda, comience a escribir **servicios**y, a continuación, haga clic en servicios en la lista de resultados.  
  
2.  En el panel de detalles, haga clic en **administración de máquinas virtuales de Hyper-V**y, a continuación, haga clic en **propiedades**.  
  
3.  En el **General** ficha **inicio** escriba, haga clic en **automática**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>Para volver a configurar el servicio para empezar a usar automáticamente el comando SC Config  
  
1.  Abra Windows PowerShell.  
  
2.  Tipo:  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  Si ya no se está ejecutando el servicio, escriba:  
  
    ```  
    start-service -name vmms  
    ```  
  


