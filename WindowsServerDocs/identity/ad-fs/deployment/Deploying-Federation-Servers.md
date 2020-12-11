---
description: Más información acerca de cómo implementar servidores de Federación
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Implementación de servidores de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: d8342fe7e884c856bf687a0deda6b174b94355b9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044943"
---
# <a name="deploying-federation-servers"></a>Implementación de servidores de federación

Para implementar servidores de Federación en Servicios de federación de Active Directory (AD FS) \( AD FS \) , complete cada una de las tareas de la [lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md).

> [!NOTE]
> Cuando use esta lista de comprobación, recomendamos que lea primero las referencias al planeamiento del servidor de Federación en la [Guía de diseño de AD FS en Windows server 2012](../design/ad-fs-design-guide-in-windows-server-2012.md) antes de comenzar con los procedimientos para configurar los servidores. Al seguir la lista de comprobación de esta manera, se entiende mejor el proceso de diseño e implementación de los servidores de Federación.

## <a name="about-federation-servers"></a>Acerca de los servidores de Federación
Los servidores de Federación son equipos que ejecutan Windows Server 2008 con el software AD FS instalado que se han configurado para actuar en el rol de servidor de Federación. Los servidores de Federación autentican o enrutan las solicitudes de las cuentas de usuario de otras organizaciones y de los equipos cliente que se pueden encontrar en cualquier parte de Internet.

La acción de instalar el software de AD FS en un equipo y usar el Asistente para la configuración del servidor de Federación de AD FS para configurarlo para el rol de servidor de Federación hace que ese equipo sea un servidor de Federación. También hace que el complemento de administración de AD FS \- esté disponible en ese equipo en el menú **iniciar \\ herramientas \\ administrativas** para que pueda especificar lo siguiente:

-   El nombre de host AD FS en el que las aplicaciones y las organizaciones asociadas enviarán solicitudes y respuestas de token

-   El identificador AD FS que las organizaciones y aplicaciones asociadas usarán para identificar el nombre único o la ubicación de la organización.

-   El certificado de firma de tokens \- que todos los servidores de Federación de una granja de servidores usarán para emitir y firmar tokens.

-   La ubicación de las páginas web de ASP.NET personalizadas para el inicio de sesión, la cierre de sesión y la detección de asociado de cuenta para mejorar la experiencia del cliente

    > [!NOTE]
    > La mayoría de estas opciones de interfaz de usuario principales \( \) se encuentran en el archivo web.config en cada servidor de Federación. El nombre de host de AD FS y los valores de identificador de AD FS no se especifican en el archivo web.config.

Los servidores de Federación hospedan un motor de emisión de notificaciones que emite tokens basados en las credenciales \( , por ejemplo, el nombre de usuario y la contraseña \) que se le presentan. Un token de seguridad es una unidad de datos con firma criptográfica que expresa una o más notificaciones. Una demanda es una instrucción que un servidor \( de, por ejemplo, nombre, identidad, clave, grupo, privilegio o capacidad de \) un cliente. Una vez comprobadas las credenciales en el servidor de Federación \( a través del proceso de inicio de sesión de usuario \) , se recopilan las notificaciones del usuario mediante el examen de los atributos de usuario que se almacenan en el almacén de atributos especificado.

En el inicio de sesión único Web federado de \- \- diseños de \( SSO \) \( AD FS diseños en los que hay dos o más organizaciones implicadas \) , las notificaciones pueden ser modificadas por reglas de notificación para un usuario de confianza específico. Las notificaciones se integran en un token que se envía a un servidor de Federación en la organización del asociado de recurso. Después de que un servidor de Federación del asociado de recurso reciba las notificaciones como notificaciones entrantes, ejecuta el motor de emisión de notificaciones para ejecutar un conjunto de reglas de notificación para filtrar, pasar o transformar esas notificaciones. Las notificaciones se compilan en un nuevo token que se envía al servidor Web en el asociado de recurso.

En el diseño de SSO Web \( , un diseño de AD FS en el que solo hay una organización implicada \) , se puede usar un solo servidor de Federación para que los empleados puedan iniciar sesión una vez y sigan teniendo acceso a varias aplicaciones.

