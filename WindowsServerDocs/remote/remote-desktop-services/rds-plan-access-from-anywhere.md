---
title: Servicios de escritorio remoto - acceso desde cualquier lugar
description: Información de planificación de una puerta de enlace de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2b10428ff90c50c4d7e113552ddda3cca5447b06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845836"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Servicios de escritorio remoto - acceso desde cualquier lugar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los usuarios finales pueden conectarse a los recursos de red interna segura desde fuera del firewall corporativo a través de la puerta de enlace de escritorio remoto.

Independientemente de cómo configurar los equipos de escritorio para los usuarios finales, puede conectar fácilmente la puerta de enlace de escritorio remoto en el flujo de conexión para una conexión rápida y segura. Para los usuarios finales de la conexión a través de las fuentes de distribución publicados, puede configurar la propiedad de la puerta de enlace de escritorio remoto después de configurar las propiedades de implementación general. Para conectarse a través a sus escritorios sin una fuente de los usuarios finales, puede agregar fácilmente el nombre de puerta de enlace de escritorio remoto de la organización como una propiedad de conexión, independientemente de la aplicación de cliente de escritorio remoto que usan.

Los tres principales objetivos de la puerta de enlace de escritorio remoto, en el orden de la secuencia de conexión son:
1. **Establecer un túnel SSL cifrado entre el dispositivo del usuario final y el servidor de puerta de enlace de escritorio remoto**: Para poder conectarse a través de cualquier servidor de puerta de enlace de escritorio remoto, el servidor de puerta de enlace de escritorio remoto debe tener instalado un certificado que reconoce el dispositivo del usuario final. En las pruebas y las pruebas de conceptos, se pueden usar certificados autofirmados, pero solo certificados de confianza pública desde una entidad emisora de certificados se deben usar en cualquier entorno de producción.
2. **Autenticar al usuario en el entorno**: La puerta de enlace de escritorio remoto utiliza la Bandeja de entrada de servicio para realizar la autenticación IIS e incluso puede usar el protocolo RADIUS para aprovechar [autenticación multifactor](rds-plan-mfa.md) soluciones como Azure MFA. Aparte de las directivas predeterminadas que se creó, puede crear directivas de autorización de recursos de escritorio remoto (RAP de RD) y directivas de autorización de conexión a Escritorio remoto (CAP de RD) adicionales para definir más específicamente qué usuarios deben tener acceso a qué recursos dentro de los seguros entorno.
3. **Pasar el tráfico entre el dispositivo del usuario final y el recurso especificado y hacia atrás**: Continúa la puerta de enlace de escritorio remoto realizar esta tarea para siempre y cuando se establece la conexión. Puede especificar las propiedades de tiempo de espera diferente en los servidores de puerta de enlace de escritorio remoto para mantener la seguridad del entorno en caso de que el usuario aleja el dispositivo.

Puede encontrar detalles adicionales sobre la arquitectura general de una implementación de servicios de escritorio remoto [en la arquitectura de referencia de hospedaje de escritorio](desktop-hosting-reference-architecture.md).