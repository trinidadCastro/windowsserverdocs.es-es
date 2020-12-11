---
description: 'Más información sobre: escenario: Directiva de acceso central'
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: Directiva de acceso central de escenario
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 74fab5d6d790c4cf8ff4548f723a58dee50a941e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044613"
---
# <a name="scenario-central-access-policy"></a>Escenario: Directiva de acceso central

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Las directivas de acceso central para los archivos permiten a las organizaciones implementar y administrar de manera centralizada las directivas de autorización que incluyen expresiones condicionales que usan grupos e usuarios, notificaciones de usuario, notificaciones de dispositivo y propiedades de recursos. (Las notificaciones son aserciones sobre los atributos del objeto al que están asociadas). Por ejemplo, para tener acceso a datos de alto impacto de negocio (HBI), un usuario tiene que ser un empleado a tiempo completo, obtener acceso desde un dispositivo administrado e iniciar sesión con una tarjeta inteligente. Estas directivas se definen y se hospedan en Servicios de dominio de Active Directory (AD DS).

El cumplimiento de normas y los requisitos de las normas empresariales controlan las directivas de acceso organizativas. Por ejemplo, si una organización tiene un requisito empresarial de restringir el acceso a la información de identificación personal (PII) en los archivos solo al propietario del archivo y a los miembros del Departamento de recursos humanos (HR) que tienen permiso para ver la información de PII, esta Directiva se aplica a los archivos PII dondequiera que se encuentren en los servidores de archivos de toda la organización. En este ejemplo, tienes que ser capaz de:

-   Identificar y marcar los archivos que contienen PII.

-   Identificar el grupo de los miembros de RR.HH. que están autorizados para ver información PII.

-   Crear una directiva de acceso central que se aplique a todos los archivos que contengan PII ubicados en servidores de archivos en toda la organización.

La iniciativa de implementar y exigir una directiva de autorización puede provenir por distintos motivos y se puede aplicar a distintos niveles de la organización. A continuación se muestran algunos ejemplos de tipos de directivas:

-   **Directiva de autorización para toda la organización.** Esta directiva de autorización, que se suele iniciar desde la oficina de seguridad de la información, la derivan los requisitos de cumplimiento o de una organización de nivel muy alto y es relevante para toda la organización. Por ejemplo, solo los empleados a tiempo completo tienen acceso a los archivos HBI.

-   **Directiva de autorización departamental.** Cada departamento de una organización tiene algunos requisitos especiales de tratamiento de datos que quiere exigir. Por ejemplo: es posible que el Departamento de Finanzas quiera limitar por completo el acceso a los servidores de finanzas a los empleados de ese departamento.

-   **Directiva de administración de datos específicos.** Esta directiva suele estar relacionada con el cumplimiento y los requisitos empresariales y su finalidad es proteger el acceso correcto a la información que se administra. Por ejemplo, las instituciones financieras pueden implementar los muros informativos para que los analistas no tengan acceso a información de corretaje y los corredores de bolsa no puedan acceder a información de análisis.

-   **Directiva de necesidad de conocimiento de la información.** Este tipo de directiva de autorización se usa normalmente junto con los tipos de directiva anterior. Por ejemplo, los proveedores solo deben poder acceder y editar los archivos que pertenecen a un proyecto en el que trabajen.

Los entornos de la vida real también nos enseñan que cada directiva de autorización tiene que tener excepciones para que las organizaciones pueden reaccionar rápidamente cuando surjan necesidades empresariales importantes. Por ejemplo, los ejecutivos que no encuentran su tarjeta inteligente y necesitan un acceso rápido a la información HBI pueden llamar al departamento de soporte técnico para obtener una excepción temporal para tener acceso a esa información.

Las directivas de acceso central actúan como paraguas de seguridad que una organización aplica entre los servidores. Estas directivas mejoran (pero no sustituyen) las directivas de acceso local o a las listas de control de acceso discrecional (DACL) que se aplican a archivos y carpetas. Por ejemplo, si una DACL de un archivo permite el acceso a un usuario específico, pero una directiva central que se aplica al archivo restringe el acceso al mismo usuario, el usuario no puede tener acceso al archivo. Si la directiva de acceso central permite el acceso, pero la DACL no permite el acceso, el usuario no puede tener acceso al archivo.

Una regla de directiva de acceso central tiene las partes lógicas siguientes:

-   **Aplicabilidad.** Una condición que define los datos a los que se aplica la directiva, como Resource.BusinessImpact=High.

-   **Condiciones de acceso.** Una lista de una o más entradas de control de acceso (ACE) que definen quién puede acceder a los datos, como Permitir | Control total| User.EmployeeType=FTE.

-   **Excepciones.** Una lista adicional de una o varias entradas de ACE que definen una excepción de la directiva, como MemberOf(HBIExceptionGroup).

Las dos ilustraciones siguientes muestran el flujo de trabajo en el acceso central y las directivas de auditoría.

![guías de soluciones](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)

**Figura 1** Conceptos de acceso central y directiva de auditoría

![guías de soluciones](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)

**Figura 2** Flujo de trabajo de directiva de acceso central

La directiva de autorización central combina los componentes siguientes:

-   Una lista de reglas de acceso definidas centralmente destinadas a determinados tipos de información, como HBI o PII.

-   Una directiva definida centralmente que contiene una lista de reglas.

-   Un identificador de directiva que se asigna a cada archivo de los servidores de archivos para que señale a una directiva de acceso central específica que se debe aplicar durante la autorización de acceso.

En la ilustración siguiente se muestra cómo puedes combinar las directivas en listas de directiva para controlar centralmente el acceso a archivos.

![guías de soluciones](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)

**Figura 3** Combinación de directivas

## <a name="in-this-scenario"></a>En este escenario
La orientación siguiente está disponible para las directivas de acceso central:

-   [Planeación de una implementación de directiva de acceso central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)

-   [Implementar una directiva de acceso central &#40;pasos de la demostración&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)

-   [Control de acceso dinámico: Información general sobre el escenario](Dynamic-Access-Control--Scenario-Overview.md)

## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Roles y características incluidos en este escenario
En la tabla siguiente, se enumeran los roles y las características que forman parte de este escenario y se describe la manera en que son compatibles con él.

|Rol/característica|Compatibilidad con este escenario|
|-----------------|---------------------------------|
|Rol de Servicios de dominio de Active Directory|AD DS en Windows Server 2012 presenta una plataforma de autorización basada en notificaciones que permite la creación de notificaciones de usuario y notificaciones de dispositivo, identidad compuesta (notificaciones de usuario más dispositivo), nuevos modelos de directivas de acceso central (CAP) y el uso de información de clasificación de archivos en decisiones de autorización.|
|Rol del servidor de servicios de archivos y almacenamiento|Los servicios de archivos y almacenamiento incluyen tecnologías que te permiten configurar y administrar uno o más servidores de archivos que proporcionan ubicaciones centrales en tu red donde puedes almacenar archivos y compartirlos con los usuarios. Si los usuarios de la red necesitan tener acceso a los mismos archivos y aplicaciones, o si la administración centralizada de archivos y copias de seguridad es importante en su organización, deberá configurar uno o más equipos como servidor de archivos. Para ello, debe agregar a los equipos el rol Servicios de archivos y almacenamiento y los servicios de rol pertinentes.|
|Equipo cliente de Windows|Los usuarios pueden acceder a archivos y carpetas de la red a través del equipo cliente.|



