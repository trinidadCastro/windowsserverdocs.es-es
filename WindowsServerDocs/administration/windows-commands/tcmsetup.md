---
title: tcmsetup
description: Obtenga información acerca de cómo configurar y deshabilitar el cliente TAPI.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e453ef94aedb8920c0310123ff6033fafbaddab
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958627"
---
# <a name="tcmsetup"></a>tcmsetup



Configura o deshabilita el cliente TAPI.

## <a name="syntax"></a>Sintaxis

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/q|Impide que se muestren los cuadros de mensaje.|
|/x|Especifica que se usarán devoluciones de llamada orientadas a la conexión en las redes de tráfico intenso donde la pérdida de paquetes es elevada. Si se omite este parámetro, se usarán devoluciones de llamada sin conexión.|
|/C|Necesario. Especifica la configuración del cliente.|
|\<Server1>|Necesario. Especifica el nombre del servidor remoto que tiene los proveedores de servicios TAPI que utilizará el cliente. El cliente usará las líneas y los teléfonos de los proveedores de servicios. El cliente debe encontrarse en el mismo dominio que el servidor o bien en un dominio que tenga una relación de confianza bidireccional con el dominio que contiene el servidor.|
|\<Server2>…|Especifica los servidores adicionales que estarán a disposición de este cliente. Si especifica una lista de servidores, separe los nombres con un espacio.|
|/d|Borra la lista de servidores remotos. Deshabilita el cliente TAPI, ya que le impide usar los proveedores de servicios TAPI que se encuentran en los servidores remotos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores del equipo local o tener delegada la autoridad adecuada. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio podrían llevar a cabo este procedimiento. Como práctica de seguridad recomendada, piense en usar **Ejecutar como** para llevar a cabo este procedimiento.
-   Para que TAPI funcione correctamente, debe ejecutar **tcmsetup** para especificar los servidores remotos que usarán los clientes TAPI.
-   Para que un usuario cliente pueda usar un teléfono o una línea en un servidor TAPI, el administrador del servidor de telefonía debe asignar el usuario al teléfono o la línea.
-   La lista de servidores de telefonía creada con este comando reemplazará cualquier lista existente que esté a disposición del cliente. Este comando no se puede usar para agregar a la lista existente.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Información general del shell de comandos](/previous-versions/windows/it-pro/windows-server-2003/cc737438(v=ws.10))

[Especificar los servidores de telefonía en un equipo cliente](/previous-versions/windows/it-pro/windows-server-2003/cc759226(v=ws.10))

[Asignación de un usuario de telefonía a una línea o un teléfono](/previous-versions/windows/it-pro/windows-server-2003/cc736875(v=ws.10))
