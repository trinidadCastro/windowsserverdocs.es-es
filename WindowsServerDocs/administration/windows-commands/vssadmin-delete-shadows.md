---
title: Vssadmin eliminar sombras
description: Descripción del comando vssadmin Delete Shadows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9807efa2c570b8ed63c2d776327b8e3311846488
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830118"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin eliminar sombras

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Elimina las instantáneas del volumen especificado.

## <a name="syntax"></a>Sintaxis

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---|---|
|/for =\<ParaVolumenEspec >|Especifica la instantánea del volumen que se eliminará.|
|/oldest|Elimina solo la instantánea más antigua.|
|/all|Elimina todas las instantáneas del volumen especificado.|
|/Shadow =\<ShadowID >|Elimina la instantánea especificada por ShadowID. Para obtener el identificador de la instantánea, use el comando **vssadmin List Shadows** . Al escribir un identificador de instantánea, use el formato siguiente, donde cada *X* representa un carácter hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|Especifica que el comando no mostrará mensajes mientras se ejecuta.|

## <a name="remarks"></a>Comentarios

Solo se pueden eliminar instantáneas con el tipo accesible desde el cliente.

## <a name="examples"></a>Ejemplos

Para eliminar la instantánea más antigua del volumen C, escriba este comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referencias adicionales

* [Clave de sintaxis de línea de comandos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [List](vssadmin.md)
* [Vssadmin List Shadows](vssadmin-list-shadows.md)