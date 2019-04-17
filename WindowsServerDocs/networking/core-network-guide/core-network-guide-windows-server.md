---
title: Guía básica de red para Windows Server
description: Este tema proporciona una visión general de la guía básica de red, que permite a planear e implementar los componentes básicos necesarios para una red totalmente funcional y un nuevo dominio de Active Directory en un bosque nuevo con Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63e4cf8c5bf56ef5131e835163a5fcb5dfd98b55
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide-for-windows-server"></a>Guía básica de red para Windows Server

>Se aplica a: Windows Server, Windows Server 2016

Este tema proporciona una visión general de la Guía de red principal para Windows Server&reg; de 2016 y contiene las siguientes secciones.  
  
-   [Introducción a la red de Windows Server Core](#bkmk_intro)  
  
-   [Guía básica de red para Windows Server](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Introducción a la red de Windows Server Core

Una red principal es una colección de hardware de red, dispositivos, y necesidades de software que proporciona los servicios fundamentales para la tecnología de la organización de la información (TI).

Una red de Windows Server core proporciona muchas ventajas, incluidas las siguientes acciones.

- Protocolos principales para la conectividad de red entre equipos y otros dispositivos compatibles con el protocolo o a Internet Protocol (TCP). TCP/IP es un conjunto de protocolos estándar para conectar equipos y crear redes. TCP/IP es un software de protocolo de red incluido con Microsoft&reg; Windows&reg; conjunto de protocolos de sistemas operativos que implementa y admite el TCP/IP.

- Protocolo de configuración de Host (DHCP) server automática direcciones IP dinámicas. Configuración manual de direcciones IP en todos los equipos de la red es lenta y menos flexible que proporcionar dinámicamente equipos y otros dispositivos con concesiones de direcciones IP de un servidor DHCP.

- Servicio de resolución de nombres de sistema de nombres de dominio (DNS). DNS permite a los usuarios, equipos, aplicaciones y servicios buscar las direcciones IP de equipos y dispositivos en la red mediante el nombre de dominio completo del equipo o dispositivo.

- Un bosque, que es uno o varios dominios de Active Directory que comparten la misma clase definiciones y atributos (esquema), información del sitio y de replicación (configuración) y capacidades de búsqueda para todo el bosque (catálogo global).

- Crea un dominio raíz del bosque, que es el primer dominio en un bosque nuevo. Los grupos Administradores de empresa y administradores de esquema, que son grupos administrativos de todo el bosque, se encuentran en el dominio raíz. Además, un dominio raíz del bosque, al igual que con otros dominios, es una colección de equipo, los usuarios y los objetos de grupo que están definidos por el Administrador de servicios de dominio de Active Directory (AD DS). Estos objetos comparten un directivas de seguridad y la base de datos de directorio comunes. También pueden compartir relaciones de seguridad con otros dominios si agregar dominios a medida que crece la organización. El servicio de directorio también almacena datos del directorio y permite que los equipos autorizados, aplicaciones y los usuarios acceder a los datos.

- Un usuario y el equipo cuenta base de datos. El servicio de directorio proporciona una base de datos de cuentas de usuario centralizadas que te permite crear cuentas de usuario y del equipo para usuarios y equipos que están autorizados a conectarse a la red y la red de acceso a recursos, como aplicaciones, bases de datos, archivos compartidos y carpetas, impresoras y.

Una red principal también te permite ampliar la red que la organización crece y cambios en los requisitos de TI. Por ejemplo, con una red principal puede agregar dominios, subredes IP, servicios de acceso remoto, servicios inalámbricos y otras características y los roles de servidor proporcionados por Windows Server 2016.

## <a name="bkmk_core"></a>Guía básica de red para Windows Server

La Guía de red de Windows Server 2016 Core proporciona instrucciones sobre cómo planear e implementar los componentes básicos necesarios para una red totalmente funcional y un nuevo Active Directory&reg; dominio en un bosque nuevo. Con esta guía, puedes implementar los equipos configurados con los siguientes componentes de servidor de Windows:

- El rol de servidor de servicios de dominio de Active Directory (AD DS)

- El rol de servidor de sistema de nombres de dominio (DNS)

- El rol de servidor de protocolo de configuración dinámica de Host (DHCP)

- El servicio de rol de servidor de directivas de redes (NPS) del rol de servidor de servicios de acceso y directivas de redes

- El rol de servidor Web Server (IIS)

- Transmisión Control protocolo/protocolo de Internet versión 4 (TCP/IP) conexiones en servidores individuales

Esta guía está disponible en la siguiente ubicación.

- La [principales de la Guía de red](../core-network-guide/Core-Network-Guide.md) en la biblioteca técnica de Windows Server 2016.
  


