---
title: logman
description: Tema de referencia del comando Logman, que crea y administra registros de rendimiento y sesión de seguimiento de eventos y admite muchas funciones del monitor de rendimiento desde la línea de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 574a5203-5b3b-4759-a678-f26d00dde447
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44b5e134440d71eed61ca8e03739abcc962df1f9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820555"
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