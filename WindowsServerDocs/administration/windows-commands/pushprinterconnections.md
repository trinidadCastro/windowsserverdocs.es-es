---
title: pushprinterconnections
description: Artículo de referencia para el comando PushPrinterConnections, que lee la configuración de conexión de impresora implementada de directiva de grupo e implementa o quita conexiones de impresora según sea necesario.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b22fd5143a9477b40a515df44c9a0b5663dfd7a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931381"
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
