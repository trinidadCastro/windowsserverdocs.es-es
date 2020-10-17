---
title: wbadmin delete catalog
description: Artículo de referencia del comando Wbadmin Delete Catalog, que elimina el catálogo de copias de seguridad que está almacenado en el equipo local.
ms.topic: reference
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4a19d856b65fdcf17fb369d81607445530f77275
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156102"
---
# <a name="wbadmin-delete-catalog"></a>wbadmin delete catalog

Elimina el catálogo de copia de seguridad que está almacenado en el equipo local. Use este comando si el catálogo de copia de seguridad está dañado y no puede restaurarlo mediante el comando [Wbadmin restore Catalog](wbadmin-restore-catalog.md) .

Para eliminar un catálogo de copia de seguridad mediante este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

## <a name="syntax"></a>Sintaxis

```
wbadmin delete catalog [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -quiet | Ejecuta el comando sin preguntar al usuario. |

#### <a name="remarks"></a>Comentarios

- Si elimina el catálogo de copia de seguridad de un equipo, ya no podrá obtener acceso a las copias de seguridad creadas para ese equipo mediante el complemento Copias de seguridad de Windows Server. Sin embargo, si puede acceder a otra ubicación de copia de seguridad y ejecutar el comando [Wbadmin restore Catalog](wbadmin-restore-catalog.md) , puede restaurar el catálogo de copia de seguridad desde esa ubicación.

- Le recomendamos encarecidamente que cree una nueva copia de seguridad después de eliminar un catálogo de copia de seguridad.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin restore Catalog](wbadmin-restore-catalog.md)

- [Remove-WBCatalog](/powershell/module/windowsserverbackup/Remove-WBCatalog)
