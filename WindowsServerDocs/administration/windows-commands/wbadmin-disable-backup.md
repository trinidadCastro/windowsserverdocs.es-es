---
title: copia de seguridad de Wbadmin disable
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fcaf9e8b6ef052b01b5a3184dd8f94bba433cd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821796"
---
# <a name="wbadmin-disable-backup"></a>copia de seguridad de Wbadmin disable



Deje de ejecutarse el copias de seguridad diaria programada existente.

Para deshabilitar una copia de seguridad diaria programada, debe ser miembro de la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo** y, a continuación, haga clic en **ejecutar como administrador**.)

## <a name="syntax"></a>Sintaxis

```
wbadmin disable backup
[-quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)