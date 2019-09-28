---
title: 'Servicios de Escritorio remoto: Acceso desde cualquier lugar'
description: Información de planificación de una Puerta de enlace de Escritorio remoto
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: c79afeb38ce0b196c0f1edddd01a6166df53583b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403918"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Servicios de Escritorio remoto: Acceso desde cualquier lugar

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Los usuarios finales pueden conectarse a los recursos de red internos de manera segura desde fuera del firewall corporativo a través de Puerta de enlace de Escritorio remoto.

Independientemente de cómo configures los escritorios para los usuarios finales, puedes conectar fácilmente Puerta de enlace de Escritorio remoto en el flujo de conexión para una conexión rápida y segura. Para los usuarios finales que se conectan a través de fuentes publicadas, puedes configurar la propiedad de Puerta de enlace de Escritorio remoto al configurar las propiedades generales de implementación. Para los usuarios finales que se conectan a través de sus escritorios sin una fuente, pueden agregar fácilmente el nombre de la Puerta de enlace de Escritorio remoto de la organización como propiedad de conexión, independientemente de la aplicación cliente de Escritorio remoto que usen.

Los tres objetivos principales de Puerta de enlace de Escritorio remoto, en el orden de la secuencia de conexión, son:
1. **Establecer un túnel SSL cifrado entre el dispositivo del usuario final y el servidor de puerta de enlace de Escritorio remoto**: Para poder conectarse a través de cualquier servidor de puerta de enlace de Escritorio remoto, el servidor de puerta de enlace de Escritorio remoto debe tener instalado un certificado que el dispositivo del usuario final reconozca. En las pruebas y las pruebas de conceptos, se pueden usar certificados autofirmados, pero en todo entorno de producción solo deben usarse certificados de confianza pública emitidos por una entidad emisora de certificados.
2. **Autenticar al usuario en el entorno**: Puerta de enlace de Escritorio remoto usa el servicio IIS de bandeja de entrada para la autenticación e, incluso, puede usar el protocolo RADIUS para aprovechar soluciones de [autenticación multifactor](rds-plan-mfa.md), como Azure MFA. Aparte de las directivas predeterminadas creadas, puedes crear directivas de administración de recursos de Escritorio remoto (RAP de RD) y directivas de autorización de conexiones de Escritorio remoto (CAP de RD) adicionales para definir más específicamente qué usuarios deben tener acceso a qué recursos dentro del entorno seguro.
3. **Pasar el tráfico entre el dispositivo del usuario final y el recurso especificado**: Puerta de enlace de Escritorio remoto continúa realizando esta tarea siempre que se mantenga la conexión. Puedes especificar distintas propiedades de tiempo de espera en los servidores de puerta de enlace de Escritorio remoto para mantener la seguridad del entorno en caso de que el usuario se aleje del dispositivo.

Puedes encontrar detalles adicionales sobre la arquitectura general de una implementación de Servicios de Escritorio remoto [en la arquitectura de referencia de hospedaje de escritorio](desktop-hosting-reference-architecture.md).