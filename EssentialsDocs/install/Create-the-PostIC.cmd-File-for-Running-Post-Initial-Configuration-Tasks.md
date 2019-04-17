---
title: "Crear el archivo PostIC.cmd para ejecutar tareas posteriores a la configuración inicial"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f5042204cd189e3101f5e0126fd98e786a49032d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Crear el archivo PostIC.cmd para ejecutar tareas posteriores a la configuración inicial

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes agregar personalizaciones de configuración posteriores a la inicial escribiendo tu propio código y, a continuación, llamar a ese código desde un archivo de script llamado PostIC.cmd. Al usar el archivo PostIC.cmd, debes respetar las siguientes indicaciones:  
  
-   El código de personalización debe ejecutarse en silencio (no se puede mostrar una interfaz de usuario).  
  
-   El código de personalización no puede iniciar un reinicio del servidor. La configuración inicial reiniciará el servidor como última tarea.  
  
-   El código de personalización debe ejecutarse en tres minutos o menos.  
  
 Define tu archivo PostIC.cmd para que devuelva 0 si el código se ejecuta correctamente. Si se devuelve cualquier otro valor, el sistema operativo busca un archivo denominado [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure), que contiene código que debería ejecutarse si el código en el archivo PostIC.cmd no se ejecuta correctamente. Archivo PostIC.cmd y el archivo SetupFailure.cmd deben C:\Windows\Setup\Scripts encuentra.  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>Para definir personalizaciones posteriores a la inicial de configuración  
  
1.  Escribe el código que se llama desde el script PostIC.cmd.  
  
2.  Con el Bloc de notas, crea un archivo llamado PostIC.cmd y agrega la llamada al código que creaste en el paso 1. Asegúrate de que el código devuelva un valor de operación correcta.  
  
3.  Guarda PostIC.cmd en C:\Windows\Setup\Scripts.  
  
4.  (Opcional) Crea un archivo SetupFailure.cmd que ejecute código si PostIC.cmd devuelve cualquier distinto de 0.  
  
###  <a name="BKMK_SetupFailure"></a>SetupFailure.cmd  
 Puedes proporcionar notificaciones de problemas en la configuración inicial usando SetupFailure.cmd. El archivo SetupFailure.cmd contiene el código que quieres ejecutar si surgen problemas. El archivo SetupFailure.cmd se coloca en C:\Windows\Setup\Scripts y se ejecuta cuando se produce un problema con una tarea de instalación o cuando el archivo PostIC.cmd devuelve un valor distinto de 0.  
  
##### <a name="to-define-notifications"></a>Para definir las notificaciones  
  
1.  Escribe el código que se llama desde el script SetupFailure.cmd.  
  
2.  Con el Bloc de notas, crea un archivo llamado SetupFailure.cmd y agrega la llamada al código que creaste en el paso 1. Asegúrate de que el código devuelva un valor de operación correcta.  
  
3.  Guarda SetupFailure.cmd en C:\Windows\Setup\Scripts.  
  
## <a name="see-also"></a>Consulta también  
 [Tareas iniciales con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)