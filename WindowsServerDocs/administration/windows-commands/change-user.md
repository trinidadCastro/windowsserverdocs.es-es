---
title: change user
description: Windows Commands topic for Change User, que cambia el modo de instalación del servidor host de sesión Escritorio remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b13fe66991d6e8bbb91938b550236f3aa9f51ee9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848068"
---
# <a name="change-user"></a>change user

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el modo de instalación del servidor host de sesión Escritorio remoto.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
change user {/execute | /install | /query}
```
### <a name="parameters"></a>Parámetros

| Parámetro |                                                                                                 Descripción                                                                                                  |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Execute  |                                                                Habilita la asignación de archivos. ini en el directorio particular. Esta es la configuración predeterminada.                                                                 |
| /install  | Deshabilita la asignación de archivos. ini en el directorio particular. Todos los archivos. ini se leen y se escriben en el directorio del sistema. Debe deshabilitar la asignación de archivos. ini al instalar aplicaciones en un servidor host de sesión de escritorio remoto. |
|  /Query   |                                                                             Muestra la configuración actual para la asignación de archivos. ini.                                                                              |
|    /?     |                                                                                     Muestra la Ayuda en el símbolo del sistema.                                                                                     |

## <a name="remarks"></a>Comentarios
- Use **Change User/Install** antes de instalar una aplicación para crear archivos. ini para la aplicación en el directorio del sistema. Estos archivos se usan como origen cuando se crean archivos. ini específicos del usuario. Después de instalar la aplicación, use **Change User/Execute** para revertir a la asignación de archivos. ini estándar.
- La primera vez que ejecute la aplicación, buscará los archivos. ini en el directorio principal. Si no se encuentran los archivos. ini en el directorio particular, pero se encuentran en el directorio del sistema, Servicios de Escritorio remoto copia los archivos. ini en el directorio particular, asegurándose de que cada usuario tenga una copia única de los archivos. ini de la aplicación. Los nuevos archivos. ini se crean en el directorio particular.
- Cada usuario debe tener una copia única de los archivos. ini para una aplicación. Esto evita las instancias en las que distintos usuarios pueden tener configuraciones de aplicación incompatibles (por ejemplo, diferentes directorios predeterminados o resoluciones de pantalla).
- Cuando el sistema está en modo de instalación (es decir, **Change User/Install**), se producen varias acciones. Todas las entradas del registro que se crean se prevalecen en **HKEY_LOCAL_MACHINE \Software\microsoft\windows NT\Currentversion\Terminal Server\Install**, en la subclave **\Software** o en la subclave **\MACHINE** Las subclaves agregadas a **HKEY_CURRENT_USER** se copian bajo la subclave **\Software** y las subclaves agregadas a **HKEY_LOCAL_MACHINE** se copian en la subclave **\MACHINE** . Si la aplicación consulta el directorio de Windows mediante llamadas del sistema, como GetWindowsdirectory, el servidor host de sesión de escritorio remoto devuelve el directorio SystemRoot. Si se agregan entradas de archivo. ini mediante llamadas del sistema, como WritePrivateProfileString, se agregan a los archivos. ini en el directorio SystemRoot.
- Cuando el sistema vuelve al modo de ejecución (es decir, **cambia el usuario/Execute**) y la aplicación intenta leer una entrada del registro en **HKEY_CURRENT_USER** que no existe, servicios de escritorio remoto comprueba si existe una copia de la clave en la subclave **\Terminal Server\Install** . Si es así, las subclaves se copian en la ubicación adecuada en **HKEY_CURRRENT_USER**. Si la aplicación intenta leer desde un archivo. ini que no existe, Servicios de Escritorio remoto busca ese archivo. ini en la raíz del sistema. Si el archivo. ini se encuentra en la raíz del sistema, se copia en el subdirectorio \Windows del directorio principal del usuario. Si la aplicación consulta el directorio de Windows, el servidor host de sesión de escritorio remoto devuelve el subdirectorio \Windows del directorio principal del usuario.
- Al iniciar sesión, Servicios de Escritorio remoto comprueba si sus archivos System. ini son más recientes que los archivos. ini del equipo. Si la versión del sistema es más reciente, el archivo. ini se reemplaza o se combina con la versión más reciente. Esto depende de si el bit INISYNC, 0x40, se establece para este archivo. ini. Se cambia el nombre de la versión anterior del archivo. ini como inifile. CTX. Si los valores del registro del sistema en la subclave **\Terminal Server\Install** son más recientes que la versión en **HKEY_CURRENT_USER**, la versión de las subclaves se elimina y se reemplaza con las nuevas subclaves de **\Terminal Server\Install**.
  ## <a name="examples"></a><a name=BKMK_examples></a>Example
- Para deshabilitar la asignación de archivos. ini en el directorio particular, escriba:
  ```
  change user /install
  ```
- Para habilitar la asignación de archivos. ini en el directorio particular, escriba:
  ```
  change user /execute
  ```
- Para mostrar la configuración actual de la asignación de archivos. ini, escriba:
  ```
  change user /query
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [cambiar](change.md)
  [referencia de comandos servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
