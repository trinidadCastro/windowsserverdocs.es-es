---
title: auditpol backup
description: Artículo de referencia del comando de copia de seguridad Auditpol, que realiza una copia de seguridad de la configuración de la Directiva de auditoría del sistema, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría en un archivo de texto de valores separados por comas (CSV).
ms.topic: reference
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1bee18b3bbad093364301543dda1199598e8ad00
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633239"
---
# <a name="auditpol-backup"></a>auditpol backup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Realiza una copia de seguridad de la configuración de la Directiva de auditoría del sistema, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría en un archivo de texto de valores separados por comas (CSV).

Para realizar operaciones de *copia de seguridad* en las directivas *por usuario* y *del sistema* , debe tener el permiso de **control total** o de **escritura** para ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de *copia* de seguridad si tiene el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar las operaciones de *copia de seguridad* generales.

## <a name="syntax"></a>Sintaxis

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|-----------|------------- |
| /file | Especifica el nombre del archivo en el que se realizará la copia de seguridad de la Directiva de auditoría. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para realizar una copia de seguridad de la configuración de la Directiva de auditoría por usuario para todos los usuarios, la configuración de la Directiva de auditoría del sistema y todas las opciones de auditoría en un archivo de texto con formato CSV denominado auditpolicy.csv, escriba:

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> Si no se especifica ninguna unidad, se usa el directorio actual.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [auditpol restore](auditpol-restore.md)

- [comandos Auditpol](auditpol.md)
