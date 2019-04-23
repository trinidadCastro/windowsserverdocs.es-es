---
title: obtener el estado de Wbadmin
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35fd640aa56bca7c5f5d6f3901fe095d0b8a73cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863416"
---
# <a name="wbadmin-get-status"></a>obtener el estado de Wbadmin



Informa del estado de la operación de copia de seguridad o recuperación que se está ejecutando actualmente.

Para usar este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.)

## <a name="syntax"></a>Sintaxis

```
wbadmin get status
```

## <a name="parameters"></a>Parámetros

Este subcomando no tiene parámetros.

## <a name="remarks"></a>Comentarios

-   Este subcomando no se detendrá hasta que la copia de seguridad actual o finalice la operación de recuperación: el subcomando seguirá ejecutándose aunque cierre la ventana de comandos.
-   Si desea detener la operación de recuperación o copia de seguridad actual, use el **wbadmin Detener trabajo** subcomando.

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx) cmdlet