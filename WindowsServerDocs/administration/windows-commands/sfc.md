---
title: sfc
description: Artículo de referencia de SFC, que examina y comprueba la integridad de todos los archivos del sistema protegidos y reemplaza las versiones incorrectas con las correctas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f4b0798f9c0e3e1c70ca701de1ea2246bddf7b9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931610"
---
# <a name="sfc"></a>sfc

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Examina y comprueba la integridad de todos los archivos del sistema protegidos y reemplaza las versiones incorrectas con las correctas.


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
|\<file>|Ruta de acceso completa y nombre de archivo especificados|
|/verifyfile|comprueba la integridad del archivo especificado. No se realiza ninguna operación de reparación.|
|/offwindir|Especifica la ubicación del directorio de Windows sin conexión, para la reparación sin conexión.|
|/offbootdir|Especifica la ubicación del directorio de arranque sin conexión para sin conexión|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Debe haber iniciado sesión como miembro del grupo administradores para ejecutar **sfc.exe**.
-   Si **SFC** detecta que se ha sobrescrito un archivo protegido, recupera la versión correcta del archivo de la carpeta **systemroot\system32\dllcache** y, a continuación, reemplaza el archivo incorrecto.
-   Existen diferencias funcionales entre **SFC** en windows Server 2003, windows Server 2008 y windows Server 2008 R2:
-   para obtener más información acerca de **SFC** en Windows Server 2003, consulte el [artículo 310747](https://go.microsoft.com/fwlink/?LinkId=227069) de Microsoft Knowledge base.
-   para obtener más información acerca de **SFC** en windows Server 2008 y windows Server 2008 R2, consulte [Comprobador de archivos del sistema](https://go.microsoft.com/fwlink/?LinkId=227071).

## <a name="examples"></a>Ejemplos
Para comprobar el **archivo dekernel32.dll**, escriba:
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Para configurar la reparación sin conexión del archivo de **kernel32.dll** con un directorio de arranque sin conexión establecido en **d:** y el directorio de Windows sin conexión establecido en **d:\Windows**, escriba:
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

