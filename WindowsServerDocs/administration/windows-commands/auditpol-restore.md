---
title: auditpol restore
description: Artículo de referencia del comando Auditpol restore, que restaura la configuración de la Directiva de auditoría del sistema, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría de un archivo que es sintácticamente coherente con el formato de archivo de valores separados por comas (CSV) que usa la opción/backup.
ms.topic: reference
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d6dc8dde77189cbd1134779bf89f253402f181dc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633109"
---
# <a name="auditpol-restore"></a>auditpol restore

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Restaura la configuración de la Directiva de auditoría del sistema, la configuración de la Directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría de un archivo que es sintácticamente coherente con el formato de archivo de valores separados por comas (CSV) que usa la opción/backup.

Para realizar operaciones de *restauración* en las directivas *por usuario* y *del sistema* , debe tener el permiso de **control total** o de **escritura** para ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de *restauración* si tiene el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege), lo que resulta útil al restaurar el descriptor de seguridad si se produce un error o un ataque malintencionado.

## <a name="syntax"></a>Sintaxis

```
auditpol /restore /file:<filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /file | Especifica el archivo del que se debe restaurar la Directiva de auditoría. El archivo se debe haber creado con la opción/backup o debe ser sintácticamente coherente con el formato de archivo CSV usado por la opción/backup. |
| /? |Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para restaurar la configuración de la Directiva de auditoría del sistema, la configuración de la Directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría de un archivo denominado auditpolicy.csv creado mediante el comando/backup, escriba:

```
auditpol /restore /file:c:\auditpolicy.csv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [auditpol backup](auditpol-backup.md)

- [comandos Auditpol](auditpol.md)
