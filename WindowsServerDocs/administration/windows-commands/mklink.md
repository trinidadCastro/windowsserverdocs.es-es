---
title: mklink
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d930cbf7acbfceab16f2fa619aaaac6e789c131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373642"
---
# <a name="mklink"></a>mklink
Crea un vínculo simbólico.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/d|Crea un vínculo simbólico de directorio. De forma predeterminada, **Mklink** crea un vínculo simbólico de archivo.|
|/h|Crea un vínculo físico en lugar de un vínculo simbólico.|
|/j|Crea una Unión de directorio.|
|@no__t 0Link >|Especifica el nombre del vínculo simbólico que se va a crear.|
|@no__t 0Target >|Especifica la ruta de acceso (relativa o absoluta) a la que hace referencia el nuevo vínculo simbólico.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguientes se muestra la creación y la eliminación de un vínculo simbólico denominado mis carpetas y el archivo. file desde el directorio raíz al directorio \Users\User1\Documents y un archivo example ubicado en el directorio:
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
