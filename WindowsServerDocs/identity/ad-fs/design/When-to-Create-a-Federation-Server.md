---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: "Cuándo se debe crear un servidor de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8013764b88a1061cfcaa3a507466c111bfd59aad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server"></a>Cuándo se debe crear un servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando creas un federación servidorde los servicios de federación de Active Directory \(AD FS\), proporcionan un medio por el que la organización puede:  
  
-   Participar en la Web single\ sign\ en \ (SSO\): la comunicación con otra organización basada en \ (que también tiene al menos un server\ federación) y, cuando sea necesario, los empleados de tu propia organización \ (que necesitan acceso a través de la Internet\).  
  
-   Habilitar los servicios de front-end suplantar a los usuarios a los servicios de infraestructura mediante la delegación de identidad. Para obtener más información, consulta [cuándo usar identidad delegación](When-to-Use-Identity-Delegation.md).  
  
Las siguientes secciones describen algunas de las decisiones clave para determinar cuándo y dónde quieres crear uno o varios servidores de federación.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Determinar el rol del servidor de federación organizativo  
Para decisiones informadas sobre cuándo se debe crear un servidor de federación de nuevo, primero debes determinar en qué organización residirá el servidor. El papel que desempeña un servidor de federación de una organización depende de si el servidor de federación se coloca en la organización de partner de la cuenta o en la organización de partner de recurso.  
  
Cuando un servidor de federación se coloca en la red corporativa de asociado de cuenta, su función es autenticar las credenciales de usuario del explorador, el servicio Web o los clientes de selector de identidad y enviar los tokens de seguridad a los clientes. Para obtener más información, consulta [revisar el rol de servidor de federación de la cuenta de Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Cuando un servidor de federación se coloca en la red corporativa de socio de recursos, su función es autenticar usuarios, en función de un token de seguridad emitido por un servidor de federación de la organización de partner de recurso, o su función es redirigir solicitudes de token de servicios Web o aplicaciones Web configuradas para la organización de partner de cuenta que pertenece el cliente. Para obtener más información, consulta [revisar el rol de servidor de federación de asociado de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Determinar qué diseño de AD FS para implementar  
Crear los servidores de federación de su organización siempre que quieras implementar cualquiera de los siguientes diseños de AD FS:  
  
-   [Diseño de inicio de sesión único Web](Web-SSO-Design.md)  
  
-   [Diseño SSO Web federado](Federated-Web-SSO-Design.md)  
  
Si es necesario, una organización que implementa un diseño SSO Web federado puede configurar un servidor de federación único para que actúa en la función de socio de cuenta y en la función de socio de recursos. En este caso, el servidor de federación puede generar tokens de lenguaje de marcado de seguridad aserción \(SAML\), en función de las cuentas de usuario en su propia organización, o volver a enrutar token solicita a la organización, en función de donde residen las cuentas de los usuarios.  
  
> [!NOTE]  
> Para el diseño SSO Web federado, debe haber al menos un servidor de federación de asociado de la cuenta y al menos un servidor de federación de asociado de recurso.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Diferencias entre un servidor de federación y un proxy de federación de servidor  
Un servidor de federación puede servir páginas Web para sign\ de directiva, la autenticación y detección de la misma manera que se encargue de un proxy de federación de servidor. Las diferencias principales entre un servidor de federación y un proxy de servidor de federación tienen que ver con las operaciones que una federación servidor puede realizar que no se puede realizar un proxy de federación de servidor.  
  
Las siguientes son las operaciones que puede realizar un servidor de federación de solo:  
  
-   El servidor de federación realiza las operaciones de cifrado que producen el token. Aunque proxies de servidor de federación no pueden generar tokens, que pueden usarse para enrutar o redirigir los tokens para los clientes y, cuando sea necesario, al servidor de federación. Para obtener más información sobre el uso de los servidores de federación, consulta [cuándo se debe crear un Proxy de servidor de federación](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   Los servidores de federación admiten el uso de autenticación integrada en Windows para clientes de la red corporativa. proxies de servidor de federación no. Para obtener más información sobre cómo usar la autenticación integrada de Windows con el servidor de federación, consulta [cuándo se debe crear una granja de servidores de federación](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> Comunicación entre instancias de AD LDS y bases de datos de configuración de SQL Server, almacenes de atributo de SQL Server, controladores de dominio y servidores de federación no es de integridad o confidencialidad protegido de manera predeterminada. Para mitigar este riesgo, considera la posibilidad de proteger el canal de comunicación entre estos servidores mediante IPSEC o mediante una conexión segura físicamente entre todos estos servidores. Para la comunicación entre servidores de federación y SQL Server, considera la posibilidad de usar la protección de SSL en la cadena de conexión. Para las conexiones entre controladores de dominio y servidores de federación, considera la posibilidad de activar el cifrado y firma de Kerberos. Para LDAP, LDAP\/S no es compatible para AD LDS\ o AD DS.  
  
## <a name="how-to-create-a-federation-server"></a>Cómo crear un servidor de federación  
Puedes crear un servidor de federación mediante el Asistente para configuración del servidor de federación de AD FS o la herramienta de línea de Web\ Fsconfig.exe. Al usar cualquiera de estas herramientas, puedes seleccionar cualquiera de las siguientes opciones para crear un servidor de federación.  
  
-   Crear un servidor de federación stand\ solo  
  
    Para obtener más información acerca de cómo configurar un servidor de federación stand\ solo, consulta [crear un servidor de federación independiente](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Crear el primer servidor de federación en una granja de servidores de federación  
  
    Para obtener más información acerca de cómo configurar el primer servidor de federación o agregar un servidor de federación a una granja de servidores, consulta [crea el primer servidor de federación en una granja de servidores de federación](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Agregar un servidor de federación a una granja de servidores de federación  
  
    Para obtener más información sobre cómo agregar un servidor de federación a una granja de servidores, consulta [agregar un servidor de federación a una granja de servidores de federación](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Para obtener más información acerca de cómo cada una de estas opciones de trabajo, consulta [el rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Para obtener más información sobre cómo configurar todos los requisitos previos necesarios para implementar un servidor de federación, consulta [lista de comprobación: configurar un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

