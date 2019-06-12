---
title: mklink
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ca9f5f00bc92d7929f782be45562e80bba455d74
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501452"
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
|/d|Crea un vínculo simbólico del directorio. De forma predeterminada, **mklink** crea un vínculo simbólico del archivo.|
|/h|Crea un vínculo físico en lugar de un vínculo simbólico.|
|/j|Crea a una unión de directorios.|
|\<Vínculo >|Especifica el nombre del vínculo simbólico que se va a crear.|
|\<Destino >|Especifica la ruta de acceso (relative o absolute) que hace referencia el nuevo vínculo simbólico.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente muestra la creación y eliminación de un vínculo simbólico denominado MiCarpeta y MyFile.file desde el directorio raíz en el directorio \Users\User1\Documents y un example.file ubicado en el directorio:
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>Referencias adicionales
-   [New-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/rd)
