---
title: bitsadmin wrap
description: Tema de comandos de Windows para el **ajuste de bitsadmin**, que ajusta cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos a la línea siguiente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e754a765d94661baf24190431b455584d29991ec
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122564"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajusta cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos a la línea siguiente. Debe especificar este modificador antes que cualquier otro modificador.

De forma predeterminada, todos los conmutadores excepto el modificador de la [monitor bitsadmin](bitsadmin-monitor.md) ajustan el texto de salida.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| Trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se recupera información del trabajo denominado *myDownloadJob* y se incluye la salida.

```
C:\>bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
