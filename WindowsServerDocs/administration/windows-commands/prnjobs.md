---
title: prnjobs
description: Tema de referencia del comando prnjobs, que pone en pausa, reanuda, cancela y enumera los trabajos de impresión.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: e13e217d422aa6d8f2c585c8890915af7e396ddb
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472270"
---
# <a name="prnjobs"></a>prnjobs

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Pausa, reanuda, cancela y enumera los trabajos de impresión. Este comando es un script de Visual Basic ubicado en el `%WINdir%\System32\printing_Admin_Scripts\<language>` directorio. Para usar este comando en un símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prnjobs o cambie los directorios a la carpeta correspondiente. Por ejemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs`.

## <a name="syntax"></a>Sintaxis

```
cscript prnjobs {-z | -m | -x | -l | -?} [-s <Servername>] [-p <Printername>] [-j <JobID>] [-u <Username>] [-w <password>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -Z | Pausa el trabajo de impresión especificado por el parámetro **-j** . |
| -M | Reanuda el trabajo de impresión especificado por el parámetro **-j** . |
| -X | Cancela el trabajo de impresión especificado por el parámetro **-j** . |
| -l | Enumera todos los trabajos de impresión de una cola de impresión. |
| -s`<Servername>` | Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local. |
| -p`<Printername>` | Necesario. Especifica el nombre de la impresora que desea administrar. |
| -j`<JobID>` | Especifica (por número de identificador) el trabajo de impresión que desea cancelar. |
| -u `<Username>` -w`<password>` | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, "nombre del equipo").

### <a name="examples"></a>Ejemplos

Para pausar un trabajo de impresión con un ID. de trabajo 27 enviado al equipo remoto llamado ServidorRH para imprimirlo en la impresora denominada colorprinter, escriba:

```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```

Para enumerar todos los trabajos de impresión actuales en la cola de la impresora local llamada colorprinter_2, escriba:

```
cscript prnjobs.vbs -l -p colorprinter_2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
