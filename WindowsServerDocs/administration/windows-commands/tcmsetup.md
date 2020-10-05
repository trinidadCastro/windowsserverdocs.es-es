---
title: tcmsetup
description: Artículo de referencia del comando tcmsetup, que configura y deshabilita el cliente TAPI.
ms.topic: reference
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 315c13310d8059d5553dff9c780fbdd656867a57
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718072"
---
# <a name="tcmsetup"></a>tcmsetup

Configura o deshabilita el cliente TAPI. Para que TAPI funcione correctamente, debe ejecutar este comando para especificar los servidores remotos que usarán los clientes TAPI.

> [!IMPORTANT]
> Para usar este comando, debe ser miembro del grupo **administradores** en el equipo local o bien, tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo **Admins** . del dominio podrían realizar este procedimiento. Como práctica de seguridad recomendada, piense en usar **Ejecutar como** para llevar a cabo este procedimiento.

## <a name="syntax"></a>Sintaxis

```
tcmsetup [/q] [/x] /c <server1> [<server2> …]
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /q | Impide que se muestren los cuadros de mensaje. |
| /x | Especifica que se usarán devoluciones de llamada orientadas a la conexión en las redes de tráfico intenso donde la pérdida de paquetes es elevada. Si se omite este parámetro, se usarán devoluciones de llamada sin conexión. |
| /C | Necesario. Especifica la configuración del cliente. |
| `<server1>` | Necesario. Especifica el nombre del servidor remoto que tiene los proveedores de servicios TAPI que utilizará el cliente. El cliente usará las líneas y los teléfonos de los proveedores de servicios. El cliente debe encontrarse en el mismo dominio que el servidor o bien en un dominio que tenga una relación de confianza bidireccional con el dominio que contiene el servidor. |
| `<server2>…` | Especifica los servidores adicionales que estarán a disposición de este cliente. Si especifica una lista de servidores, separe los nombres con un espacio. |
| /d | Borra la lista de servidores remotos. Deshabilita el cliente TAPI, ya que le impide usar los proveedores de servicios TAPI que se encuentran en los servidores remotos. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Para que un usuario cliente pueda usar un teléfono o una línea en un servidor TAPI, el administrador del servidor de telefonía debe asignar el usuario al teléfono o la línea.

- La lista de servidores de telefonía creada con este comando reemplazará cualquier lista existente que esté a disposición del cliente. No puede usar este comando para agregar a la lista existente.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Información general del shell de comandos](/previous-versions/windows/it-pro/windows-server-2003/cc737438(v=ws.10))

- [Especificar los servidores de telefonía en un equipo cliente](/previous-versions/windows/it-pro/windows-server-2003/cc759226(v=ws.10))

- [Asignación de un usuario de telefonía a una línea o un teléfono](/previous-versions/windows/it-pro/windows-server-2003/cc736875(v=ws.10))
