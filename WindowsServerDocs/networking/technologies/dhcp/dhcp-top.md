---
title: Protocolo de configuración dinámica de Host (DHCP)
description: En este tema proporciona una introducción breve de protocolo de configuración de dinámica Host (DHCP) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 770cfd78c7b9a0e122bd9f9936623a73b56af808
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Protocolo de configuración dinámica de Host (DHCP)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema encontrarás una breve introducción de DHCP en Windows Server 2016.

>[!NOTE]
>Además de este tema, la siguiente documentación DHCP está disponible.
>
>- [Novedades de DHCP](What-s-New-in-DHCP.md)
>- [Implementar DHCP mediante Windows PowerShell](dhcp-deploy-wps.md)

El protocolo de configuración dinámica de Host (DHCP) es un protocolo de cliente/servidor que proporciona automáticamente un host de protocolo de Internet (IP) con su dirección IP y otra información de configuración relacionadas, como la puerta de enlace de predeterminada y la máscara de subred. RFC 2131 y 2132 definen DHCP como un Internet ingeniería (IETF) estándar basado en arranque protocolo (BOOTP), un protocolo con el que DHCP comparte muchos detalles de implementación. DHCP permite hosts obtener información de configuración de TCP/IP requerida de un servidor DHCP.

Windows Server 2016 incluye servidor DHCP, que es un rol de servidor de red opcional que se puede implementar en la red para conceden direcciones IP y otra información a los clientes DHCP. Todos los sistemas operativos de cliente de Windows incluyen al cliente DHCP como parte de TCP/IP y cliente DHCP está habilitado de manera predeterminada.

## <a name="why-use-dhcp"></a>¿Por qué usar DHCP?

Todos los dispositivos en una red basada en TCP/IP deben tener una dirección IP de unidifusión único para acceder a la red y sus recursos. Sin DHCP, direcciones IP para equipos nuevos o que se mueve de una subred a otra deben configurarse manualmente. Direcciones IP para equipos que se quitan de la red se deben reclamar manualmente.

Con DHCP, todo el proceso es automática y administrar de forma centralizada. El servidor DHCP mantiene un grupo de direcciones IP y concede una dirección a cualquier cliente DHCP cuando se inicia en la red. Dado que las direcciones IP dinámicas (concedida) en lugar de estático (permanentemente asignado), se devuelven automáticamente direcciones ya no está en uso al grupo para la reasignación.

El Administrador de red establece servidores DHCP que mantienen información de configuración de TCP/IP y configuración de la dirección a los clientes DHCP en forma de una oferta de concesión. El servidor DHCP almacena la información de configuración en una base de datos que se incluye:

- Parámetros de configuración de TCP/IP válidos para todos los clientes en la red.

- Direcciones IP, mantienen en un grupo para la asignación a los clientes, así como excluye de direcciones.

- Direcciones IP reservadas asociadas a determinados clientes DHCP. Esto permite la asignación coherente de una sola dirección IP para un solo cliente DHCP.

- La duración de concesión o el período de tiempo para los que puede utilizar la dirección IP antes de que se requiere una renovación.

Recibe un cliente DHCP, al aceptar una oferta de concesión:

- Una dirección IP válida para la subred al cual se conecta.  
  
- Solicita opciones de DHCP, que son parámetros adicionales que se configura un servidor DHCP para asignar a los clientes. Algunos ejemplos de opciones de DHCP son enrutador (puerta de enlace predeterminada), los servidores DNS y nombre de dominio DNS.

## <a name="benefits-of-dhcp"></a>Ventajas de DHCP

DHCP proporciona las siguientes ventajas.

- **Configuración de dirección IP confiable**. DHCP reduce los errores de configuración causados por la configuración de dirección IP manual, como los errores tipográficos, o dirección de los conflictos causados por la asignación de una dirección IP a más de un equipo al mismo tiempo.

- **Reduce la administración de red**. DHCP incluye las siguientes características para reducir la administración de red:

    - Configuración de TCP/IP centralizada y automatizado.

    - La capacidad de definir la configuración de TCP/IP desde una ubicación central.

    - La capacidad para asignar una gama completa de los valores de configuración de TCP/IP adicionales mediante las opciones de DHCP.

    - Cambia el control de dirección IP eficaz para los clientes que deben actualizarse con frecuencia, como los dispositivos portátiles que se mueven a diferentes ubicaciones en una red inalámbrica.

    - El reenvío de mensajes DHCP iniciales mediante el uso de un agente de transmisión DHCP, que elimina la necesidad de un servidor DHCP en cada subred.

