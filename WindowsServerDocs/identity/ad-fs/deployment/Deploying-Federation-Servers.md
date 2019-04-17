---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: "Implementación de los servidores de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-servers"></a>Implementación de los servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implementar los servidores de federación de los servicios de federación de Active Directory \(AD FS\), complete cada una de las tareas en [lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Al usar esta lista de comprobación, te recomendamos que leas primero las referencias a la planificación de servidor de federación en la [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de comenzar los procedimientos para configurar los servidores. La lista de comprobación de este modo proporciona una mejor comprensión del proceso de diseño e implementación de los servidores de federación.  
  
## <a name="about-federation-servers"></a>Acerca de los servidores de federación  
Los servidores de federación son equipos que ejecutan Windows Server 2008 con el software de AD FS instalado que se han configurado para el rol de servidor de federación de act. Los servidores de federación autentican o enrutan las solicitudes de cuentas de usuario en otras organizaciones y de los equipos cliente que se pueden ubicados en cualquier lugar en Internet.  
  
El act de instalar el software de AD FS en un equipo y usar al Asistente para configuración del servidor de federación de AD FS a configurarla para el rol de servidor de federación lo convierte en un servidor de federación. También facilita la administración de AD FS en snap\ disponibles en el equipo en el **Start\\Administrative Tools\\** menú para que pueda especificar lo siguiente:  
  
-   El nombre de host de AD FS donde aplicaciones y las organizaciones asociadas envía solicitudes de token y respuestas  
  
-   El identificador de AD FS que las organizaciones asociadas y las aplicaciones se usa para identificar el nombre único o la ubicación de la organización  
  
-   El certificado de firma token\ que todos los servidores de federación en una granja de servidores usará para el problema e inicia sesión tokens  
  
-   La ubicación de personalizada ASP.NET páginas Web de inicio de sesión del cliente, el cierre de sesión y detección de asociado de la cuenta que mejorará la experiencia del cliente  
  
    > [!NOTE]  
    > La mayoría de estas opciones de \(UI\) de interfaz de usuario principal se encuentran en el archivo web.config en cada servidor de federación. El nombre de host de AD FS y los valores de identificador de AD FS no se especifican en el archivo web.config.  
  
Un motor de emisión de notificaciones que emite tokens basados en las credenciales de host de los servidores de federación \ (por ejemplo, nombre de usuario y password\) que se le presentan. Un token de seguridad es una unidad de datos firmada mediante cifrado que expresa una o más notificaciones. Una notificación es una declaración que sea un servidor \ (por ejemplo, nombre, identidad, clave, grupo, privilegios o capability\) acerca de un cliente. Después de comprobar las credenciales en el servidor de federación \ (a través de la process\ de inicio de sesión del usuario), se recopilan notificaciones para el usuario mediante el examen de los atributos de usuario que se almacenan en la tienda de atributo especificado.  
  
En federados Web Single\-Sign\-On \(SSO\) diseña \ (en el que dos o más organizaciones están involved\ los diseños de AD FS), reclamaciones pueden modificarse según las reglas de notificación para una parte específica de usuario de confianza. Las notificaciones están integradas en un token que se envía a un servidor de federación de la organización de partner de recurso. Después de que un servidor de federación de asociado de recurso recibe las notificaciones como notificaciones entrantes, ejecuta el motor de emisión de notificaciones para ejecutar un conjunto de reglas de notificación de filtro, pasa a través de, o transformar esas notificaciones. Las notificaciones están integradas, a continuación, en un nuevo token que se envía al servidor Web en el partner de recursos.  
  
En el diseño Web SSO \ (un diseño de AD FS en el que solo una organización es involved\), un servidor de federación solo puede usarse para que los empleados puedan iniciar sesión una vez y seguir teniendo acceso a varias aplicaciones.  
  
