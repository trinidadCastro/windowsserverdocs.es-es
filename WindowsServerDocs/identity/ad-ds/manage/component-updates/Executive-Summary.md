---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Resumen
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c218a4d66e8208cf627bc93be50bf11ea2fbf862
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="executive-summary"></a>Resumen

>Se aplica a: Windows Server 2012

>[!IMPORTANT] 
>La siguiente documentación se escribió en 2013 y se proporciona únicamente con fines históricos.  Actualmente revisamos esta documentación y está sujeta a cambios.  No pueden reflejar los procedimientos recomendados actuales.

Ninguna organización con una infraestructura de TI de información es inmune de los ataques, pero si se implementan los controles, los procesos y las directivas apropiadas para proteger claves segmentos de infraestructura de la organización, es posible que sea posible evitar que un evento de incumplimiento de crecimiento en peligro el entorno informático de venta por mayor.  
  
Este resumen ejecutivo está pensado para ser útiles como un documento independiente resumir el contenido del documento, que incluye recomendaciones que le ayudarán a las organizaciones en lo que mejora la seguridad de sus instalaciones de Active Directory. Al implementar estas recomendaciones, las organizaciones podrán identificar y dar prioridad a las actividades de seguridad, proteger segmentos clave de la infraestructura informática de su organización y crear controles que reducir considerablemente la probabilidad de ataques contra los componentes críticos del entorno de TI.  
  
Aunque este documento se describe cómo los ataques contra Active Directory y contramedidas para reducir la superficie de ataque más comunes, también contiene recomendaciones para la recuperación en caso de peligro todo. La única forma recuperarse en el caso de un compromiso completo de Active Directory es estar preparado para el compromiso antes de que suceda.  
  
Las secciones principales de este documento son:  
  
-   Vías peligro  
  
-   Reducir la superficie de ataque de Active Directory  
  
-   Supervisión de Active Directory para signos de compromiso  
  
-   Planeación de comprometer  
  
## <a name="avenues-to-compromise"></a>Vías peligro  
Esta sección proporciona información sobre algunas de las vulnerabilidades aprovechadas normalmente usadas los atacantes para poner en peligro infraestructuras de los clientes. Contiene las categorías generales de las vulnerabilidades y cómo se utilizan inicialmente atraviese infraestructuras de los clientes, propagación comprometer en sistemas adicionales y seleccionar como destino finalmente Active Directory y controladores de dominio para obtener el control total de los bosques de la organización. No proporciona recomendaciones detalladas sobre direccionamiento cada tipo de vulnerabilidad, especialmente en las áreas en las que las vulnerabilidades no se usan para dirigirse directamente a Active Directory. Sin embargo, para cada tipo de vulnerabilidad, proporcionamos vínculos a información adicional a usar para desarrollar contramedidas y reducir la superficie de ataque de la organización.  
  
Incluye son los siguientes temas:  
  
-   **Inicial incumplimiento destinos** -la mayoría de las infracciones de seguridad de información que se inicie con el compromiso de pequeños fragmentos de la organización infraestructura frecuencia uno o dos sistemas a la hora. Estos eventos iniciales, o puntos de entrada a la red, a menudo aprovechan las vulnerabilidades que se han corregido podrían, pero no. Las vulnerabilidades habituales son:  
  
    -   Diferencias en las implementaciones de antivirus y antimalware  
  
    -   Aplicación de revisión incompleta  
  
    -   Obsoletas aplicaciones y sistemas operativos  
  
    -   Error de configuración  
  
    -   Falta de las prácticas de desarrollo de aplicaciones seguras  
  
-   **Cuentas atractivas para el robo de credenciales** -ataques de robo de credenciales son los en el que un atacante obtiene inicialmente con privilegios de acceso a un equipo en una red y, a continuación, usa las herramientas disponibles de forma gratuita para extraer las credenciales de las sesiones de otras cuentas de inicio de sesión.   
    En esta sección son los siguientes:  
  
    -   **Las actividades que aumentan la probabilidad de compromiso** : ya que el destino de robo de credenciales suele ser cuentas de dominio con privilegios elevados y "persona muy importante" cuentas (VIP), es importante para los administradores ser consciente de actividades que aumentan la probabilidad de éxito de un ataque de robo de credenciales. Estas actividades son:  
  
        -   Inicio de sesión en equipos no protegidos con cuentas con privilegios  
  
        -   Navegar por Internet con una cuenta con privilegios elevados  
  
        -   Configurar cuentas con privilegios locales con las mismas credenciales en sistemas  
  
        -   Overpopulation y el uso excesivo de grupos con privilegios de dominio  
  
        -   No hay suficiente administración de la seguridad de los controladores de dominio.  
  
    -   **La propagación de elevación de privilegios** -cuentas específicas, servidores y componentes de infraestructura por lo general son los principales objetivos de ataques contra Active Directory. Estas cuentas son:  
  
        -   Cuentas con privilegios de forma permanente  
  
        -   Cuentas VIP  
  
        -   "Adjuntos privilegios" cuentas de Active Directory  
  
        -   Controladores de dominio  
  
        -   Otros servicios de infraestructura que afectan a la administración de identidad, el acceso y la configuración, como los servidores de infraestructura de clave pública (PKI) y servidores systems management  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Reducir la superficie de ataque de Active Directory  
En esta sección se centra en los controles técnicos para reducir la superficie de ataque de una instalación de Active Directory. En esta sección son los siguientes temas:  
  
-   La **cuentas con privilegios y los grupos de Active Directory** sección describen las cuentas con privilegios más altas y grupos en Active Directory y los mecanismos que están protegidas cuentas con privilegios. Dentro de Active Directory, tres grupos integrados son los grupos de privilegios más altos en el directorio (administradores de empresa, administradores de dominio y administradores), aunque un número de otros grupos y cuentas se debe proteger.  
  
-   La **implementar modelos administrativos de privilegios mínimos** se centra la sección acerca de cómo identificar el riesgo de que el uso de privilegios muy tenga en cuenta que presenta administración diaria, además a proporcionar recomendaciones para reducir el riesgo.  
  
Privilegios excesivo solo no se encuentran en Active Directory en entornos en peligro. Cuando una organización ha desarrollado adquirir el hábito de concesión de privilegios más de lo necesario, normalmente se encuentra en toda la infraestructura:  
  
-   En Active Directory  
  
-   En los servidores miembro  
  
-   En estaciones de trabajo  
  
-   En las aplicaciones  
  
-   En los repositorios de datos  
  
-   La **implementar seguro Hosts administrativas** sección describe hosts administrativos seguros, que son equipos que están configurados para admitir la administración de Active Directory y sistemas conectados. Estos hosts se dedican a funciones administrativas y no ejecutarán de software, como aplicaciones de correo electrónico, los exploradores web o software de productividad (como Microsoft Office).  
  
En esta sección son los siguientes:  
  
-   **Principios para la creación de Hosts administrativas seguro** -son los principios generales que debes tener en cuenta:  
  
    -   No administrar nunca un sistema de confianza desde un host de menor confianza.  
  
    -   No dependen de un factor de autenticación solo cuando se lleve a cabo actividades con privilegios.  
  
    -   No te olvides de seguridad física, al diseñar e implementar hosts administrativos seguros.  
  
-   **Proteger los controladores de dominio contra ataques** -si un usuario malintencionado obtiene acceso con privilegios en un controlador de dominio, que el usuario puede modificar, dañar y destruir la base de datos de Active Directory y por extensión, todos los sistemas y las cuentas que administran por Active Directory.  
  
En esta sección son los siguientes temas:  
  
-   **Seguridad física para controladores de dominio** -incluye recomendaciones para proporcionar seguridad física para controladores de dominio de centros de datos, las sucursales y ubicaciones remotas.  
  
-   **Sistemas operativos de controlador de dominio** -contiene recomendaciones para proteger los sistemas operativos de controlador de dominio.  
  
-   **Proteger la configuración de controladores de dominio** -herramientas de configuración disponibles de forma gratuita y nativos y opciones de configuración que pueden usarse para crear líneas de base de configuración para controladores de dominio que posteriormente se pueden aplicar por los objetos de directiva de grupo (GPO) de seguridad.  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory para signos de compromiso  
En esta sección se proporciona información sobre la auditoría heredada categorías y subcategorías de directiva de auditoría (que se introdujeron en Windows Vista y Windows Server 2008) y directiva avanzada de auditoría (que se introdujo en Windows Server 2008 R2). También proporciona información sobre los eventos y objetos que se supervisan pueden indicar intentos de comprometer el entorno y algunas referencias adicionales que se puede usar para crear una directiva de auditoría completa para Active Directory.  
  
En esta sección son los siguientes temas:  
  
-   **Directiva de auditoría de Windows** : registros de eventos de seguridad de Windows tienen categorías y subcategorías que determinan los eventos de seguridad se realiza un seguimiento y registra.  
  
-   **Recomendaciones de la directiva de auditoría** -esta sección describe la configuración de directiva de auditoría predeterminada de Windows, configuración de directiva que se recomienda por Microsoft y recomendaciones más exigentes para las organizaciones utilizar para auditar críticos servidores y estaciones de trabajo de auditoría.  
  
## <a name="planning-for-compromise"></a>Planeación de comprometer  
Esta sección contiene las recomendaciones que te ayudarán a las organizaciones a preparar un compromiso antes de esto ocurra, implementación de controles que puede detectar un evento de comprometer antes de que se ha producido una infracción completa y se proporcionan directrices de respuesta y la recuperación en los casos en que se consigue un compromiso completado del directorio de los atacantes. En esta sección son los siguientes temas:  
  
-   **Replanteamiento el enfoque** -contiene los principios y las directrices para crear los entornos de seguridad en el que una organización puede ubicar sus activos más importantes. Estas directrices son los siguientes:  
  
    -   Identificar los principios para separar y proteger los activos críticos  
  
    -   Definir un plan de migración limitado, en función del riesgo  
  
    -   Sacar provecho de las migraciones "nonmigratory" cuando es necesario  
  
    -   Implementar "creativa destrucción"  
  
    -   Aislar sistemas heredados y aplicaciones  
  
    -   La simplificación de seguridad para los usuarios finales  
  
-   **Mantenimiento de un entorno seguro más** -incluye recomendaciones de alto nivel diseñadas para usarse como instrucciones para usar en el desarrollo de seguridad eficaz, pero no solo administración eficaz del ciclo de vida. En esta sección son los siguientes temas:  
  
    -   **Crear centrado en los negocios procedimientos de seguridad de Active Directory** - para administrar el ciclo de vida de los usuarios, datos, aplicaciones y sistemas que administrados por Active Directory eficazmente, sigue estos principios.  
  
        -   **Asignar una propiedad de negocio a los datos de Active Directory** -asignar la propiedad de los componentes de infraestructura para TI; para los datos que se agregan a los servicios de dominio de Active Directory (AD DS) para admitir la empresa, por ejemplo, los nuevos empleados, nuevas aplicaciones y los repositorios de información nueva, un usuario o una unidad de negocio designado debe asociarse con los datos.  
  
        -   **Implementar la administración del ciclo de vida de Business-Driven** -administración del ciclo de vida debe implementarse para los datos de Active Directory.  
  
        -   **Clasificar todos los datos de Active Directory** -propietarios de empresas deben proporcionar la clasificación para datos de Active Directory. Dentro del modelo de clasificación de datos, debe incluirse de clasificación para los siguientes datos de Active Directory:  
  
            -   **Sistemas** -clasificar poblaciones de servidor, su sistema operativo su función, las aplicaciones que se ejecutan en ellos y de TI y los propietarios de empresas de registro.  
  
            -   **Aplicaciones** -clasificar las aplicaciones por funcionalidad, base de usuarios y su sistema operativo.  
  
            -   **Los usuarios** -deben ser etiquetadas y supervisar las cuentas en las instalaciones de Active Directory que están más probable que el objetivo de los atacantes.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Resumen de los procedimientos recomendados para proteger los servicios de dominio de Active Directory  
La siguiente tabla proporciona un resumen de las recomendaciones proporcionadas en este documento para proteger una instalación de AD DS. Algunas recomendaciones son de naturaleza estratégica y requieren planificación integral y proyectos de implementación. otros son táctica y apuntar a los componentes específicos de Active Directory y la infraestructura relacionada.  
  
Procedimientos se enumeran en orden aproximado de prioridad, ese Salomón, bajas indican una mayor prioridad. Donde procedas, mejores prácticas se identifican como preventivas o detective naturaleza. Todas estas recomendaciones deben minuciosamente probado y modificar según sea necesario para las características y los requisitos de la organización.  
  
  
|-|-|-|-|  
||**Procedimiento recomendado**|**táctica o estratégica**|**preventivas o Detective**|  
| 1 | Revisiones de aplicaciones. | Táctica | Preventivas |  
| 2 | Sistemas operativos de revisiones. | Táctica | Preventivas |  
| 3 | Implementar y actualizar rápidamente software antivirus y antimalware en todos los sistemas y monitor para intentos para quitar o deshabilitarla. | Táctica | Ambos |  
| 4 | Supervisar los objetos de Active Directory con información confidencial para intentos de modificación y de Windows para los eventos que se pueden indicar intento comprometer. | Táctica | Detective |  
| 5 | Proteger y supervisar las cuentas de usuarios que tienen acceso a datos confidenciales | Táctica | Ambos |  
| 6 | Impedir que se usa en sistemas no autorizados cuentas potentes. | Táctica | Preventivas |  
| 7 | Eliminar permanente pertenencia a grupos con privilegios elevados. | Táctica | Preventivas |  
| 8 | Implementar controles para conceder temporal pertenencia a grupos con privilegios cuando sea necesario. | Táctica | Preventivas |  
| 9 | Implementar hosts administrativos seguros. | Táctica | Preventivas |  
| 10 | Usar la lista blanca de la aplicación en los controladores de dominio, hosts administrativos y otros sistemas con información confidencial. | Táctica | Preventivas |  
| 11 | Identificar los activos críticos y dar prioridad a su seguridad y supervisión. | Táctica | Ambos |  
| 12 | Implementar controles de acceso basado en función de privilegios mínimos para la administración de sistemas unidos a un dominio, su infraestructura de soporte y el directorio. | Estratégica | Preventivas |  
| 13 | Aislar aplicaciones y sistemas heredados. | Táctica | Preventivas |  
| 14 | Retirada de aplicaciones y sistemas heredados. | Estratégica | Preventivas |  
| 15 | Implementar desarrollo seguro de programas del ciclo de vida para las aplicaciones personalizadas. | Estratégica | Preventivas |  
| 16 | Implementar la administración de configuración, revisa el cumplimiento con regularidad y evaluar la configuración con cada nueva versión de software o hardware. | Estratégica | Preventivas |  
| 17 | Migrar los activos críticos a bosques impecables con seguridad más estrictos y los requisitos de seguimiento. | Estratégica | Ambos |  
| 18 | Simplificar la seguridad para los usuarios finales. | Estratégica | Preventivas |  
| 19 | Usar los servidores de seguridad basada en host para el control y proteger las comunicaciones. | Táctica | Preventivas |  
| 20 | Revisión de dispositivos. | Táctica | Preventivas |  
| 21 | Implementar la administración del ciclo de vida centrado en los negocios para los activos de TI. | Estratégica | N/D |  
| 22 | Crear o actualizar los planes de recuperación incidentes. | Estratégica | N/D |  
  


