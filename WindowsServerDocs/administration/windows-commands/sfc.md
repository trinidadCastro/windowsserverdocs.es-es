---
title: sfc
description: El tema comandos de Windows para SFC, que examina y comprueba la integridad de todos los archivos del sistema protegidos y reemplaza las versiones incorrectas con las correctas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7663c8e3527995e2d3ec874dff6fa972e7e83ddd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834328"
---
# <a name="sfc"></a>sfc

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Examina y comprueba la integridad de todos los archivos del sistema protegidos y reemplaza las versiones incorrectas con las correctas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/scannow|Examina la integridad de todos los archivos del sistema protegidos y repara los archivos con problemas cuando sea posible.|
|/verifyonly|Examina la integridad de todos los archivos del sistema protegidos. No se realiza ninguna operación de reparación.|
|/scanfile|Examina la integridad del archivo especificado y repara el archivo si se detectan problemas, siempre que sea posible.|
|\<> de archivo|Ruta de acceso completa y nombre de archivo especificados|
|/verifyfile|comprueba la integridad del archivo especificado. No se realiza ninguna operación de reparación.|
|/offwindir|Especifica la ubicación del directorio de Windows sin conexión, para la reparación sin conexión.|
|/offbootdir|Especifica la ubicación del directorio de arranque sin conexión para sin conexión|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Debe haber iniciado sesión como miembro del grupo administradores para ejecutar **SFC. exe**.
-   Si **SFC** detecta que se ha sobrescrito un archivo protegido, recupera la versión correcta del archivo de la carpeta **systemroot\system32\dllcache** y, a continuación, reemplaza el archivo incorrecto.
-   Existen diferencias funcionales entre **SFC** en windows Server 2003, windows Server 2008 y windows Server 2008 R2:
-   para obtener más información acerca de **SFC** en Windows Server 2003, consulte el [artículo 310747](https://go.microsoft.com/fwlink/?LinkId=227069) de Microsoft Knowledge base.
-   para obtener más información acerca de **SFC** en windows Server 2008 y windows Server 2008 R2, consulte [Comprobador de archivos del sistema](https://go.microsoft.com/fwlink/?LinkId=227071).

## <a name="examples"></a><a name=BKMK_examples></a>Example
Para comprobar el **archivo Kernel32. dll**, escriba:
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Para configurar la reparación sin conexión del archivo **Kernel32. dll** con un directorio de arranque sin conexión establecido en **d:** y el directorio de Windows sin conexión establecido en **d:\Windows**, escriba:
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

