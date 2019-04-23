---
title: Migrar desde versiones anteriores a Windows Server Essentials o Windows Server Essentials Experience
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: 213ee4304d9d4ebdb7580f7f78fdaca78aa454c9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>Migrar desde versiones anteriores a Windows Server Essentials o Windows Server Essentials Experience

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta guía describe cómo migrar desde versiones anteriores de Windows Small Business Server y Windows Server Essentials (incluidos Windows Server Essentials, Windows Small Business Server 2011 Standard, Windows Small Business Server 2011 Essentials, Windows Small Business Server 2008 y Windows Small Business Server 2003) para Windows Server Essentials o Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado.  
  
 **Para entornos con hasta 25 usuarios y 50 dispositivos**, puedes seguir los pasos descritos en esta guía para migrar de versiones anteriores de Windows SBS a Windows Server Essentials.  
  
 **Para entornos con hasta 100 usuarios y dispositivos de 200**, puedes seguir la misma guía para migrar a las ediciones Standard o Datacenter de Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado.  
  
> [!NOTE]
>  Para evitar problemas durante la migración, el equipo de desarrollo del producto de Windows Server Essentials recomienda encarecidamente que leas este documento antes de empezar la migración.  
  
## <a name="terms-and-definitions"></a>Términos y definiciones  
 **Servidor de origen** el servidor existente desde el que vas a migrar la configuración y los datos.  
  
 **Servidor de destino** el servidor nuevo a la que vas a migrar la configuración y los datos.  
  
## <a name="migration-process-summary"></a>Resumen del proceso de migración  
 Esta guía de migración incluye los siguientes pasos:  
  
1.  [Paso 1: Preparar la migración de servidor de origen para Windows Server Essentials](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debes asegurarte de que el servidor de origen y la red están listos para la migración. En esta sección te guiará a través de la copia de seguridad del servidor de origen, evaluar el estado del sistema de servidor de origen, instalar el service Pack y correcciones más recientes y comprobación de la configuración de red.  
  
2.  [Paso 2: Instalar Windows Server Essentials como un nuevo controlador de dominio de réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md). Esta sección describe cómo instalar Windows Server Essentials o Windows Server 2012 R2 Standard (con el rol de Windows Server Essentials Experience habilitado) como un controlador de dominio.  
  
3.  [Paso 3: Unirte a equipos con el servidor de Windows Server Essentials nuevo](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Esta sección explica cómo unirse a los equipos cliente para el nuevo servidor que ejecuta Windows Server Essentials y actualiza la configuración de directiva de grupo.  
  
4.  [Paso 4: Mover la configuración y los datos a la migración de servidor de destino para Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre la migración de datos y opciones de configuración del servidor de origen.  
  
5.  [Paso 5: Habilitar la redirección de carpetas en la migración de servidor de destino para Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Si se habilita la redirección de carpetas del servidor de origen, puedes habilitar la redirección de carpetas del servidor de destino y, a continuación, eliminar la configuración de directiva de grupo de redirección de carpetas antigua.  
  
6.  [Paso 6: Degradar y quitar el servidor de origen de la nueva red de Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de directiva de grupo y degradar al servidor de origen.  
  
7.  [Paso 7: Realizar tareas posteriores a la migración de la migración de Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  Después de terminar de migrar todas las configuraciones y datos a Windows Server Essentials, puedes asignar equipos permitidos a cuentas de usuario.  
  
8.  [Paso 8: Ejecutar el analizador de procedimientos recomendados de Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termines de migrar la configuración y datos a Windows Server Essentials, debes ejecutar el Windows Server Essentials Best Practices Analyzer (BPA).  
  
 Varios de los procedimientos de migración requieren que abra una ventana del símbolo del sistema como administrador. Los siguientes procedimientos explican cómo hacerlo.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>Para abrir una ventana de símbolo del sistema en el servidor de origen como administrador  
  
1.  Haz clic en **inicio**.  
  
2.  En el cuadro de búsqueda, escriba **cmd**.  
  
3.  En la lista de resultados, haz clic en **cmd**y, a continuación, haz clic en **ejecutar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir una ventana de símbolo del sistema en el servidor de destino como administrador  
  
1.  En la **inicio** pantalla, en el cuadro de búsqueda, escriba **cmd**.  
  
2.  En la lista de resultados, haz clic en **cmd**y, a continuación, haz clic en **ejecutar como administrador**.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Migrar los datos del servidor para Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar los datos del servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)
