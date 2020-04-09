---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: Cuándo se debe usar la delegación de identidad
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7a594332900c8b3afb95c139bcde8458a10f186b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858468"
---
# <a name="when-to-use-identity-delegation"></a>Cuándo se debe usar la delegación de identidad
  
## <a name="what-is-identity-delegation"></a>¿Qué es la delegación de identidad?  
La delegación de identidad es una característica de Servicios de federación de Active Directory (AD FS) \(AD FS\) que permite a los administradores\-cuentas especificadas para suplantar a los usuarios. La cuenta que suplanta al usuario se denomina *delegada*. Esta capacidad de delegación es fundamental para muchas aplicaciones distribuidas para las que hay una serie de comprobaciones de control de acceso que deben realizarse de forma secuencial para cada aplicación, base de datos o servicio que está en la cadena de autorización para la solicitud original. Existen muchos escenarios reales de\-mundo en los que una aplicación web "front-end" debe recuperar datos de un "back end" más seguro, como un servicio Web que está conectado a una base de datos Microsoft SQL Server.  
  
Por ejemplo, una parte existente\-sitio web de ordenación se puede mejorar mediante programación para que permita a las organizaciones asociadas ver su propio historial de compras y el estado de la cuenta. Por motivos de seguridad, todos los datos financieros de los asociados se almacenan en una base de datos segura en una Lenguaje de consulta estructurado dedicada \(servidor SQL\). En esta situación, el código de la aplicación Front\-end no sabe nada sobre los datos financieros de la organización asociada. Por lo tanto, debe recuperar los datos de otro equipo en otra parte de la red que hospede \(en este caso\) el servicio web para la base de datos de elementos \(la\)de back-end.  
  
Para que estos datos\-proceso de recuperación se realicen correctamente, se debe llevar a cabo alguna sucesión de "mano\-sacudida" entre la aplicación web y el servicio web para la base de datos de elementos, tal como se muestra en la siguiente ilustración.  
  
![Delegación de identidad](media/adfs2_identitydelegationconcept.gif)  
  
Dado que la solicitud original se realizó en el propio servidor web, que es probable que se encuentre en una organización completamente diferente de la organización del usuario que está intentando tener acceso al servidor web, el token de seguridad que se envía con la solicitud no cumple los criterios de autorización necesarios para tener acceso a cualquier otro equipo además del servidor web. Por lo tanto, la única manera en que se puede cumplir la solicitud del usuario original es colocando un servidor de federación intermedio en la organización del asociado de recursos para ayudar a volver a emitir un token de seguridad que tenga los privilegios de acceso necesarios.  
  
## <a name="how-does-identity-delegation-work"></a>¿Cómo funciona la delegación de identidades?  
Las aplicaciones web de arquitecturas de aplicación de varios niveles a menudo llaman a servicios web para tener acceso a funcionalidades o datos comunes. Es importante para estos servicios web conocer la identidad del usuario original para que el servicio pueda tomar decisiones de autorización y facilitar la auditoría. En este caso, la aplicación web Front\-end representa al usuario en el servicio web como delegado. AD FS facilita este escenario permitiendo que las cuentas de Active Directory actúen como un usuario en otro usuario de confianza. En la siguiente ilustración, se muestra un escenario de delegación de identidad.  
  
![Delegación de identidad](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank intenta tener acceso a la parte\-historial de pedidos desde una aplicación Web de otra organización. Su equipo cliente solicita y recibe un token de AD FS para la parte Front\-end\-aplicación Web de ordenación.  
  
2.  El equipo cliente envía una solicitud a la aplicación Web, incluido el token obtenido en el paso 1, para demostrar la identidad del cliente.  
  
3.  La aplicación web necesita comunicarse con el servicio web para completar su transacción para el cliente. La aplicación web se pone en contacto AD FS para obtener un token de delegación para interactuar con el servicio Web. Los tokens de delegación son tokens de seguridad que se emiten a un delegado para que actúen como un usuario. AD FS devuelve un token de delegación con notificaciones sobre el cliente, destinadas al servicio Web.  
  
4.  La aplicación Web usa el token que se obtuvo de AD FS en el paso 3 para tener acceso al servicio Web que actúa como el cliente. Al examinar el token de delegación, el servicio web puede determinar que la aplicación web actúa como el cliente. El servicio web ejecuta su directiva de autorización, registra la solicitud y proporciona los datos del historial de elementos necesarios solicitados originalmente por Frank a la aplicación web y, por tanto a Frank.  
  
Para un determinado delegado, AD FS puede limitar los servicios web para los que la aplicación web puede solicitar un token de delegación. El equipo cliente no tiene ninguna cuenta de Active Directory para realizar esta operación correctamente. Por último, como se indicó anteriormente, el servicio web puede determinar fácilmente la identidad del delegado que actúa como usuario. Esto permite a los servicios web mostrar un comportamiento diferente en función de si están hablando directamente con el equipo cliente o a través de un delegado.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configuración de AD FS para la delegación de identidad  
Puede usar el complemento de administración de AD FS\-en para configurar AD FS para la delegación de identidad siempre que necesite facilitar el proceso de recuperación de datos. Después de configurarlo, AD FS puede generar nuevos tokens de seguridad que incluirán el contexto de autorización que el servicio Back\-end puede requerir antes de que pueda proporcionar acceso a los datos protegidos.  
  
AD FS no restringe los usuarios que se pueden suplantar. Después de configurar AD FS para la delegación de identidad, hace lo siguiente:  
  
-   Determina a qué servidores se puede delegar la autoridad para solicitar a los tokens que suplanten a un usuario.  
  
-   Establece y mantiene separados el contexto de identidad para la cuenta de cliente que se delega y el servidor que actúa como delegado.  
  
Puede configurar la delegación de identidad mediante la adición de reglas de autorización de delegación a una relación de confianza para usuario autenticado en el complemento de administración de AD FS\-en. Para obtener más información sobre cómo hacerlo, consulte [Checklist: Creating Claim Rules for a Relying Party Trust](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configuración de la aplicación web Front\-end para la delegación de identidad  
Los desarrolladores tienen varias opciones que pueden usar para programar correctamente la aplicación o el servicio Front\-end web para redirigir las solicitudes de delegación a un equipo AD FS. Para obtener más información sobre cómo personalizar una aplicación web para que funcione con la delegación de identidad, consulte el [SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
