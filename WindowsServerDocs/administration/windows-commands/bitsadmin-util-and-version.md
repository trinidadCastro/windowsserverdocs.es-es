---
title: versión y versión de bitsadmin
description: Tema de comandos de Windows para el **util y la versión de bitsadmin**, que muestra la versión del servicio bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c2518eb7a8f15d9a592ed9a77dd67a6f8d8afac
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122476"
---
# <a name="bitsadmin-util-and-version"></a>versión y versión de bitsadmin

Muestra la versión del servicio BITS (por ejemplo, 2,0).

> [!NOTE]
> Este comando no es compatible con BITS 1,5 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /util /version [/verbose]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /verbose | Use este modificador para mostrar la versión de archivo de cada archivo DLL relacionado con BITS y comprobar si el servicio BITS se puede iniciar.|

## <a name="examples"></a>Ejemplos

En el siguiente ejemplo se trata de la versión del servicio BITS.

```
C:\>bitsadmin /util /version
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)