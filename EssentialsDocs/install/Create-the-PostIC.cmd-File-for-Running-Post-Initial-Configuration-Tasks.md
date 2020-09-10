---
title: Cree el archivo PostIC.cmd utilizando tareas posteriores a la configuración inicial
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5a3ae6711d54d1c1f2bebdae6db3065fce970312
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623714"
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Cree el archivo PostIC.cmd utilizando tareas posteriores a la configuración inicial

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede agregar personalizaciones posteriores a la configuración inicial escribiendo su propio código y llamando dicho código desde un archivo de script llamado PostIC.cmd. Cuando utilice el archivo PostIC.cmd, debe cumplir las siguientes pautas:

- El código de personalización debe ejecutarse en segundo plano (no puede mostrar una interfaz de usuario).

- El código de personalización no puede iniciar o reiniciar el servidor. La Configuración inicial reiniciará el servidor como última tarea.

- La ejecución del código de personalización debe tener una duración máxima de 3 minutos.

  Defina el archivo PostIC.cmd para que de como resultado un 0 si el código se ejecuta correctamente. Si se produce como resultado cualquier otro valor, el sistema operativo buscará el archivo [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure), que contiene el código que deberá ejecutarse si el archivo PostIC.cmd no se ejecuta correctamente. Los archivos PostIC.cmd y SetupFailure.cmd deben encontrarse en C:\Windows\Setup\Scripts.

#### <a name="to-define-post-initial-configuration-customizations"></a>Para definir las personalizaciones posteriores a la configuración inicial

1.  Escriba el código que se llamará desde PostIC.cmd.

2.  Use el Bloc de notas para crear un archivo llamado PostIC.cmd y agregue la llamada al código creado en el paso 1. Asegúrese de que el código devuelva un valor de éxito.

3.  Guarde PostIC.cmd en C:\Windows\Setup\Scripts.

4.  (Opcional) Cree un archivo SetupFailure.cmd que ejecute código si PostIC.cmd da como resultado un valor distinto a 0.

###  <a name="setupfailurecmd"></a><a name="BKMK_SetupFailure"></a> SetupFailure. cmd
 Puede proporcionar la notificación de problemas en la configuración inicial utilizando SetupFailure.cmd. El archivo SetupFailure.cmd contiene el código que desea ejecutar si se producen problemas. El archivo SetupFailure.cmd se encuentra en C:\Windows\Setup\Scripts y se ejecutará si se produce un problema con la tarea de configuración o si el archivo PostIC.cmd devuelve un valor distinto a 0.

##### <a name="to-define-notifications"></a>Para definir notificaciones

1.  Escriba el código que se llamará desde el script SetupFailure.cmd.

2.  Use el Bloc de notas para crear un archivo llamado SetupFailure.cmd y agregue la llamada al código creado en el paso 1. Asegúrese de que el código devuelva un valor de éxito.

3.  Guarde SetupFailure.cmd en C:\Windows\Setup\Scripts.

## <a name="see-also"></a>Consulte también
 [Introducción con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para](Preparing-the-Image-for-Deployment.md) [probar la implementación de la experiencia del cliente](Testing-the-Customer-Experience.md)