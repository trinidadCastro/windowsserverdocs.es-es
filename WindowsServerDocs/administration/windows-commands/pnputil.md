---
title: pnputil
description: Obtenga información acerca de cómo administrar el almacén de controladores con la utilidad pnputil. exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4c6bcb138e8bd7308c01c2c53fba83b69362298a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436370"
---
# <a name="pnputil"></a>pnputil

Pnputil. exe es una utilidad de línea de comandos que puede usar para administrar el almacén de controladores. Puede usar pnputil para agregar paquetes de controladores, quitar paquetes de controladores y enumerar los paquetes de controladores que se encuentran en el almacén.

## <a name="syntax"></a>Sintaxis

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-a|Especifica que se va a agregar el archivo INF identificado.|
|-d|Especifica que se elimine el archivo INF identificado.|
|-E|Especifica la enumeración de todos los archivos INF de terceros.|
|-f|Especifica que se debe forzar la eliminación del archivo INF identificado. No se puede usar junto con el parámetro **– i** .|
|-i|Especifica que se instale el archivo INF identificado. No se puede usar junto con el parámetro **-f** .|
|/?|Muestra la ayuda en el símbolo del sistema.|


## <a name="examples"></a>Ejemplos

-   pnputil. exe-a a:\usbcam\USBCAM. INF agrega el archivo INF especificado por USBCAM. INF
-   pnputil. exe: un c:\drivers \* . inf agrega todos los archivos INF en c:\drivers\
-   pnputil. exe-i-a a:\usbcam\USBCAM. INF agrega e instala el controlador especificado.
-   pnputil. exe – e enumera todos los controladores de terceros.
-   pnputil. exe-d oem0. inf elimina el especificado.
-   pnputil. exe-f-d oem0. inf fuerza la eliminación del archivo INF especificado.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Popd](popd.md)