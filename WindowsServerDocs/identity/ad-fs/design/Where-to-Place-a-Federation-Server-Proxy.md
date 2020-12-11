---
description: Más información acerca de cómo colocar un servidor proxy de Federación
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Dónde colocar a un servidor proxy de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 734ac147176d7650c068e8920b4c704cdb936ab5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042053"
---
# <a name="where-to-place-a-federation-server-proxy"></a>Dónde colocar a un servidor proxy de federación

Puede colocar Servicios de federación de Active Directory (AD FS) \( \) servidores proxy de federación de AD FS en una red perimetral para proporcionar un nivel de protección contra usuarios malintencionados que pueden provenir de Internet. Los servidores proxy de federación son ideales para el entorno de red perimetral porque no tienen acceso a las claves privadas que se usan para crear tokens. Sin embargo, los servidores proxy de Federación pueden enrutar de forma eficaz las solicitudes entrantes a los servidores de Federación que están autorizados para producir esos tokens.

No es necesario colocar un servidor proxy de Federación dentro de la red corporativa para el asociado de cuenta o el asociado de recurso porque los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de Federación. En este escenario, el servidor de Federación también proporciona la funcionalidad de servidor proxy de Federación para equipos cliente que proceden de la red corporativa.

Como es habitual con las redes perimetrales, \- se establece un firewall con conexión a Intranet entre la red perimetral y la red corporativa, y \- a menudo se establece un firewall con conexión a Internet entre la red perimetral e Internet. En este escenario, el servidor proxy de Federación se encuentra entre ambos firewalls en la red perimetral.

## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configuración de los servidores de firewall para un servidor proxy de federación
Para que el proceso de redirección de servidor proxy de Federación se realice correctamente, todos los servidores de Firewall deben estar configurados para permitir el tráfico HTTPS del protocolo seguro de transferencia de hipertexto \( \) . El uso de HTTPS es necesario porque los servidores de Firewall deben publicar el servidor proxy de Federación mediante el puerto 443, de modo que el servidor proxy de Federación de la red perimetral pueda obtener acceso al servidor de Federación de la red corporativa.

> [!NOTE]
> Todas las comunicaciones a y desde los equipos cliente también ocurren a través de HTTPS.

Además, el servidor de \- firewall con conexión a Internet, como un equipo que ejecuta Microsoft Internet Security and Acceleration \( ISA \) Server, usa un proceso conocido como publicación de servidor para distribuir las solicitudes de cliente de Internet a los servidores de red perimetral y corporativa adecuados, como los servidores proxy de Federación o los servidores de Federación.

Las reglas de publicación de servidor determinan el funcionamiento de la publicación de servidor, básicamente, filtrando todas las solicitudes entrantes y salientes a través del equipo que ejecuta ISA Server. Las reglas de publicación de servidor asignan las solicitudes de cliente entrantes a los servidores adecuados que hay detrás del equipo que ejecuta ISA Server. Para obtener información sobre cómo configurar ISA Server para publicar un servidor, vea [crear una regla de publicación web segura](https://go.microsoft.com/fwlink/?LinkId=75182).

En el mundo federado de AD FS, estas solicitudes de cliente suelen realizarse en una dirección URL específica, por ejemplo, una dirección URL de identificador de servidor de Federación como http: \/ /FS.fabrikam.com. Dado que estas solicitudes de cliente provienen de Internet, el \- servidor de firewall con conexión a Internet debe estar configurado para publicar la dirección URL del identificador del servidor de Federación para cada servidor proxy de Federación implementado en la red perimetral.

### <a name="configuring-isa-server-to-allow-ssl"></a>Configuración de ISA Server para permitir SSL
Para facilitar las comunicaciones seguras AD FS, debe configurar ISA Server para permitir \( \) las comunicaciones de capa de sockets seguros SSL entre lo siguiente:

-   **Servidores de Federación y proxies de servidor de Federación.** Se requiere un canal SSL para todas las comunicaciones entre los servidores de Federación y los proxies de servidor de Federación. Por lo tanto, debe configurar ISA Server para que permita una conexión SSL entre la red corporativa y la red perimetral.

-   **Equipos cliente, servidores de Federación y servidores proxy de Federación.** Para que se puedan producir comunicaciones entre los equipos cliente y los servidores de Federación, o entre los equipos cliente y los servidores proxy de Federación, puede colocar un equipo que ejecute ISA Server delante del servidor de Federación o del servidor proxy de Federación.

    Si su organización realiza la autenticación de cliente SSL en el servidor de Federación o en el servidor proxy de Federación, al colocar un equipo que ejecuta ISA Server delante del servidor de Federación o el servidor proxy de Federación, el servidor debe estar configurado para pasar \- a través de la conexión SSL porque la conexión SSL debe terminar en el servidor de Federación o en el servidor proxy de Federación.

    Si su organización no realiza la autenticación de cliente SSL en el servidor de Federación o en el servidor proxy de Federación, una opción adicional es terminar la conexión SSL en el equipo que ejecuta ISA Server y, a continuación, volver a \- establecer una conexión SSL con el servidor de Federación o el servidor proxy de Federación.

> [!NOTE]
> El servidor de Federación o el proxy de servidor de Federación requiere que SSL proteja la conexión para proteger el contenido del token de seguridad.

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
