---
title: msinfo32
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3de5088b64105e970fc38f55ecaf54382670549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843466"
---
# <a name="msinfo32"></a>msinfo32

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se abre la herramienta información del sistema para mostrar una vista completa del hardware, los componentes del sistema y entorno de software en el equipo local. 
## <a name="syntax"></a>Sintaxis
```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computerName>] [/showcategories] [/category <CategoryID>] [/categories {+<CategoryID>(+<CategoryID>)|+all(-<CategoryID>)}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<path>|Especifica el archivo que se puede abrir en el formato *C:\Folder1\File1.XXX*, donde *C* es la letra de unidad, *carpeta1* es la carpeta, *File1*es el nombre de archivo y *XXX* es la extensión de nombre de archivo.<br /><br />Este archivo puede ser un **.nfo**, **.xml**, **.txt**, o **.cab** archivo.|
|<computerName>|Especifica el nombre del destino o el equipo local. Esto puede ser un nombre UNC, una dirección IP o nombre completo de equipo.|
|<CategoryID>|Especifica el identificador del elemento de categoría. También puede obtener el identificador de categoría con **/showcategories**.|
|/pch|Muestra la vista de historial del sistema en la herramienta información del sistema.|
|/nfo|Guarda el archivo exportado como un **.nfo** archivo. Si el nombre del archivo que se especifica en *ruta* no termina en un **.nfo** extensión, el **.nfo** extensión se anexa automáticamente al nombre de archivo.|
|/ Report|Guarda el archivo en *ruta* como un archivo de texto. El nombre de archivo se guarda exactamente como aparece en *ruta de acceso*. No se anexa la extensión .txt en el archivo a menos que se especifica en la ruta de acceso.|
|/computer|inicia la herramienta información del sistema para el equipo remoto especificado. Debe tener los permisos adecuados para tener acceso al equipo remoto.|
|/showcategories|se inicia la herramienta información del sistema con muestran los identificadores de categoría disponibles, en lugar de mostrar los nombres descriptivos o localizados. Por ejemplo, se muestra la categoría de entorno de Software como el **SWEnv** categoría.|
|/Category|Información del sistema se inicia con la categoría especificada seleccionada. Use **/showcategories** para mostrar una lista de identificadores de categoría disponibles.|
|/categories|Información del sistema se inicia con solo la categoría especificada o categorías que se muestran. También limita la salida a las categorías o la categoría seleccionada. Use **/showcategories** para mostrar una lista de identificadores de categoría disponibles.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
Algunas categorías de información del sistema contienen grandes cantidades de datos. Puede usar el **start /wait** comando para optimizar el rendimiento de los informes de estas categorías. Para obtener más información, consulte [información del sistema](https://technet.microsoft.com/library/cc783305(v=ws.10).aspx).
## <a name="BKMK_Examples"></a>Ejemplos
Para obtener una lista de los identificadores de categoría disponibles, escriba:
```
msinfo32 /showcategories
```
Para iniciar la herramienta de información del sistema con toda la información disponible muestra, excepto los módulos cargados, escriba:
```
msinfo32 /categories +all -loadedmodules
```
Para mostrar solo la información de resumen del sistema y crear un archivo .nfo llamado ressis.nfo, que contiene información de la categoría de resumen del sistema, escriba:
```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```
Para mostrar información de conflictos de recursos y crear un archivo que contiene información sobre conflictos de recursos, escriba:
```
msinfo32 /nfo conflicts.nfo /categories    +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

