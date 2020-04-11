---
title: bitsadmin setnotifycmdline
description: Windows Commands topic for **bitsadmin setnotifycmdline**, que establece el comando de línea de comandos que se ejecutará cuando el trabajo termine de transferir datos, o cuando un trabajo entre en un estado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b268b68cbd355a7fe7f993d678a98f6fcb99f0ab
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122897"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Establece el comando de línea de comandos que se ejecutará cuando el trabajo finalice la transferencia de datos o cuando un trabajo entre en un estado especificado.

> [!NOTE]
> Este comando no es compatible con BITS 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| program_name | Nombre del comando que se va a ejecutar cuando se complete el trabajo. Puede establecer este valor en NULL, pero si lo hace, *program_parameters* también debe establecerse en NULL. |
| program_parameters | Parámetros que se van a pasar a *program_name*. Este valor se puede establecer como NULL. Si *program_parameters* no está establecido en null, el primer parámetro de *program_parameters* debe coincidir con el *program_name*. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establece el comando de línea de comandos que usa el servicio para ejecutar Notepad. exe después de que se complete el trabajo denominado *myDownloadJob* .

```
C:\>bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe NULL
```

```
C:\>bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)