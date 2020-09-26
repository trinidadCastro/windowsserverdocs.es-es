---
title: sfc
description: Artículo de referencia para el comando SFC, que examina y comprueba la integridad de todos los archivos del sistema protegidos y reemplaza las versiones incorrectas con las correctas.
ms.topic: reference
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 829c6e328ad0ea993e11cb5eb5d96d99f0d52476
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388945"
---
# <a name="sfc"></a>sfc

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Examina y comprueba la integridad de todos los archivos del sistema protegidos y reemplaza las versiones incorrectas con las correctas. Si este comando detecta que se ha sobrescrito un archivo protegido, recupera la versión correcta del archivo de la carpeta **systemroot\system32\dllcache** y, a continuación, reemplaza el archivo incorrecto.

> [!IMPORTANT]
> Debe haber iniciado sesión como miembro del grupo administradores para ejecutar este comando.

## <a name="syntax"></a>Sintaxis

```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /scannow | Examina la integridad de todos los archivos del sistema protegidos y repara los archivos con problemas cuando sea posible. |
| /verifyonly | Examina la integridad de todos los archivos del sistema protegidos, sin realizar reparaciones. |
| /scanfile `<file>` | Examina la integridad del archivo especificado (ruta de acceso completa y nombre de archivo) e intenta reparar los problemas si se detectan. |
| /verifyfile `<file>` | Comprueba la integridad del archivo especificado (ruta de acceso completa y nombre de archivo), sin realizar reparaciones. |
| /offwindir `<offline windows directory>` | Especifica la ubicación del directorio de Windows sin conexión, para la reparación sin conexión. |
| /offbootdir `<offline boot directory>` | Especifica la ubicación del directorio de arranque sin conexión para la reparación sin conexión. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para comprobar el * archivo dekernel32.dll*, escriba:

```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```

Para configurar la reparación sin conexión del archivo *kernel32.dll* con un directorio de arranque sin conexión establecido en * D: \* y un directorio de Windows sin conexión establecido en *D:\Windows*, escriba:

```
sfc /scanfile=D:\windows\system32\kernel32.dll /offbootdir=D:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
