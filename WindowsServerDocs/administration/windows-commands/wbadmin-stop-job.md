---
title: wbadmin stop job
description: Artículo de referencia para el trabajo de detención de Wbadmin, que cancela la operación de copia de seguridad o recuperación que se está ejecutando actualmente. Las operaciones canceladas no se pueden reiniciar; debe volver a ejecutar una copia de seguridad o una operación de recuperación cancelada desde el principio.
ms.topic: article
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e28a59620888a14e170f293ef2b08ffa9053e515
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879553"
---
# <a name="wbadmin-stop-job"></a>wbadmin stop job



Cancela la operación de copia de seguridad o de recuperación que se está ejecutando actualmente. Las operaciones canceladas no se pueden reiniciar; debe volver a ejecutar una copia de seguridad o una operación de recuperación cancelada desde el principio.

Para detener una copia de seguridad o una operación de recuperación con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien debe tener delegada la autoridad correspondiente. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema** y haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin stop job
[-quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)