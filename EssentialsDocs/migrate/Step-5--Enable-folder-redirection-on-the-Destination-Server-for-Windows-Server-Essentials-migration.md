---
title: 'Paso 5: Habilitar la redirección de carpetas en la migración del servidor de destino para Windows Server Essentials'
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3925f80-552d-431f-b2a6-2af202e50ca4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 613ff4c80a80ed4f3207cb0c1ead6db12c723e85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815386"
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>Paso 5: Habilitar la redirección de carpetas en la migración del servidor de destino para Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Si el redireccionamiento de carpetas está habilitado en el servidor de origen, puede habilitarlo en el servidor de destino y, a continuación, eliminar la antigua configuración de redireccionamiento de carpetas de la Directiva de grupo.  
  
 En primer lugar, use el panel de Windows Server Essentials para habilitar la redirección de carpetas en el servidor de destino. A continuación, elimine la antigua configuración de directiva de grupo de redirección de carpetas.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar la redirección de carpetas en el servidor de destino  
  
1.  En el servidor de destino, abra el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haga clic en **DISPOSITIVOS**.  
  
3.  En el panel **Tareas de dispositivos**, haga clic en **Implementar directiva de grupo**.  
  
4.  En la página **Habilitar la directiva de grupo de redirección de carpetas**, seleccione las carpetas que desea redirigir y haga clic en **Siguiente**.  
  
5.  En la página **Habilitar la configuración de directivas de seguridad** , haga clic en **Finalizar**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para eliminar la antigua configuración de directiva de grupo de redirección de carpetas  
  
1.  En el servidor de destino, abra la herramienta administrativa **Administración de directivas de grupo**.  
  
2.  En **Group Policy Management**, expanda **bosque: *** Nombredominiored*, expanda **dominios**, expanda *Nombredominiored* y, a continuación, expanda **Group Policy Objects**.  
  
3.  Haga clic con el botón secundario en la directiva que desea eliminar y con el botón primario en **Eliminar**.  
  
4.  Lea la advertencia y haga clic en **Sí**.  
  
5.  Cierre **Administración de directivas de grupo**.  
  
 Para aplicar el cambio de redirección de carpetas, los usuarios de red deben cerrar sesión en sus equipos e iniciarla de nuevo. Así se garantiza la transferencia de todas las carpetas redirigidas al servidor de destino.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Ya ha habilitado la redirección de carpetas en el servidor de destino. Ahora, vaya a [paso 6: Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  
  

Para ver todos los pasos, consulte [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

