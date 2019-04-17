---
title: Vssadmin sombras de lista
description: Una descripción de la lista vssadmin sombras comando.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082731"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin sombras de lista

>Se aplica a: 10 de Windows, Windows 8.1, 2016 de Windows Server, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Se enumeran todas las instantáneas existentes de un volumen especificado. Si usa este comando sin parámetros, muestra todas las instantáneas de volumen en el equipo en el orden determinado por la **Sombra de conjunto de copia**.

## <a name="syntax"></a>Sintaxis

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---|---|
|/ for = \ < ParaVolumenEspec >|Especifica qué volumen se enumerarán las instantáneas de volumen para.|
|/ shadow = \ < IdDeInstantánea >|Enumera la instantánea especificada por IdDeInstantánea. Para obtener el identificador de instantánea, utilice el comando **vssadmin sombras de lista** . Cuando escriba un identificador de instantánea, use el siguiente formato, donde cada *X* representa un carácter hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Referencias adicionales

* [Clave de la sintaxis de la línea de comandos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)