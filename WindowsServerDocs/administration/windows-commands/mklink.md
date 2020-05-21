---
title: mklink
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4bfa1c928b5bc5f4c5a885378f0f1d1c9b99cf5
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437140"
---
# <a name="mklink"></a>mklink
Crea un vínculo simbólico.



## <a name="syntax"></a>Sintaxis

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/d|Crea un vínculo simbólico de directorio. De forma predeterminada, **Mklink** crea un vínculo simbólico de archivo.|
|/h|Crea un vínculo físico en lugar de un vínculo simbólico.|
|/j|Crea una Unión de directorio.|
|\<> de vínculos|Especifica el nombre del vínculo simbólico que se va a crear.|
|\<Target>|Especifica la ruta de acceso (relativa o absoluta) a la que hace referencia el nuevo vínculo simbólico.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para mostrar la creación y eliminación de un vínculo simbólico denominado mis carpetas y el archivo. file desde el directorio raíz al directorio \Users\User1\Documents y un archivo example ubicado en el directorio:
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>Referencias adicionales
-   [New-Item](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
