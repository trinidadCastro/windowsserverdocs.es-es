---
title: pnputil
description: Obtenga información sobre cómo administrar el almacén de controladores con la utilidad pnputil.exe.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5bde78d97be8def9f8594572869c34ef213db480
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862546"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe es una utilidad de línea de comandos que puede usar para administrar el almacén de controladores. Puede usar Pnputil para agregar paquetes de controladores, quite los paquetes de controladores y paquetes de controladores de lista que se encuentran en el almacén.

## <a name="syntax"></a>Sintaxis

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-a|Especifica que se agregue el archivo INF identificado.|
|-d|Especifica que se elimine el archivo INF identificado.|
|-e|Si se especifica para enumerar todos los archivos INF de terceros.|
|-f|Si se especifica para forzar la eliminación del archivo INF identificado. No se puede usar junto con el **– i** parámetro.|
|-i|Especifica que se instale el archivo INF identificado. No se puede usar junto con el **-f** parámetro.|
|/?|Muestra la ayuda en el símbolo del sistema.|


## <a name="examples"></a>Ejemplos

-   pnputil.exe - a a:\usbcam\USBCAM. INF agrega el archivo INF que se especifica por USBCAM. INF
-   pnputil.exe - a c:\drivers\*.inf agrega todos los archivos INF en c:\drivers\
-   pnputil.exe -i - a a:\usbcam\USBCAM. INF agrega e instala el controlador especificado.
-   pnputil.exe – e enumera todos los controladores de terceros.
-   pnputil.exe -d oem0.inf elimina especificado.
-   pnputil.exe -f -d oem0.inf fuerza la eliminación del archivo INF especificado.

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Popd](popd.md)