---
title: Migración desde versiones anteriores a Windows Server R2 Essentials o a Experiencia con Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/20116
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 107a20cae83072ee0066ba0a335eb5078341e59b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432851"
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>Migración desde versiones anteriores a Windows Server R2 Essentials o a Experiencia con Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta guía describe cómo migrar desde versiones anteriores de Windows Small Business Server y Windows Server Essentials (incluidos Windows Server Essentials, Windows Small Business Server 2011 Standard, Windows Small Business Server 2011 Essentials, Windows Small Business Server 2008 y Windows Small Business Server 2003) para Windows Server Essentials o Windows Server 2012 R2 con el rol experiencia con Windows Server Essentials instalado.  
  
 **Para entornos con hasta 25 usuarios y 50 dispositivos**, puede seguir los pasos descritos en esta guía para migrar desde versiones anteriores de Windows SBS a Windows Server Essentials.  
  
 **Para entornos con hasta 100 usuarios y 200 dispositivos**, puede seguir las mismas instrucciones para migrar a las ediciones Standard o Datacenter de Windows Server 2012 R2 con el rol experiencia con Windows Server Essentials instalado.  
  
> [!NOTE]
>  Para evitar problemas durante la migración, el equipo de desarrollo de productos de Windows Server Essentials recomienda encarecidamente que lea este documento antes de empezar la migración.  
  
## <a name="terms-and-definitions"></a>Términos y definiciones  
 **Servidor de origen** Servidor existente desde el cual se migran la configuración y los datos.  
  
 **Servidor de destino** Nuevo servidor al cual se migran la configuración y los datos.  
  
## <a name="migration-process-summary"></a>Resumen del proceso de migración  
 Esta guía de migración incluye los siguientes pasos:  
  
1. [Paso 1: Preparar la migración del servidor de origen para Windows Server Essentials](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debe asegurarse de que el servidor de origen y la red estén listos para la migración. Esta sección le guía por el proceso de copia de seguridad del servidor de origen, evaluación del estado de mantenimiento del servidor de origen, instalación de los Service Packs y correcciones más recientes y comprobación de la configuración de la red.  
  
2. [Paso 2: Instalar Windows Server Essentials como nuevo controlador de dominio de réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md). En esta sección se describe cómo instalar Windows Server Essentials o Windows Server 2012 R2 Standard (con el rol de experiencia con Windows Server Essentials habilitado) como un controlador de dominio.  
  
3. [Paso 3: Unir equipos al nuevo servidor de Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Esta sección explica cómo unir los equipos cliente al nuevo servidor que ejecuta Windows Server Essentials y actualizar la configuración de directiva de grupo.  
  
4. [Paso 4: Mover los datos y configuración para la migración del servidor de destino para Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre cómo migrar los datos y la configuración desde el servidor de origen.  
  
5. [Paso 5: Habilitar la redirección de carpetas en la migración del servidor de destino para Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Si el redireccionamiento de carpetas está habilitado en el servidor de origen, puede habilitarlo en el servidor de destino y, a continuación, eliminar la antigua configuración de redireccionamiento de carpetas de la Directiva de grupo.  
  
6. [Paso 6: Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de la Directiva de grupo y disminuir el nivel del servidor de origen.  
  
7. [Paso 7: Realizar tareas postmigración para la migración de Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  Cuando termine de migrar toda la configuración y datos a Windows Server Essentials, es posible que desee asignar los equipos permitidos a cuentas de usuario.  
  
8. [Paso 8: Ejecutar Windows Server Essentials Best Practices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termine de migrar la configuración y datos a Windows Server Essentials, debe ejecutar el Windows Server Essentials Best Practices Analyzer (BPA).  
  
   Algunos de los procedimientos de migración requieren que abra una ventana del símbolo del sistema como administrador. Los procedimientos siguientes explican cómo hacerlo.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> Para abrir una ventana de símbolo del sistema en el servidor de origen como administrador  
  
1.  Haga clic en **Inicio**.  
  
2.  En el cuadro de búsqueda, escriba **cmd**.  
  
3.  En la lista de resultados, haga clic con el botón secundario en **cmd**y, después, haga clic de nuevo en **Ejecutar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir una ventana del símbolo del sistema como administrador en el servidor de destino  
  
1.  En la pantalla **Inicio**, en el cuadro de búsqueda, escriba **cmd**.  
  
2.  En la lista de resultados, haga clic con el botón secundario en **cmd**y, después, haga clic de nuevo en **Ejecutar como administrador**.  
  
## <a name="see-also"></a>Vea también  
  
-   [Migrar datos del servidor a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar datos del servidor a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

