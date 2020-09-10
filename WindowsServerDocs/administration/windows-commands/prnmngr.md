---
title: prnmngr
description: Artículo de referencia para el comando PRNMNGR, que agrega, elimina y enumera impresoras o conexiones de impresora, además de establecer y mostrar la impresora predeterminada.
ms.topic: reference
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: cfbba48747dfca7230f0ef397201cea610fdc3dd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628053"
---
# <a name="prnmngr"></a>prnmngr

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega, elimina y enumera impresoras o conexiones de impresora, además de establecer y mostrar la impresora predeterminada. Este comando es un script de Visual Basic ubicado en el `%WINdir%\System32\printing_Admin_Scripts\<language>` directorio. Para usar este comando en un símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo PRNMNGR o cambie los directorios a la carpeta correspondiente. Por ejemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr`.

## <a name="syntax"></a>Sintaxis

```
cscript prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <Servername>] [-p <Printername>] [-m <printermodel>] [-r <portname>] [-u <Username>]
[-w <password>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -a | Agrega una conexión de impresora local. |
| -d | Elimina una conexión de impresora. |
| -X | Elimina todas las impresoras del servidor especificado por el parámetro **-s** . Si no especifica un servidor, Windows elimina todas las impresoras en el equipo local. |
| -g | Muestra la impresora predeterminada. |
| -T | Establece la impresora predeterminada en la impresora especificada por el parámetro **-p** . |
| -l | Muestra todas las impresoras instaladas en el servidor especificado por el parámetro **-s** . Si no especifica un servidor, Windows enumera las impresoras instaladas en el equipo local. |
| c | Especifica que el parámetro se aplica a las conexiones de impresora. Se puede usar con los parámetros **-a** y **-x** . |
| -s `<Servername>` | Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local. |
| -p `<Printername>` | Especifica el nombre de la impresora que desea administrar. |
| -m `<Modelname>` | Especifica (por nombre) el controlador que desea instalar. Los controladores a menudo se denominan para el modelo de impresora que admiten. Consulte la documentación de la impresora para obtener más información. |
| -r `<portname>` | Especifica el puerto al que está conectada la impresora. Si se trata de un puerto paralelo o serie, use el identificador del puerto (por ejemplo, LPT1: o COM1:). Si se trata de un puerto TCP/IP, utilice el nombre de puerto que se especificó cuando se agregó el puerto. |
| -u `<Username>` -w `<password>` | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, "nombre del equipo").

### <a name="examples"></a>Ejemplos

Para agregar una impresora llamada colorprinter_2 conectada a LPT1 en el equipo local y requiere un controlador de impresora llamado color Printer Driver1, escriba:

```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```

Para eliminar la impresora denominada colorprinter_2 del equipo remoto llamado ServidorRH, escriba:

```
cscript prnmngr -d -s HRServer -p colorprinter_2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
