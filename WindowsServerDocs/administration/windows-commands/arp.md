---
title: arp
description: Comando comandos de Windows para **ARP**, que muestra y modifica las entradas de la caché del Protocolo de resolución de direcciones (ARP) que se usa para almacenar las direcciones IP y sus direcciones físicas resueltas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 289ab87c99512c7c34154d4b85d541db82ab296b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851318"
---
# <a name="arp"></a>arp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra y modifica las entradas de la caché del Protocolo de resolución de direcciones (ARP). La caché ARP contiene una o más tablas que se usan para almacenar las direcciones IP y sus direcciones físicas de token ring o Ethernet resueltas. Hay una tabla independiente para cada adaptador de red Ethernet o token ring instalado en el equipo. Cuando se usa sin parámetros, **ARP** muestra información de ayuda.

## <a name="syntax"></a>Sintaxis
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/a [<Inetaddr>] [/n <ifaceaddr>]` | Muestra las tablas de caché ARP actuales para todas las interfaces. El parámetro `/n` distingue entre mayúsculas y minúsculas. Para mostrar la entrada de caché ARP para una dirección IP específica, use **ARP/a** con el parámetro *Direccióndeinternet* , donde *direccióndeinternet* es una dirección IP. Si no se especifica *direccióndeinternet* , se usa la primera interfaz aplicable. Para mostrar la tabla de caché ARP para una interfaz específica, use el parámetro * */n * * * direccióndeinterfaz* junto con el parámetro **/a** , donde *direccióndeinterfaz* es la dirección IP asignada a la interfaz. |
| `/g [<Inetaddr>] [/n <ifaceaddr> `] | Idéntico a **/a**. |
|`[/d <Inetaddr> [<ifaceaddr>]` | Elimina una entrada con una dirección IP específica, donde *direccióndeinternet* es la dirección IP. Para eliminar una entrada en una tabla para una interfaz específica, use el parámetro *direccióndeinterfaz* , donde *direccióndeinterfaz* es la dirección IP asignada a la interfaz. Para eliminar todas las entradas, use el carácter comodín de asterisco (\*) en lugar de *direccióndeinternet*. |
|`/s <Inetaddr> <Etheraddr> [<ifaceaddr>]` | Agrega una entrada estática a la caché ARP que resuelve la dirección IP *direccióndeinternet* en la dirección física *Etheraddr*. Para agregar una entrada de caché ARP estática a la tabla para una interfaz específica, use el parámetro *direccióndeinterfaz* , donde *direccióndeinterfaz* es una dirección IP asignada a la interfaz. |
|`/?`| Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios
- Las direcciones IP de *direccióndeinternet* y *direccióndeinterfaz* se expresan en notación decimal con puntos.
- La dirección física de *Etheraddr* consta de seis bytes expresada en notación hexadecimal y separados por guiones (por ejemplo, 00-AA-00-4F-2A-9c).

- Las entradas agregadas con el parámetro **/s** son estáticas y no agotan el tiempo de espera de la caché ARP. Las entradas se quitan si el protocolo TCP/IP se detiene y se inicia. Para crear entradas de caché ARP estáticas permanentes, coloque los comandos **ARP** adecuados en un archivo por lotes y use tareas programadas para ejecutar el archivo por lotes en el inicio.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Para mostrar las tablas de caché ARP para todas las interfaces, escriba:

```
arp /a
```

Para mostrar la tabla de caché ARP de la interfaz a la que se asigna la dirección IP 10.0.0.99, escriba:

```
arp /a /n 10.0.0.99
```

Para agregar una entrada de caché ARP estática que resuelve la dirección IP 10.0.0.80 en la dirección física 00-AA-00-4F-2A-9C, escriba:

```
arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
