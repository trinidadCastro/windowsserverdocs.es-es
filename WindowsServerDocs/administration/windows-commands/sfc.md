---
title: sfc
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1db0ab81c9469c88ddb64a367a9dc98a1fd9b70c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832396"
---
# <a name="sfc"></a>sfc

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Examina y comprueba la integridad del sistema protegido todos los archivos y reemplaza las versiones incorrectas con las versiones correctas.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/scannow|Examina la integridad de todos los archivos protegidos del sistema y repara los archivos con problemas cuando sea posible.|
|/verifyonly|Examina la integridad de todos los archivos protegidos del sistema. Se realiza ninguna operación de reparación.|
|/scanfile|Examina la integridad del archivo especificado y repara el archivo si se detectan problemas, siempre que sea posible.|
|\<file>|Nombre de archivo y ruta de acceso completa|
|/verifyfile|comprueba la integridad del archivo especificado. Se realiza ninguna operación de reparación.|
|/offwindir|Especifica la ubicación del directorio de windows sin conexión, para la reparación sin conexión.|
|/offbootdir|Especifica la ubicación del directorio de arranque sin conexión sin conexión|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Debe haber iniciado sesión como miembro del grupo Administradores para ejecutar **sfc.exe**.
-   Si **sfc** detecta que se ha sobrescrito un archivo protegido, recupera la versión correcta del archivo desde el **systemroot\system32\dllcache** carpeta y, a continuación, reemplaza el archivo incorrecto.
-   Hay diferencias funcionales entre **sfc** en Windows Server 2003, Windows Server 2008 y Windows Server 2008 R2:
-   Para obtener más información acerca de **sfc** en Windows Server 2003, consulte [artículo 310747](https://go.microsoft.com/fwlink/?LinkId=227069) en Microsoft Knowledge Base.
-   Para obtener más información acerca de **sfc** en Windows Server 2008 y Windows Server 2008 R2, consulte [Comprobador de archivos de sistema](https://go.microsoft.com/fwlink/?LinkId=227071).

## <a name="BKMK_examples"></a>Ejemplos
Para comprobar la **archivo kernel32.dll**, tipo:
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Para reparar el programa de instalación sin conexión la **kernel32.dll** archivo con un directorio de arranque sin conexión establecido en **d:** y el directorio de windows sin conexión establecido en **d:\windows**, tipo:
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

