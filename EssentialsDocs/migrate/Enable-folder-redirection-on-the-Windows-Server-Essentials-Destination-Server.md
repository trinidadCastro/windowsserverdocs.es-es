---
title: Habilitación del redireccionamiento de carpetas en el servidor 1 de destino de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
H1: Habilitar la redirección de carpetas en el servidor de destino de Windows Server Essentials
ms.assetid: f67d195e-36f6-495a-8361-6d5faa889441
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 301224256c196b9788e80d38112ed94a6073e054
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319004"
---
# <a name="enable-folder-redirection-on-the-windows-server-essentials-destination-server1"></a>Habilitación del redireccionamiento de carpetas en el servidor 1 de destino de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede hacer esta tarea si está habilitada la redirección de carpetas en el servidor de origen.  
  
 En primer lugar, use el panel de Windows Server Essentials para habilitar el redireccionamiento de carpetas en el servidor de destino. A continuación, elimine la antigua configuración de directiva de grupo de redirección de carpetas.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar la redirección de carpetas en el servidor de destino  
  
1.  En el servidor de destino, abra el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haga clic en **DISPOSITIVOS**.  
  
3.  En el panel **Tareas de dispositivos**, haga clic en **Implementar directiva de grupo**.  
  
4.  En la página **Habilitar la directiva de grupo de redirección de carpetas**, seleccione las carpetas que desea redirigir y haga clic en **Siguiente**.  
  
5.  En la página **Habilitar la configuración de directivas de seguridad**, haga clic en **Finalizar**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para eliminar la antigua configuración de directiva de grupo de redirección de carpetas  
  
1. En el servidor de destino, abra la herramienta administrativa **Administración de directivas de grupo**.  
  
2. En **Administración de directivas de grupo**, expandaaa **Bosque:** <em>NombreDominioRed</em>, expandaaa **Dominios**, expandaaa *NombreDominioRed*y **Objetos de directiva de grupo**.  
  
3. Haga clic con el botón secundario en **Redirección de carpetas de W7PVP** y después haga clic en **Eliminar**.  
  
4. Lea la advertencia y haga clic en **Sí**.  
  
5. Cierre **Administración de directivas de grupo**.  
  
   Para aplicar el cambio a la redirección de carpetas, los usuarios de la red deben cerrar sesión en su equipo y después reiniciarla. Así se garantiza la transferencia de todas las carpetas redirigidas al servidor de destino.
