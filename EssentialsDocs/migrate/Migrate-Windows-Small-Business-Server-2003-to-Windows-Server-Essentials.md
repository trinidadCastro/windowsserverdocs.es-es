---
title: Migración de Windows Small Business Server 2003 a Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 119a7fbc-2c76-4aa3-8a7f-c7073d461b5b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4c89a508745c8db36272aa8d6c646878618656f6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432882"
---
# <a name="migrate-windows-small-business-server-2003-to-windows-server-essentials"></a>Migración de Windows Small Business Server 2003 a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta guía describe cómo migrar un dominio de Windows SBS 2003 existente a Windows Server® 2012 Essentials en un hardware nuevo y, después, migrar la configuración y los datos. Esta guía también describe cómo quitar el servidor existente de la red de Windows Server Essentials después de finalizar la migración.  
  
> [!IMPORTANT]
>   Windows Server Essentials requiere un entorno de 64 bits.  Windows Server Essentials no admite un entorno de 32 bits.  
> 
> [!NOTE]
>  Para evitar problemas durante la migración, el equipo de desarrollo del producto de Windows Server Essentials recomienda encarecidamente que lea este documento antes de comenzar la migración.  
> 
> [!NOTE]
> 
>  Para migrar los datos del servidor a la versión más reciente de Windows Server Essentials, consulte [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
> 
>  Para migrar los datos del servidor a la versión más reciente de Windows Server Essentials, consulte [migrar a Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Recursos adicionales  
 Para obtener vínculos a información adicional, herramientas y recursos de la Comunidad que le guiarán a través del proceso de migración, visite la [migración de Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520) sitio Web.  
  
## <a name="terms-and-definitions"></a>Términos y definiciones  
 **Servidor de origen:** el servidor existente desde el cual se migran la configuración y los datos.  
  
 **Servidor de destino:** el nuevo servidor al cual se migran la configuración y los datos.  
  
## <a name="migration-process-summary"></a>Resumen del proceso de migración  
 Esta guía de migración incluye los siguientes pasos:  
  

1.  [Preparar la migración del servidor de origen para Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debe asegurarse de que el servidor de origen y la red estén listos para la migración. Esta sección le guía por el proceso de copia de seguridad del servidor de origen, evaluación del estado de mantenimiento del servidor de origen, instalación de los Service Packs y correcciones más recientes y comprobación de la configuración de la red.  
  
2.  [Instalar Windows Server Essentials en modo de migración](Install-Windows-Server-Essentials-in-migration-mode.md).  Esta sección describen los pasos que debe seguir para instalar Windows Server Essentials en el servidor de destino en modo de migración.  
  
3.  [Unir equipos a la nueva red de Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-network.md).  En esta sección se trata unir equipos cliente a la nueva red de Windows Server Essentials y actualizar la configuración de directiva de grupo.  
  
4.  [Mover los datos y configuración de SBS 2003 al servidor de destino](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre cómo migrar los datos y la configuración desde el servidor de origen.  
  
5.  [Habilitar la redirección de carpetas en el servidor de destino de Windows Server Essentials](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Si el redireccionamiento de carpetas está habilitado en el servidor de origen, puede habilitarlo en el servidor de destino y, a continuación, eliminar la antigua configuración de redireccionamiento de carpetas de la Directiva de grupo.  
  
6.  [Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de la Directiva de grupo y disminuir el nivel del servidor de origen.  
  
7.  [Realizar tareas postmigración para la migración de Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Cuando termine de migrar toda la configuración y datos a Windows Server Essentials, es posible que desee asignar los equipos permitidos a cuentas de usuario.  
  
8.  [Ejecutar Windows Server Essentials Best Practices Analyzer](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termine de migrar la configuración y datos a Windows Server Essentials, debe descargar y ejecutar el BPA de Windows Server Essentials.  

1.  [Preparar la migración del servidor de origen para Windows Server Essentials](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debe asegurarse de que el servidor de origen y la red estén listos para la migración. Esta sección le guía por el proceso de copia de seguridad del servidor de origen, evaluación del estado de mantenimiento del servidor de origen, instalación de los Service Packs y correcciones más recientes y comprobación de la configuración de la red.  
  
2.  [Instalar Windows Server Essentials en modo de migración](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md).  Esta sección describen los pasos que debe seguir para instalar Windows Server Essentials en el servidor de destino en modo de migración.  
  
3.  [Unir equipos a la nueva red de Windows Server Essentials](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-network.md).  En esta sección se trata unir equipos cliente a la nueva red de Windows Server Essentials y actualizar la configuración de directiva de grupo.  
  
4.  [Mover los datos y configuración de SBS 2003 al servidor de destino](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre cómo migrar los datos y la configuración desde el servidor de origen.  
  
5.  [Habilitar la redirección de carpetas en el servidor de destino de Windows Server Essentials](../migrate/Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Si el redireccionamiento de carpetas está habilitado en el servidor de origen, puede habilitarlo en el servidor de destino y, a continuación, eliminar la antigua configuración de redireccionamiento de carpetas de la Directiva de grupo.  
  
6.  [Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de la Directiva de grupo y disminuir el nivel del servidor de origen.  
  
7.  [Realizar tareas postmigración para la migración de Windows Server Essentials](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Cuando termine de migrar toda la configuración y datos a Windows Server Essentials, es posible que desee asignar los equipos permitidos a cuentas de usuario.  
  
8.  [Ejecutar Windows Server Essentials Best Practices Analyzer](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termine de migrar la configuración y datos a Windows Server Essentials, debe descargar y ejecutar el BPA de Windows Server Essentials.  

  
 Algunos de los procedimientos de migración requieren que abra una ventana del símbolo del sistema como administrador.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> Para abrir una ventana de símbolo del sistema en el servidor de origen como administrador  
  
1.  Haga clic en Inicio.  
  
2.  En el cuadro de búsqueda, escriba cmd.  
  
3.  En la lista de resultados, haga clic con el botón secundario en cmd y, después, con el botón primario en Ejecutar como administrador.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir una ventana del símbolo del sistema como administrador en el servidor de destino  
  
1.  En la pantalla **Inicio**, en el cuadro de búsqueda, escriba **cmd**.  
  
2.  En la lista de resultados, haga clic con el botón secundario en **cmd**y, después, haga clic de nuevo en **Ejecutar como administrador**.
