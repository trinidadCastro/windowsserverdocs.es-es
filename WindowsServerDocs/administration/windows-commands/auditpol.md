---
title: auditpol
description: 'Temas de comandos de Windows para **Auditpol** : muestra información sobre y realiza funciones para manipular directivas de auditoría.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5e249a9e2a07505f052b774208c514b4d16879b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382382"
---
# <a name="auditpol"></a>auditpol



Muestra información sobre y realiza funciones para manipular las directivas de auditoría.

Para obtener ejemplos de cómo se puede usar este comando, consulte la sección ejemplos de cada tema.

## <a name="syntax"></a>Sintaxis

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Parámetros

|Subcomando|Descripción|
|-----------|-----------|
|/Get|Muestra la Directiva de auditoría actual.</br>Vea [Auditpol Get](auditpol-get.md) para ver la sintaxis y las opciones.|
|/Set|Establece la Directiva de auditoría.</br>Vea [Auditpol Set](auditpol-set.md) para ver la sintaxis y las opciones.|
|/List|Muestra los elementos de directiva seleccionables.</br>Vea la [lista Auditpol](auditpol-list.md) para ver la sintaxis y las opciones.|
|/backup|Guarda la Directiva de auditoría en un archivo.</br>Vea [copia de seguridad de Auditpol](auditpol-backup.md) para ver la sintaxis y las opciones.|
|/restore|Restaura la Directiva de auditoría de un archivo creado previamente mediante Auditpol/backup.</br>Vea [Auditpol restore](auditpol-restore.md) para ver la sintaxis y las opciones.|
|/Clear|Borra la Directiva de auditoría.</br>Vea [Auditpol Clear](auditpol-clear.md) para ver la sintaxis y las opciones.|
|/remove|Quita todas las configuraciones de directiva de auditoría por usuario y deshabilita todas las configuraciones de directiva de auditoría del sistema.</br>Vea [Auditpol Remove](auditpol-remove.md) para ver la sintaxis y las opciones.|
|/resourceSACL|Configura las listas de control de acceso (SACL) del sistema de recursos globales.</br>Nota: Solo se aplica a Windows 7 y Windows Server 2008 R2.</br>Consulte [Auditpol resourceSACL](auditpol-resourcesacl.md).|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

La herramienta de línea de comandos de directiva de auditoría se puede usar para:
-   Establecer y consultar una directiva de auditoría del sistema.
-   Establecer y consultar una directiva de auditoría por usuario.
-   Establezca y consulte las opciones de auditoría.
-   Establezca y consulte el descriptor de seguridad que se usa para delegar el acceso a una directiva de auditoría.
-   Informe o copia de seguridad de una directiva de auditoría en un archivo de texto de valores separados por comas (CSV).
-   Cargar una directiva de auditoría desde un archivo de texto CSV.
-   Configurar las SACL de recursos globales.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)