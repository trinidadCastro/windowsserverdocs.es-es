---
title: Vssadmin List Shadows
description: Una descripción del comando vssadmin List Shadows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 38eda178b6c9e34fec1d63ed6c59f01023b859c4
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436685"
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
|/for = \< paravolumenespec>|Especifica en qué volumen se mostrarán las instantáneas.|
|/Shadow = \< ShadowID>|Muestra la instantánea especificada por ShadowID. Para obtener el identificador de la instantánea, use el comando **vssadmin List Shadows** . Cuando escriba un identificador de instantánea, use el formato siguiente, donde cada *X* representa un carácter hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Referencias adicionales

* [Clave de sintaxis de línea de comandos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [List](vssadmin.md)