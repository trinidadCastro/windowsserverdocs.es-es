---
title: netsh
description: Artículo de referencia para el comando netsh, que es una utilidad de scripting de línea de comandos que le permite, ya sea de forma local o remota, mostrar o modificar la configuración de red de un equipo actualmente en ejecución.
ms.topic: reference
ms.assetid: 96fc069d-53c0-4d0a-9f7f-f9f3d49a02bd carmonmills
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc8f6aff94494422150643fed6ce6681dfe54036
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037783"
---
# <a name="netsh"></a>netsh

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

La utilidad de scripting de la línea de comandos de Shell de red que permite mostrar o modificar la configuración de red de un equipo actualmente en ejecución de forma local o remota. Puede iniciar esta utilidad en el símbolo del sistema o en Windows PowerShell.

## <a name="syntax"></a>Sintaxis

```
netsh [-a <Aliasfile>][-c <Context>][-r <Remotecomputer>][-u [<domainname>\<username>][-p <Password> | [{<NetshCommand> | -f <scriptfile>}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -a `<Aliasfile>` | Especifica que se devuelva al símbolo del sistema de Netsh después de ejecutar Aliasfile y el nombre del archivo de texto que contiene uno o más comandos Netsh. |
| -c `<Context>` | Especifica que netsh entra en el contexto de Netsh especificado y en el contexto de Netsh que se va a escribir. |
| -r `<Remotecomputer>` | Especifica el equipo remoto que se va a configurar.<p>**Importante:** Si usa este parámetro, debe asegurarse de que el servicio de registro remoto se está ejecutando en el equipo remoto. Si no se está ejecutando, Windows muestra un mensaje de error "ruta de acceso de red no encontrada". |
| -u `<domainname>\<username>` | Especifica el dominio y el nombre de la cuenta de usuario que se utilizará al ejecutar el comando netsh en una cuenta de usuario. Si omite el dominio, se utiliza el dominio local de forma predeterminada. |
| -p `<Password>` | Especifica la contraseña de la cuenta de usuario especificada por el `-u <username>` parámetro. |
| `<NetshCommand>` | Especifica el comando netsh que se va a ejecutar. |
| -f `<scriptfile>` | Sale del comando netsh después de ejecutar el archivo de script especificado. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si especifica **-r** seguido de otro comando, netsh ejecuta el comando en el equipo remoto y, a continuación, vuelve al símbolo del sistema de Cmd.exe. Si especifica **-r** sin otro comando, netsh se abre en modo remoto. El proceso es similar al uso de **set machine** en el símbolo del sistema de netsh. Cuando se usa **-r**, solo se establece el equipo de destino para la instancia actual de Netsh. Después de salir de netsh y volver a entrar, el equipo de destino se restablece como equipo local. Puedes ejecutar comandos netsh en un equipo remoto si especificas un nombre de equipo almacenado en WINS, un nombre de UNC, un nombre de Internet que deba resolver el servidor DNS, o una dirección IP.

- Si el valor de cadena contiene espacios entre caracteres, debe escribir el valor de cadena entre comillas. Por ejemplo: `-r "contoso remote device"`

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
