---
title: mqbkup
description: Tema de referencia del comando Mqbkup, que realiza una copia de seguridad de los archivos de mensajes de MSMQ y de la configuración del registro en un dispositivo de almacenamiento y restaura los mensajes y configuraciones almacenados previamente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c07dd5f912a70157052017fc17875c00eaedd3b
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354426"
---
# <a name="mqbkup"></a>mqbkup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Realiza una copia de seguridad de los archivos de mensajes y la configuración del registro de MSMQ en un dispositivo de almacenamiento y restaura los mensajes y configuraciones almacenados previamente.

Las operaciones de copia de seguridad y restauración detienen el servicio MSMQ local. Si el servicio MSMQ se inició de antemano, la utilidad intentará reiniciar el servicio MSMQ al final de la copia de seguridad o de la operación de restauración. Si el servicio ya se detuvo antes de ejecutar la utilidad, no se intentará reiniciar el servicio.

Antes de utilizar la utilidad de copia de seguridad/restauración de mensajes de MSMQ, debe cerrar todas las aplicaciones locales que utilizan MSMQ.

## <a name="syntax"></a>Sintaxis

```
mqbkup {/b | /r} <folder path_to_storage_device>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /b | Especifica la operación de copia de seguridad. |
| /r | Especifica la operación de restauración. |
| `<folder path_to_storage_device>` | Especifica la ruta de acceso donde se almacenan los archivos de mensajes y la configuración del registro de MSMQ. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si una carpeta especificada no existe mientras se realiza la operación de copia de seguridad o restauración, la utilidad crea automáticamente la carpeta.

- Si decide especificar una carpeta existente, debe estar vacía. Si especifica una carpeta que no está vacía, la utilidad elimina todos los archivos y subcarpetas que contiene. En este caso, se le pedirá que conceda permiso para eliminar archivos y subcarpetas existentes. Puede usar el parámetro **/y** para indicar que acepta de antemano la eliminación de todos los archivos y subcarpetas existentes en la carpeta especificada.

- Las ubicaciones de las carpetas utilizadas para almacenar los archivos de mensajes de MSMQ se almacenan en el registro. Por lo tanto, la utilidad restaura los archivos de mensajes de MSMQ en las carpetas especificadas en el registro y no en las carpetas de almacenamiento usadas antes de la operación de restauración.

### <a name="examples"></a>Ejemplos

Para realizar una copia de seguridad de todos los archivos de mensajes de MSMQ y la configuración del registro, y almacenarlos en la carpeta *msmqbkup* de la unidad C:, escriba:

```
mqbkup /b c:\msmqbkup
```

Para eliminar todos los archivos y subcarpetas existentes en la carpeta *oldbkup* de la unidad C: y, a continuación, almacenar los archivos de mensajes de MSMQ y la configuración del registro en la carpeta, escriba:

```
mqbkup /b /y c:\oldbkup
```

Para restaurar la configuración del registro y los mensajes de MSMQ, escriba:

```
mqbkup /r c:\msmqbkup
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de PowerShell para MSMQ](https://docs.microsoft.com/powershell/module/msmq/?view=win10-ps)
