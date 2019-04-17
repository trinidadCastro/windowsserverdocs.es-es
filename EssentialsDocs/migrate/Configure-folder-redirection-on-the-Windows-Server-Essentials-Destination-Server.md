---
title: "Configurar la redirección de carpetas en el servidor de destino de Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7cc1207f36d3a921b49cc3ecd02acf3fe4fa243c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>Configurar la redirección de carpetas en el servidor de destino de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Si se habilita la redirección de carpetas del servidor de origen, realizar esta tarea.  
  
 En primer lugar, eliminar la configuración de directiva de grupo de redirección de carpetas antigua. A continuación, usar el escritorio de Windows Server Essentials para permitir la redirección de carpetas del servidor de destino.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para eliminar la configuración de directiva de grupo de redirección de carpetas antigua  
  
1.  En el servidor de destino, abre el **Group Policy Management** herramienta administrativa.  
  
2.  En **Group Policy Management**, expanda **bosque:***YourNetworkDomainName*, expanda **dominios**, expande *YourNetworkDomainName*y, a continuación, expande **objetos de directiva de grupo**.  
  
3.  Haz clic en **redirección de carpetas de directiva de grupo SBS**y, a continuación, haz clic en **eliminar**.  
  
4.  Haz clic en **plantillas de seguridad de directiva de grupo SBS**y, a continuación, haz clic en **eliminar**.  
  
5.  Lea la advertencia y, a continuación, haz clic en **Sí**.  
  
6.  Cerrar **administración de directivas de grupo**.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar la redirección de carpetas del servidor de destino  
  
1.  En el servidor de destino, abre el escritorio de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **dispositivos**.  
  
3.  En la **dispositivos tareas** panel, haz clic en **implementar Directiva de grupo**.  
  
4.  En la **habilitar la directiva de grupo de redirección de carpetas** página, selecciona las carpetas que se redireccione y, a continuación, haz clic en **siguiente**.  
  
5.  En la **habilitar la configuración de directiva de seguridad** página, haz clic en **finalizar**.  
  
 Para aplicar el cambio a la redirección de carpetas, los usuarios de red deben cerrar sesión en su equipo y, a continuación, vuelva a iniciar sesión. Esto garantiza que la transferencia de todas las carpetas redirigidas al servidor de destino.
