---
title: Estado de Wbadmin get
description: Windows Commands topic for Wbadmin get status, que informa del estado de la operación de copia de seguridad o recuperación que se está ejecutando actualmente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8ebf1a078632f78dc8d58c232550345f0de78f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829748"
---
# <a name="wbadmin-get-status"></a>Estado de Wbadmin get



Informa del estado de la operación de copia de seguridad o de recuperación que se está ejecutando actualmente.

Para usar este subcomando, debe ser miembro del grupo operadores de **copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin get status
```

### <a name="parameters"></a>Parámetros

Este subcomando no tiene parámetros.

## <a name="remarks"></a>Comentarios

-   Este subcomando no se detendrá hasta que finalice la operación de copia de seguridad o recuperación actual; el subcomando continuará ejecutándose incluso si cierra la ventana de comandos.
-   Si desea detener la operación de copia de seguridad o de recuperación actual, use el subcomando **Wbadmin STOP Job** .

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx)