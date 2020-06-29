---
title: query process
description: Tema de referencia del comando QUERY Process, que muestra información sobre los procesos que se ejecutan en un servidor host de sesión Escritorio remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32f755fe3e275f2f1adccffacaf2a6999d46298a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472030"
---
# <a name="query-process"></a>query process

Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información sobre los procesos que se ejecutan en un servidor host de sesión Escritorio remoto. Puede usar este comando para averiguar qué programas ejecuta un usuario específico y también qué usuarios ejecutan un programa específico. Este comando devuelve la siguiente información:

- Usuario propietario del proceso

- Sesión que posee el proceso

- IDENTIFICADOR de la sesión

- Nombre del proceso

- IDENTIFICADOR del proceso

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
query process [*|<processID>|<username>|<sessionname>|/id:<nn>|<programname>] [/server:<servername>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| * | Muestra los procesos de todas las sesiones. |
| `<processID>` | Especifica el identificador numérico que identifica el proceso que desea consultar. |
| `<username>` | Especifica el nombre del usuario cuyos procesos desea enumerar. |
| `<sessionname>` | Especifica el nombre de la sesión activa cuyos procesos desea enumerar. |
| /ID`<nn>` | Especifica el identificador de la sesión cuyos procesos desea enumerar. |
| `<programname>` | Especifica el nombre del programa cuyos procesos desea consultar. Se requiere la extensión. exe. |
| /server:`<servername>` | Especifica el servidor host de sesión de Escritorio remoto cuyos procesos desea enumerar. Si no se especifica, se usa el servidor en el que ha iniciado sesión actualmente. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Los administradores tienen acceso total a todas las funciones de **proceso de consulta** .

- Si no especifica la <*nombre de usuario*>, <*nombresesión*>, */id `<nn>` :*, <*nombreprograma*> o *&#42;* parámetros, esta consulta muestra solo los procesos que pertenecen al usuario actual.

- Cuando el **proceso de consulta** devuelve información, se muestra un símbolo mayor que antes de `(>)` cada proceso que pertenece a la sesión actual.

## <a name="examples"></a>Ejemplos

Para mostrar información sobre los procesos que están usando todas las sesiones, escriba:

```
query process *
```

Para mostrar información sobre los procesos utilizados por el *ID. de sesión 2*, escriba:

```
query process /ID:2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de consulta](query.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
