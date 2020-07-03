---
title: arp
description: Artículo de referencia del comando ARP, que muestra y modifica las entradas de la caché del Protocolo de resolución de direcciones (ARP) que se usa para almacenar las direcciones IP y sus direcciones físicas resueltas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41f9ebde5faa3eda99402aa86a0aef5e55b42eba
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924006"
---
# <a name="arp"></a>arp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra y modifica las entradas de la caché del Protocolo de resolución de direcciones (ARP). La caché ARP contiene una o más tablas que se usan para almacenar las direcciones IP y sus direcciones físicas de token ring o Ethernet resueltas. Hay una tabla independiente para cada adaptador de red Ethernet o token ring instalado en el equipo. Cuando se usa sin parámetros, **ARP** muestra información de ayuda.

## <a name="syntax"></a>Sintaxis

```
arp [/a [<inetaddr>] [/n <ifaceaddr>]] [/g [<inetaddr>] [-n <ifaceaddr>]] [/d <inetaddr> [<ifaceaddr>]] [/s <inetaddr> <etheraddr> [<ifaceaddr>]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `[/a [<inetaddr>] [/n <ifaceaddr>]` | Muestra las tablas de caché ARP actuales para todas las interfaces. El parámetro **/n** distingue entre mayúsculas y minúsculas. Para mostrar la entrada de caché ARP para una dirección IP específica, use **ARP/a** con el parámetro **direccióndeinternet** , donde **direccióndeinternet** es una dirección IP. Si no se especifica **direccióndeinternet** , se usa la primera interfaz aplicable. Para mostrar la tabla de caché ARP para una interfaz específica, use el parámetro **/n direccióndeinterfaz** junto con el parámetro **/a** , donde **direccióndeinternet** es la dirección IP asignada a la interfaz. |
| `[/g [<inetaddr>] [/n <ifaceaddr>]` | Idéntico a **/a**. |
| `[/d <inetaddr> [<ifaceaddr>]` | Elimina una entrada con una dirección IP específica, donde **direccióndeinternet** es la dirección IP. Para eliminar una entrada en una tabla para una interfaz específica, use el parámetro **direccióndeinterfaz** , donde **direccióndeinterfaz** es la dirección IP asignada a la interfaz. Para eliminar todas las entradas, use el carácter comodín de asterisco (*) en lugar de **direccióndeinternet**. |
| `[/s <inetaddr> <etheraddr> [<ifaceaddr>]` | Agrega una entrada estática a la caché ARP que resuelve la dirección IP **direccióndeinternet** en la dirección física **etheraddr**. Para agregar una entrada de caché ARP estática a la tabla para una interfaz específica, use el parámetro **direccióndeinterfaz** , donde **direccióndeinterfaz** es una dirección IP asignada a la interfaz. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="remarks"></a>Comentarios

- Las direcciones IP de **direccióndeinternet** y **direccióndeinterfaz** se expresan en notación decimal con puntos.

- La dirección física de **etheraddr** consta de seis bytes expresada en notación hexadecimal y separados por guiones (por ejemplo, 00-AA-00-4F-2A-9c).

- Las entradas agregadas con el parámetro **/s** son estáticas y no agotan el tiempo de espera de la caché ARP. Las entradas se quitan si el protocolo TCP/IP se detiene y se inicia. Para crear entradas de caché ARP estáticas permanentes, coloque los comandos **ARP** adecuados en un archivo por lotes y use tareas programadas para ejecutar el archivo por lotes en el inicio.

## <a name="examples"></a>Ejemplos

Para mostrar las tablas de caché ARP para todas las interfaces, escriba:

```
arp /a
```

Para mostrar la tabla de caché ARP de la interfaz a la que se asigna la dirección IP *10.0.0.99*, escriba:

```
arp /a /n 10.0.0.99
```

Para agregar una entrada de caché ARP estática que resuelve la dirección IP *10.0.0.80* en la dirección física *00-AA-00-4F-2A-9C*, escriba:

```
arp /s 10.0.0.80 00-AA-00-4F-2A-9C
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
