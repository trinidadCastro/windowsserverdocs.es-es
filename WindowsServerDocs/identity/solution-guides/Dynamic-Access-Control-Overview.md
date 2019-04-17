---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: "Información general sobre el Control de acceso dinámico"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5cf74042c9b511abb1fbeb88224dea0c7f2c8706
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="dynamic-access-control-overview"></a>Información general sobre el Control de acceso dinámico

>Se aplica a: Windows Server 2012 R2, Windows Server 2012

Este tema de introducción para profesionales de TI describe el Control de acceso dinámico y tiene asociados los elementos, que se introdujeron en Windows Server 2012 y Windows 8.  
  
Control de acceso dinámico basado en dominio permite a los administradores aplicar restricciones basadas en las reglas bien definidas que pueden incluir la confidencialidad de los recursos, el trabajo o la función del usuario y la configuración del dispositivo que se usa para acceder a estos recursos y permisos de control de acceso.  
  
Por ejemplo, un usuario podría tener permisos diferentes cuando tienen acceso a un recurso desde su equipo office frente a cuando la estén usando un equipo portátil a través de una red privada virtual. O puede permitir acceso solo si un dispositivo cumple los requisitos de seguridad que se definen con los administradores de red. Cuando se usa el Control de acceso dinámico, los permisos de un usuario cambian dinámicamente sin la intervención del administrador adicionales si el usuario trabajo o rol cambia (resultantes cambios en atributos de la cuenta del usuario en AD DS).  
  
Control de acceso dinámico no se admite en sistemas operativos Windows anteriores a Windows Server 2012 y Windows 8. Cuando se configura el Control de acceso dinámico en entornos con versiones compatibles y no compatible de Windows, solo las versiones compatibles implementará los cambios.  
  
Características y conceptos asociados con el Control de acceso dinámico incluyen:  
  
-   [Reglas de acceso central](#BKMK_Rules)  
  
-   [Directivas de acceso central](#BKMK_Policies)  
  
-   [Reclamaciones](#BKMK_Claims)  
  
-   [Expresiones](#BKMK_Expressions2)  
  
-   [Propuesta permisos](#BKMK_Permissions2)  
  
### <a name="BKMK_Rules"></a>Reglas de acceso central  
Una regla de acceso central es una expresión de reglas de autorización que puede incluir una o varias condiciones relacionado con los grupos de usuarios, las notificaciones de usuario, notificaciones de dispositivo y propiedades de recurso. Varias reglas de acceso central se pueden combinar en una directiva de acceso central.  
  
Si se ha definido una o varias reglas de acceso central para un dominio, administradores de recurso compartido de archivos pueden coincidir con las reglas específicas para los requisitos empresariales y los recursos específicos.  
  
### <a name="BKMK_Policies"></a>Directivas de acceso central  
Directivas de acceso central son las directivas de autorización que incluyen expresiones condicionales. Por ejemplo, supongamos que una organización tiene un requisito de negocio para restringir el acceso a información personalmente identificable (PII) en archivos solo el propietario del archivo y los miembros del departamento de recursos humanos (HR) que tienen permiso para ver información PII. Representa una directiva de toda la organización que se aplica a los archivos PII siempre que se encuentran en servidores de archivos en toda la organización. Para implementar esta directiva, debe ser capaz de una organización:  
  
-   Identificar y marcar los archivos que contengan el PII.  
  
-   Identifica el grupo de miembros HR que tienen permiso para ver la información de PII.  
  
-   Agregar la directiva de acceso central a una regla de acceso central y aplicar la regla de acceso central a todos los archivos que contienen la PII, siempre que se encuentran entre los servidores de archivos en toda la organización.  
  
Directivas de acceso central actúan como paraguas de seguridad que se aplica una organización a través de sus servidores. Estas directivas están además (pero no reemplazan) las directivas de acceso local o listas de control de acceso discrecional (DACL) que se aplican a archivos y carpetas.  
  
### <a name="BKMK_Claims"></a>Reclamaciones  
Una reclamación es un único elemento de información acerca de un usuario, el dispositivo o el recurso que se ha publicado un controlador de dominio. Título del usuario, la clasificación del departamento de un archivo o el estado de un equipo son válidos ejemplos de una reclamación. Una entidad puede implicar más de una notificación, y cualquier combinación de notificaciones puede usarse para autorizar el acceso a los recursos. Los siguientes tipos de notificaciones están disponibles en las versiones compatibles de Windows:  
  
-   **Notificaciones de usuario** atributos de Active Directory que están asociados con un usuario específico.  
  
-   **Notificaciones de dispositivo** atributos de Active Directory que están asociados con un objeto de equipo específico.  
  
-   **Atributos de los recursos** propiedades de recurso Global que están marcados para su uso en las decisiones de autorización y se publican en Active Directory.  
  
Reclamaciones hacen que sea posible para los administradores puedan realizar instrucciones de la organización o empresa wide preciso acerca de los usuarios, dispositivos y recursos que se puede integrar en expresiones, reglas y directivas.  
  
### <a name="BKMK_Expressions2"></a>Expresiones  
Expresiones condicionales son una mejora de administración de control de acceso que permitir o denegar el acceso a los recursos solo cuando se cumplen ciertas condiciones, por ejemplo, la pertenencia al grupo, la ubicación o el estado de seguridad del dispositivo. Expresiones se administran a través del cuadro de diálogo Configuración de seguridad avanzada del Editor de ACL o el Editor de reglas de acceso Central en el centro de administrativas de directorio (ADAC) activo.  
  
Expresiones ayudar a los administradores a administrar el acceso a recursos con información confidencial con condiciones flexibles en entornos empresariales cada vez más complejos.  
  
### <a name="BKMK_Permissions2"></a>Propuesta permisos  
Propuesta permisos permiten a un administrador con más precisión modelar el impacto de posibles cambios para tener acceso a configuración de control sin en realidad modificarlas.  
  
Predecir el acceso eficaz a un recurso te ayuda a planear y configurar los permisos de esos recursos antes de implementar los cambios.  
  
## <a name="additional-changes"></a>Cambios adicionales  
Mejoras adicionales de las versiones compatibles de Windows que admite el Control de acceso dinámico incluyen:  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Se admiten en el protocolo de autenticación Kerberos para proporcionar de forma confiable notificaciones de usuario, las notificaciones de dispositivo y grupos de dispositivos.  
De manera predeterminada, los dispositivos que ejecutan cualquiera de las versiones compatibles de Windows son capaces de procesar los vales Kerberos relacionados con el Control de acceso dinámico, que incluyen los datos necesarios para la autenticación compuesta. Controladores de dominio son capaces de emitir y responder a los vales Kerberos con información relacionada con la autenticación compuesta. Cuando un dominio está configurado para reconocer el Control de acceso dinámico, dispositivos reciban notificaciones de controladores de dominio durante la autenticación inicial y recibir vales de autenticación compuesta al enviar solicitudes de vales de servicio. La autenticación compuesta da como resultado un token de acceso que incluye la identidad del usuario y el dispositivo a los recursos que reconoce el Control de acceso dinámico.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Compatibilidad para usar la configuración de directiva de grupo de claves (KDC) de centro de distribución de clave para habilitar el Control de acceso dinámico para un dominio.  
Cada controlador de dominio debe tener la misma configuración de directiva de plantillas administrativas, que se encuentra en **Control de acceso dinámico de equipo equipo\Directivas\Plantillas Templates\System\KDC\Support y protección de Kerberos**.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Compatibilidad para usar la configuración de directiva de grupo de claves (KDC) de centro de distribución de clave para habilitar el Control de acceso dinámico para un dominio.  
Cada controlador de dominio debe tener la misma configuración de directiva de plantillas administrativas, que se encuentra en **Control de acceso dinámico de equipo equipo\Directivas\Plantillas Templates\System\KDC\Support y protección de Kerberos**.  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>Se admiten en Active Directory para almacenar las reclamaciones de usuarios y dispositivos, las propiedades de recursos y objetos de directiva de acceso central.  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>Compatibilidad para usar la directiva de grupo para implementar objetos de directiva de acceso central.  
La siguiente configuración de directiva de grupo permite implementar objetos de directiva de acceso central a servidores de archivos en la organización: **directiva de equipo Configuration\Policies\ Windows Windows\Configuración Settings\File System\Central acceso**.  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>Soporte para la autorización de archivos basado en autenticaciones y auditoría para sistemas de archivos con la directiva de grupo y la auditoría de acceso a objetos Global  
Debes habilitar la auditoría de directivas de acceso central por fases auditar el acceso eficaz de directiva de acceso central mediante permisos propuestos. Para configurar este valor para el equipo en **configuración de directiva de auditoría avanzada** en la **la configuración de seguridad** de un objeto de directiva de grupo (GPO). Después de configurar la configuración de seguridad en el GPO, puedes implementar el GPO a los equipos en la red.  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>Soporte técnico para transformar o filtrar objetos de directiva de reclamación que recorren confianzas de bosque de Active Directory  
Puedes filtrar o transformar notificaciones entrantes y salientes que recorren una confianza de bosque. Hay tres escenarios básicos para filtrar y transformar notificaciones:  
  
-   **Filtrado basado en el valor** filtros pueden basarse en el valor de una reclamación. Esto permite que el bosque de confianza evitar reclamaciones con determinados valores que se envíen al bosque de confianza. Controladores de dominio de bosques de confianza pueden utilizar el filtrado basado en el valor para protegerse contra ataques de elevación de privilegios filtrando las notificaciones entrantes con valores específicos del bosque de confianza.  
  
-   **Los filtros basados en el tipo de notificación** filtros se basan en el tipo de notificación, en lugar de con el valor de dicha reclamación. Para identificar el tipo de notificación, el nombre de la notificación. Usar filtrado en el bosque de confianza basada en tipos de notificación y evita que Windows pueda enviar notificaciones que divulgar información al bosque de confianza.  
  
-   **Transformación basada en tipos de notificación** manipula una reclamación antes de enviarlo al destino previsto. Usar transformación basada en tipos de notificación en el bosque de confianza para generalizar una reclamación conocida que contiene información específica. Puedes usar transformaciones para generalizar el tipo de notificación, el valor de notificación o ambos.  
  
## <a name="software-requirements"></a>Requisitos de software  
Dado reclamaciones y autenticación compuesta de Control de acceso dinámico requieren extensiones de autenticación de Kerberos, todos los dominios que admite el Control de acceso dinámico deben tener suficiente controladores de dominio que ejecutan las versiones compatibles de Windows para admitir la autenticación de clientes de Kerberos con reconocimiento de Control de acceso dinámico. De manera predeterminada, los dispositivos deben utilizar controladores de dominio en otros sitios. Si no hay controladores de dominio están disponibles, se producirá un error de autenticación. Por lo tanto, debe admitir una de las siguientes condiciones:  
  
-   Cada dominio que admite el Control de acceso dinámico debe tener suficiente controladores de dominio que ejecutan las versiones compatibles de Windows Server para admitir la autenticación de todos los dispositivos que ejecutan las versiones compatibles de Windows o Windows Server.  
  
-   Dispositivos que ejecutan las versiones compatibles de Windows o no proteger los recursos mediante notificaciones o identidad compuesto, debes deshabilitar la compatibilidad con el protocolo Kerberos para el Control de acceso dinámico.  
  
Para los dominios que admiten notificaciones de usuario, debe configurarse cada controlador de dominio que ejecutan las versiones compatibles de Windows server con el valor adecuado para admitir notificaciones y autenticación compuesta y para proporcionar protección de Kerberos. Configurar opciones de configuración en la directiva de plantilla administrativa de KDC como sigue:  
  
-   **Siempre proporcionar notificaciones** usar esta opción si todos los controladores de dominio ejecutan las versiones compatibles de Windows Server. Además, Establece el nivel funcional del dominio a Windows Server 2012 o superior.  
  
-   **Admite** al usar esta configuración, supervisar los controladores de dominio para garantizar que el número de controladores de dominio que ejecutan las versiones compatibles de Windows Server es suficiente para el número de equipos cliente que necesitan acceder a recursos protegidos por el Control de acceso dinámico.  
  
Si el dominio del usuario y el dominio del servidor de archivos están en diferentes bosques, todos los controladores de dominio raíz del bosque del servidor de archivos deben establecerse en el Windows Server 2012 o un mayor nivel funcional.  
  
Si los clientes no reconocen el Control de acceso dinámico, debe haber una relación de confianza bidireccional entre los dos bosques.  
  
Si se transforman reclamaciones cuando abandonan un bosque, todos los controladores de dominio raíz del bosque del usuario deben establecerse en el Windows Server 2012 o un mayor nivel funcional.  
  
Un servidor de archivos que ejecutan Windows Server 2012 o Windows Server 2012 R2 debe tener una configuración de directiva de grupo que especifica si necesita para obtener tokens de usuario que no incluyen notificaciones de las notificaciones de usuario. Este valor se establece de forma predeterminada para **automática**, lo que genera esta configuración de directiva de grupo para activar **en** si hay una directiva central que contiene las reclamaciones de usuario o el dispositivo para ese servidor de archivos. Si el servidor de archivos contiene discrecionales ACL que incluyan notificaciones de usuario, debes establecer esta directiva de grupo **en** para que el servidor conoce para solicitar notificaciones en el nombre de los usuarios que no proporcionan notificaciones para acceder al servidor.  
  
## <a name="additional-resource"></a>Recursos adicionales  
Para obtener información sobre la implementación de soluciones basadas en esta tecnología, vea [Control de acceso dinámico: información general del escenario](Dynamic-Access-Control--Scenario-Overview.md).  
  


