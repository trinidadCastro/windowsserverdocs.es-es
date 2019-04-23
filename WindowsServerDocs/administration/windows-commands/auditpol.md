---
title: auditpol
description: 'Tema de los comandos de Windows para **auditpol** : muestra información acerca de y realiza funciones para manipular las directivas de auditoría.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7e8364be977e868ac161704e67c37ec5c400e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849226"
---
# <a name="auditpol"></a>auditpol



Muestra información relacionada y realiza funciones para manipular las directivas de auditoría.

Para obtener ejemplos de cómo se puede usar este comando, vea la sección de ejemplos de cada tema.

## <a name="syntax"></a>Sintaxis

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Parámetros

|Subcomando|Descripción|
|-----------|-----------|
|/get|Muestra la directiva de auditoría actual.</br>Consulte [Auditpol get](auditpol-get.md) sintaxis y opciones.|
|/set|Establece la directiva de auditoría.</br>Consulte [Auditpol set](auditpol-set.md) sintaxis y opciones.|
|/list|Muestra los elementos de directiva seleccionables.</br>Consulte [Auditpol lista](auditpol-list.md) sintaxis y opciones.|
|/backup|Guarda la directiva de auditoría en un archivo.</br>Consulte [Auditpol backup](auditpol-backup.md) sintaxis y opciones.|
|/restore|Restaura la directiva de auditoría desde un archivo que se ha creado previamente utilizando auditpol /backup.</br>Consulte [Auditpol restauración](auditpol-restore.md) sintaxis y opciones.|
|/ clear|Borra la directiva de auditoría.</br>Consulte [Auditpol claro](auditpol-clear.md) sintaxis y opciones.|
|/remove|Quita todas las configuraciones de directiva de auditoría por usuario y deshabilita a todas las opciones de directiva de auditoría del sistema.</br>Consulte [Auditpol remove](auditpol-remove.md) sintaxis y opciones.|
|/resourceSACL|Configura las listas de control de acceso de recursos globales del sistema (SACL).</br>Nota: Solo se aplica a Windows 7 y Windows Server 2008 R2.</br>Consulte [resourceSACL Auditpol](auditpol-resourcesacl.md).|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

La herramienta de línea de comandos de directiva de auditoría se puede usar para:
-   Establecer y consultar una directiva de auditoría del sistema.
-   Establecer y consultar una directiva de auditoría por usuario.
-   Establecer y consultar las opciones de auditorías.
-   Establecer y consultar el descriptor de seguridad que se usa para delegar el acceso a una directiva de auditoría.
-   Notificar o copia de seguridad de una directiva de auditoría en un archivo de texto de valores separados por comas (CSV).
-   Cargar una directiva de auditoría de un archivo de texto CSV.
-   Configurar las SACL de recursos globales.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)