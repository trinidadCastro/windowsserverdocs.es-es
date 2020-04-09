---
title: mklink
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d6328d972b07b1bebd272788b896fd491e47380
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839558"
---
# <a name="mklink"></a>mklink
Crea un vínculo simbólico.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

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
|\<vínculo >|Especifica el nombre del vínculo simbólico que se va a crear.|
|> de destino de \<|Especifica la ruta de acceso (relativa o absoluta) a la que hace referencia el nuevo vínculo simbólico.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se muestra la creación y la eliminación de un vínculo simbólico denominado mis carpetas y el archivo. file desde el directorio raíz al directorio \Users\User1\Documents y un archivo example ubicado en el directorio:
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
