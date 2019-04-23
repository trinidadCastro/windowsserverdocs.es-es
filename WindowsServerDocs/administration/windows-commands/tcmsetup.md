---
title: tcmsetup
description: Aprenda a configurar y deshabilitar al cliente TAPI.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac92c7b793274227bd20e6fa90a4106a32ea0446
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862006"
---
# <a name="tcmsetup"></a>tcmsetup



Establece o deshabilita al cliente TAPI.

## <a name="syntax"></a>Sintaxis

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/q|Impide la presentación de cuadros de mensaje.|
|/x|Especifica que las devoluciones de llamada orientados a conexiones se utilizará para las redes de tráfico intenso donde la pérdida de paquetes es alta. Si se omite este parámetro, se usará las devoluciones de llamada sin conexión.|
|/c|Obligatorio. Especifica el programa de instalación de cliente.|
|\<Server1>|Obligatorio. Especifica el nombre del servidor remoto que tiene los proveedores de servicios TAPI que utilizará el cliente. El cliente usará las líneas y teléfonos de los proveedores de servicios. El cliente debe ser del mismo dominio que el servidor o en un dominio que tenga una relación de confianza bidireccional con el dominio que contiene el servidor.|
|\<Server2 >...|Especifica los servidores que estarán disponibles para este cliente adicionales. Si especifica que una lista de servidores, use un espacio para separar los nombres de servidor.|
|/d|Borra la lista de servidores remotos. Deshabilita al cliente TAPI impide usar los proveedores de servicios TAPI que se encuentran en los servidores remotos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio podrían llevar a cabo este procedimiento. Como práctica de seguridad recomendada, piense en usar **Ejecutar como** para llevar a cabo este procedimiento.
-   TAPI funcione correctamente, debe ejecutar **tcmsetup** para especificar los servidores remotos que se usará en los clientes TAPI.
-   Antes de que un usuario de cliente puede usar un teléfono o una línea en un servidor TAPI, el administrador del servidor de telefonía debe asignar al usuario para el teléfono o la línea.
-   La lista de servidores de telefonía que se crea mediante este comando reemplazará cualquier lista existente de servidores de telefonía disponibles para el cliente. No se puede usar este comando para agregar a la lista existente.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Introducción al shell de comandos](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Especificar servidores de telefonía en un equipo cliente](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Asignar a un usuario de telefonía a una línea o un teléfono](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

