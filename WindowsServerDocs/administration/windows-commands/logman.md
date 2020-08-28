---
title: logman
description: Artículo de referencia para el comando Logman, que crea y administra registros de rendimiento y sesión de seguimiento de eventos y admite muchas funciones del monitor de rendimiento desde la línea de comandos.
ms.topic: reference
ms.assetid: 574a5203-5b3b-4759-a678-f26d00dde447
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b155e3cca0b9f5e35c1c817cfee4e96ca79b6610
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030554"
---
# <a name="logman"></a>logman

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea y administra registros de rendimiento y de sesión de seguimiento de eventos, y admite muchas funciones del Monitor de rendimiento desde la línea de comandos.

## <a name="syntax"></a>Sintaxis

```
logman [create | query | start | stop | delete| update | import | export | /?] [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [logman create](logman-create.md) | Crea un contador, un seguimiento, un recopilador de datos de configuración o una API. |
| [logman query](logman-query.md) | Consulta las propiedades del recopilador de datos. |
| [logman start &#124; stop](logman-start-stop.md) | Inicia o detiene la recopilación de datos. |
| [logman delete](logman-delete.md) | Elimina un recopilador de datos existente. |
| [logman update](logman-update.md) | Actualiza las propiedades de un recopilador de datos existente. |
| [logman import &#124; export](logman-import-export.md) | Importa un conjunto de recopiladores de datos de un archivo XML o exporta un conjunto de recopiladores de datos a un archivo XML. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)