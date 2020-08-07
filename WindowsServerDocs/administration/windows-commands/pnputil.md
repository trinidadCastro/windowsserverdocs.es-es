---
title: pnputil
description: Artículo de referencia para el comando pnputil, que agrega paquetes de controladores, quita paquetes de controladores y enumera los paquetes de controladores que se encuentran en el almacén de controladores mediante la utilidad pnputil.exe.
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4bd4a5ed4dcadb1011dfbf28647ba4f223249a34
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884924"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe es una utilidad de línea de comandos que puede usar para administrar el almacén de controladores. Puede usar este comando para agregar paquetes de controladores, quitar paquetes de controladores y enumerar los paquetes de controladores que se encuentran en la tienda.

## <a name="syntax"></a>Sintaxis

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -a | Especifica que se va a agregar el archivo INF identificado. |
| -d | Especifica que se elimine el archivo INF identificado. |
| -E | Especifica la enumeración de todos los archivos INF de terceros. |
| -f | Especifica que se debe forzar la eliminación del archivo INF identificado. No se puede usar junto con el parámetro **– i** . |
| -i | Especifica que se instale el archivo INF identificado. No se puede usar junto con el parámetro **-f** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para agregar un archivo INF, llamado USBCAM. INF, escriba:

```
pnputil.exe -a a:\usbcam\USBCAM.INF
```

Para agregar todos los archivos INF, ubicados en c:\drivers, escriba:

```
pnputil.exe -a c:\drivers\*.inf
```

Para agregar e instalar USBCAM. Controlador INF, escriba:

```
pnputil.exe -i -a a:\usbcam\USBCAM.INF
```

Para enumerar todos los controladores de terceros, escriba:

```
pnputil.exe –e
```

Para eliminar el archivo INF y el controlador denominado oem0. inf, escriba:

```
pnputil.exe -d oem0.inf
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [popd (comando)](popd.md)
