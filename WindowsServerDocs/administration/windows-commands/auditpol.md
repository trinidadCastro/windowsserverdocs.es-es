---
title: auditpol
description: Artículo de referencia para el comando Auditpol, que muestra información sobre y realiza funciones para manipular directivas de auditoría.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3f503b957175ba2a3997202d83c171cf8683032
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923667"
---
# <a name="auditpol"></a>auditpol

Muestra información sobre y realiza funciones para manipular directivas de auditoría, entre las que se incluyen:

- Establecer y consultar una directiva de auditoría del sistema.

- Establecer y consultar una directiva de auditoría por usuario.

- Establecer y consultar las opciones de auditoría.

- Establecer y consultar el descriptor de seguridad que se usa para delegar el acceso a una directiva de auditoría.

- Informe o copia de seguridad de una directiva de auditoría en un archivo de texto de valores separados por comas (CSV).

- Carga de una directiva de auditoría desde un archivo de texto CSV.

- Configuración de las SACL de recursos globales.

## <a name="syntax"></a>Sintaxis

```
auditpol command [<sub-command><options>]
```

### <a name="parameters"></a>Parámetros

| Subcomando | Descripción |
| ----------- | ----------- |
| /Get | Muestra la Directiva de auditoría actual. Para obtener más información, vea [Auditpol Get](auditpol-get.md) para ver la sintaxis y las opciones. |
| /set | Establece la Directiva de auditoría. Para obtener más información, vea [Auditpol Set](auditpol-set.md) para ver la sintaxis y las opciones. |
| /list | Muestra los elementos de directiva seleccionables. Para obtener más información, vea la [lista Auditpol](auditpol-list.md) para ver la sintaxis y las opciones. |
| /backup | Guarda la Directiva de auditoría en un archivo. Para obtener más información, vea [copia de seguridad de Auditpol](auditpol-backup.md) para ver la sintaxis y las opciones. |
| /restore | Restaura la Directiva de auditoría de un archivo creado previamente mediante Auditpol/backup. Para obtener más información, vea [Auditpol restore](auditpol-restore.md) para ver la sintaxis y las opciones. |
| /Clear | Borra la Directiva de auditoría. Para obtener más información, vea [Auditpol Clear](auditpol-clear.md) para ver la sintaxis y las opciones. |
| /remove | Quita todas las configuraciones de directiva de auditoría por usuario y deshabilita todas las configuraciones de directiva de auditoría del sistema. Para obtener más información, vea [Auditpol Remove](auditpol-remove.md) para ver la sintaxis y las opciones. |
| /resourceSACL | Configura las listas de control de acceso (SACL) del sistema de recursos globales. **Nota:** Solo se aplica a Windows 7 y Windows Server 2008 R2. Para obtener más información, vea [Auditpol resourceSACL](auditpol-resourcesacl.md). |
| /?| Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
