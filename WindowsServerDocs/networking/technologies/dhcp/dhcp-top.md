---
title: Protocolo de configuración dinámica de host (DHCP)
description: En este tema se proporciona una breve descripción del protocolo de configuración de Dynamic Host (DHCP) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 08b07e902486ae633b30949270e15f8bf94afaaf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857496"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Protocolo de configuración dinámica de host (DHCP)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener una introducción breve de DHCP en Windows Server 2016.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación de DHCP.
>
>- [Novedades de DHCP](What-s-New-in-DHCP.md)
>- [Implementar DHCP con Windows PowerShell](dhcp-deploy-wps.md)

El protocolo de configuración dinámica de Host (DHCP) es un protocolo de cliente/servidor que proporciona automáticamente un host de protocolo de Internet (IP) con su dirección IP y otra información de configuración relacionados, como la puerta de enlace de predeterminada y la máscara de subred. RFC 2131 y 2132 definen DHCP como un Internet Engineering Task Force (IETF) estándar basado en protocolo Bootstrap (BOOTP), un protocolo con el que DHCP comparte muchos detalles de implementación. DHCP permite a los hosts obtener información de configuración de TCP/IP necesaria de un servidor DHCP.

Windows Server 2016 incluye el servidor DHCP, que es un rol de servidor de red opcional que puede implementar en la red a las direcciones IP de concesión y otra información a los clientes DHCP. Todos los sistemas operativos de cliente basado en Windows, se incluyen al cliente DHCP como parte de TCP/IP y el cliente DHCP está habilitado de forma predeterminada.

## <a name="why-use-dhcp"></a>¿Por qué usar DHCP?

Todos los dispositivos en una red basada en TCP/IP deben tener una dirección IP de unidifusión único para tener acceso a la red y sus recursos. Sin DHCP, se deben configurar manualmente; direcciones IP para equipos nuevos o que se han movido de una subred a otra Direcciones IP para equipos que se quitan de la red se deben reclamar manualmente.

Con DHCP, todo este proceso es automatizado y administran de forma centralizada. El servidor DHCP mantiene un grupo de direcciones IP y concede una dirección a cualquier cliente DHCP cuando se inicia en la red. Dado que las direcciones IP son dinámicas (concedida) en lugar de estático (asignado de forma permanente), direcciones ya no está en uso se devuelven automáticamente al grupo para la reasignación.

El Administrador de red establece los servidores DHCP que mantienen la información de configuración de TCP/IP y proporcionan la configuración de direcciones a los clientes habilitados para DHCP en forma de una oferta de concesión. El servidor DHCP almacena la información de configuración en una base de datos que incluye:

- Parámetros de configuración de TCP/IP válidos para todos los clientes en la red.

- Direcciones IP válidas, mantenerse en un grupo para la asignación a los clientes, así como excluir las direcciones.

- Direcciones IP reservadas asociada con los clientes DHCP concreto. Esto permite la asignación coherente de una única dirección IP a un único cliente DHCP.

- La duración de concesión, o el período de tiempo para el que se puede usar la dirección IP antes de solicitar una renovación de concesión.

Recibe un cliente DHCP habilitado, al aceptar una oferta de concesión:

- Una dirección IP válida para la subred a la que se está conectando.  
  
- Solicita las opciones de DHCP, que son parámetros adicionales que se configura un servidor DHCP para asignar a los clientes. Algunos ejemplos de las opciones DHCP son enrutador (puerta de enlace predeterminada), los servidores DNS y nombre de dominio DNS.

## <a name="benefits-of-dhcp"></a>Ventajas de DHCP

DHCP proporciona las siguientes ventajas.

- **Configuración de dirección IP confiable**. DHCP minimiza los errores de configuración causados por la configuración manual de direcciones IP, como errores tipográficos, o solucionar los conflictos causados por la asignación de una dirección IP a más de un equipo al mismo tiempo.

- **Reduce la administración de red**. DHCP incluye las siguientes características para reducir la administración de red:

    - Configuración de TCP/IP centralizada y automatizada.

    - La capacidad para definir las configuraciones TCP/IP desde una ubicación central.

    - La capacidad para asignar una gama completa de valores de configuración de TCP/IP adicionales por medio de las opciones de DHCP.

    - El control eficaz de los cambios de dirección IP para los clientes que se deben actualizar con frecuencia, como los de los dispositivos portátiles que se mueven a ubicaciones diferentes en una red inalámbrica.

    - El reenvío de mensajes DHCP iniciales mediante el uso de un agente de retransmisión DHCP, lo que elimina la necesidad de un servidor DHCP en cada subred.

