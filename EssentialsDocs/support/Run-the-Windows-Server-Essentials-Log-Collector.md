---
title: Ejecución del compilador de registros de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 34c853abb19424d94fa768c70fcb6c9d8d8feea8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625287"
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Ejecución del compilador de registros de Windows Server Essentials
Puede ejecutar el compilador de registros de Windows Server Essentials desde el servidor o un equipo de la red. Si ejecuta el Compilador de registros desde el servidor, solo podrá recopilar registros del servidor. Si ejecuta el Compilador de registros desde un equipo de red, puede recopilar registros del servidor además de los registros de ese equipo.

 Debe tener los privilegios administrativos necesarios para ejecutar el Compilador de registros. Si va a recopilar archivos de registro de un servidor, debe ser administrador del servidor. Si va a recopilar archivos de registro en un equipo de red, debe ser administrador de cliente de ese equipo.

#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Para ejecutar el Compilador de registros en el servidor usando el asistente

1. En la página **Inicio** del servidor, haga clic en **recopilador de registros de Windows Server Essentials**.

   > [!NOTE]
   > - Si el programa del compilador de registros no aparece en la página de **Inicio** , vaya a **%System%\Program Files files (x86) \Windows Server Essentials log Collector**y, a continuación, haga doble clic en **LogCollector**.
   >   -   Si no ha iniciado sesión en el servidor con privilegios administrativos, el Compilador de registros le pide que escriba sus credenciales.

2. Cuando se le pida una ubicación para guardar los archivos de registro recopilados, puede elegir la ubicación predeterminada, ** \\ \\<ServerName \> \logs**o especificar otra ubicación. Para aceptar la ubicación predeterminada, haga clic en **Siguiente**. Para cambiar la ubicación, haga clic en **Examinar**, vaya hasta la carpeta donde desea guardar los archivos de registro y, a continuación, haga clic en **Guardar**.

   > [!NOTE]
   >  No es necesario que proporcione los nombres de archivo de los archivos de registro. El compilador de registros nombra la colección de archivos zip mediante la concatenación del nombre del equipo y la marca de tiempo del archivo.

3. Se muestra una barra de progreso mientras se recopilan los registros.

4. Para ver el contenido del archivo de recopilación de registros, seleccione la casilla **Abrir la ubicación del archivo donde se han guardado los registros** y, a continuación, haga clic en **Cerrar** para cerrar el asistente y abrir el archivo de recopilación de registros.

#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Para ejecutar el Compilador de registros en un equipo de la red usando el asistente

1.  Vaya a **%System%\Program Files files (x86) \Windows Server Essentials log Collector**y, a continuación, haga doble clic en el archivo **LogCollector.exe**.

    > [!NOTE]
    >  Si no ha iniciado una sesión en el equipo de red con privilegios de administrador, escriba el nombre de usuario y la contraseña cuando se le pida y, a continuación, haga clic en **Siguiente**.

2.  Seleccione los registros que quiera recopilar de la siguiente manera:

    1.  Seleccione la casilla **Archivos de registros del servidor** para recopilar los archivos de registros en el servidor.

    2.  La casilla **Archivos de registros del equipo cliente (este equipo)** está activada de forma predeterminada e indica que el Compilador de registros recopilará los registros desde el equipo de red que está ejecutando Si solo desea recopilar registros del servidor, desactive la casilla **Archivos de registros del equipo cliente (este equipo)**.

    3.  Haga clic en **Next**.

3.  Cuando se le pida, escriba el nombre de usuario y la contraseña de un administrador del servidor y, a continuación, haga clic en **Siguiente**.

4.  Escriba o busque la ubicación donde quiera guardar los archivos de registros y, a continuación, haga clic en **Siguiente**.

    > [!NOTE]
    >  No es necesario que proporcione los nombres de archivo de los archivos de registro. El compilador de registros nombra la colección de archivos zip mediante la concatenación del nombre del equipo y la marca de tiempo del archivo.

5.  Se muestra una barra de progreso mientras se recopilan los registros.

6.  Para ver el contenido del archivo de recopilación de registros, seleccione la casilla **Abrir la ubicación del archivo donde se han guardado los registros** y, a continuación, haga clic en **Cerrar** para cerrar el asistente y abrir el archivo de recopilación de registros.

### <a name="running-the-log-collector-manually"></a>Ejecutar el Compilador de registros manualmente
 Una vez instalado el Compilador de registros, se crea una tarea programada para ejecutar la herramienta. Posteriormente, puede ejecutar el Compilador de registros del **Administrador de tareas programadas** sin usar el asistente, en caso de que haya problemas para iniciar el asistente.

##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Para ejecutar manualmente el Compilador de registros en el servidor

1.  Inicie sesión directamente o de forma remota en el servidor.

2.  Abra el **Programador de tareas**.

3.  En la raíz de la **Biblioteca del programador de tareas**, vaya a la tarea programada **LogCollector**.

4.  Haga clic con el botón secundario en **LogCollector** y, a continuación, haga clic en **Ejecutar**. El recopilador de registros coloca los registros en la carpeta predeterminada en el servidor, ** \\ \\<ServerName \> \Logs**. Si no tiene permiso de escritura para la carpeta o la carpeta no existe, los registros se colocan en el subdirectorio **<Temp \> ** .

##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Para ejecutar manualmente el Compilador de registros en un equipo de red

1.  Inicie sesión directamente o de forma remota en el equipo de red.

2.  Abra el **Programador de tareas**.

3.  En la raíz de la **Biblioteca del programador de tareas**, vaya a la tarea programada **LogCollector**.

4.  Haga clic con el botón secundario en **LogCollector** y, a continuación, haga clic en **Ejecutar**. El recopilador de registros coloca los registros en la carpeta **temp \><** del equipo de red.
