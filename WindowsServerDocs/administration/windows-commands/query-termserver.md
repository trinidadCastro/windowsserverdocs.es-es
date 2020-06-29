---
title: query termserver
description: Tema de referencia del comando QUERY termserver, que muestra una lista de todos los servidores host de sesión de Escritorio remoto en la red.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c41ed824ee0b1e9dc2672646ef0af03e2593ec07
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471980"
---
# <a name="query-termserver"></a>query termserver

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista de todos los servidores host de sesión de Escritorio remoto en la red. Este comando busca todos los servidores host de sesión Escritorio remoto asociados en la red y devuelve la siguiente información:

- Nombre del servidor

- Red (y dirección de nodo si se usa la opción/Address)

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
query termserver [<servername>] [/domain:<domain>] [/address] [/continue]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<servername>` | Especifica el nombre que identifica el servidor host de sesión Escritorio remoto. |
| /Domain`<domain>` | Especifica el dominio que se va a consultar para los servidores de Terminal Server. No es necesario especificar un dominio si está consultando el dominio en el que está trabajando actualmente. |
| /Address | Muestra las direcciones de red y nodo para cada servidor. |
| /Continue | Impide que se realice una pausa después de mostrar cada pantalla de información. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para mostrar información sobre todos los Escritorio remoto servidores host de sesión en la red, escriba:

```
query termserver
```

Para mostrar información sobre el servidor host de sesión de Escritorio remoto llamado *Server3*, escriba:

```
query termserver Server3
```

Para mostrar información acerca de todos los servidores host de sesión de Escritorio remoto en el dominio *contoso*, escriba:

```
query termserver /domain:CONTOSO
```

Para mostrar la dirección de red y nodo del servidor host de sesión de Escritorio remoto llamado *Server3*, escriba:

```
query termserver Server3 /address
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de consulta](query.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
