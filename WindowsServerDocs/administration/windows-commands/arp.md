---
title: arp
description: Tema de los comandos de Windows para **arp** -muestra y modifica las entradas de la caché de protocolo de resolución (de direcciones arp) dirección utilizada para almacenar las direcciones IP y sus direcciones físicas resueltos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f7393c993601a5e1990cde3e6bc6763811062f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435293"
---
# <a name="arp"></a>arp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra y modifica las entradas de la caché del protocolo de resolución de direcciones (ARP). La caché de ARP contiene una o varias tablas que se usan para almacenar las direcciones IP y sus direcciones físicas Ethernet o Token Ring resueltas. Hay una tabla independiente para cada adaptador de red Ethernet o Token Ring instalado en el equipo. Se utiliza sin parámetros, **arp** muestra información de ayuda.
## <a name="syntax"></a>Sintaxis
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>Parámetros

|                Parámetro                |                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                               |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /a [<Inetaddr>] [/n <ifaceaddr>]     | Muestra las tablas de caché arp actual para todas las interfaces. El parámetro /n distingue mayúsculas de minúsculas.<br /><br />Para mostrar la entrada de caché de arp para una dirección IP específica, use **arp /a** con el *direcciónDeInternet* parámetro, donde *direcciónDeInternet* es una dirección IP. Si *direcciónDeInternet* no se especifica, se usa la primera interfaz aplicable.<br /><br />Para mostrar la tabla de la memoria caché de arp para una interfaz específica, use el **/n** *direcciónDeInterfaz* parámetro junto con el **/a** parámetro donde *direcciónDeInterfaz* es la dirección IP asignada a la interfaz. |
|    /g [<Inetaddr>] [/n <ifaceaddr>]     |                                                                                                                                                                                                                                                          Idéntico a **/a**.                                                                                                                                                                                                                                                           |
|      [/d <Inetaddr> [<ifaceaddr>]       |                                                                                           elimina una entrada con una dirección IP específica, donde *direcciónDeInternet* es la dirección IP.<br /><br />Para eliminar una entrada en una tabla para una interfaz específica, use el *direcciónDeInterfaz* parámetro donde *direcciónDeInterfaz* es la dirección IP asignada a la interfaz.<br /><br />Para eliminar todas las entradas, use el asterisco (\*) caracteres comodín en lugar de *direcciónDeInternet*.                                                                                           |
| /s <Inetaddr> <Etheraddr> [<ifaceaddr>] |                                                                                                                     Agrega una entrada estática a la caché de arp que resuelve la dirección IP *direcciónDeInternet* a la dirección física *direcciónEthernet*.<br /><br />Para agregar una entrada de caché de arp estática a la tabla para una interfaz específica, use el *direcciónDeInterfaz* parámetro donde *direcciónDeInterfaz* es una dirección IP asignada a la interfaz.                                                                                                                     |
|                   /?                    |                                                                                                                                                                                                                                                  Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                   |

## <a name="remarks"></a>Comentarios
- Las direcciones IP para *direcciónDeInternet* y *direcciónDeInterfaz* se expresan en notación decimal con puntos.
- La dirección física de *direcciónEthernet* consta de seis bytes expresada en notación hexadecimal y separados por guiones (por ejemplo, 00-AA-00-4F-2A-9C).
- Las entradas agregadas con la **/s** parámetro son estático y no agotar tiempo fuera de la caché de arp. Si se detiene e inicia el protocolo TCP/IP, se quitan las entradas. Para crear las entradas de caché de arp estática permanente, coloque adecuado **arp** comandos en un lote de archivos y usar tareas programadas para ejecutar el archivo por lotes en el inicio.
  ## <a name="BKMK_Examples"></a>Ejemplos
  Para mostrar las tablas de la memoria caché de arp para todas las interfaces, escriba:
  ```
  arp /a
  ```
  Para mostrar la tabla de la memoria caché de arp para la interfaz que tiene asignada la dirección IP 10.0.0.99, escriba:
  ```
  arp /a /n 10.0.0.99
  ```
  Para agregar una entrada de caché de arp estática que se resuelve la dirección IP 10.0.0.80 en la dirección física 00-AA-00-4F-2A-9C, escriba:
  ```
  arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
  ```
  ## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
