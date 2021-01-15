---
title: pktmon agregar filtro
description: Artículo de referencia para el comando pktmon Filter Add.
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: e1e69bbad5433eedd582b1d6218e462b4c70f71d
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241171"
---
# <a name="pktmon-filter-add"></a>pktmon agregar filtro

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

Pktmon Filter Add le permite agregar un filtro para controlar los paquetes que se muestran. Para que se notifique un paquete, debe coincidir con todas las condiciones especificadas en al menos un filtro. Puede haber hasta ocho filtros activos a la vez.

## <a name="syntax"></a>Sintaxis

```
pktmon filter add <name> [-m mac [mac2]] [-v vlan] [-d { IPv4 | IPv6 | number }]
                         [-t { TCP [flags...] | UDP | ICMP | ICMPv6 | number }]
                         [-i ip [ip2]] [-p port [port2]] [-e [port]]
```

Puede proporcionar un nombre opcional o una descripción del filtro.

  > [!NOTE]
  > Cuando se especifican dos equipos Mac (-m), direcciones IP (-i) o puertos (-p), el filtro coincide con los paquetes que contienen ambos. No se distinguirá entre el origen o el destino para este fin.

### <a name="parameters"></a>Parámetros

| **Parámetro** | **Descripción** |
| ------------- | --------------- |
| **-m,--Mac [-address]** | Coincide con la dirección MAC de origen o de destino. Vea la nota anterior. |
| **-v,--VLAN** | Coincide con el identificador de VLAN (VID) en el encabezado Q de 802.1. |
| **-d,--Data-Link [-Protocol],--Ethertype** | Coincide con el protocolo de vínculo de datos (capa 2). Puede ser IPv4, IPv6, ARP o un número de protocolo. |
| **-t,--Transport [-Protocol],--IP-Protocol** | Coincide con el protocolo de transporte (nivel 4). Puede ser TCP, UDP, ICMP, ICMPv6 o un número de protocolo. Para filtrar aún más los paquetes TCP, se puede proporcionar una lista opcional de marcas TCP para la coincidencia. Las marcas admitidas son FIN, SYN, RST, PSH, ACK, URG, ECE y CWR. |
| **-i,--IP [-address]** | Coincide con la dirección IP de origen o de destino. Vea la nota anterior. Para buscar coincidencias por subred, use la notación CIDR con la longitud del prefijo. |
| **-p,--Port** | Coincide con el número de puerto de origen o de destino. Vea la nota anterior. |
| **-e,--encap** | Este filtro también se aplica a los paquetes internos encapsulados, además del paquete externo. Los métodos de encapsulación admitidos son VXLAN, GRE, NVGRE y IP-in-IP. El puerto VXLAN personalizado es opcional y su valor predeterminado es 4789. |

## <a name="examples"></a>Ejemplos

El siguiente conjunto de filtros capturará cualquier tráfico ICMP desde o hacia la dirección IP 10.0.0.10 junto con cualquier tráfico en el puerto 53.

```PowerShell
C:\Test> pktmon filter add -i 10.0.0.10 -t icmp
C:\Test> pktmon filter add -p 53
```

El siguiente filtro capturará todos los paquetes SYN enviados o recibidos por la dirección IP 10.0.0.10:

```PowerShell
C:\Test> pktmon filter add -i 10.0.0.10 -t tcp syn
```

El siguiente **filtro denominado comando ping en** 10.10.10.10 usa el protocolo ICMP:

```PowerShell
C:\Test> pktmon filter add MyPing -i 10.10.10.10 -t ICMP
```

El siguiente filtro llamado **MySmbSyb** captura el tráfico SMB sincronizado TCP:

```PowerShell
C:\Test> pktmon filter add MySmbSyn -i 10.10.10.10 -t TCP SYN -p 445
```

El siguiente filtro denominado **subred** captura el tráfico en la máscara de subred 255.255.255.0 o/24 en la notación CIDR:

```PowerShell
C:\Test> pktmon filter add MySubnet -i 10.10.10.0/24
```

## <a name="other-references"></a>Otras referencias

- [Pktmon](pktmon.md)
- [Comp. Pktmon](pktmon-comp.md)
- [Contadores de Pktmon](pktmon-counters.md)
- [Filtro de Pktmon](pktmon-filter.md)
- [Formato Pktmon](pktmon-format.md)
- [Lista de Pktmon](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Restablecimiento de Pktmon](pktmon-reset.md)
- [Inicio de Pktmon](pktmon-start.md)
- [Descarga de Pktmon](pktmon-unload.md)
- [Información general del monitor de paquetes](/windows-server/networking/technologies/pktmon/pktmon)
