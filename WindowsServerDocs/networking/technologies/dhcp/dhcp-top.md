---
title: Protocolo de configuración dinámica de host (DHCP)
description: En este tema se proporciona una breve introducción al Protocolo de configuración dinámica de host (DHCP) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c71517bc742cf9eda62cc7d83128f1ab9bd04547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355398"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Protocolo de configuración dinámica de host (DHCP)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener una breve introducción a DHCP en Windows Server 2016.

> [!NOTE]
> Además de este tema, está disponible la siguiente documentación de DHCP.
>
> - [Novedades de DHCP](What-s-New-in-DHCP.md)
> - [Implementación de DHCP mediante Windows PowerShell](dhcp-deploy-wps.md)

El protocolo de configuración dinámica de host (DHCP) es un protocolo cliente/servidor que proporciona automáticamente un host de protocolo de Internet (IP) con su dirección IP y otra información de configuración relacionada, como la máscara de subred y la puerta de enlace predeterminada. RFC 2131 y 2132 definen DHCP como un estándar de Internet Engineering Task Force (IETF) basado en el protocolo bootstrap (BOOTP), un protocolo con el que DHCP comparte muchos detalles de implementación. DHCP permite a los hosts obtener la información de configuración de TCP/IP necesaria desde un servidor DHCP.

Windows Server 2016 incluye el servidor DHCP, que es un rol de servidor de red opcional que se puede implementar en la red para conceder direcciones IP y otra información a los clientes DHCP. Todos los sistemas operativos cliente basados en Windows incluyen el cliente DHCP como parte de TCP/IP y el cliente DHCP está habilitado de forma predeterminada.

## <a name="why-use-dhcp"></a>¿Por qué usar DHCP?

Todos los dispositivos de una red basada en TCP/IP deben tener una dirección IP de unidifusión única para tener acceso a la red y sus recursos. Sin DHCP, las direcciones IP de los equipos nuevos o los equipos que se mueven de una subred a otra deben configurarse manualmente. Las direcciones IP de los equipos que se quitan de la red se deben reclamar manualmente.

Con DHCP, todo este proceso se automatiza y administra de forma centralizada. El servidor DHCP mantiene un grupo de direcciones IP y concede una dirección a cualquier cliente habilitado para DHCP cuando se inicia en la red. Dado que las direcciones IP son dinámicas (alquiladas) en lugar de estáticas (asignadas de forma permanente), las direcciones que ya no se usan se devuelven automáticamente al grupo para su reasignación.

El administrador de red establece los servidores DHCP que mantienen la información de configuración de TCP/IP y proporcionan la configuración de direcciones a los clientes habilitados para DHCP en forma de oferta de concesión. El servidor DHCP almacena la información de configuración en una base de datos que incluye:

- Parámetros de configuración de TCP/IP válidos para todos los clientes de la red.

- Direcciones IP válidas, mantenidas en un grupo para su asignación a los clientes, así como direcciones excluidas.

- IP reservada direcciones asociadas a determinados clientes DHCP. Esto permite la asignación coherente de una única dirección IP a un solo cliente DHCP.

- La duración de la concesión o el período de tiempo durante el que se puede usar la dirección IP antes de que se requiera una renovación de concesión.

Un cliente habilitado para DHCP, al aceptar una oferta de concesión, recibe:

- Una dirección IP válida para la subred a la que se está conectando.  
  
- Opciones DHCP solicitadas, que son parámetros adicionales que un servidor DHCP configura para asignar a los clientes. Algunos ejemplos de opciones de DHCP son enrutador (puerta de enlace predeterminada), servidores DNS y nombre de dominio DNS.

## <a name="benefits-of-dhcp"></a>Ventajas de DHCP

DHCP ofrece las siguientes ventajas.

- **Configuración de direcciones IP confiables**. DHCP minimiza los errores de configuración causados por la configuración manual de direcciones IP, como errores tipográficos, o conflictos de direcciones causados por la asignación de una dirección IP a más de un equipo al mismo tiempo.

- **Administración de red reducida**. DHCP incluye las siguientes características para reducir la administración de la red:

    - Configuración de TCP/IP centralizada y automatizada.

    - La capacidad de definir configuraciones TCP/IP desde una ubicación central.

    - La capacidad de asignar una gama completa de valores de configuración de TCP/IP adicionales por medio de opciones de DHCP.

    - El control eficaz de los cambios de direcciones IP para los clientes que se deben actualizar con frecuencia, como los de los dispositivos portátiles que se mueven a ubicaciones diferentes en una red inalámbrica.

    - El reenvío de mensajes DHCP iniciales mediante un agente de retransmisión DHCP, que elimina la necesidad de un servidor DHCP en cada subred.

