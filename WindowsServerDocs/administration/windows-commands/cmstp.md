---
title: cmstp
description: Artículo de referencia de cmstp, que instala o quita un perfil de servicio de administrador de conexiones.
ms.topic: reference
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99e5e3d81855069b8a4465d554e7d9699c4bc08e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030963"
---
# <a name="cmstp"></a>cmstp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Instala o quita un perfil de servicio de administrador de conexiones. Si se usa sin parámetros opcionales, **cmstp** instala un perfil de servicio con la configuración predeterminada adecuada para el sistema operativo y los permisos del usuario.

## <a name="syntax"></a>Sintaxis

Sintaxis 1: se trata de la sintaxis típica usada en una aplicación de instalación personalizada. Para usar esta sintaxis, debe ejecutar **cmstp** desde el directorio que contiene el `<serviceprofilefilename>.exe` archivo.

```
<serviceprofilefilename>.exe /q:a /c:cmstp.exe <serviceprofilefilename>.inf [/nf] [/s] [/u]
```

Sintaxis 2
```
cmstp.exe [/nf] [/s] [/u] [drive:][path]serviceprofilefilename.inf
```

#### <a name="parameters"></a>Parámetros
| Parámetro | Descripción |
| --------- | ----------- |
| `<serviceprofilefilename>.exe` | Especifica, por nombre, el paquete de instalación que contiene el perfil que desea instalar.<p>Se requiere para la sintaxis 1, pero no es válida para la sintaxis 2. |
| /q:a | Especifica que el perfil debe instalarse sin preguntar al usuario. El mensaje de comprobación que indica que la instalación se ha realizado correctamente seguirá apareciendo.<p>Se requiere para la sintaxis 1, pero no es válida para la sintaxis 2. |
| [unidad:] camino `<serviceprofilefilename>.inf` | Necesario. Especifica, por nombre, el archivo de configuración que determina cómo se debe instalar el perfil.<p>El parámetro [unidad:] [ruta de acceso] no es válido para la sintaxis 1. |
| /NF | Especifica que no se deben instalar los archivos de compatibilidad. |
| /s | Especifica que el perfil de servicio debe instalarse o desinstalarse en modo silencioso (sin solicitar la respuesta del usuario ni mostrar el mensaje de comprobación). Este es el único parámetro que puede usar en combinación con **/u**.|
| /U | Especifica que debe desinstalarse el perfil de servicio. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para instalar el perfil de servicio *ficción* sin ningún archivo de compatibilidad, escriba:

```
fiction.exe /c:cmstp.exe fiction.inf /nf
```

Para instalar de forma silenciosa el perfil de servicio *ficción* para un solo usuario, escriba:

```
fiction.exe /c:cmstp.exe fiction.inf /s /su
```

Para desinstalar de forma silenciosa el perfil del servicio *ficción* , escriba:

```
fiction.exe /c:cmstp.exe fiction.inf /s /u
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
