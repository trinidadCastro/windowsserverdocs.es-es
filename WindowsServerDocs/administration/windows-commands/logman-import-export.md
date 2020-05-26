---
title: Logman Import y Logman Export
description: Tema de referencia sobre Logman Import y Logman Export, que importa un conjunto de recopiladores de datos desde un archivo XML o exporta un conjunto de recopiladores de datos a un archivo XML.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44a659abc9e364bf10487e93a7937c1cf8d51bbc
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820575"
---
# <a name="logman-import-and-logman-export"></a>Logman Import y Logman Export

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Importa un conjunto de recopiladores de datos de un archivo XML o exporta un conjunto de recopiladores de datos a un archivo XML.

## <a name="syntax"></a>Sintaxis

```
logman import <[-n] <name>> <-xml <name>> [options]
logman export <[-n] <name>> <-xml <name>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s`<computer name>` | Ejecute el comando en el equipo remoto especificado. |
| -config`<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| [-n]`<name>` | Nombre del objeto de destino. |
| -XML`<name>` | Nombre del archivo XML que se va a importar o exportar. |
| -ETS | Envía comandos a sesiones de seguimiento de eventos directamente sin guardar o programar. |
| -[-] u`<user [password]>` | Especifica el usuario que se va a ejecutar como. Al escribir un `*` para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en la solicitud de contraseña. |
| -y | Responde afirmativamente a todas las preguntas sin preguntar. |
| /? | Muestra la ayuda contextual. |

### <a name="examples"></a>Ejemplos

Para importar el archivo XML *c:\windows\ perf_log. XML* del equipo *server_1* como un conjunto de recopiladores de datos denominado *perf_log*, escriba:

```
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [logman](logman.md)
