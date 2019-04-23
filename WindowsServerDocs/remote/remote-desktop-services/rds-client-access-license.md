---
title: Licencia de su implementación de RDS con licencias de acceso de cliente (CAL)
description: Información general sobre licencias de servicios de escritorio remoto de cliente.
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
ms.openlocfilehash: 6648a52bb4d09725935a2197d6ce6fa6d8cc74a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853446"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Licencia de su implementación de RDS con licencias de acceso de cliente (CAL)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cada usuario y el dispositivo que se conecta a un host de sesión de escritorio remoto necesitan un licencias de acceso de cliente (CAL). Use licencias de escritorio remoto para instalar, emitir y realizar un seguimiento de CAL de RDS.  

Cuando un usuario o un dispositivo se conecta a un servidor Host de sesión de escritorio remoto, el servidor Host de sesión de escritorio remoto determina si es necesaria una CAL de RDS. El servidor Host de sesión de escritorio remoto, a continuación, solicita una CAL de RDS desde el servidor de licencias de escritorio remoto. Si una CAL de RDS adecuada está disponible en un servidor de licencias, la CAL de RDS se emite al cliente y el cliente es capaz de conectarse al servidor Host de sesión de escritorio remoto y desde allí para el escritorio o las aplicaciones que se está intentando usar.

Aunque no hay un período de gracia durante qué ninguna licencia de servidor es necesario, tras el período de gracia finalice, los clientes debe tener una CAL de RDS válido emitido por un servidor de licencias antes de puede iniciar sesión en un servidor Host de sesión de escritorio remoto.

Para obtener información acerca del funcionan de las licencias de acceso de cliente de servicios de escritorio remoto y para implementar y administrar las licencias, use la siguiente información:

- [Entender el modelo CAL](#understanding-the-cals-model)
- [Activar el servidor de licencias](rds-activate-license-server.md)
- [Instalar CAL de RDS en el servidor de licencias](rds-install-cals.md)
- [Seguimiento de CAL utilizadas en la implementación](rds-track-cals.md)

## <a name="understanding-the-cals-model"></a>Descripción del modelo de CAL

Hay dos tipos de CAL:

- RDS CAL por dispositivo
- RDS CAL por usuario

En la tabla siguiente se describe las diferencias entre los dos tipos de CAL:

| Por dispositivo                                                     | Por usuario                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| CAL físicamente se asignan a cada dispositivo.                   | CAL se asignan a un usuario en Active Directory.                                 |
| Se realiza un seguimiento de CAL del servidor de licencias.                        | Se realiza un seguimiento de CAL del servidor de licencias.                                          |
| Pueden realizar el seguimiento de CAL, independientemente de la pertenencia a Active Directory. | No realizar el seguimiento de CAL dentro de un grupo de trabajo.                                       |
| Puede revocar hasta un 20% de las CAL.                              | No se puede revocar las CAL.                                                      |
| Licencias CAL temporales son válidas para 52 a 89 días.                       | Licencias CAL temporales no están disponibles.                                                |
| No pueden estar sobreasignadas CAL.                                  | CAL pueden estar sobreasignadas (en incumplimiento de contrato de licencia de escritorio remoto). |

Cuando se usa el modelo por dispositivo, una licencia temporal se emite la primera vez que se conecta un dispositivo para el Host de sesión de escritorio remoto. La segunda vez que el dispositivo se conecta, siempre que el servidor de licencias está activado y hay CAL disponibles, los problemas del servidor de licencias una CAL de RDS por dispositivo permanente.

Cuando se usa el modelo por usuario, no se aplica la licencia y se concede a cada usuario una licencia para conectarse a un Host de sesión de escritorio remoto desde cualquier número de dispositivos. El servidor de licencias emite licencias desde el grupo de CAL disponible o el grupo de CAL Over-Used. Es responsabilidad suya asegurarse de que todos los usuarios tienen una licencia válida y cero Over-Used CAL; en caso contrario, se encuentra en la infracción de los términos de licencia de servicios de escritorio remoto.

Para asegurarse de que cumple con los términos de licencia de servicios de escritorio remoto, vía el número de CAL por usuario de RDS en su organización y asegúrese de tener un suficientes CAL por usuario instalada en el servidor de licencias para todos los usuarios.

Puede usar el Administrador de licencias de escritorio remoto para realizar un seguimiento y generar informes de CAL por usuario de RDS.

## <a name="note-about-cal-versions"></a>Tenga en cuenta acerca de las versiones de CAL

La CAL utilizada por los usuarios o dispositivos debe corresponder a la versión de Windows Server que se está conectando el usuario o dispositivo. No se puede utilizar CAL anteriores para tener acceso a las versiones más recientes de Windows Server, pero puede utilizar CAL más recientes para tener acceso a versiones anteriores de Windows Server.

La siguiente tabla muestra las CAL que son compatibles en Hosts de sesión de escritorio remoto y los Hosts de virtualización de escritorio remoto.

|                  |2008 R2 y versiones anterior CAL|2012 CAL|2016 CAL|2019 CAL|
|---------------------------------|--------|--------|--------|--------|
| **Servidor de licencias de 2008, 2008 R2**| Sí    | No     | No     | No     |
| **servidor de licencias de 2012**         | Sí    | Sí    | No     | No     |
| **Servidor de licencias de 2012 R2**      | Sí    | Sí    | No     | No     |
| **servidor de licencias de 2016**         | Sí    | Sí    | Sí    | No     |
| **servidor de licencias de 2019**         | Sí    | Sí    | Sí    | Sí    |

Cualquier servidor de licencias RDS puede hospedar las licencias de todas las versiones anteriores de servicios de escritorio remoto y la versión actual de servicios de escritorio remoto. Por ejemplo, un servidor de licencias de RDS de Windows Server 2016 puede hospedar las licencias de todas las versiones anteriores de RDS, mientras que un servidor de licencias de Windows Server 2012 R2 RDS solo puede hospedar licencias hasta Windows Server 2012 R2.
