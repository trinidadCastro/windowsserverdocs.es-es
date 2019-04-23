---
title: restauración Auditpol
description: Tema de los comandos de Windows para **auditpol restauración** -restaura la configuración de directiva de auditoría del sistema, configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditorías desde un archivo sintácticamente coherente con la separados por comas formato de archivo de valores (CSV) usado por el /backup opción.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f1961387083a8a61b27f3e44a2380a6060a02f98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868986"
---
# <a name="auditpol-restore"></a>restauración Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restaura la configuración de directiva de auditoría del sistema, configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditorías desde un archivo sintácticamente coherente con el formato de archivo de valores separados por comas (CSV) utilizado por el /backup opción.

## <a name="syntax"></a>Sintaxis
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/file|Especifica el archivo desde el que se debe restaurar la directiva de auditoría. El archivo se debe haber creado mediante el uso de la /backup opción o debe ser sintácticamente coherente con el formato de archivo CSV usando el /backup opción.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
para las operaciones de restauración de la directiva de por usuario y la directiva del sistema, se debe escribir o permiso Control total sobre ese objeto se establece en el descriptor de seguridad. También puede realizar la operación de restauración mediante que poseen el **Administrar registro de auditoría y seguridad** derecho de usuario (SeSecurityPrivilege). SeSecurityPrivilege resulta útil al restaurar el descriptor de seguridad si se produce un error accidental o un ataque malintencionado.
## <a name="BKMK_examples"></a>Ejemplos
Para restaurar la configuración de directiva de auditoría del sistema, configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditorías desde un archivo denominado auditpolicy.csv que se creó utilizando el/backup, escriba:
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[auditpol copia de seguridad](auditpol-backup.md)
