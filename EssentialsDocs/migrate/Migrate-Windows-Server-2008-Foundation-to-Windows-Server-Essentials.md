---
title: Migrar de Windows Server 2008 Foundation a Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22fc0a4-cb82-4e60-afe6-2d03145745e7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 22721833d3ba97c7860c949764d565415bbc7cab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-windows-server-2008-foundation-to-windows-server-essentials"></a>Migrar de Windows Server 2008 Foundation a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta guía describe cómo migrar un dominio de Windows Server 2008 Foundation existente a Windows Server® 2012 Essentials en un hardware nuevo y, a continuación, migrar las configuraciones y datos. Esta guía también describe cómo quitar el servidor existente de la red de Windows Server Essentials después de finalizar la migración.  
  
> [!NOTE]
>  Para evitar problemas durante la migración, el equipo de desarrollo del producto de Windows Server Essentials recomienda encarecidamente que leas este documento antes de empezar la migración.  
  
## <a name="additional-resources"></a>Recursos adicionales  
 Para obtener vínculos a información adicional, herramientas y recursos de la Comunidad que te guiarán por el proceso de migración, consulta [migración de Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520).  
  
## <a name="terms-and-definitions"></a>Términos y definiciones  
 **Servidor de origen:** el servidor existente desde el que vas a migrar la configuración y los datos.  
  
 **Servidor de destino:** el servidor nuevo a la que vas a migrar la configuración y los datos.  
  
## <a name="migration-process-summary"></a>Resumen del proceso de migración  
 Esta guía de migración incluye los siguientes pasos:  
  

1.  [Preparar la migración de servidor de origen para Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debes asegurarte de que el servidor de origen y la red están listos para la migración. En esta sección te guiará a través de la copia de seguridad del servidor de origen, evaluar el estado del sistema de servidor de origen, instalar el service Pack y correcciones más recientes y comprobación de la configuración de red.  
  
2.  [Instalar Windows Server Essentials en modo de migración](Install-Windows-Server-Essentials-in-migration-mode.md).  Esta sección describe los pasos que debes seguir para instalar Windows Server Essentials en el servidor de destino en modo de migración.  
  
3.  [Únete a los equipos a la nueva red de Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-network.md).  Esta sección se describe unir los equipos cliente a la nueva red de Windows Server Essentials y actualizar la configuración de directiva de grupo.  
  
4.  [Mover la configuración de Windows Server 2008 Foundation y los datos al servidor de destino](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre la migración de datos y opciones de configuración del servidor de origen.  
  
5.  [Degradar y quitar el servidor de origen de la nueva red de Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de directiva de grupo y degradar al servidor de origen.  
  
6.  [Realizar tareas posteriores a la migración de migración de Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Después de terminar de migrar todas las configuraciones y datos a Windows Server Essentials, puedes asignar equipos permitidos a cuentas de usuario.  
  
7.  [Ejecuta el analizador de procedimientos recomendados de Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termines de migrar la configuración y datos a Windows Server Essentials, debes ejecutar el BPA de Windows Server Essentials.  

1.  [Preparar la migración de servidor de origen para Windows Server Essentials](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debes asegurarte de que el servidor de origen y la red están listos para la migración. En esta sección te guiará a través de la copia de seguridad del servidor de origen, evaluar el estado del sistema de servidor de origen, instalar el service Pack y correcciones más recientes y comprobación de la configuración de red.  
  
2.  [Instalar Windows Server Essentials en modo de migración](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md).  Esta sección describe los pasos que debes seguir para instalar Windows Server Essentials en el servidor de destino en modo de migración.  
  
3.  [Únete a los equipos a la nueva red de Windows Server Essentials](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-network.md).  Esta sección se describe unir los equipos cliente a la nueva red de Windows Server Essentials y actualizar la configuración de directiva de grupo.  
  
4.  [Mover la configuración de Windows Server 2008 Foundation y los datos al servidor de destino](../migrate/Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre la migración de datos y opciones de configuración del servidor de origen.  
  
5.  [Degradar y quitar el servidor de origen de la nueva red de Windows Server Essentials](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de directiva de grupo y degradar al servidor de origen.  
  
6.  [Realizar tareas posteriores a la migración de migración de Windows Server Essentials](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Después de terminar de migrar todas las configuraciones y datos a Windows Server Essentials, puedes asignar equipos permitidos a cuentas de usuario.  
  
7.  [Ejecuta el analizador de procedimientos recomendados de Windows Server Essentials](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termines de migrar la configuración y datos a Windows Server Essentials, debes ejecutar el BPA de Windows Server Essentials.  

  
 Varios de los procedimientos de migración requieren que abra una ventana del símbolo del sistema como administrador.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>Para abrir una ventana de símbolo del sistema en el servidor de origen como administrador  
  
1.  Haz clic en **inicio**.  
  
2.  En el cuadro de búsqueda, escriba **cmd**.  
  
3.  En la lista de resultados, haz clic en **cmd**y, a continuación, haz clic en **ejecutar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir una ventana de símbolo del sistema en el servidor de destino como administrador  
  
1.  En la **inicio** pantalla, en el cuadro de búsqueda, escriba **cmd**.  
  
2.  En la lista de resultados, haz clic en **cmd**y, a continuación, haz clic en **ejecutar como administrador**.
