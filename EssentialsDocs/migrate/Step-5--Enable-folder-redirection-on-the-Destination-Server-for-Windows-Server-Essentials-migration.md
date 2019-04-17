---
title: "Paso 5: Habilitar la redirección de carpetas en la migración de servidor de destino para Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>Paso 5: Habilitar la redirección de carpetas en la migración de servidor de destino para Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Si se habilita la redirección de carpetas del servidor de origen, puedes habilitar la redirección de carpetas del servidor de destino y, a continuación, eliminar la configuración de directiva de grupo de redirección de carpetas antigua.  
  
 En primer lugar, usa el panel de Windows Server Essentials para habilitar la redirección de carpetas del servidor de destino. A continuación, eliminar la configuración de directiva de grupo de redirección de carpetas antigua.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar la redirección de carpetas del servidor de destino  
  
1.  En el servidor de destino, abre el escritorio de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **dispositivos**.  
  
3.  En la **dispositivos tareas** panel, haz clic en **implementar Directiva de grupo**.  
  
4.  En la **habilitar la directiva de grupo de redirección de carpetas** página, selecciona las carpetas que se redireccione y, a continuación, haz clic en **siguiente**.  
  
5.  En la **habilitar la configuración de directiva de seguridad** página, haz clic en **finalizar**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para eliminar la configuración de directiva de grupo de redirección de carpetas antigua  
  
1.  En el servidor de destino, abre el **Group Policy Management** herramienta administrativa.  
  
2.  En **Group Policy Management**, expanda **bosque:***YourNetworkDomainName*, expanda **dominios**, expande *YourNetworkDomainName*y, a continuación, expande **objetos de directiva de grupo**.  
  
3.  Haz clic en la directiva que quieras eliminar y, a continuación, haz clic en **eliminar**.  
  
4.  Lea la advertencia y, a continuación, haz clic en **Sí**.  
  
5.  Cerrar **administración de directivas de grupo**.  
  
 Para aplicar el cambio de la redirección de carpetas, los usuarios de red deben cerrar la sesión en sus equipos y, a continuación, vuelva a iniciar sesión. Esto garantiza que la transferencia de todas las carpetas redirigidas al servidor de destino.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Ha habilitado la redirección de carpetas del servidor de destino. Ahora ve a [paso 6: degradar y quitar el servidor de origen de la nueva red de Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  
  

Para ver todos los pasos, consulta [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

