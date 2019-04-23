---
title: Orientación de red principal para Windows Server
description: Este tema proporciona una visión general de la Guía de red principal, lo que permite a planear e implementar los componentes principales necesarios para una red plenamente funcional y un nuevo dominio de Active Directory en un nuevo bosque con Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a905fd0c11237edd3a408998f8f71aa25a054328
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847906"
---
# <a name="core-network-guidance-for-windows-server"></a>Orientación de red principal para Windows Server

>Se aplica a: Windows Server, Windows Server 2016

Este tema proporciona información general de la Guía de red principal para Windows Server&reg; 2016 y contiene las siguientes secciones.  
  
-   [Introducción a la red de Windows Server Core](#bkmk_intro)  
  
-   [Guía de red principal para Windows Server](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Introducción a la red de Windows Server Core

Una red principal es una colección de hardware, dispositivos y software de red que proporciona los servicios fundamentales para satisfacer las necesidades de las tecnologías de la información (TI) de la organización.

Una red principal de Windows Server le ofrece muchas ventajas, entre las que se incluyen las siguientes.

- Protocolos principales para la conexión de red entre equipos y otros dispositivos compatibles con Protocolo de control de transmisión/Protocolo de Internet (TCP/IP). TCP/IP es un conjunto de protocolos estándar pensado para conectar equipos y crear redes. TCP/IP es un software de protocolo de red que se valió Microsoft&reg; Windows&reg; conjunto de protocolos de los sistemas operativos que se implementa y es compatible con TCP/IP.

- Direccionamiento IP automático de servidor de Protocolo de configuración dinámica de host (DHCP). La configuración manual de direcciones IP en todos los equipos de la red es una tarea que consume mucho tiempo y es menos flexible que la opción de proporcionar dinámicamente a equipos y otros dispositivos concesiones de direcciones IP desde un servidor DHCP.

- Servicio de resolución de nombres del Sistema de nombres de dominio (DNS). Con DNS, los usuarios, equipos, aplicaciones y servicios pueden usar el nombre de dominio completo de un equipo o un dispositivo para encontrar la dirección IP de dicho equipo o dispositivo.

- Un bosque, que es uno o más dominios de Active Directory que comparten las mismas definiciones de clase y atributo (esquema), información de sitio y replicación (configuración) y capacidades de búsqueda para todo el bosque (catálogo global).

- Un dominio raíz del bosque, que es el primer dominio creado en un nuevo bosque. Los grupos Administradores de empresas y Administradores de esquema, que son grupos administrativos para todo el bosque, se encuentran en el dominio raíz del bosque. Además, un dominio raíz del bosque, como los demás dominios, es una colección de objetos de equipo, usuario y grupo definidos por el administrador en Servicios de dominio de Active Directory (AD DS). Estos objetos comparten una base de datos de directorios común y directivas de seguridad. También comparten relaciones de seguridad con otros dominios, si se agregan dominios a medida que la organización crece. El servicio de directorio también almacena datos de directorio y permite que los equipos, aplicaciones y usuarios autorizados tengan acceso a los datos.

- Una base de datos de cuentas de usuario y equipo. El servicio de directorio proporciona una base de datos de cuentas de usuario centralizada que le permite crear cuentas de usuario y equipo para las personas y equipos que están autorizados para conectarse a la red y tener acceso a recursos de red, como aplicaciones, bases de datos, carpetas y archivos compartidos e impresoras.

Una red principal también le permite escalar la red a medida que crece la organización y cambian los requisitos de TI. Por ejemplo, con una red principal puede agregar dominios, subredes IP, servicios de acceso remoto, servicios inalámbricos y otras características y roles de servidor proporcionados por Windows Server 2016.

## <a name="bkmk_core"></a>Guía de red principal para Windows Server

La Guía de red de Windows Server 2016 Core proporciona instrucciones sobre cómo planear e implementar los componentes principales necesarios para una red plenamente funcional y un nuevo Active Directory&reg; dominio en un bosque nuevo. Por medio de esta guía, podrá implementar equipos configurados con los siguientes componentes de servidor de Windows:

- El rol de servidor Active Directory Domain Services (AD DS)

- El rol de servidor Sistema de nombres de dominio (DNS)

- El rol de servidor Protocolo de configuración dinámica de host (DHCP)

- El servicio de rol Servidor de directivas de redes (NPS) del rol de servidor Servicios de acceso y directivas de redes

- Rol de servidor Servidor web (IIS)

- Conexiones de Protocolo de control de transmisión/Protocolo de Internet (TCP/IP) versión 4 en servidores individuales.

Esta guía está disponible en la siguiente ubicación.

- El [Guía de red principal](../core-network-guide/Core-Network-Guide.md) en la biblioteca técnica de Windows Server 2016.
  

