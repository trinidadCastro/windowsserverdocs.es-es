---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: "Cuándo usar la delegación de identidad"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-use-identity-delegation"></a>Cuándo usar la delegación de identidad

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>¿Qué es la delegación de identidad?  
Delegación de identidad es una característica de \(AD FS\) los servicios de federación de Active Directory que permite a las cuentas especificado administrator\ suplantar a los usuarios. Se llama a la cuenta que representa al usuario la *delegado*. Esta funcionalidad de delegación es fundamental para muchas aplicaciones distribuidas para que hay una serie de acceso comprobaciones de control que deben realizarse de forma secuencial para cada aplicación, la base de datos o el servicio que está en la cadena de autorización para la solicitud de origen. Muchos real\-world escenarios en que una aplicación "front-end Web" debe recuperar los datos de una más segura "back-end", como un servicio Web que está conectado a una base de datos de Microsoft SQL Server.  
  
Por ejemplo, un sitio Web existente de ordenación parts\ pueden mejorarse mediante programación para que permita las organizaciones ver su propio estado compra, historial y cuenta de partner. Por motivos de seguridad, datos financieros de todos los partners se almacena en una base de datos segura en un servidor de lenguaje de consulta estructurado \(SQL\) dedicado. En esta situación, el código de la aplicación front\ final sabe nada datos financieros en de la organización de partner. Por lo tanto, deben recuperar los datos desde otro equipo en otra parte de la red que hospeda \ (en este case\) el servicio Web para la base de datos de partes \(the back end\).  
  
Para que este proceso de recuperación de data\ correctamente, algunas sucesión de autorización "sacudida hand\" debe tener se coloca entre la aplicación Web y el servicio Web para la base de datos de elementos, como se muestra en la siguiente ilustración.  
  
![delegación de identidad](media/adfs2_identitydelegationconcept.gif)  
  
Dado que el propio servidor Web, que es probable que se encuentra en una organización completamente distinta de la organización del usuario que intenta obtener acceso al servidor Web, se realizó la solicitud original el token de seguridad que se envía junto con la solicitud no cumple los criterios de autorización necesarios para acceder a cualquier otro equipo, además del servidor Web. Por lo tanto, la única manera de que se puede cumplir la solicitud de usuario de origen es colocar un servidor de federación intermedio en la organización de partner de recurso para ayudar con volver a emitir un token de seguridad que tienen los privilegios necesarios.  
  
## <a name="how-does-identity-delegation-work"></a>¿Cómo funciona la delegación de identidad?  
Aplicaciones Web en arquitecturas de varios niveles de aplicaciones a menudo llamar a servicios Web para acceder a datos o funciones comunes. Es importante para estos servicios Web saber la identidad del usuario original para que el servicio pueda tomar decisiones de autorización y facilitar la auditoría. En este caso, la aplicación Web front\ representa al usuario al servicio Web como una función delegada. AD FS facilita este escenario al permitir que las cuentas de Active Directory para que actúe como un usuario a otro usuario de confianza. Se muestra un escenario de delegación de identidad en la siguiente ilustración.  
  
![delegación de identidad](media/adfs2_identitydelegationsteps.gif)  
  
1.  Francisco intenta obtener acceso a historial de pedidos part\ desde una aplicación Web en otra organización. Su equipo cliente solicita y recibe un token de AD FS para la aplicación Web para solicitar part\ front\-end.  
  
2.  El equipo cliente envía una solicitud a la aplicación Web, incluido el token obtenido en el paso 1, para demostrar la identidad del cliente.  
  
3.  La aplicación Web necesita comunicarse con el servicio Web para completar la transacción para el cliente. La aplicación Web pone en contacto con AD FS para obtener un token de delegación para interactuar con el servicio Web. Los tokens de delegación son tokens de seguridad que se emiten en un delegado para que actúe como un usuario. AD FS devuelve un token de delegación con solicitudes acerca del cliente, de destino para el servicio Web.  
  
4.  La aplicación Web usa el token que se ha obtenido de AD FS en el paso 3 para acceder al servicio Web que actúa como el cliente. Examina el token de delegación, el servicio Web puede determinar que la aplicación Web actúa como el cliente. El servicio Web ejecuta su directiva de autorización, inicia la solicitud y proporciona datos del historial que se solicitó originalmente por Frank a la aplicación Web de los elementos necesarios y, por lo tanto a Tomás.  
  
Para un delegado particular, AD FS puede limitar los servicios Web para que la aplicación Web puede solicitar un token de la delegación. El equipo cliente no tengan una cuenta de Active Directory para realizar esta operación correctamente. Por último, como se mencionó anteriormente, el servicio Web puede determinar fácilmente la identidad del delegado que actúa como el usuario. Esto permite a los servicios Web un comportamiento diferente en función de si están hablando directamente en el equipo cliente o a través de un delegado.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configurar AD FS para la delegación de identidad  
Puedes usar la administración de AD FS snap\ en configurar AD FS para la delegación de identidad siempre que necesitas facilitar el proceso de recuperación de datos. Después de configurarlo, AD FS puede generar nuevos tokens de seguridad que se incluyen el contexto de autorización que puede requerir el servicio back\-end para que pueda proporcionar acceso a los datos protegidos.  
  
AD FS no restringe los usuarios que puedan representar. Después de configurar AD FS para la delegación de identidad, realiza lo siguiente:  
  
-   Determina qué servidores pueden delegar la entidad de solicitud tokens para suplantar a un usuario.  
  
-   Establece y mantiene separados el contexto de identidad para la cuenta del cliente que se delega y el servidor que actúa como una función delegada.  
  
Puede configurar la delegación de identidad agregando las reglas de autorización de la delegación a una usuario de confianza confianza de terceros en la administración de AD FS snap\ en. Para obtener más información sobre cómo hacerlo, consulta [lista de comprobación: crear reglas de notificación para una confianza de terceros confiar](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configuración de la aplicación de Web front\ final para la delegación de identidad  
Los desarrolladores tienen varias opciones que se pueden usar para programar correctamente el final de front\ aplicación o servicio Web para redirigir las solicitudes de delegación a un equipo de AD FS. Para obtener más información sobre cómo personalizar una aplicación Web para que funcione con la delegación de identidad, consulta el [SDK de identidad de Windows Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
