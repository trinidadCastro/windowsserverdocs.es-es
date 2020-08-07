---
title: flattemp
description: Artículo de referencia del comando flattemp, que habilita o deshabilita las carpetas temporales sin formato.
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b90bfdf06fad151ab92ea82782a4171877e6d0ad
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890199"
---
# <a name="flattemp"></a>flattemp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Habilita o deshabilita las carpetas temporales sin formato. Debe tener credenciales administrativas para ejecutar este comando.

> [!NOTE]
> Este comando solo está disponible si ha instalado el servicio de rol host de sesión de Escritorio remoto.

## <a name="syntax"></a>Sintaxis

```
flattemp {/query | /enable | /disable}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /Query | Consulta la configuración actual. |
| /Enable | Habilita las carpetas temporales sin formato. Los usuarios compartirán la carpeta temporal a menos que la carpeta temporal resida en la carpeta principal del usuario. |
| /Disable | Deshabilita las carpetas temporales sin formato. Cada carpeta temporal de usuario residirá en una carpeta independiente (determinada por el identificador de sesión del usuario). |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Después de que cada usuario tenga una carpeta temporal única, use `flattemp /enable` para habilitar las carpetas temporales sin formato.

- El método predeterminado para crear carpetas temporales para varios usuarios (que normalmente apuntan por las variables de entorno TEMP y TMP) es crear subcarpetas en la carpeta **\temp** mediante logonID como el nombre de la subcarpeta. Por ejemplo, si la variable de entorno TEMP apunta a C:\Temp, la carpeta temporal asignada al usuario logonID 4 es C:\Temp\4.

    Con **flattemp**, puede apuntar directamente a la carpeta \temp e impedir que se formen las subcarpetas. Esto resulta útil cuando desea que las carpetas temporales del usuario estén contenidas en carpetas particulares, ya sea en una unidad local del servidor host de sesión de Escritorio remoto o en una unidad de red compartida. Solo debe usar el `flattemp /enable*` comando cuando cada usuario tenga una carpeta temporal independiente.

- Podría encontrar errores de la aplicación si la carpeta temporal del usuario se encuentra en una unidad de red. Esto sucede cuando la unidad de red compartida deja de estar accesible momentáneamente en la red. Dado que los archivos temporales de la aplicación son inaccesibles o no están sincronizados, responde como si el disco se ha detenido. No se recomienda mover la carpeta temporal a una unidad de red. El valor predeterminado es conservar las carpetas temporales en el disco duro local. Si experimenta un comportamiento inesperado o errores de daños en disco con ciertas aplicaciones, estabilice la red o vuelva a colocar las carpetas temporales en el disco duro local.

- Si deshabilita el uso de carpetas temporales independientes por sesión, se omite la configuración de **flattemp** . Esta opción se establece en la herramienta de configuración de Servicios de Escritorio remoto.

### <a name="examples"></a>Ejemplos

Para mostrar la configuración actual de las carpetas temporales sin formato, escriba:

```
flattemp /query
```

Para habilitar las carpetas temporales sin formato, escriba:

```
flattemp /enable
```

Para deshabilitar las carpetas temporales sin formato, escriba:

```
flattemp /disable
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

