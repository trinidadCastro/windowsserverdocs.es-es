---
title: El servicio de administración de máquinas virtuales de Hyper-V debe estar en ejecución
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: kbdazure
ms.date: 10/03/2016
ms.openlocfilehash: 50f101f9dad824e13fa5827175cc1c944a96a91b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859328"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>El servicio de administración de máquinas virtuales de Hyper-V debe estar en ejecución

>Se aplica a: Windows Server 2016
  
Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Requisitos previos|  

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema  
  
*El servicio necesario para administrar máquinas virtuales no se está ejecutando.*  
  
## <a name="impact"></a>Impacto  
  
*No se puede realizar ninguna operación de administración de máquinas virtuales.*  
  
Las máquinas virtuales que se están ejecutando seguirán ejecutándose. Sin embargo, no podrá administrar máquinas virtuales ni crearlas o eliminarlas hasta que se ejecute el servicio.  
  
## <a name="resolution"></a>Resolución  
  
*Use el complemento servicios o la herramienta de línea de comandos SC config para volver a configurar el servicio para que se inicie automáticamente.*  
  
> [!TIP]  
> Si no encuentra el servicio en la aplicación de escritorio o la herramienta de línea de comandos informa de que el servicio no existe, es probable que las herramientas de administración de Hyper-V no estén instaladas. Y si no puede ver la consola MMC de Hyper-V desde el menú Inicio, debe instalar las herramientas de administración de Hyper-V.

Para instalar las herramientas de administración de Hyper-V:  
>   
> - En Windows Server, abra Administrador del servidor y use el Asistente para agregar roles y características. Para obtener más información, vea [instalar el rol Hyper-V en Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  También puede usar PowerShell para instalar las herramientas (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - En Windows, desde el escritorio, empiece a escribir **programas**, haga clic en **programas y características** (panel de control) > **activar o desactivar las características de Windows** > **Hyper-v** > **herramientas de administración de Hyper-v**. A continuación, haga clic en **Aceptar**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Para volver a configurar el servicio para que se inicie automáticamente con la aplicación de escritorio servicios  
  
1.  Abra la aplicación de escritorio servicios. (Haga clic en **Inicio**, haga clic en el cuadro **Iniciar búsqueda** , escriba **Services. msc**y, a continuación, presione Entrar).  
  
2.  En el panel de detalles, haga clic con el botón secundario en **Administración de máquinas virtuales de Hyper-V**y, a continuación, haga clic en **propiedades**.  
  
3.  En la pestaña **General** , en tipo de **Inicio** , haga clic en **automático**.  
  
4.  Para iniciar el servicio, haga clic en **iniciar**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>Para volver a configurar el servicio para que se inicie automáticamente con SC config  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).  
  
2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.  
  
3.  Para volver a configurar el servicio, escriba:  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  Para iniciar el servicio, escriba:  
  
    ```  
    sc start vmms  
    ```  
  
Si el servicio ya está configurado para iniciarse automáticamente y solo tiene que reiniciar el servicio, puede hacerlo desde el administrador de Hyper-V o desde el comando SC Start vMMS mostrado anteriormente.  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>Para reiniciar el servicio desde el administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de navegación, haga clic en el nombre del servidor si aún no está seleccionado.  
  
3.  En el panel **acciones** , haga clic en **Iniciar servicio**.  
  


