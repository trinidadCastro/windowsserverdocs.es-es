---
title: copia de seguridad Auditpol
description: Tema de comandos de Windows para **copia de seguridad Auditpol**, que realiza una copia de seguridad de la configuración de la Directiva de auditoría del sistema, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría en un archivo de texto de valores separados por comas (CSV).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8895f312606a6a6c45a77c659a1cd98d115babe3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851218"
---
# <a name="auditpol-backup"></a>copia de seguridad Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Realiza una copia de seguridad de la configuración de la Directiva de auditoría del sistema, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría en un archivo de texto de valores separados por comas (CSV).

## <a name="syntax"></a>Sintaxis

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|-----------|------------- |
| /file | Especifica el nombre del archivo en el que se realizará la copia de seguridad de la Directiva de auditoría. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

En el caso de las operaciones de copia de seguridad de la Directiva por usuario y de la Directiva del sistema, debe tener el permiso de control total o de escritura en ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de copia de seguridad con el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar la operación de lista.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para realizar una copia de seguridad de la configuración de la Directiva de auditoría por usuario para todos los usuarios, la configuración de la Directiva de auditoría del sistema y todas las opciones de auditoría en un archivo de texto con formato CSV denominado AuditPolicy. csv, escriba:

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> Si no se especifica ninguna unidad, se usa el directorio actual.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [restauración Auditpol](auditpol-restore.md)
