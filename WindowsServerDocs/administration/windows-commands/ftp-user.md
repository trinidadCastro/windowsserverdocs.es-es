---
title: ftp user
description: Artículo de referencia para el comando de usuario de FTP, que especifica un usuario en el equipo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b1eea449765b58461d410f0e015d4978c3ad016
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957359"
---
# <a name="ftp-user"></a>ftp user

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Especifica un usuario para el equipo remoto.

## <a name="syntax"></a>Sintaxis

```
user <username> [<password>] [<account>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<username>` | Especifica un nombre de usuario con el que iniciar sesión en el equipo remoto. |
| `[<password>]` | Especifica la contraseña del *nombre de usuario*. Si no se especifica una contraseña, pero se requiere, el comando **FTP** solicita la contraseña. |
| `[<account>]` | Especifica una cuenta con la que se iniciará sesión en el equipo remoto. Si no se especifica una *cuenta* , pero se requiere, el comando **FTP** solicita la cuenta. |

### <a name="examples"></a>Ejemplos

Para especificar *user1* con la contraseña *contraseña1*, escriba:

```
user User1 Password1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
