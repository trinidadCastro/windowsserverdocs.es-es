---
title: catálogo de Wbadmin delete
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d9f60cf79fcd856fa972d8f43f195c5823e5d81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841416"
---
# <a name="wbadmin-delete-catalog"></a>catálogo de Wbadmin delete



Elimina el catálogo de copia de seguridad que se almacena en el equipo local. Use este comando cuando el catálogo de copia de seguridad está dañado y no puede restaurarla mediante **wbadmin restauración catálogo**.

Para eliminar un catálogo de copia de seguridad con este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.)

## <a name="syntax"></a>Sintaxis

```
wbadmin delete catalog
[-quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|

## <a name="remarks"></a>Comentarios

Si elimina el catálogo de copia de seguridad para un equipo, no podrá tener acceso a las copias de seguridad creadas de ese equipo mediante el complemento de copia de seguridad de Windows Server. En este caso, si puede tener acceso a otra ubicación de copia de seguridad, use **wbadmin restauración catálogo** para restaurar el catálogo de copia de seguridad desde esa ubicación. Debe crear una nueva copia de seguridad una vez que se elimina el catálogo de copia de seguridad.

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)