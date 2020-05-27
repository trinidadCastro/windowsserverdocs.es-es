---
title: Wbadmin deshabilitar copia de seguridad
description: Tema de referencia de Wbadmin Disable backup, que deja de ejecutar las copias de seguridad diarias programadas existentes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bc211946c908b44b196995a9e5000d6ff3b7f82
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821425"
---
# <a name="wbadmin-disable-backup"></a>Wbadmin deshabilitar copia de seguridad



Detiene la ejecución de las copias de seguridad diarias programadas existentes.

Para deshabilitar una copia de seguridad diaria programada, debe ser miembro del grupo **administradores** o tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema** y haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin disable backup
[-quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)