---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: Directiva de acceso Central de escenario
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1ec4165209b726609b1f9b2caeab02fb5072c756
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-central-access-policy"></a>Escenario: Directiva de acceso Central

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Directivas de acceso central para archivos permiten a las organizaciones a implementar y administrar directivas de autorización que incluyen expresiones condicionales que usan los grupos de usuarios, las notificaciones de usuario, notificaciones de dispositivo y propiedades de recurso de forma centralizada. (Las notificaciones son las aserciones acerca de los atributos del objeto con el que se asocian). Por ejemplo, para acceder a datos de gran impacto en la empresa (Repercusión), un usuario debe ser un empleado a tiempo completo, obtener acceso desde un dispositivo administrado e iniciar sesión con una tarjeta inteligente. Estas directivas se definen y hospedadas en los servicios de dominio de Active Directory (AD DS).  
  
Las directivas de acceso organizativas se controlan mediante el cumplimiento y requisitos normativos de empresas. Por ejemplo, si una organización tiene un requisito de negocio para restringir el acceso a información personalmente identificable (PII) en archivos solo el propietario del archivo y los miembros del departamento de recursos humanos (HR) que tienen permiso para ver información PII, esta directiva se aplica a los archivos PII siempre que se encuentran en servidores de archivos en toda la organización. En este ejemplo, debe poder:  
  
-   Identificar y marcar los archivos que contengan PII.  
  
-   Identifica el grupo de miembros HR quién tiene permiso para ver información PII.  
  
-   Crear una directiva de acceso central que se aplica a todos los archivos que contienen PII siempre que se encuentran en servidores de archivos en toda la organización.  
  
La iniciativa para implementar y aplicar una directiva de autorización puede proceder por muchas razones y aplicar a varios niveles de la organización. Estos son algunos tipos de directivas de ejemplo:  
  
-   **Directiva de autorización de toda la organización.** Normalmente se inicia desde la oficina de seguridad de información, esta directiva de autorización se realiza a través de cumplimiento o los requisitos de una organización de alto nivel y que sea relevante en toda la organización. Por ejemplo, archivos de Repercusión son accesibles para los empleados solo.  
  
-   **Directiva de autorización departamento.** Cada departamento de una organización tiene algunos requisitos especiales de administración de datos que quiere aplicar. Por ejemplo, el departamento de finanzas desea limitar el acceso a servidores de finanzas a los empleados de finanzas.  
  
-   **Directiva de administración de datos específica.** Esta directiva generalmente está relacionado con los requisitos empresariales y cumplimiento, y está dirigido a proteger el acceso a la información que se está administrando correcto. Por ejemplo, las instituciones financieras podrían implementar las paredes de la información para que analistas no tienen acceso a información de corretaje y agentes no tienen acceso a información de análisis.  
  
-   **Necesidad de conocer la directiva.** Por lo general, este tipo de directiva de autorización se usa en combinación con los tipos de directiva anterior. Por ejemplo, los proveedores podrán tener acceso y modificar solo los archivos que pertenecen a un proyecto que están trabajando.  
  
Entornos de la vida real nos enseñan que todas las directivas de autorización debe tener las excepciones para que las organizaciones pueden reaccionar rápidamente cuando surjan de necesidades empresariales importantes. Por ejemplo, ejecutivos que no pueden encontrar sus tarjetas inteligentes y necesitas acceso rápido a información Repercusión pueden llamar el servicio de asistencia para obtener una excepción temporal para tener acceso a esa información.  
  
Directivas de acceso central actúan como paraguas de seguridad que se aplica una organización a través de sus servidores. Estas directivas mejorarán (pero no reemplazan) las directivas de acceso local o listas de control de acceso discrecional (DACL) que se aplican a archivos y carpetas. Por ejemplo, si una DACL en un archivo permite el acceso a un usuario específico, pero una directiva central que se aplica al archivo restringe el acceso al mismo usuario, el usuario no puede obtener acceso al archivo. Si la directiva de acceso central permite el acceso, pero la DACL no permite el acceso, el usuario no puede obtener acceso al archivo.  
  
Una regla de directiva de acceso central tiene las siguientes partes lógicas:  
  
-   **Aplicabilidad.** Una condición que define los datos de la directiva se aplica, como Resource.BusinessImpact=High.  
  
-   **Condiciones de acceso.** Una lista de uno o más entradas control de acceso (ACE) que definen quién tiene acceso a los datos, como permitir | Control total | User.EmployeeType=FTE.  
  
-   **Excepciones.** Una lista más de uno o más ACE que definen una excepción para la directiva, como MemberOf(HBIExceptionGroup).  
  
Las siguientes dos figuras mostrar el flujo de trabajo de acceso central y las directivas de auditoría.  
  
![guías de solución](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**Figura 1** conceptos de directiva de acceso y auditoría Central  
  
![guías de solución](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**Figura 2** flujo de trabajo de directiva de acceso Central  
  
La directiva de autorización central combina los siguientes componentes:  
  
-   Una lista de reglas de acceso definidas centralmente destinadas a determinados tipos de información, como Repercusión o PII.  
  
-   Una directiva de forma centralizada definida que contiene una lista de reglas.  
  
-   Un identificador de directiva que se asigna a cada archivo en los servidores de archivos para apuntar a una directiva de acceso central específicos que debe aplicarse durante la autorización de acceso.  
  
La siguiente ilustración muestra cómo se pueden combinar directivas en listas de directiva para controlar forma centralizada el acceso a archivos.  
  
![guías de solución](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**Figura 3** combinar directivas  
  
## <a name="in-this-scenario"></a>En este escenario  
La siguiente guía está disponible para las directivas de acceso central:  
  
-   [Planear una implementación de directiva de acceso Central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [Implementar una directiva de acceso Central & #40; pasos de demostración & #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Roles y características incluidas en este escenario  
La siguiente tabla enumera los roles y características que forman parte de este escenario y describe cómo apoyan.  
  
|Rol o característica|¿Cómo admite este escenario|  
|-----------------|---------------------------------|  
|Rol de servicios de dominio de directorio activo|AD DS en Windows Server 2012 presenta una plataforma de notificaciones de autorización que permite la creación de notificaciones de usuario y reclamaciones de dispositivo, identidad compuesto, (usuario más notificaciones de dispositivo), nuevos modelos de acceso central (PAC) de la directiva y el uso de información de clasificación de archivos en las decisiones de autorización.|  
|Rol de servidor de servicios de almacenamiento y de archivo|Servicios de archivos y almacenamiento proporciona tecnologías que te ayudarán a configuración y administran uno o varios servidores de archivos que proporcionan ubicaciones centrales de la red donde puedes almacenar archivos y compartirlos con los usuarios. Si los usuarios de red necesitan acceso a los mismos archivos y las aplicaciones, o si centralizada copia de seguridad y administración de archivos es importante para la organización, debe configurar uno o más equipos como un servidor de archivos agregando la función File and Storage Services y los servicios de rol adecuado a los equipos.|  
|Equipo de cliente de Windows|Los usuarios pueden acceder a archivos y carpetas en la red a través del equipo cliente.|  
  


