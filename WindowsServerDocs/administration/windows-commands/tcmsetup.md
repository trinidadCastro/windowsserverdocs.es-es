---
title: tcmsetup
description: Obtenga información acerca de cómo configurar y deshabilitar el cliente TAPI.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0c646acef51f06c57f16ec7e5310e3319a11383f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370685"
---
# <a name="tcmsetup"></a>tcmsetup



Configura o deshabilita el cliente TAPI.

## <a name="syntax"></a>Sintaxis

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/q|Impide que se muestren cuadros de mensaje.|
|/x|Especifica que se usarán devoluciones de llamada orientadas a la conexión para redes de tráfico intensiva en las que la pérdida de paquetes es alta. Cuando se omite este parámetro, se usarán devoluciones de llamada sin conexión.|
|/c|Obligatorio. Especifica la configuración del cliente.|
|@no__t 0Server1 >|Obligatorio. Especifica el nombre del servidor remoto que tiene los proveedores de servicios TAPI que utilizará el cliente. El cliente usará las líneas y los teléfonos de los proveedores de servicios. El cliente debe estar en el mismo dominio que el servidor o en un dominio que tenga una relación de confianza bidireccional con el dominio que contiene el servidor.|
|\<Server2 >...|Especifica el servidor o los servidores adicionales que estarán disponibles para este cliente. Si especifica una lista de servidores, use un espacio para separar los nombres de los servidores.|
|/d|Borra la lista de servidores remotos. Deshabilita el cliente TAPI evitando el uso de los proveedores de servicios TAPI que se encuentran en los servidores remotos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio podrían llevar a cabo este procedimiento. Como práctica de seguridad recomendada, piense en usar **Ejecutar como** para llevar a cabo este procedimiento.
-   Para que TAPI funcione correctamente, debe ejecutar **tcmsetup** para especificar los servidores remotos que usarán los clientes TAPI.
-   Para que un usuario cliente pueda usar un teléfono o una línea en un servidor TAPI, el administrador del servidor de telefonía debe asignar el usuario al teléfono o la línea.
-   La lista de servidores de telefonía que se crea mediante este comando reemplaza cualquier lista existente de servidores de telefonía disponibles para el cliente. No puede usar este comando para agregar a la lista existente.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Información general del shell de comandos](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Especificar los servidores de telefonía en un equipo cliente](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Asignación de un usuario de telefonía a una línea o un teléfono](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

