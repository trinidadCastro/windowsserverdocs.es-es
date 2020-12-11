---
description: 'Más información sobre: información general sobre el Access Control dinámico'
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: Introducción al control de acceso dinámico
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: fb0099b730e21e9dd9208bd1ba12c35c487bdd86
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044483"
---
# <a name="dynamic-access-control-overview"></a>Introducción al control de acceso dinámico

>Se aplica a: Windows Server 2012 R2, Windows Server 2012

En este tema introductorio para profesionales de TI se describe el Control de acceso dinámico y sus elementos asociados, que se introdujeron en Windows Server 2012 y Windows 8.

El control de acceso dinámico basado en el dominio permite a los administradores aplicar permisos de control de acceso y restricciones de acuerdo a reglas bien definidas que pueden contemplar la confidencialidad de los recursos, el trabajo o el rol del usuario y la configuración del dispositivo que se usa para acceder a tales recursos.

Por ejemplo, es posible que un usuario tenga permisos diferentes si tiene acceso a un recurso desde el equipo de su oficina o si usa un equipo portátil a través de una red privada virtual. También puede suceder que solo tenga acceso si el dispositivo cumple con los requisitos de seguridad que definen los administradores de la red. Cuando se usa el Access Control dinámico, los permisos de un usuario cambian dinámicamente sin intervención adicional del administrador si cambia el trabajo o el rol del usuario (lo que da lugar a cambios en los atributos de la cuenta del usuario en AD DS).

No se admite el Control de acceso dinámico en sistemas operativos Windows anteriores a Windows Server 2012 y Windows 8. Cuando se configura el Control de acceso dinámico en entornos con versiones admitidas y no admitidas de Windows, solo las versiones admitidas implementarán los cambios.

Entre las características y conceptos asociados con el Control de acceso dinámico se incluyen las siguientes:

-   [Reglas de acceso central](#BKMK_Rules)

-   [Directivas de acceso central](#BKMK_Policies)

-   [Notificaciones](#BKMK_Claims)

-   [Expresiones](#BKMK_Expressions2)

-   [Permisos propuestos](#BKMK_Permissions2)

### <a name="central-access-rules"></a><a name="BKMK_Rules"></a>Reglas de acceso central
Una regla de acceso central es una expresión de reglas de autorización que puede incluir una o más condiciones que competen a grupos de usuarios, notificaciones de usuario, notificaciones de dispositivos y propiedades de recursos. Se pueden combinar varias reglas de acceso central en una directiva de acceso central.

Si se han definido una o más reglas de acceso central relativas a un dominio, los administradores de recursos compartidos de archivos pueden relacionar reglas específicas a recursos y requisitos empresariales concretos.

### <a name="central-access-policies"></a><a name="BKMK_Policies"></a>Directivas de acceso central
Las directivas de acceso central son directivas de autorización que incluyen expresiones condicionales. Por ejemplo, supongamos que una organización tiene un requisito empresarial de restringir el acceso a la información de identificación personal (PII) en los archivos solo al propietario del archivo y a los miembros del Departamento de recursos humanos (HR) que tienen permiso para ver la información de PII. Se trata de una directiva de toda la organización que se aplica a archivos de PII que estén en servidores de archivos de la organización. Para implementar esta directiva, una organización debe ser capaz de:

-   Identificar y marcar los archivos que contienen PII.

-   Identificar el grupo de los miembros de RR.HH. que están autorizados para ver información PII.

-   Agregar la directiva de acceso central a una regla de acceso central y aplicar dicha regla a todos los archivos que contengan PII, que se encuentren entre los servidores de archivos de toda la organización.

Las directivas de acceso central actúan como paraguas de seguridad que una organización aplica entre los servidores. Estas directivas son complementarias a las directivas de acceso local o a las listas de control de acceso discrecional (DACL) que se aplican a archivos y carpetas (si bien no las sustituyen).

### <a name="claims"></a><a name="BKMK_Claims"></a>Surja
Una notificación es una información única sobre un usuario, un dispositivo o un recurso que publicó un controlador de dominio. El título del usuario, la clasificación del Departamento de un archivo o el estado de mantenimiento de un equipo son ejemplos válidos de una demanda. Una entidad puede conllevar más de una notificación y se puede usar cualquier combinación de notificaciones para autorizar el acceso a los recursos. Están disponibles los siguientes tipos de notificaciones en las versiones admitidas de Windows:

-   **Notificaciones de usuario**: atributos de Active Directory relacionados con un usuario específico.

-   **Notificaciones de dispositivo**: atributos de Active Directory relacionados con un objeto de equipo específico.

-   **Atributos de recurso**: propiedades de recurso global marcadas para su uso en decisiones de autorización y que se publican en Active Directory.

Las notificaciones permiten a los administradores crear instrucciones precisas en toda la organización o la empresa acerca de los usuarios, los dispositivos y los recursos que se pueden incorporar en expresiones, reglas y directivas.

### <a name="expressions"></a><a name="BKMK_Expressions2"></a>Expresiones
Las expresiones condicionales constituyen una mejora en la administración del control de acceso. Con ellas se permite o deniega el acceso a los recursos solo si se cumplen ciertas condiciones, por ejemplo, la pertenencia a un grupo, la ubicación o estado de seguridad del dispositivo. Las expresiones se administran a través del cuadro de diálogo Configuración de seguridad avanzada del Editor ACL o del Editor de reglas de acceso central en el Centro de administración de Active Directory (ADAC).

Las expresiones ayudan a los administradores a administrar acceso a recursos confidenciales con condiciones flexibles en entornos empresariales cada vez más complejos.

### <a name="proposed-permissions"></a><a name="BKMK_Permissions2"></a>Permisos propuestos
Con los permisos propuestos, un administrador puede modelar con más precisión el impacto de los posibles cambios en la configuración de control de acceso sin materializarlos de facto.

Predecir el acceso eficaz a un recurso ayuda a planear y configurar permisos para esos recursos antes de implementar dichos cambios.

## <a name="additional-changes"></a>Cambios adicionales
Otras mejoras en las versiones compatibles de Windows que admiten Control de acceso dinámico incluyen las siguientes:

### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Compatibilidad con el protocolo de autenticación Kerberos para proporcionar notificaciones de usuario, notificaciones de dispositivo y grupos de dispositivo de manera confiable.
De manera predeterminada, los dispositivos que ejecutan cualquiera de las versiones admitidas de Windows pueden procesar vales Kerberos relacionados con el Control de acceso dinámico, que incluyen los datos necesarios para la autenticación compuesta. Los controladores de dominio pueden emitir vales Kerberos y responder a ellos con información relacionada con la autenticación compuesta. Cuando un dominio se configura para que reconozca el Control de acceso dinámico, los dispositivos reciben notificaciones de los controladores de dominio durante la autenticación inicial y reciben vales de autenticación compuesta al enviar solicitudes de vales de servicio. La autenticación compuesta genera un token de acceso que incluye la identidad del usuario y el dispositivo en recursos que reconocen el Control de acceso dinámico.

### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Compatibilidad para usar la configuración de la directiva de grupo del Centro de distribución de claves (KDC) para habilitar el control de acceso dinámico de un dominio.
Cada controlador de dominio debe tener la misma configuración de directiva de plantilla administrativa, que se encuentra en **Configuración del equipo\Directivas\Plantillas administrativas\Sistema\KDC\Compatibilidad con el Control de acceso dinámico y protección de Kerberos**.

### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Compatibilidad para usar la configuración de la directiva de grupo del Centro de distribución de claves (KDC) para habilitar el control de acceso dinámico de un dominio.
Cada controlador de dominio debe tener la misma configuración de directiva de plantilla administrativa, que se encuentra en **Configuración del equipo\Directivas\Plantillas administrativas\Sistema\KDC\Compatibilidad con el Control de acceso dinámico y protección de Kerberos**.

### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>Compatibilidad en Active Directory para almacenar notificaciones de usuario y dispositivo, propiedades de recursos y objetos de directiva de acceso central.

### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>Compatibilidad con el uso de la directiva de grupo para implementar objetos de directiva de acceso central.
La siguiente nueva configuración de directiva de grupo le permite implementar objetos de directiva de acceso central en los servidores de archivos de la organización **Configuración del equipo\Directivas\Configuración de Windows\Configuración de seguridad\Sistema de archivos\Directiva de acceso central**.

### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>Compatibilidad con la autorización y auditoría de archivo basadas en notificaciones para sistemas de archivos usando la directiva de grupo y la Auditoría de acceso a objetos global
Debe habilitar la auditoría de directiva de acceso central almacenada provisionalmente para auditar la eficacia de acceso de una directiva de acceso central mediante permisos propuestos. Este valor se puede configurar para el equipo en **Configuración de directiva de auditoría avanzada** en la **Configuración de seguridad** de un objeto de directiva de grupo (GPO). Tras definir la configuración de seguridad en GPO, podrá implementarlo en los equipos de la red.

### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>Compatibilidad para transformar o filtrar objetos de directiva de notificación que atraviesan las confianzas del bosque de Active Directory
Puede filtrar o transformar las notificaciones entrantes o salientes que atraviesan la confianza de un bosque. Existen tres escenarios básicos de filtrado y transformación de notificaciones:

-   **Filtrado basado en valores**: los filtros pueden basarse en el valor de una notificación. Esto hace que el bosque de confianza impida que se envíen notificaciones con ciertos valores al bosque que confía. Los controladores de dominio en los bosques que confían pueden usar el filtrado basado en valores para ofrecer protección contra un ataque por elevación de privilegios al filtrar las notificaciones entrantes con valores específicos del bosque de confianza.

-   **Filtrado basado en el tipo de notificación**: filtros basados en el tipo de notificación, y no en el valor de la notificación. Se identifica el tipo de notificación a partir del nombre de la notificación. Puede usar el filtrado basado en el tipo de notificación en el bosque de confianza e impedir que Windows envíe notificaciones que revelen información al bosque que confía.

-   **Transformación basada en el tipo de notificación**: manipula una notificación antes de enviarla al objetivo previsto. La transformación basada en el tipo de notificación se usa en el bosque de confianza para generalizar una notificación conocida que contiene información específica. Estas transformaciones pueden servir para generalizar el tipo de notificación, el valor de la notificación o ambos aspectos.

## <a name="software-requirements"></a>Requisitos de software
Dado que las notificaciones y la autenticación compuesta de Control de acceso dinámico requieren extensiones de autenticación Kerberos, cualquier dominio que admita el Control de acceso dinámico debe contar con suficientes controladores de dominio que ejecuten las versiones compatibles de Windows para admitir la autenticación de los clientes Kerberos compatibles con el Control de acceso dinámico. De forma predeterminada, los dispositivos deben usar controladores de dominio en otros sitios. Si no hay disponible ninguno de esos controladores de dominio, se generará un error de autenticación. Por lo tanto, se debe dar cabida a una de las siguientes condiciones:

-   Todos los dominios que admiten el Control de acceso dinámico deben tener suficientes controladores de dominio que ejecuten las versiones compatibles de Windows Server para admitir la autenticación en todos los dispositivos que ejecutan las versiones admitidas de Windows o Windows Server.

-   Los dispositivos que ejecuten las versiones compatibles de Windows o que no protejan los recursos con notificaciones o con identidad compuesta deben deshabilitar la compatibilidad del protocolo Kerberos con el control de acceso dinámico.

En cuanto a los dominios que admiten notificaciones de usuario, cada controlador de dominio que ejecute las versiones compatibles de Windows debe estar configurado con los parámetros apropiados para admitir notificaciones y autenticación compuesta y proporcionar protección de Kerberos. Configure las opciones de la directiva de plantilla administrativa de KDC de la siguiente manera:

-   **Siempre proporcionar notificaciones**: use esta opción si todos los controladores de dominio ejecutan las versiones compatibles de Windows Server. Además, establezca el nivel funcional del dominio a Windows Server 2012 o posterior.

-   **Compatible**: si usa esta opción, supervise los controladores de dominio para asegurarse de que la cantidad de controladores de dominio que ejecutan las versiones compatibles de Windows Server sea suficiente para la cantidad de equipos cliente que necesita acceso a los recursos protegidos por el control de acceso dinámico.

Si el dominio del usuario y el dominio del servidor de archivos se encuentran en bosques diferentes, todos los controladores de dominio de la raíz del bosque del servidor de archivos deben establecerse en el nivel funcional de Windows Server 2012 o superior.

Si los clientes no reconocen el control de acceso dinámico, debe existir una relación de confianza bidireccional entre los dos bosques.

Si las notificaciones se transforman cuando abandonan un bosque, todos los controladores de dominio de la raíz del bosque del usuario deben establecerse en el nivel funcional de Windows Server 2012 o superior.

Un servidor de archivos que ejecuta Windows Server 2012 o Windows Server 2012 R2 debe tener una configuración de directiva de grupo que especifica si es necesario obtener notificaciones de usuario para los tokens de usuario que no transportan notificaciones. Esta configuración está establecida de forma predeterminada en **Automática**, lo que hace que esta configuración de directiva de grupo se **active** si hay una directiva central que contiene notificaciones de usuario o dispositivo para dicho servidor de archivos. Si el servidor de archivos contiene ACL discrecionales que incluyen notificaciones de usuario, deberáactivar esta directiva de grupo para que el servidor de archivos sepa que tiene que solicitar notificaciones en nombre de los usuarios que no proporcionen notificaciones cuando obtengan acceso al servidor.

## <a name="additional-resource"></a>Recurso adicional
Para obtener información sobre la implementación de soluciones basadas en esta tecnología, vea [Access Control dinámica: información general sobre el escenario](Dynamic-Access-Control--Scenario-Overview.md).



