---
title: bitsadmin rawreturn
description: Tema de comandos de Windows para **bitsadmin rawreturn**, que devuelve los datos adecuados para el análisis.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8cd84b6082b1fda8061ee7b324dd66991d3b206
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849888"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Devuelve los datos adecuados para el análisis. Normalmente, este comando se usa junto con los modificadores **/Create** e **/Get*** para recibir solo el valor. Debe especificar este modificador antes que otros modificadores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /rawreturn
```

## <a name="remarks"></a>Comentarios

- Quita los caracteres de nueva línea y el formato de la salida.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recuperan los datos sin procesar para el estado del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)