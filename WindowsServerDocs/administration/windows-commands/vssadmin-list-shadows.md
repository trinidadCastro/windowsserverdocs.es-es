---
title: Vssadmin List Shadows
description: Una descripción del comando vssadmin List Shadows.
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: d3616902fa0b971969e5e906d6dbb200c633d15a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891793"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin List Shadows

> Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Muestra todas las instantáneas existentes de un volumen especificado. Si usa este comando sin parámetros, se muestran todas las instantáneas de volumen en el equipo en el orden indicado por el **conjunto de instantáneas**.

## <a name="syntax"></a>Sintaxis

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---|---|
|/for =\<ForVolumeSpec>|Especifica en qué volumen se mostrarán las instantáneas.|
|/Shadow =\<ShadowID>|Muestra la instantánea especificada por ShadowID. Para obtener el identificador de la instantánea, use el comando **vssadmin List Shadows** . Cuando escriba un identificador de instantánea, use el formato siguiente, donde cada *X* representa un carácter hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Referencias adicionales

* [Clave de sintaxis de línea de comandos](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [List](vssadmin.md)
