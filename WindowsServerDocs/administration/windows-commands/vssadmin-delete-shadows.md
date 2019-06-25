---
title: Vssadmin delete sombras
description: Una descripción del comando vssadmin eliminar sombras.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 83074017e4ae412cf0aec654f6ab5901ad8039e2
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63706628"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin delete sombras

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Elimina las instantáneas de un volumen especificado.

## <a name="syntax"></a>Sintaxis

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---|---|
|/for=\<ForVolumeSpec>|Especifica qué de instantáneas se eliminarán.|
|/oldest|Elimina solo las instantáneas más antiguas.|
|/ all|Todas las instantáneas del volumen especificado elimina.|
|/shadow=\<ShadowID>|Elimina la instantánea especificada por IdDeInstantánea. Para obtener el identificador de instantánea, use el **sombras de lista vssadmin** comando. Al especificar un Id. de instantánea, use el formato siguiente, donde cada *X* representa un carácter hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|Especifica que el comando no mostrará los mensajes mientras se está ejecutando.|

## <a name="remarks"></a>Comentarios

Solo puede eliminar las instantáneas con el tipo accesible para el cliente.

## <a name="examples"></a>Ejemplos

Para eliminar la instantánea más antigua del volumen C, escriba este comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referencias adicionales

* [Clave de sintaxis de línea de comandos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Sombras de lista vssadmin](vssadmin-list-shadows.md)