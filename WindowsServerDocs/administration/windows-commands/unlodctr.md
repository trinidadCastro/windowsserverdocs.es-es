---
title: unlodctr
description: Artículo de referencia para el comando unlodctr, que quita nombres de contadores de rendimiento y texto explicativo de un servicio o controlador de dispositivo del registro del sistema.
ms.topic: reference
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3983f98df0de6a8048f78b6ebdb16cccafea8da4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156752"
---
# <a name="unlodctr"></a>unlodctr

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita los nombres de los **contadores de rendimiento** y el **texto explicativo** de un servicio o controlador de dispositivo del registro del sistema.

> [!WARNING]
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

## <a name="syntax"></a>Sintaxis

```
unlodctr <drivername>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<drivername>` | Quita la configuración del **nombre del contador de rendimiento** y el **texto explicativo** del controlador o servicio `<drivername>` del registro de Windows Server. Si `<drivername>` incluye espacios, debe usar comillas alrededor del texto, por ejemplo "nombre del controlador". |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para quitar los **nombres de contador de rendimiento** actuales y el **texto explicativo** del servicio de Protocolo simple de transferencia de correo (SMTP), escriba:

```
unlodctr SMTPSVC
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
