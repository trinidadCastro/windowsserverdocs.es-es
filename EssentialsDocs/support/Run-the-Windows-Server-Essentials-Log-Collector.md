---
title: Ejecutar el recolector de registro de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6b49fee7ca4a19d5a501cf96c1ce356f8242c81f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Ejecutar el recolector de registro de Windows Server Essentials
Puedes ejecutar el recolector de registro de Windows Server Essentials desde el servidor o un equipo en la red. Si ejecutas el recolector de registro del servidor, solo puede recopilar registros del servidor. Si ejecutas el recolector de registro de un equipo de red, puedes recopilar registros desde el servidor, además de los registros de ese equipo.  
  
 Debes tener privilegios administrativos adecuados para ejecutar el recolector de registro. Si estás Recopilando archivos de registro de un servidor, debe ser un administrador del servidor; Si estás Recopilando archivos de registro en un equipo de red, debe ser un administrador de cliente para ese equipo.  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Para ejecutar el recolector de registro en el servidor mediante el Asistente  
  
1.  En la **inicio** página del servidor, haz clic en **recopilador de registro de Windows Server Essentials**.  
  
    > [!NOTE]
    >  -   Si el programa de selector de registro no aparece en la **inicio** página, busca **%system%\Program archivos (x86) \Windows recolector de registro del servidor Essentials**y, a continuación, haz doble clic en **LogCollector**.  
    > -   Si no ha iniciado sesión el servidor con privilegios administrativos, el recolector de registro pide que escribas las credenciales.  
  
2.  Cuando se te pide una ubicación para guardar los archivos de registro recopilados, puedes elegir la ubicación predeterminada, **\\\ < ServerName\ > \logs**, o especificar otra ubicación. Para aceptar la ubicación predeterminada, haz clic en **siguiente**. Para cambiar la ubicación, haz clic en **examinar**, navega hasta la carpeta donde quieres guardar los archivos de registro y, a continuación, haz clic en **guardar**.  
  
    > [!NOTE]
    >  No es necesario proporcionar nombres de archivo para los archivos de registro. El selector de registro de nombres de la colección de archivos zip concatenando el nombre del equipo y la marca de tiempo del archivo.  
  
3.  Se muestra una barra de progreso mientras los registros se recopilan.  
  
4.  Para ver el contenido del archivo de registro de la colección, selecciona el **abrir la ubicación del archivo donde se guardaron los registros** casilla de verificación y haz clic en **cerrar** para cerrar el asistente y abrir el archivo de registro de la colección.  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Para ejecutar el recolector de registro en un equipo de red mediante el Asistente  
  
1.  Busca **%system%\Program archivos (x86) \Windows recolector de registro del servidor Essentials**y, a continuación, haz doble clic en el archivo **LogCollector.exe**.  
  
    > [!NOTE]
    >  Si no ha iniciado sesión el equipo de red con privilegios administrativos, escribe tu nombre de usuario y contraseña cuando aparezca el mensaje y, a continuación, haz clic en **siguiente**.  
  
2.  Selecciona qué registros que quieras recopilar, como sigue:  
  
    1.  Selecciona el **archivos de registro de servidor** casilla de verificación para recopilar archivos de registro en el servidor.  
  
    2.  La **archivos de registro del equipo de cliente (este equipo)** casilla está activada de manera predeterminada, que indica que el recolector de registro recopilará los registros desde el equipo de red que está ejecutando el. Si solo quieres recopilar registros del servidor, desactive la **archivos de registro del equipo de cliente (este equipo)** casilla de verificación.  
  
    3.  Haz clic en **siguiente**.  
  
3.  Cuando se te pida, escribe el nombre de usuario y contraseña para un administrador del servidor y, a continuación, haz clic en **siguiente**.  
  
4.  Escribe o busca la ubicación donde quieres guardar los archivos de registro y, a continuación, haz clic en **siguiente**.  
  
    > [!NOTE]
    >  No es necesario proporcionar nombres de archivo para los archivos de registro. El selector de registro de nombres de la colección de archivos zip concatenando el nombre del equipo y la marca de tiempo del archivo.  
  
5.  Se muestra una barra de progreso mientras los registros se recopilan.  
  
6.  Para ver el contenido del archivo de registro de la colección, selecciona el **abrir la ubicación del archivo donde se guardaron los registros** casilla de verificación y haz clic en **cerrar** para cerrar el asistente y abrir el archivo de registro de la colección.  
  
### <a name="running-the-log-collector-manually"></a>Ejecuta el selector de registro manualmente  
 Después de instalar el selector de registro, se crea una tarea programada para ejecutar la herramienta. Posteriormente se puede ejecutar el recolector de registro de la **Administrador de tareas programadas** sin usar el asistente, si existen problemas para iniciar el asistente.  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Para ejecutar manualmente el recolector de registro en el servidor  
  
1.  Iniciar sesión directamente o remotamente en el servidor.  
  
2.  Abre el **programador de tareas**.  
  
3.  En la raíz de la **biblioteca del programador de tareas**, vaya a la tarea programada denominada **LogCollector**.  
  
4.  Haz clic en **LogCollector**y, a continuación, haz clic en **ejecutar**. El recolector de registro coloca los registros en la carpeta predeterminada en el servidor, **\\\ < ServerName\ > \Logs**. Si no tienes permiso de escritura para la carpeta o la carpeta no existe, los registros se colocan en la **< temp\ >** subdirectorio.  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Para ejecutar manualmente el recolector de registro en un equipo de red  
  
1.  Iniciar sesión directamente o remotamente en el equipo de red.  
  
2.  Abre el **programador de tareas**.  
  
3.  En la raíz de la **biblioteca del programador de tareas**, vaya a la tarea programada denominada **LogCollector**.  
  
4.  Haz clic en **LogCollector**y, a continuación, haz clic en **ejecutar**. El recolector de registro coloca los registros en la **< temp\ >** carpeta en el equipo de red.
