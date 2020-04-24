---
title: Licencia para la implementación de RDS con licencias de acceso de cliente (CAL)
description: Información general sobre licencias de cliente en Servicios de Escritorio remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 02/12/2020
manager: dongill
ms.openlocfilehash: 295536afc77d0559fd7d2d4a22f555231a1aab75
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80858078"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Licencia para la implementación de RDS con licencias de acceso de cliente (CAL)

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Cada usuario y dispositivo que se conecta a un host de sesión de Escritorio remoto necesita una licencia de acceso de cliente (CAL). Las licencias de Escritorio remoto se usan para instalar, emitir y realizar un seguimiento de las CAL de RDS.  

Cuando un usuario o un dispositivo se conecta a un servidor host de sesión de Escritorio remoto, el servidor host de sesión de Escritorio remoto determina si es necesaria una CAL de RDS. Luego, el servidor host de sesión de Escritorio remoto solicita una CAL de RDS al servidor de licencias de Escritorio remoto. Si hay disponible una CAL de RDS adecuada en un servidor de licencias, se emite esta CAL de RDS para el cliente, y el cliente puede conectarse al servidor host de sesión de Escritorio remoto y, desde allí, al escritorio o las aplicaciones que intenta usar.

Aunque hay un período de gracia de la licencia durante el que no se necesita ningún servidor de licencias, después de que el período de gracia finaliza, los clientes deben tener una CAL de RDS válida emitida por un servidor de licencias para poder iniciar sesión en un servidor host de sesión de Escritorio remoto.

Para saber cómo funcionan las licencias de acceso de cliente en servicios de Escritorio remoto y para implementar y administrar las licencias, usa la siguiente información:

- [Licencia para la implementación de RDS con licencias de acceso de cliente (CAL)](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [Descripción del modelo de CAL de RDS](#understanding-the-rds-cal-model)
  - [Compatibilidad de la versión de CAL de RDS](#rds-cal-version-compatibility)

## <a name="understanding-the-rds-cal-model"></a>Descripción del modelo de CAL de RDS

Hay dos tipos de CAL de RDS:

- CAL de RDS por dispositivo
- CAL de RDS por usuario

En la tabla siguiente se describen las diferencias entre los dos tipos de CAL:

| Por dispositivo                                                     | Por usuario                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Las CAL de RDS se asignan físicamente a cada dispositivo.                   | Las CAL de RDS se asignan a un usuario en Active Directory.                                 |
| El servidor de licencias realiza un seguimiento de las CAL de RDS.                        | El servidor de licencias realiza un seguimiento de las CAL de RDS.                                          |
| El seguimiento de las CAL de RDS puede hacerse independientemente de la pertenencia a Active Directory. | No se hace ningún seguimiento de las CAL de RDS dentro de un grupo de trabajo.                                       |
| Puedes revocar hasta un 20 % de las CAL de RDS.                              | No puedes revocar las CAL de RDS.                                                      |
| Las CAL de RDS temporales son válidas entre 52 y 89 días.                       | Las CAL de RDS temporales no están disponibles.                                                |
| Las CAL de RDS no pueden sobreasignarse.                                  | Las CAL de RDS pueden sobreasignarse (en incumplimiento del contrato de licencia de Escritorio remoto). |

Cuando se usa el modelo por dispositivo, se emite una licencia temporal la primera vez que un dispositivo se conecta al host de sesión de Escritorio remoto. La segunda vez que el dispositivo se conecta, siempre que el servidor de licencias esté activado y haya CAL de RDS disponibles, el servidor de licencias emite una CAL por dispositivo de RDS permanente.

Cuando se usa el modelo por usuario, no se exige la concesión de licencias y se concede una licencia a cada usuario para que se conecte a un host de sesión de Escritorio remoto desde cualquier cantidad de dispositivos. El servidor de licencias emite licencias desde el grupo de CAL de RDS disponible o el grupo de CAL de RDS usado en exceso. Es tu responsabilidad asegurarte de que todos los usuarios tengan una licencia válida y ninguna CAL usada en exceso. En caso contrario, estarás en infracción de los términos de licencia de los Servicios de Escritorio remoto.

Para asegurarte de que cumples con los términos de licencia de los Servicios de Escritorio remoto, haz un seguimiento del número de CAL por usuario de RDS en tu organización y asegúrate de tener suficientes instaladas en el servidor de licencias para todos los usuarios.

Puedes usar el Administrador de licencias de Escritorio remoto para el seguimiento y generar informes de CAL de RDS por usuario.

## <a name="rds-cal-version-compatibility"></a>Compatibilidad de la versión de CAL de RDS

La CAL de RDS que usan tus usuarios o dispositivos debe ser compatible con la versión de Windows Server a la que se conecta el usuario o dispositivo. No puedes usar CAL de RDS para versiones anteriores a fin de acceder a versiones posteriores de Windows Server, pero sí que puedes usar versiones posteriores de CAL de RDS para acceder a versiones anteriores de Windows Server. Por ejemplo, se necesita una CAL de RDS 2016 o una versión posterior para conectarse a un host de sesión de Escritorio remoto de Windows Server 2016, pero se requiere una CAL de RDS 2012 o posterior para conectarse a un host de sesión de Escritorio remoto de Windows Server 2012 R2.

En la tabla siguiente se muestran las versiones de CAL de RDS y de host de sesión de Escritorio remoto que son compatibles entre sí.

|                  | CAL de RDS 2008 R2 y versiones anteriores | CAL DE RDS 2012 | CAL DE RDS 2016 | CAL DE RDS 2019 |
|---------------------------------|--------|--------|--------|--------|
| **Host de sesión 2008 y 2008 R2** | Sí    | Sí    | Sí    | Sí     |
| **Host de sesión 2012**         | No     | Sí    | Sí    | Sí    |
| **Host de sesión 2012 R2**      | No     | Sí    | Sí    | Sí    |
| **Host de sesión 2016**         | No     | No     | Sí    | Sí    |
| **Host de sesión 2019**         | No     | No     | No     | Sí    |

Debes instalar tu CAL de RDS en un servidor de licencias de Escritorio remoto compatible. Cualquier servidor de licencias de RDS puede hospedar las licencias de todas las versiones anteriores de los Servicios de Escritorio remoto y la versión actual de los Servicios de Escritorio remoto. Por ejemplo, un servidor de licencias de RDS para Windows Server 2016 puede hospedar licencias de todas las versiones anteriores de RDS, mientras que un servidor de licencias de RDS para Windows Server 2012 R2 solo puede hospedar licencias hasta Windows Server 2012 R2.

En la tabla siguiente se muestran las versiones de CAL de RDS y de servidor de licencias que son compatibles entre sí.

|                  | CAL de RDS 2008 R2 y versiones anteriores | CAL DE RDS 2012 | CAL DE RDS 2016 | CAL DE RDS 2019 |
|---------------------------------|--------|--------|--------|--------|
| **Servidor de licencias para 2008, 2008 R2** | Sí    | No   | No   | No    |
| **Servidor de licencias para 2012**         | Sí     | Sí    | No   | No    |
| **Servidor de licencias para 2012 R2**      | Sí     | Sí    | No   | No    |
| **Servidor de licencias para 2016**         | Sí     | Sí    | Sí   | No    |
| **Servidor de licencias para 2019**         | Sí     | Sí    | Sí  | Sí   |
