---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: Cuándo se debe crear un servidor de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e61c734780baa1482670af3f24697c10345b292
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190588"
---
# <a name="when-to-create-a-federation-server"></a>Cuándo se debe crear un servidor de federación

Cuando creas un Servidorde federación Active Directory Federation Services \(AD FS\), proporcionan un medio por el que su organización puede:  
  
-   Participar en Web solo\-sesión\-en \(SSO\): comunicación con otra organización basada en \(que también tiene al menos un servidor de federación\) y, cuando sea necesario, con el los empleados de su propia organización \(que necesitan acceso a través de Internet\).  
  
-   Habilite servicios front-end para suplantar a los usuarios en los servicios de infraestructura mediante la delegación de identidad. Para obtener más información, consulte [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md).  
  
Las secciones siguientes describen algunas de las decisiones claves para determinar cuándo y dónde crear uno o más servidores de federación.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Determinar la función organizativa del servidor de federación  
Para tomar una decisión fundamentada sobre cuándo se debe crear un nuevo servidor de federación, primero debe determinar en qué organización van a residir el servidor. La función que desempeña un servidor de federación en una organización depende de si se coloca el servidor de federación en la organización del asociado de cuenta o en la organización del asociado de recurso.  
  
Cuando un servidor de federación se coloca en la red corporativa del asociado de cuenta, su función es autenticar las credenciales de usuario del explorador, el servicio Web o los clientes del selector de identidad y enviar los tokens de seguridad a los clientes. Para obtener más información, consulte [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Cuando un servidor de federación se coloca en la red corporativa del asociado de recurso, su función es autenticar usuarios, según un token de seguridad emitido por un servidor de federación de la organización del asociado de recurso, o su función es redirigir solicitudes de token configurar aplicaciones Web o servicios Web para la organización del asociado de cuenta que pertenece el cliente. Para obtener más información, consulte [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Determinar qué diseño de AD FS implementar  
Crear servidores de federación de su organización siempre que desee implementar cualquiera de los diseños AD FS:  
  
-   [Diseño de SSO web](Web-SSO-Design.md)  
  
-   [Diseño de SSO web federado](Federated-Web-SSO-Design.md)  
  
Si es necesario, una organización que implementa un diseño SSO Web federado puede configurar un servidor de federación único para que actúe en ambos el rol de asociado de cuenta y en el rol de asociado de recurso. En este caso, el servidor de federación puede generar el lenguaje de marcado de aserción de seguridad \(SAML\) tokens, basándose en las cuentas de usuario en su propia organización o volver a enrutar las solicitudes de token a la organización, según donde residen las cuentas de los usuarios .  
  
> [!NOTE]  
> El diseño SSO Web federado, debe haber al menos un servidor de federación del asociado de cuenta y al menos un servidor de federación del asociado de recurso.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Diferencias entre un servidor de federación y un servidor proxy de federación  
Un servidor de federación puede servir páginas Web para el inicio de sesión\-en Directiva, autenticación y detección en la misma forma que un servidor proxy de federación. Las principales diferencias entre un servidor de federación y un servidor proxy de federación tienen que ver con qué operaciones una federación servidor puede llevar a cabo que no se puede realizar un servidor proxy de federación.  
  
Los siguientes son las operaciones que puede realizar solo un servidor de federación:  
  
-   El servidor de federación realiza las operaciones criptográficas que producen el token. Aunque los servidores proxy de federación no pueden generar tokens, se puede usar para enrutar o redirigir los tokens a los clientes y, cuando sea necesario, vuelva al servidor de federación. Para obtener más información sobre el uso de los servidores de federación, consulte [cuándo se debe crear un servidor Proxy de federación](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   Los servidores de federación admiten el uso de la autenticación integrada de Windows para clientes de la red corporativa; servidores proxy de federación no lo hacen. Para obtener más información sobre el uso de la autenticación integrada de Windows con el servidor de federación, consulte [cuándo se debe crear una granja de servidores de federación](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> La integridad y confidencialidad de la comunicación entre los servidores de federación y las bases de datos de configuración de SQL Server, los almacenes de atributos de SQL Server, los controladores de dominio y las instancias de AD LDS no está protegida de forma predeterminada. Para mitigar esto, considere la posibilidad de proteger el canal de comunicación entre estos servidores mediante IPSEC o con una conexión segura físicamente entre todos estos servidores. Para la comunicación entre los servidores de federación y SQL, considere la posibilidad de usar la protección de SSL en la cadena de conexión. Para las conexiones entre los servidores de federación y los controladores de dominio, considere la posibilidad de activar la firma y el cifrado de Kerberos. Para LDAP, LDAP\/S no es compatible con AD LDS\/AD DS.  
  
## <a name="how-to-create-a-federation-server"></a>Cómo se crea un servidor de federación  
Puede crear un servidor de federación mediante el Asistente para configuración del servidor de federación de AD FS o el comando Fsconfig.exe\-herramienta de línea. Cualquiera de estas herramientas permite elegir las siguientes opciones para crear un servidor de federación.  
  
-   Crear un soporte\-servidor de federación independiente  
  
    Para obtener más información acerca de cómo configurar un soporte\-servidor de federación independiente, consulte [Create a Stand-Alone Federation Server](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Crear el primer servidor de federación en una granja de servidores de federación  
  
    Para obtener más información acerca de cómo configurar el primer servidor de federación o agregar un servidor de federación a una granja de servidores, consulte [Create the First Federation Server in a Federation Server Farm](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Agregar un servidor de federación a una granja de servidores de federación  
  
    Para obtener más información sobre cómo agregar un servidor de federación a una granja de servidores, consulte [Add a Federation Server to a Federation Server Farm](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Para obtener más información detallada acerca de cómo funciona cada una de estas opciones, consulte [The Role of the AD FS Configuration Database](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Para obtener más información acerca de cómo configurar todos los requisitos previos necesarios para implementar un servidor de federación, consulte [lista de comprobación: Configuración de un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

