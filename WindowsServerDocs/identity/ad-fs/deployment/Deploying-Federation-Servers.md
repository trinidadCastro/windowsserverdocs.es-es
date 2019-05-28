---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Implementación de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1f3b2e1ce901df1df1a232dfba51c292c8e1c29c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192182"
---
# <a name="deploying-federation-servers"></a>Implementación de servidores de federación

Para implementar servidores de federación de Active Directory Federation Services \(AD FS\), realice cada una de las tareas de [lista de comprobación: Configuración de un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Cuando se usa esta lista de comprobación, se recomienda que lea primero las referencias al planeamiento del servidor de federación en el [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de comenzar los procedimientos para configurar los servidores. La lista de comprobación de este modo proporciona una mejor comprensión del proceso de diseño e implementación de servidores de federación.  
  
## <a name="about-federation-servers"></a>Acerca de los servidores de federación  
Los servidores de federación son equipos que ejecutan Windows Server 2008 con el software AD FS instalado que se han configurado para actuar en el rol de servidor de federación. Los servidores de federación autentican o enrutan las solicitudes de cuentas de usuario de otras organizaciones y de los equipos cliente que pueden encontrarse en cualquier lugar en Internet.  
  
El hecho de instalar el software AD FS en un equipo y usar al Asistente para configuración del servidor de federación de AD FS para configurarlo para el rol de servidor de federación lo convierte en un servidor de federación. También hace que el complemento de administración de AD FS\-en disponibles en el equipo en el **iniciar\\herramientas administrativas\\**  menú para que pueda especificar lo siguiente:  
  
-   El nombre de host de AD FS que las organizaciones asociadas y las aplicaciones enviará las solicitudes de token y respuestas  
  
-   El identificador de AD FS que las organizaciones asociadas y las aplicaciones se usará para identificar el nombre único o la ubicación de su organización  
  
-   El token\-firma de certificado que va a usar todos los servidores de federación en una granja de servidores a los tokens de problema y el inicio de sesión  
  
-   La ubicación de las páginas Web de ASP.NET personalizadas para el inicio de sesión de cliente, cierre de sesión y detección de asociado de cuenta que mejorará la experiencia del cliente  
  
    > [!NOTE]  
    > La mayoría de estos principales de interfaz de usuario \(UI\) configuración está contenida en el archivo web.config en cada servidor de federación. El nombre de host de AD FS y los valores de identificador de AD FS no se especifican en el archivo web.config.  
  
Un motor de emisión de notificaciones que emite tokens basados en las credenciales de host de los servidores de federación \(por ejemplo, nombre de usuario y contraseña\) que se presentan a él. Un token de seguridad es una unidad de datos firmada mediante cifrado que expresa una o más notificaciones. Una notificación es una instrucción que hace que un servidor \(por ejemplo, nombre, identidad, clave, grupo, privilegio o capacidad\) acerca de un cliente. Una vez comprobadas las credenciales en el servidor de federación \(a través del proceso de inicio de sesión de usuario\), notificaciones para el usuario se recopilan mediante el examen de los atributos de usuario que se almacenan en el almacén de atributos especificado.  
  
En Web único federado\-inicio de sesión\-en \(SSO\) diseños \(diseños AD FS en la que intervienen dos o más organizaciones\), las notificaciones se pueden modificar mediante reglas de notificación para un usuario de confianza específico la entidad. Las notificaciones están integradas en un símbolo (token) que se envía a un servidor de federación de la organización del asociado de recurso. Una vez que un servidor de federación del asociado de recurso recibe las notificaciones como las notificaciones entrantes, ejecuta el motor de emisión de notificaciones para ejecutar un conjunto de reglas de notificación para filtrar, transformas o pasas por dichas notificaciones. Las notificaciones están integradas, a continuación, en un nuevo token que se envía al servidor Web del asociado de recurso.  
  
En el diseño SSO Web \(un diseño de AD FS en la que está implicada solo una organización\), un servidor de federación único puede usarse para que los empleados pueden iniciar sesión una vez y seguir teniendo acceso a varias aplicaciones.  
  
