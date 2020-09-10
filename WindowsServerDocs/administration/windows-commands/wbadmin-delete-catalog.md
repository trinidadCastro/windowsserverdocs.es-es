---
title: wbadmin delete catalog
description: Artículo de referencia de Wbadmin Delete Catalog, que elimina el catálogo de copias de seguridad que se almacena en el equipo local.
ms.topic: reference
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 64b5664ffeb676bf63ece83bd419f154cb96bb20
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637883"
---
# <a name="wbadmin-delete-catalog"></a>wbadmin delete catalog



Elimina el catálogo de copia de seguridad que está almacenado en el equipo local. Use este comando si el catálogo de copia de seguridad está dañado y no se puede restaurar mediante el uso de **Wbadmin restore Catalog**.

Para eliminar un catálogo de copias de seguridad con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin delete catalog
[-quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="remarks"></a>Observaciones

Si elimina el catálogo de copia de seguridad de un equipo, no podrá obtener acceso a las copias de seguridad creadas del equipo mediante el complemento Copias de seguridad de Windows Server. En este caso, si puede tener acceso a otra ubicación de copia de seguridad, use **Wbadmin restore Catalog** para restaurar el catálogo de copias de seguridad desde esa ubicación. Debe crear una nueva copia de seguridad una vez que se elimine el catálogo de copias de seguridad.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](/powershell/module/windowserverbackup/?view=winserver2012r2-ps)
