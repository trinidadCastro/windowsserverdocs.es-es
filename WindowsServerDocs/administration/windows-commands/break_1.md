---
title: break
description: Windows Commands tema para break_1, que establece o borra la comprobación extendida de CTRL + C en sistemas MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 809a9321b8b4f8b2d201582f767da132076826d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848368"
---
# <a name="break"></a>break

Establece o borra la comprobación extendida de CTRL + C en sistemas MS-DOS. Si se usa sin parámetros, **break** muestra la configuración actual.

> [!NOTE]
> Este comando ya no está en uso. Solo se incluye para mantener la compatibilidad con los archivos de MS-DOS existentes, pero no tiene ningún efecto en la línea de comandos porque su funcionalidad es automática.

## <a name="syntax"></a>Sintaxis

```
break=[on|off]
```

## <a name="remarks"></a>Comentarios

Si las extensiones de comando se habilitan y se ejecutan en la plataforma Windows, la inserción del comando **break** en un archivo por lotes entra en un punto de interrupción codificado de forma rígida si lo depura un depurador.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)