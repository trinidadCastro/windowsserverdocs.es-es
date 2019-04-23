---
title: Sombras de lista vssadmin
description: Una descripción de la lista vssadmin sombrea el comando.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861156"
---
# <a name="vssadmin-list-shadows"></a>Sombras de lista vssadmin

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Enumera todas las instantáneas de un volumen especificado. Si usa este comando sin parámetros, muestra todas las instantáneas de volumen en el equipo en el orden dictado por **de conjunto de copia sombra**.

## <a name="syntax"></a>Sintaxis

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---|---|
|/for=\<ForVolumeSpec>|Especifica qué volumen de las instantáneas se mostrarán como.|
|/shadow=\<ShadowID>|Muestra la instantánea especificada por IdDeInstantánea. Para obtener el identificador de instantánea, use el **sombras de lista vssadmin** comando. Cuando escriba un Id. de instantánea, use el formato siguiente, donde cada *X* representa un carácter hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Referencias adicionales

* [Clave de sintaxis de línea de comandos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)