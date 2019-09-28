---
title: Wbadmin Delete Catalog
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 58b8bc6043437755675af28c084257ba0d8b176d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362532"
---
# <a name="wbadmin-delete-catalog"></a>Wbadmin Delete Catalog



Elimina el catálogo de copia de seguridad que está almacenado en el equipo local. Use este comando si el catálogo de copia de seguridad está dañado y no se puede restaurar mediante el uso de **Wbadmin restore Catalog**.

Para eliminar un catálogo de copias de seguridad con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin delete catalog
[-quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="remarks"></a>Comentarios

Si elimina el catálogo de copia de seguridad de un equipo, no podrá obtener acceso a las copias de seguridad creadas del equipo mediante el complemento Copias de seguridad de Windows Server. En este caso, si puede tener acceso a otra ubicación de copia de seguridad, use **Wbadmin restore Catalog** para restaurar el catálogo de copias de seguridad desde esa ubicación. Debe crear una nueva copia de seguridad una vez que se elimine el catálogo de copias de seguridad.

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)