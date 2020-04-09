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
ms.openlocfilehash: bbfc9a0238d258f11233b0e48a30048c1d62cef4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833418"
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
|/c|Obligatorio. Especifica la configuración del cliente.|
|\<Servidor1 >|Obligatorio. Especifica el nombre del servidor remoto que tiene los proveedores de servicios TAPI que utilizará el cliente. El cliente usará las líneas y los teléfonos de los proveedores de servicios. El cliente debe encontrarse en el mismo dominio que el servidor o bien en un dominio que tenga una relación de confianza bidireccional con el dominio que contiene el servidor.|
|\<servidor2 >...|Especifica los servidores adicionales que estarán a disposición de este cliente. Si especifica una lista de servidores, separe los nombres con un espacio.|
|/d|Borra la lista de servidores remotos. Deshabilita el cliente TAPI, ya que le impide usar los proveedores de servicios TAPI que se encuentran en los servidores remotos.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para realizar este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tienen que delegarle la autoridad adecuada. Si el equipo está unido a un dominio, los miembros del grupo Administradores del dominio podrían realizar este procedimiento. Como práctica de seguridad recomendada, piense en usar **Ejecutar como** para llevar a cabo este procedimiento.
-   Para que TAPI funcione correctamente, debe ejecutar **tcmsetup** para especificar los servidores remotos que usarán los clientes TAPI.
-   Para que un usuario cliente pueda usar un teléfono o una línea en un servidor TAPI, el administrador del servidor de telefonía debe asignar el usuario al teléfono o la línea.
-   La lista de servidores de telefonía creada con este comando reemplazará cualquier lista existente que esté a disposición del cliente. Este comando no se puede usar para agregar a la lista existente.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Información general del shell de comandos](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Especificar los servidores de telefonía en un equipo cliente](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Asignación de un usuario de telefonía a una línea o un teléfono](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

