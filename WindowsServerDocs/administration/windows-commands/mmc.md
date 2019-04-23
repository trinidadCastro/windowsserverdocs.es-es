---
title: mmc
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 403f7569eaa2380d26cda805c40b1a623743952c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842016"
---
# <a name="mmc"></a>mmc

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Con las opciones de línea de comandos de mmc, puede abrir un determinado **mmc** consola, abra **mmc** en modo de autor o especificar que la versión de 32 bits o 64 bits de **mmc** se abre.
## <a name="syntax"></a>Sintaxis
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<path>\\<filename>.msc|se inicia **mmc** y abre una consola guardada. Deberá especificar la ruta de acceso y el nombre completo para el archivo de consola guardado. Si no especifica un archivo de consola, **mmc** abre una nueva consola.|
|/a|Abre una consola guardada en modo de autor.  Se usa para realizar cambios en las consolas guardadas.|
|/64|Abre la versión de 64 bits de **mmc** (mmc64). Use esta opción solo si está ejecutando un sistema operativo de Microsoft de 64 bits y desea usar un complemento de 64 bits.|
|/32|Abre la versión de 32 bits de **mmc** (mmc32). Cuando se ejecuta un sistema operativo de Microsoft de 64 bits, puede ejecutar complementos de 32 bits por abrir mmc con esta opción de línea de comandos cuando tiene sólo complementos de 32 bits.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
-   Mediante el <path> **\\** <filename> **.msc** opción de línea de comandos que puede usar variables de entorno para crear líneas de comandos o accesos directos que no dependen de la configuración explícita ubicación de archivos de la consola. Por ejemplo, si la ruta de acceso a un archivo de consola está en la carpeta del sistema (por ejemplo, **mmc c:\winnt\system32\console_name.msc**), puede usar la cadena de datos expandible **% systemroot %** para especificar la ubicación (**mmc%systemroot%\system32\console_name.msc**). Esto puede ser útil si va a delegar tareas a las personas de su organización que trabajan en equipos diferentes.
-   Mediante el **/a** opción de línea de comandos cuando se abren las consolas con esta opción, se abren en modo de autor, independientemente del modo predeterminado. Esto no cambia definitivamente la configuración del modo predeterminado para los archivos; Si omite esta opción, mmc abre los archivos según su configuración predeterminada del modo de la consola.
-   Después de abrir **mmc** o un archivo de consola en modo de autor, puede abrir cualquier consola existente haciendo **abrir** en el **consola** menú.
-   Puede usar la línea de comandos para crear accesos directos para abrir **mmc** y las consolas guardadas. Un comando de línea de comandos funciona con el **ejecutar** comando el **iniciar** menú, en cualquier ventana de símbolo del sistema, en accesos directos o en cualquier programa que llame al comando o archivo por lotes.
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

