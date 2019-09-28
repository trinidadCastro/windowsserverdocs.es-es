---
title: restauración Auditpol
description: 'Temas de comandos de Windows para **restauración de Auditpol** : restaura la configuración de la Directiva de auditoría del sistema, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría de un archivo que es sintácticamente coherente con el formato de archivo de valores separados por comas (CSV). usado por la opción/backup.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b91f3745354c695c4ab0c71b429718bff05d8098
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382413"
---
# <a name="auditpol-restore"></a>restauración Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restaura la configuración de la Directiva de auditoría del sistema, la configuración de la Directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría de un archivo que es sintácticamente coherente con el formato de archivo de valores separados por comas (CSV) que usa la opción/backup.

## <a name="syntax"></a>Sintaxis
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/File|Especifica el archivo del que se debe restaurar la Directiva de auditoría. El archivo se debe haber creado con la opción/backup o debe ser sintácticamente coherente con el formato de archivo CSV usado por la opción/backup.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
en el caso de las operaciones de restauración de la Directiva de usuario y la Directiva del sistema, debe tener el permiso de control total o de escritura en ese objeto establecido en el descriptor de seguridad. También puede realizar la operación de restauración con el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). SeSecurityPrivilege es útil cuando se restaura el descriptor de seguridad en caso de que se produzca un error accidental o un ataque malintencionado.
## <a name="BKMK_examples"></a>Example
Para restaurar la configuración de la Directiva de auditoría del sistema, la configuración de la Directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría de un archivo denominado AuditPolicy. csv que se creó con el comando/backup, escriba:
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[copia de seguridad Auditpol](auditpol-backup.md)
