---
title: cmdkey
description: Tema de referencia del comando cmdkey, que crea, enumera y elimina nombres de usuario y contraseñas almacenados, o credenciales.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4005707785101fcc1fb0030ffe895668bd65f730
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712153"
---
# <a name="cmdkey"></a>cmdkey

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea, enumera y elimina los nombres de usuario y las contraseñas o credenciales almacenados.

## <a name="syntax"></a>Sintaxis

```
cmdkey [{/add:<targetname>|/generic:<targetname>}] {/smartcard | /user:<username> [/pass:<password>]} [/delete{:<targetname> | /ras}] /list:<targetname>
```

### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| /Add`<targetname>` | Agrega un nombre de usuario y una contraseña a la lista.<p>Requiere el parámetro de `<targetname>` que identifica el nombre de equipo o dominio con el que se asociará esta entrada. |
| /genérico`<targetname>` | Agrega credenciales genéricas a la lista.<p>Requiere el parámetro de `<targetname>` que identifica el nombre de equipo o dominio con el que se asociará esta entrada. |
| /SmartCard | Recupera la credencial de una tarjeta inteligente. Si se encuentra más de una tarjeta inteligente en el sistema cuando se usa esta opción, **cmdkey** muestra información acerca de todas las tarjetas inteligentes disponibles y, a continuación, solicita al usuario que especifique cuál desea usar. |
| /User`<username>` | Especifica el nombre de usuario o de cuenta que se va a almacenar con esta entrada. Si `<username>` no se proporciona, se le solicitará. |
|/Pass`<password>` | Especifica la contraseña que se va a almacenar con esta entrada. Si `<password>` no se proporciona, se le solicitará. Las contraseñas no se muestran después de su almacenamiento. |
| /delete{:`<targetname>` | acceso | Elimina un nombre de usuario y una contraseña de la lista. Si `<targetname>` se especifica, se elimina esa entrada. Si `/ras` se especifica, se elimina la entrada de acceso remoto almacenada. |
| /List`<targetname>` | Muestra la lista de nombres de usuario y credenciales almacenados. Si `<targetname>` no se especifica, se enumeran todos los nombres de usuario y las credenciales almacenados. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para mostrar una lista de todos los nombres de usuario y las credenciales que se almacenan, escriba:

```
cmdkey /list
```

Para agregar un nombre de usuario y una contraseña para el usuario *Mikedan* para acceder al equipo *Server01* con la contraseña *KLEO*, escriba:

```
cmdkey /add:server01 /user:mikedan /pass:Kleo
```

Para agregar un nombre de usuario y una contraseña para el usuario *Mikedan* para acceder al equipo *Server01* y solicitar la contraseña cada vez que se tenga acceso a Server01, escriba:

```
cmdkey /add:server01 /user:mikedan
```

Para eliminar una credencial almacenada por acceso remoto, escriba:

```
cmdkey /delete /ras
```

Para eliminar una credencial almacenada para *Server01*, escriba:

```
cmdkey /delete:server01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
