---
title: Vssadmin delete sombras
description: Una descripción del comando vssadmin delete sombras.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 71119174c7fc5929eb4e1982183ba0aed7eb1735
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082640"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin delete sombras

>Se aplica a: 10 de Windows, Windows 8.1, 2016 de Windows Server, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Elimina las instantáneas de un volumen especificado.

## <a name="syntax"></a>Sintaxis

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---|---|
|/ for = \ < ParaVolumenEspec >|Especifica qué de instantáneas se eliminarán.|
|/ más antiguo|Elimina sólo la instantánea más antigua.|
|/ all|Todos los de instantáneas de volumen especificado elimina.|
|/ shadow = \ < IdDeInstantánea >|Elimina la instantánea especificada por IdDeInstantánea. Para obtener el identificador de instantánea, utilice el comando **vssadmin sombras de lista** . Cuando se especifica un identificador de instantánea, use el siguiente formato, donde cada *X* representa un carácter hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/ silencioso|Especifica que el comando no mostrará los mensajes mientras se está ejecutando.|

## <a name="remarks"></a>Comentarios

Sólo se pueden eliminar instantáneas de volumen con el tipo accesible para el cliente.

## <a name="examples"></a>Ejemplos

Para eliminar la instantánea más antigua del volumen C, escriba este comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referencias adicionales

* [Clave de la sintaxis de la línea de comandos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin sombras de lista](vssadmin-list-shadows.md)