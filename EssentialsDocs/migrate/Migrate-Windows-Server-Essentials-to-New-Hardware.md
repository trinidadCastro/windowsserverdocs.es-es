---
title: Migración de Windows Server Essentials a un nuevo hardware
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f695ae90-3160-407b-bebf-9e460f22c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 61106439a63a75143a9cca0989c70370adfedd38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853416"
---
# <a name="migrate-windows-server-essentials-to-new-hardware"></a>Migración de Windows Server Essentials a un nuevo hardware

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta guía describe cómo migrar un dominio existente de Windows Server® 2012 Essentials a Windows Server Essentials en un hardware nuevo y, después, migrar la configuración y los datos. Esta guía también describe cómo quitar el servidor existente de la red de Windows Server Essentials después de finalizar la migración.  
  
> [!NOTE]
>  Para evitar problemas durante la migración, el equipo de desarrollo del producto de Windows Server Essentials recomienda encarecidamente que lea este documento antes de comenzar la migración.  
  
> [!NOTE]

>  Para migrar los datos del servidor a la versión más reciente de Windows Server Essentials, consulte [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Recursos adicionales  
 Para obtener vínculos a información adicional, herramientas y recursos de la Comunidad que le guiarán a través del proceso de migración, visite la [migración de Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520) sitio Web.  
  
## <a name="terms-and-definitions"></a>Términos y definiciones  
 **Servidor de origen:** el servidor existente desde el cual se migran la configuración y los datos.  
  
 **Servidor de destino:** el nuevo servidor al cual se migran la configuración y los datos.  
  
## <a name="migration-process-summary"></a>Resumen del proceso de migración  
 Esta guía de migración incluye los siguientes pasos:  
  

1.  [Preparar la migración del servidor de origen para Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Debe asegurarse de que el servidor de origen y la red estén listos para la migración. Esta sección le guía por el proceso de copia de seguridad del servidor de origen, evaluación del estado de mantenimiento del servidor de origen, instalación de los Service Packs y correcciones más recientes y comprobación de la configuración de la red.  
  
2.  [Instalar Windows Server Essentials en modo de migración](Install-Windows-Server-Essentials-in-migration-mode.md).  Esta sección describen los pasos que debe seguir para instalar Windows Server Essentials en el servidor de destino en modo de migración.  
  
3.  [Unir equipos al nuevo servidor de Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  En esta sección se trata unir equipos cliente al nuevo servidor de Windows Server Essentials y actualizar la configuración de directiva de grupo.  
  
4.  [Mover los datos y configuración para la migración del servidor de destino para Windows Server Essentials](Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta sección proporciona información sobre cómo migrar los datos y la configuración desde el servidor de origen.  
  
5.  [Configurar la redirección de carpetas en el servidor de destino de Windows Server Essentials](Configure-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Si el redireccionamiento de carpetas está habilitado en el servidor de origen, puede habilitarlo en el servidor de destino y, a continuación, eliminar la antigua configuración de redireccionamiento de carpetas de la Directiva de grupo.  
  
6.  [Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de quitar el servidor de origen de la red, debe forzar una actualización de la Directiva de grupo y disminuir el nivel del servidor de origen.  
  
7.  [Realizar tareas postmigración para la migración de Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Cuando termine de migrar toda la configuración y datos a Windows Server Essentials, es posible que desee asignar los equipos permitidos a cuentas de usuario.  
  
8.  [Ejecutar Windows Server Essentials Best Practices Analyzer](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Cuando termine de migrar la configuración y datos a Windows Server Essentials, debe descargar y ejecutar el BPA de Windows Server Essentials.  
  
 Algunos de los procedimientos de migración requieren que abra una ventana del símbolo del sistema como administrador.  
  
#### <a name="to-open-a-command-prompt-window-as-an-administrator"></a>Para abrir una ventana del símbolo del sistema como administrador:  
  
1.  En la pantalla **Inicio**, en el cuadro de búsqueda, escriba **cmd**.  
  
2.  En la lista de resultados, haga clic con el botón secundario en **cmd**y, después, haga clic de nuevo en **Ejecutar como administrador**.
