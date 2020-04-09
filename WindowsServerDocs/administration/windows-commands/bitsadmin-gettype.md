---
title: bitsadmin gettype
description: Tema de comandos de Windows para **bitsadmin GetType**, que recupera el tipo de trabajo del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66a1fc5b0478e1eec26557dc9a7f76d50abcb8b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850448"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

Recupera el tipo de trabajo del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="output"></a>Salida

Los valores de salida incluyen:

| Tipo | Descripción |
| --------------- | ----------- |
| Descargar | El trabajo es una descarga. |
| Cargar | El trabajo es una carga. |
| Upload-Reply | El trabajo es una respuesta de carga. |
| Desconocida | El trabajo tiene un tipo desconocido. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera el tipo de trabajo para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)