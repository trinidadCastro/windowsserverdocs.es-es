---
title: logman import y logman export
description: Artículo de referencia sobre Logman Import y Logman Export, que importa un conjunto de recopiladores de datos desde un archivo XML o exporta un conjunto de recopiladores de datos a un archivo XML.
ms.topic: reference
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: eeb0c30011b70a6fa72bffca18b16ec11e27b958
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639990"
---
# <a name="logman-import-and-logman-export"></a>logman import y logman export

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Importa un conjunto de recopiladores de datos de un archivo XML o exporta un conjunto de recopiladores de datos a un archivo XML.

## <a name="syntax"></a>Sintaxis

```
logman import <[-n] <name> <-xml <name> [options]
logman export <[-n] <name> <-xml <name> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s `<computer name>` | Ejecute el comando en el equipo remoto especificado. |
| -config `<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| [-n] `<name>` | Nombre del objeto de destino. |
| -XML `<name>` | Nombre del archivo XML que se va a importar o exportar. |
| -ETS | Envía comandos a sesiones de seguimiento de eventos directamente sin guardar o programar. |
| -[-] u `<user [password]>` | Especifica el usuario que se va a ejecutar como. Al escribir un `*` para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en la solicitud de contraseña. |
| -y | Responde afirmativamente a todas las preguntas sin preguntar. |
| /? | Muestra la ayuda contextual. |

### <a name="examples"></a>Ejemplos

Para importar el archivo XML *c:\windows\perf_log.xml* desde el equipo *server_1* como un conjunto de recopiladores de datos denominado *perf_log*, escriba:

```
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Logman (comando)](logman.md)
