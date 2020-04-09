---
title: Wbadmin Delete Catalog
description: Comando de comandos de Windows para Wbadmin Delete Catalog, que elimina el catálogo de copia de seguridad que está almacenado en el equipo local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5cf069163cb18c1763de2842b518f269b9fa57dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829908"
---
# <a name="wbadmin-delete-catalog"></a>Wbadmin Delete Catalog



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

## <a name="remarks"></a>Comentarios

Si elimina el catálogo de copia de seguridad de un equipo, no podrá obtener acceso a las copias de seguridad creadas del equipo mediante el complemento Copias de seguridad de Windows Server. En este caso, si puede tener acceso a otra ubicación de copia de seguridad, use **Wbadmin restore Catalog** para restaurar el catálogo de copias de seguridad desde esa ubicación. Debe crear una nueva copia de seguridad una vez que se elimine el catálogo de copias de seguridad.

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)