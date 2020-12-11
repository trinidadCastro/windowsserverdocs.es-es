---
description: 'Más información acerca de: Cuándo crear un servidor de Federación'
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: Cuándo se debe crear un servidor de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d3ced02154d3945b8022d565d8fa834164d2d229
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049003"
---
# <a name="when-to-create-a-federation-server"></a>Cuándo se debe crear un servidor de federación

Cuando se crea un servidor de Federación \( en Servicios de federación de Active Directory (AD FS) AD FS \) , se proporciona un medio por el que la organización puede:

-   Participar en el inicio de sesión único Web \- \- en \( \) la comunicación basada en SSO con otra organización \( que también tenga al menos un servidor de Federación \) y, cuando sea necesario, con los empleados de su propia organización \( que necesitan acceso a través de Internet \) .

-   Habilite servicios front-end para suplantar a los usuarios en los servicios de infraestructura mediante la delegación de identidad. Para obtener más información, consulte [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md).

En las secciones siguientes se describen algunas de las decisiones clave para determinar cuándo y dónde crear uno o más servidores de Federación.

## <a name="determine-the-organizational-role-for-the-federation-server"></a>Determinar la función organizativa del servidor de federación
Para tomar una decisión informada sobre cuándo crear un nuevo servidor de Federación, primero debe determinar en qué organización va a residir el servidor. El rol que desempeña un servidor de Federación en una organización depende de si se coloca el servidor de Federación en la organización del asociado de cuenta o en la organización del asociado de recurso.

Cuando se coloca un servidor de Federación en la red corporativa del asociado de cuenta, su rol es autenticar las credenciales de usuario del explorador, el servicio web o los clientes del selector de identidad y enviar los tokens de seguridad a los clientes. Para obtener más información, consulte [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).

Cuando se coloca un servidor de Federación en la red corporativa del asociado de recurso, su rol es autenticar usuarios, en función de un token de seguridad emitido por un servidor de Federación en la organización del asociado de recurso, o su rol es redirigir las solicitudes de token de aplicaciones web o servicios web configurados a la organización del asociado de cuenta al que pertenece el cliente. Para obtener más información, consulte [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).

## <a name="determine-which-ad-fs-design-to-deploy"></a>Determinar qué diseño de AD FS implementar
Cree servidores de Federación en su organización siempre que desee implementar cualquiera de los siguientes diseños de AD FS:

-   [Diseño de SSO web](Web-SSO-Design.md)

-   [Diseño de SSO web federado](Federated-Web-SSO-Design.md)

Si es necesario, una organización que implementa un diseño de SSO Web federado puede configurar un único servidor de Federación para que actúe en el rol de asociado de cuenta y en el rol de asociado de recurso. En este caso, el servidor de Federación puede generar \( lenguaje de marcado de aserción de seguridad \) tokens SAML, en función de las cuentas de usuario de su propia organización, o volver a enrutar las solicitudes de token a la organización, en función de dónde residen las cuentas de los usuarios.

> [!NOTE]
> En el caso del diseño de SSO Web federado, debe haber al menos un servidor de Federación en el asociado de cuenta y al menos un servidor de Federación en el asociado de recurso.

## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Diferencias entre un servidor de federación y un servidor proxy de federación
Un servidor de Federación puede servir páginas web para \- el inicio de sesión, la Directiva, la autenticación y la detección de la misma manera que un servidor proxy de Federación. Las principales diferencias entre un servidor de Federación y un servidor proxy de Federación tienen que hacer con las operaciones que puede realizar un servidor de Federación y que no puede realizar un servidor proxy de Federación.

A continuación se indican las operaciones que solo puede realizar un servidor de Federación:

-   El servidor de Federación realiza las operaciones criptográficas que producen el token. Aunque los proxies de servidor de Federación no pueden generar tokens, se pueden usar para enrutar o redirigir los tokens a los clientes y, cuando sea necesario, de nuevo al servidor de Federación. Para obtener más información acerca del uso de servidores de Federación, consulte [Cuándo crear un servidor proxy de Federación](When-to-Create-a-Federation-Server-Proxy.md).

-   Los servidores de Federación admiten el uso de la autenticación integrada de Windows para los clientes de la red corporativa; los servidores proxy de Federación no lo hacen. Para obtener más información acerca del uso de la autenticación integrada de Windows con el servidor de Federación, consulte [Cuándo crear una granja de servidores de Federación](When-to-Create-a-Federation-Server-Farm.md).

> [!CAUTION]
> La integridad y confidencialidad de la comunicación entre los servidores de federación y las bases de datos de configuración de SQL Server, los almacenes de atributos de SQL Server, los controladores de dominio y las instancias de AD LDS no está protegida de forma predeterminada. Para mitigar esto, considere la posibilidad de proteger el canal de comunicación entre estos servidores mediante IPSEC o con una conexión segura físicamente entre todos estos servidores. Para la comunicación entre los servidores de federación y SQL, considere la posibilidad de usar la protección de SSL en la cadena de conexión. Para las conexiones entre los servidores de federación y los controladores de dominio, considere la posibilidad de activar la firma y el cifrado de Kerberos. Para LDAP, LDAP \/ S no es compatible con AD LDS \/ AD DS.

## <a name="how-to-create-a-federation-server"></a>Cómo se crea un servidor de federación
Puede crear un servidor de Federación mediante el Asistente para la configuración del servidor de Federación de AD FS o la herramienta de línea de comandos Fsconfig.exe \- . Cualquiera de estas herramientas permite elegir las siguientes opciones para crear un servidor de federación.

-   Crear un \- servidor de Federación independiente

    Para obtener más información acerca de cómo configurar un \- servidor de Federación independiente, consulte [crear un servidor de federación de Stand-Alone](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).

-   Crear el primer servidor de federación en una granja de servidores de federación

    Para obtener más información acerca de cómo configurar el primer servidor de federación o agregar un servidor de federación a una granja de servidores, consulte [Create the First Federation Server in a Federation Server Farm](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).

-   Agregar un servidor de federación a una granja de servidores de federación

    Para obtener más información sobre cómo agregar un servidor de federación a una granja de servidores, consulte [Add a Federation Server to a Federation Server Farm](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).

Para obtener más información detallada acerca de cómo funciona cada una de estas opciones, consulte [The Role of the AD FS Configuration Database](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).

Para obtener más información acerca de cómo configurar todos los requisitos previos necesarios para implementar un servidor de federación, consulte [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

