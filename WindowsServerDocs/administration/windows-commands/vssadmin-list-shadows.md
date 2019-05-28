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
ms.openlocfilehash: 3601986a51e8c5b362a28c686ed132eda8e4b640
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63706561"
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