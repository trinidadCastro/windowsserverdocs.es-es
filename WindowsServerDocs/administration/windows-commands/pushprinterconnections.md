---
title: pushprinterconnections
description: Artículo de referencia para el comando PushPrinterConnections, que lee la configuración de conexión de impresora implementada de directiva de grupo e implementa o quita conexiones de impresora según sea necesario.
ms.topic: reference
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ef1cfc110d446da461251b9c7e28a4595edee291
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639940"
---
# <a name="pushprinterconnections"></a>pushprinterconnections

Lee la configuración de la conexión de impresora implementada de directiva de grupo e implementa o quita las conexiones de impresora según sea necesario.

> [!IMPORTANT]
> Esta utilidad se usa en scripts de inicio de equipo o de inicio de sesión de usuario, y no debe ejecutarse desde la línea de comandos.

## <a name="syntax"></a>Sintaxis

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| <-registro> | Escribe un archivo de registro de depuración por usuario en *% temp*o escribe un registro de depuración por máquina en *%windir%\temp*. |
| <-? > | Muestra la Ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)

- [Implementar impresoras mediante directiva de grupo](https://go.microsoft.com/fwlink/?LinkId=230627)
