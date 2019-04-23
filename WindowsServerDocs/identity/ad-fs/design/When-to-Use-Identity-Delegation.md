---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: Cuándo se debe usar la delegación de identidad
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872556"
---
# <a name="when-to-use-identity-delegation"></a>Cuándo se debe usar la delegación de identidad

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>¿Qué es la delegación de identidad?  
Delegación de identidad es una característica de Active Directory Federation Services \(AD FS\) que permite administrador\-especificó cuentas para suplantar a los usuarios. La cuenta que suplanta al usuario se denomina *delegada*. Esta capacidad de delegación es fundamental para muchas aplicaciones distribuidas para las que hay una serie de comprobaciones de control de acceso que deben realizarse de forma secuencial para cada aplicación, base de datos o servicio que está en la cadena de autorización para la solicitud original. Muchos real\-existen situaciones del mundo en el que una aplicación Web "front-end" debe recuperar datos de un "back-end" más seguro, como un servicio Web que está conectado a una base de datos de Microsoft SQL Server.  
  
Por ejemplo, los elementos existentes\-ordenación sitio Web puede mejorarse mediante programación para que permita asociado a las organizaciones ver su propio estado de historial y la cuenta de compra. Por motivos de seguridad, todos los datos financieros de socios comerciales se almacena en una base de datos segura en un lenguaje de consulta estructurado dedicado \(SQL\) server. En esta situación, el código en la parte delantera\-aplicación end no sabe nada acerca de los datos financieros de la organización del asociado. Por lo tanto, deben recuperar los datos desde otro equipo en otro lugar de la red que hospeda \(en este caso\) el servicio Web para la base de datos de partes \(el back-end\).  
  
Para estos datos\-proceso de recuperación se realice correctamente, alguna sucesión de autorización "mano\-agita" debe tener lugar entre la aplicación Web y el servicio Web para la base de datos de partes, como se muestra en la siguiente ilustración.  
  
![delegación de identidad](media/adfs2_identitydelegationconcept.gif)  
  
Dado que la solicitud original se realizó en el propio servidor web, que es probable que se encuentre en una organización completamente diferente de la organización del usuario que está intentando tener acceso al servidor web, el token de seguridad que se envía con la solicitud no cumple los criterios de autorización necesarios para tener acceso a cualquier otro equipo además del servidor web. Por lo tanto, la única manera en que se puede cumplir la solicitud del usuario original es colocando un servidor de federación intermedio en la organización del asociado de recursos para ayudar a volver a emitir un token de seguridad que tenga los privilegios de acceso necesarios.  
  
## <a name="how-does-identity-delegation-work"></a>¿Cómo funciona la delegación de identidades?  
Las aplicaciones web de arquitecturas de aplicación de varios niveles a menudo llaman a servicios web para tener acceso a funcionalidades o datos comunes. Es importante para estos servicios web conocer la identidad del usuario original para que el servicio pueda tomar decisiones de autorización y facilitar la auditoría. En este caso, la parte delantera\-end de aplicación Web representa al usuario al servicio Web como un delegado. AD FS facilita este escenario permitiendo que las cuentas de Active Directory para que actúe como un usuario a otro usuario de confianza. En la siguiente ilustración, se muestra un escenario de delegación de identidad.  
  
![delegación de identidad](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank intenta obtener acceso a parte\-ordenación historial desde una aplicación Web en otra organización. Su equipo cliente solicita y recibe un token de AD FS para la parte delantera\-finalizar parte\-aplicación Web de pedidos.  
  
2.  El equipo cliente envía una solicitud a la aplicación web, incluido el token obtenido en el paso 1, para demostrar la identidad del cliente.  
  
3.  La aplicación web necesita comunicarse con el servicio web para completar su transacción para el cliente. La aplicación Web pone en contacto con AD FS para obtener un token de delegación para interactuar con el servicio Web. Los tokens de delegación son tokens de seguridad que se emiten a un delegado para que actúen como un usuario. AD FS devuelve un token de delegación con notificaciones sobre el cliente, destinadas al servicio Web.  
  
4.  La aplicación Web usa el token obtenido de AD FS en el paso 3 para tener acceso al servicio Web que actúa como el cliente. Al examinar el token de delegación, el servicio web puede determinar que la aplicación web actúa como el cliente. El servicio web ejecuta su directiva de autorización, registra la solicitud y proporciona los datos del historial de elementos necesarios solicitados originalmente por Frank a la aplicación web y, por tanto a Frank.  
  
Para un determinado delegado, AD FS puede limitar los servicios Web para que la aplicación Web puede solicitar un token de delegación. El equipo cliente no tiene ninguna cuenta de Active Directory para realizar esta operación correctamente. Por último, como se indicó anteriormente, el servicio web puede determinar fácilmente la identidad del delegado que actúa como usuario. Esto permite a los servicios web mostrar un comportamiento diferente en función de si están hablando directamente con el equipo cliente o a través de un delegado.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configuración de AD FS para la delegación de identidad  
Puede usar el complemento Administración de AD FS\-en para configurar AD FS para la delegación de identidad cuando necesite facilitar el proceso de recuperación de datos. Después de configurarlo, AD FS puede generar nuevos tokens de seguridad que incluyen el contexto de autorización que la parte trasera\-puede requerir el servicio de extremo para que pueda proporcionar acceso a los datos protegidos.  
  
AD FS no restringe qué usuarios se pueden suplantar. Después de configurar AD FS para la delegación de identidad, ocurre lo siguiente:  
  
-   Determina a qué servidores se puede delegar la autoridad para solicitar a los tokens que suplanten a un usuario.  
  
-   Establece y mantiene separados el contexto de identidad para la cuenta de cliente que se delega y el servidor que actúa como delegado.  
  
Puede configurar la delegación de identidad mediante la adición de reglas de autorización de delegación para un usuario de confianza confiar en el complemento Administración de AD FS\-en. Para obtener más información acerca de cómo hacerlo, consulte [lista de comprobación: Creación de reglas de notificación para una relación de confianza para usuario autenticado](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configuración de la parte delantera\-finalizar la aplicación Web para la delegación de identidad  
Los desarrolladores tienen varias opciones que pueden usar para programar correctamente la parte delantera de la Web\-finalizar la aplicación o servicio para redirigir las solicitudes de delegación en un equipo de AD FS. Para obtener más información sobre cómo personalizar una aplicación web para que funcione con la delegación de identidad, consulte el [SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
