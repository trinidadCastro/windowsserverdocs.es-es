---
title: Licencia para la implementación de RDS con licencias de acceso de cliente (CAL)
description: Información general sobre licencias de cliente en Servicios de Escritorio remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 09/20/2018
manager: dongill
ms.openlocfilehash: 0254c03396cba69a86eed021319ca2e2483ca625
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743866"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Licencia para la implementación de RDS con licencias de acceso de cliente (CAL)

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Cada usuario y dispositivo que se conecta a un host de sesión de Escritorio remoto necesita una licencia de acceso de cliente (CAL). Las licencias de Escritorio remoto se usan para instalar, emitir y realizar un seguimiento de las CAL de RDS.  

Cuando un usuario o un dispositivo se conecta a un servidor host de sesión de Escritorio remoto, el servidor host de sesión de Escritorio remoto determina si es necesaria una CAL de RDS. Luego, el servidor host de sesión de Escritorio remoto solicita una CAL de RDS al servidor de licencias de Escritorio remoto. Si hay disponible una CAL de RDS adecuada en un servidor de licencias, se emite esta CAL de RDS para el cliente, y el cliente puede conectarse al servidor host de sesión de Escritorio remoto y, desde allí, al escritorio o las aplicaciones que intenta usar.

Aunque hay un período de gracia de la licencia durante el que no se necesita ningún servidor de licencias, después de que el período de gracia finaliza, los clientes deben tener una CAL de RDS válida emitida por un servidor de licencias para poder iniciar sesión en un servidor host de sesión de Escritorio remoto.

Para saber cómo funcionan las licencias de acceso de cliente en servicios de Escritorio remoto y para implementar y administrar las licencias, usa la siguiente información:

- [Licencia para la implementación de RDS con licencias de acceso de cliente (CAL)](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [Descripción del modelo de CAL](#understanding-the-cals-model)
  - [Nota sobre las versiones de CAL](#note-about-cal-versions)

## <a name="understanding-the-cals-model"></a>Descripción del modelo de CAL

Existen dos tipos de CAL:

- CAL de RDS por dispositivo
- CAL de RDS por usuario

En la tabla siguiente se describen las diferencias entre los dos tipos de CAL:

| Por dispositivo                                                     | Por usuario                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Las CAL se asignan físicamente a cada dispositivo.                   | Las CAL se asignan a un usuario en Active Directory.                                 |
| El seguimiento de las CAL se hace por servidor de licencias.                        | El seguimiento de las CAL se hace por servidor de licencias.                                          |
| El seguimiento de las CAL puede hacerse independientemente de la pertenencia a Active Directory. | No se hace un seguimiento de las CAL dentro de un grupo de trabajo.                                       |
| Puedes revocar hasta un 20 % de las CAL.                              | No puedes revocar las CAL.                                                      |
| Las CAL temporales son válidas durante 52 a 89 días.                       | Las CAL temporales no están disponibles.                                                |
| Las CAL no pueden sobreasignarse.                                  | Las CAL pueden sobreasignarse (en incumplimiento del contrato de licencia de Escritorio remoto). |

Cuando se usa el modelo por dispositivo, se emite una licencia temporal la primera vez que un dispositivo se conecta al host de sesión de Escritorio remoto. La segunda vez que el dispositivo se conecta, siempre que el servidor de licencias esté activado y haya CAL disponibles, el servidor de licencias emite una CAL permanente de RDS por dispositivo.

Cuando se usa el modelo por usuario, no se exige la concesión de licencias y se concede una licencia a cada usuario para que se conecte a un host de sesión de Escritorio remoto desde cualquier cantidad de dispositivos. El servidor de licencias emite licencias desde el grupo de CAL disponible o el grupo de CAL Over-Used. Es tu responsabilidad asegurarte de que todos los usuarios tengan una licencia válida y ninguna CAL Over-Used; en caso contrario, estarás en infracción de los términos de licencia de los Servicios de escritorio remoto.

Para asegurarte de que cumples con los términos de licencia de los Servicios de escritorio remoto, haz un seguimiento del número de CAL de RDS por usuario en tu organización y asegúrate de tener suficientes CAL por usuario instaladas en el servidor de licencias para todos los usuarios.

Puedes usar el Administrador de licencias de Escritorio remoto para el seguimiento y generar informes de CAL de RDS por usuario.

## <a name="note-about-cal-versions"></a>Nota sobre las versiones de CAL

La CAL usada por los usuarios o dispositivos debe corresponder a la versión de Windows Server a la que se conecta el usuario o dispositivo. No puedes usar CAL anteriores para acceder a versiones más recientes de Windows Server, pero puedes usar CAL más recientes para acceder a versiones anteriores de Windows Server.

En la siguiente tabla se muestran las CAL que son compatibles con los hosts de sesión de Escritorio remoto y los hosts de virtualización de Escritorio remoto.

|                  |CAL para 2008 R2 y versiones anteriores|CAL para 2012|CAL para 2016|CAL para 2019|
|---------------------------------|--------|--------|--------|--------|
| **Servidor de licencias para 2008, 2008 R2**| Sí    | No     | No     | No     |
| **Servidor de licencias para 2012**         | Sí    | Sí    | No     | No     |
| **Servidor de licencias para 2012 R2**      | Sí    | Sí    | No     | No     |
| **Servidor de licencias para 2016**         | Sí    | Sí    | Sí    | No     |
| **Servidor de licencias para 2019**         | Sí    | Sí    | Sí    | Sí    |

Cualquier servidor de licencias de RDS puede hospedar las licencias de todas las versiones anteriores de los Servicios de Escritorio remoto y la versión actual de los Servicios de Escritorio remoto. Por ejemplo, un servidor de licencias de RDS para Windows Server 2016 puede hospedar licencias de todas las versiones anteriores de RDS, mientras que un servidor de licencias de RDS para Windows Server 2012 R2 solo puede hospedar licencias hasta Windows Server 2012 R2.
