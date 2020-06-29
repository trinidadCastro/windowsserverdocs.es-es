---
title: pushprinterconnections
description: Tema de referencia del comando PushPrinterConnections, que lee la configuración de conexión de impresora implementada de directiva de grupo e implementa o quita conexiones de impresora según sea necesario.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 701089f2597b1d4e7bc05f7949dbc80dee3535bb
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472130"
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
