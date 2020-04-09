---
title: Migración de Windows Server 2008 Foundation a Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f22fc0a4-cb82-4e60-afe6-2d03145745e7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 52763340ae2df60f37757dc2fb1344fb891c8f7a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852548"
---
# <a name="migrate-windows-server-2008-foundation-to-windows-server-essentials"></a>Migración de Windows Server 2008 Foundation a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En esta guía se describe cómo migrar un dominio existente de Windows Server 2008 Foundation a Windows Server&reg; 2012 Essentials en hardware nuevo y, después, migrar la configuración y los datos. En esta guía también se describe cómo quitar el servidor existente de la red de Windows Server Essentials después de finalizar la migración.  
  
> [!NOTE]
>  Para evitar problemas durante la migración, el equipo de desarrollo de productos de Windows Server Essentials recomienda encarecidamente que lea este documento antes de comenzar la migración.  
  
## <a name="additional-resources"></a>Recursos adicionales  
 Para obtener vínculos a información adicional, herramientas y recursos de la comunidad que le guiarán a través del proceso de migración, consulte [migración de Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520).  
  
## <a name="terms-and-definitions"></a>Términos y definiciones  
 **Servidor de origen:** El servidor existente desde el que va a migrar la configuración y los datos.  
  
 **Servidor de destino:** El nuevo servidor al que va a migrar la configuración y los datos.  
  
## <a name="migration-process-summary"></a>Resumen del proceso de migración  
 Esta guía de migración incluye los siguientes pasos:  
  

1.  [Preparar el servidor de origen para la migración a Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debe asegurarse de que el servidor de origen y la red estén listos para la migración. Esta sección le guía por el proceso de copia de seguridad del servidor de origen, evaluación del estado de mantenimiento del servidor de origen, instalación de los Service Packs y correcciones más recientes y comprobación de la configuración de la red.  
  
2.  [Instale Windows Server Essentials en modo de migración](Install-Windows-Server-Essentials-in-migration-mode.md).  En esta sección se describen los pasos que debe seguir para instalar Windows Server Essentials en el servidor de destino en modo de migración.  
  
3.  [Unir equipos a la nueva red de Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-network.md).  En esta sección se describe cómo unir equipos cliente a la nueva red de Windows Server Essentials y cómo actualizar la configuración de directiva de grupo.  
  
4.  [Mueva los datos y la configuración de Windows Server 2008 Foundation al servidor de destino](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre cómo migrar los datos y la configuración desde el servidor de origen.  
  
5.  [Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de la Directiva de grupo y disminuir el nivel del servidor de origen.  
  
6.  [Realizar tareas posteriores a la migración para la migración de Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Cuando termine de migrar todos los datos y la configuración a Windows Server Essentials, puede asignar los equipos permitidos a las cuentas de usuario.  
  
7.  [Ejecute el analizador de procedimientos recomendados de Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termine de migrar la configuración y los datos a Windows Server Essentials, debe ejecutar el BPA de Windows Server Essentials.  

1.  [Preparar el servidor de origen para la migración a Windows Server Essentials](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debe asegurarse de que el servidor de origen y la red estén listos para la migración. Esta sección le guía por el proceso de copia de seguridad del servidor de origen, evaluación del estado de mantenimiento del servidor de origen, instalación de los Service Packs y correcciones más recientes y comprobación de la configuración de la red.  
  
2.  [Instale Windows Server Essentials en modo de migración](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md).  En esta sección se describen los pasos que debe seguir para instalar Windows Server Essentials en el servidor de destino en modo de migración.  
  
3.  [Unir equipos a la nueva red de Windows Server Essentials](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-network.md).  En esta sección se describe cómo unir equipos cliente a la nueva red de Windows Server Essentials y cómo actualizar la configuración de directiva de grupo.  
  
4.  [Mueva los datos y la configuración de Windows Server 2008 Foundation al servidor de destino](../migrate/Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre cómo migrar los datos y la configuración desde el servidor de origen.  
  
5.  [Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de la Directiva de grupo y disminuir el nivel del servidor de origen.  
  
6.  [Realizar tareas posteriores a la migración para la migración de Windows Server Essentials](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Cuando termine de migrar todos los datos y la configuración a Windows Server Essentials, puede asignar los equipos permitidos a las cuentas de usuario.  
  
7.  [Ejecute el analizador de procedimientos recomendados de Windows Server Essentials](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termine de migrar la configuración y los datos a Windows Server Essentials, debe ejecutar el BPA de Windows Server Essentials.  

  
 Algunos de los procedimientos de migración requieren que abra una ventana del símbolo del sistema como administrador.  
  
###  <a name="to-open-a-command-prompt-window-on-the-source-server-as-an-administrator"></a><a name="BKMK_OpenACommandPromptAsAdmin"></a>Para abrir una ventana del símbolo del sistema en el servidor de origen como administrador  
  
1.  Haga clic en **Inicio**.  
  
2.  En el cuadro de búsqueda, escriba **cmd**.  
  
3.  En la lista de resultados, haga clic con el botón secundario en **cmd** y, después, haga clic de nuevo en **Ejecutar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir una ventana del símbolo del sistema como administrador en el servidor de destino  
  
1.  En la pantalla **Inicio**, en el cuadro de búsqueda, escriba **cmd**.  
  
2.  En la lista de resultados, haga clic con el botón secundario en **cmd** y, después, haga clic de nuevo en **Ejecutar como administrador**.
