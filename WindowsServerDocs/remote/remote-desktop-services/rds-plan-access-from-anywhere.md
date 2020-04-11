---
title: 'Servicios de Escritorio remoto: Acceso desde cualquier lugar'
description: Información de planificación de una Puerta de enlace de Escritorio remoto
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: fb0fddbe86c4c06280fdbe55f2f6a7f490a1340b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857398"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Servicios de Escritorio remoto: Acceso desde cualquier lugar

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Los usuarios finales pueden conectarse a los recursos de red internos de manera segura desde fuera del firewall corporativo a través de Puerta de enlace de Escritorio remoto.

Independientemente de cómo configures los escritorios para los usuarios finales, puedes conectar fácilmente Puerta de enlace de Escritorio remoto en el flujo de conexión para una conexión rápida y segura. Para los usuarios finales que se conectan a través de fuentes publicadas, puedes configurar la propiedad de Puerta de enlace de Escritorio remoto al configurar las propiedades generales de implementación. Para los usuarios finales que se conectan a través de sus escritorios sin una fuente, pueden agregar fácilmente el nombre de la Puerta de enlace de Escritorio remoto de la organización como propiedad de conexión, independientemente de la aplicación cliente de Escritorio remoto que usen.

Los tres objetivos principales de Puerta de enlace de Escritorio remoto, en el orden de la secuencia de conexión, son:
1. **Establecer un túnel SSL cifrado entre el dispositivo del usuario final y el servidor de puerta de enlace de Escritorio remoto**: Para poder conectarse a través de cualquier servidor de puerta de enlace de Escritorio remoto, el servidor de puerta de enlace de Escritorio remoto debe tener instalado un certificado que el dispositivo del usuario final reconozca. En las pruebas y las pruebas de conceptos, se pueden usar certificados autofirmados, pero en todo entorno de producción solo deben usarse certificados de confianza pública emitidos por una entidad emisora de certificados.
2. **Autenticar al usuario en el entorno**: Puerta de enlace de Escritorio remoto usa el servicio IIS de bandeja de entrada para la autenticación e, incluso, puede usar el protocolo RADIUS para aprovechar soluciones de [autenticación multifactor](rds-plan-mfa.md), como Azure MFA. Aparte de las directivas predeterminadas creadas, puedes crear directivas de administración de recursos de Escritorio remoto (RAP de RD) y directivas de autorización de conexiones de Escritorio remoto (CAP de RD) adicionales para definir más específicamente qué usuarios deben tener acceso a qué recursos dentro del entorno seguro.
3. **Pasar el tráfico entre el dispositivo del usuario final y el recurso especificado**: Puerta de enlace de Escritorio remoto continúa realizando esta tarea siempre que se mantenga la conexión. Puedes especificar distintas propiedades de tiempo de espera en los servidores de puerta de enlace de Escritorio remoto para mantener la seguridad del entorno en caso de que el usuario se aleje del dispositivo.

Puedes encontrar detalles adicionales sobre la arquitectura general de una implementación de Servicios de Escritorio remoto [en la arquitectura de referencia de hospedaje de escritorio](desktop-hosting-reference-architecture.md).