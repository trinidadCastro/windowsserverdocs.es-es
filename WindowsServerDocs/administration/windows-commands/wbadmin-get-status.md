---
title: wbadmin get status
description: Artículo de referencia del comando Wbadmin get status, que informa del estado de la operación de copia de seguridad o recuperación que se está ejecutando actualmente.
ms.topic: reference
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 764c5d3dc808b12488e36af5808acf4c895c5af1
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155943"
---
# <a name="wbadmin-get-status"></a>wbadmin get status

Informa del estado de la operación de copia de seguridad o de recuperación que se está ejecutando actualmente.

Para obtener el estado de la copia de seguridad o de la operación de recuperación que se está ejecutando actualmente con este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

> [!IMPORTANT]
> Este comando no se detiene hasta que finaliza la operación de copia de seguridad o recuperación. El comando continúa ejecutándose aunque cierre la ventana de comandos. Para detener la operación de copia de seguridad o recuperación actual, ejecute el comando [Wbadmin STOP Job](wbadmin-stop-job.md) .

## <a name="syntax"></a>Sintaxis

```
wbadmin get status
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin STOP Job](wbadmin-stop-job.md)

- [Get-WBJob](/powershell/module/windowserverbackup/Get-WBJob)
