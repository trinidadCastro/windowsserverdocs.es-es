---
title: ruta de acceso
description: Obtenga información sobre cómo establecer la variable de entorno PATH.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65ccaf23b0e19319383952f3a1ca436aaf4d06fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856466"
---
# <a name="path"></a>ruta de acceso



Establece la ruta de acceso en la variable de entorno PATH (el conjunto de directorios que se usa para buscar archivos ejecutables). Si se utiliza sin parámetros, **ruta** muestra la ruta de acceso actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:]<Path>|Especifica la unidad y directorio para establecer la ruta de acceso de comando.|
|;|Separa los directorios en la ruta de acceso. Si se usa sin otros parámetros, **;** borra las rutas de acceso de comandos existentes de la variable de entorno PATH y dirige Cmd.exe para buscar sólo en el directorio actual.|
|% PATH %|Anexa la ruta de acceso al conjunto existente de directorios se muestran en la variable de entorno PATH.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Al incluir **% PATH %** en la sintaxis, Cmd.exe lo reemplaza con los valores de ruta de acceso de comando que se encuentra en la variable de entorno PATH, lo que elimina la necesidad de escribir manualmente estos valores en el símbolo del sistema.
-   Siempre se busca en el directorio actual antes de los directorios especificados en la ruta de acceso.
-   Podría tener archivos en un directorio que comparten el mismo nombre pero con extensiones diferentes. Por ejemplo, podría tener un archivo denominado cuentas.com que inicia un programa de contabilidad y otro denominado cuentas.bat que conecta el servidor a la red del sistema de contabilidad.

    El sistema operativo de Windows busca un archivo mediante el uso de extensiones de nombre de archivo predeterminadas en el siguiente orden de prioridad: .exe, .com, .bat y. cmd. Para ejecutar cuentas.bat cuando cuentas.com esté en el mismo directorio, debe incluir la extensión .bat en el símbolo del sistema.
-   Si dos o más archivos en la ruta de acceso de comando tienen el mismo nombre de archivo y la extensión, **ruta** nombre busca primero el archivo especificado en el directorio actual. A continuación, busca los directorios en la ruta de acceso en el orden en que aparecen en la variable de entorno PATH.
-   Si coloca el **ruta** comando en el archivo Autoexec.nt, el sistema operativo de Windows agrega automáticamente la ruta de acceso de búsqueda de subsistema de MS-DOS especificado cada vez inicie sesión en el equipo. Cmd.exe no utiliza el archivo Autoexec.nt. Cuando se inicia desde un acceso directo, Cmd.exe hereda las variables de entorno establecidas en mi equipo/propiedades/avanzadas/entorno.

## <a name="BKMK_examples"></a>Ejemplos

Para buscar C:\User\Taxes, B:\User\Invest y B:\Bin las rutas de acceso para los comandos externos, escriba:

`path c:\user\taxes;b:\user\invest;b:\bin`

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)