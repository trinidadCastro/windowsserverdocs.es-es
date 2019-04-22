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
ms.openlocfilehash: eabbf159a64ab5df7f45ece390d0c2fdb9956b80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826336"
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

Para crear un vínculo simbólico denominado MyDocs desde el directorio raíz en el directorio \Users\User1\Documents, escriba:
```
mklink /d \MyDocs \Users\User1\Documents
```
## <a name="additional-references"></a>Referencias adicionales
-   [New-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
