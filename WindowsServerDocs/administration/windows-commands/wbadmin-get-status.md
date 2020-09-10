---
title: wbadmin get status
description: Artículo de referencia de Wbadmin get status, que informa del estado de la operación de copia de seguridad o recuperación que se está ejecutando actualmente.
ms.topic: reference
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7dcb1b59de6827500311cfd55245ac6c94a171b0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624591"
---
# <a name="wbadmin-get-status"></a>wbadmin get status



Informa del estado de la operación de copia de seguridad o de recuperación que se está ejecutando actualmente.

Para usar este subcomando, debe ser miembro del grupo operadores de **copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin get status
```

### <a name="parameters"></a>Parámetros

Este subcomando no tiene parámetros.

## <a name="remarks"></a>Observaciones

-   Este subcomando no se detendrá hasta que finalice la operación de copia de seguridad o recuperación actual; el subcomando continuará ejecutándose incluso si cierra la ventana de comandos.
-   Si desea detener la operación de copia de seguridad o de recuperación actual, use el subcomando **Wbadmin STOP Job** .

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBJob](/powershell/module/windowserverbackup/?view=winserver2012r2-ps)
