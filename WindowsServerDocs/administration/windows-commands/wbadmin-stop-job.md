---
title: detención del trabajo de Wbadmin
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a9e71fe2e4883c52c2418e21fc8764fd14e6c81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889726"
---
# <a name="wbadmin-stop-job"></a>detención del trabajo de Wbadmin



Cancela la operación de copia de seguridad o recuperación que se está ejecutando actualmente. No se puede reiniciar operaciones canceladas, debe volver a ejecutar una operación de copia de seguridad o recuperación cancelada desde el principio.

Para detener una operación de copia de seguridad o recuperación con este subcomando, debe ser miembro del **operadores de copia de seguridad** grupo o el **administradores** grupo, o debe haberle sido delegada la autoridad adecuada. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo** y, a continuación, haga clic en **ejecutar como administrador**.)

## <a name="syntax"></a>Sintaxis

```
wbadmin stop job
[-quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)