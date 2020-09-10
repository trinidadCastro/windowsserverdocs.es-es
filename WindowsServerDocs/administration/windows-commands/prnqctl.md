---
title: prnqctl
description: Artículo de referencia para el comando prnqctl, que imprime una página de prueba, y pausa o reanuda una impresora.
ms.topic: reference
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: bf828a346a8cc4a8987f4171f087e6ee0f08d7c4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641057"
---
# <a name="prnqctl"></a>prnqctl

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Imprime una página de prueba, pausa o reanuda una impresora y borra una cola de impresión. Este comando es un script de Visual Basic ubicado en el `%WINdir%\System32\printing_Admin_Scripts\<language>` directorio. Para usar este comando en un símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prnqctl o cambie los directorios a la carpeta correspondiente. Por ejemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl`.

## <a name="syntax"></a>Sintaxis

```
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <Servername>] [-p <Printername>] [-u <Username>] [-w <password>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -Z | Pausa la impresión en la impresora especificada por el parámetro **-p** . |
| -M | Reanuda la impresión en la impresora especificada por el parámetro **-p** . |
| -E | Imprime una página de prueba en la impresora especificada por el parámetro **-p** . |
| -X | Cancela todos los trabajos de impresión en la impresora especificada por el parámetro **-p** . |
| -s `<Servername>` | Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local. |
| -p `<Printername>` | Necesario. Especifica el nombre de la impresora que desea administrar. |
| -u `<Username>` -w `<password>` | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, "nombre del equipo").

### <a name="examples"></a>Ejemplos

Para imprimir una página de prueba en la impresora Laserprinter1 compartida por el \\ equipo Servidor1, escriba:

```
cscript prnqctl -e -s Server1 -p Laserprinter1
```

Para pausar la impresión en la impresora Laserprinter1 en el equipo local, escriba:

```
cscript prnqctl -z -p Laserprinter1
```

Para cancelar todos los trabajos de impresión de la impresora Laserprinter1 en el equipo local, escriba:

```
cscript prnqctl -x -p Laserprinter1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
