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
ms.openlocfilehash: 134e6ce4b1fc44450047de3287b7daac67da4b6a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837518"
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
|-e|Especifica la enumeración de todos los archivos INF de terceros.|
|-f|Especifica que se debe forzar la eliminación del archivo INF identificado. No se puede usar junto con el parámetro **– i** .|
|-i|Especifica que se instale el archivo INF identificado. No se puede usar junto con el parámetro **-f** .|
|/?|Muestra la Ayuda en el símbolo del sistema.|


## <a name="examples"></a>Ejemplos

-   pnputil. exe-a a:\usbcam\USBCAM. INF agrega el archivo INF especificado por USBCAM. INF
-   pnputil. exe: un c:\drivers\*. inf agrega todos los archivos INF en c:\drivers\
-   pnputil. exe-i-a a:\usbcam\USBCAM. INF agrega e instala el controlador especificado.
-   pnputil. exe – e enumera todos los controladores de terceros.
-   pnputil. exe-d oem0. inf elimina el especificado.
-   pnputil. exe-f-d oem0. inf fuerza la eliminación del archivo INF especificado.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Popd](popd.md)