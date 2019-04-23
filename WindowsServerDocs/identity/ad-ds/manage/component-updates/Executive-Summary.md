---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Resumen ejecutivo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3eecec9d47f91bb6a9ba549abc3bf62482b2f49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863286"
---
# <a name="executive-summary"></a>Resumen ejecutivo

>Se aplica a: Windows Server 2012

>[!IMPORTANT] 
>La siguiente documentación se escribió en 2013 y se proporciona únicamente con fines históricos.  Actualmente, revisamos esta documentación y está sujeta a cambios.  No pueden reflejar los procedimientos recomendados actuales.

Ninguna organización con una infraestructura tecnológica (TI) es inmune a los ataques, pero se implementan los controles, los procesos y las directivas apropiadas para proteger segmentos clave de la infraestructura informática de la organización, es posible evitar que crezca a un riesgo por mayor en el entorno informático de un evento de infracción.  
  
Este resumen ejecutivo está pensado para ser útiles como un documento independiente resumir el contenido del documento, que contiene las recomendaciones que le ayudarán a las organizaciones mejorar la seguridad de sus instalaciones de Active Directory. Al implementar estas recomendaciones, será capaz de identificar y priorizar las actividades de seguridad, proteger segmentos clave de la infraestructura informática de su organización y crear controles que disminuyen significativamente la probabilidad de que las organizaciones ataques contra los componentes críticos del entorno de TI.  
  
Aunque este documento describen los ataques más comunes con Active Directory y contramedidas para reducir la superficie de ataque, contiene también recomendaciones para la recuperación en caso de puesta en peligro. Es la única forma de recuperar en caso de un riesgo importante para Active Directory para estar preparado para el riesgo antes de que suceda.  
  
Las secciones principales de este documento son:  
  
-   Vías de compromiso  
  
-   Reducción de la superficie de ataque de Active Directory  
  
-   Supervisión de Active Directory en busca de indicios de riesgo  
  
-   Planeación de compromiso  
  
## <a name="avenues-to-compromise"></a>Vías de compromiso  
Esta sección proporciona información sobre algunas de las vulnerabilidades con más frecuencia aprovechadas utilizadas por los atacantes para poner en peligro las infraestructuras de los clientes. Que contiene categorías generales de las vulnerabilidades y cómo se usan para inicialmente penetrar en infraestructuras de los clientes, propagar compromiso entre sistemas adicionales y detectar finalmente Active Directory y controladores de dominio para obtener una completa control de los bosques de la organización. No proporciona recomendaciones detalladas acerca de cómo tratar cada tipo de vulnerabilidad, especialmente en las áreas en que las vulnerabilidades no se usan para tener como destino directamente de Active Directory. Sin embargo, para cada tipo de vulnerabilidad, se proporcionan vínculos a información adicional que se utilizará para desarrollar contramedidas y reducir la superficie expuesta a ataques de la organización.  
  
Incluidos son los siguientes temas:  
  
-   **Inicial infracción destinos** -inician la mayoría de las infracciones de seguridad de información con el riesgo de pequeños trozos de sistemas de infraestructura-a menudo una o dos de la organización a la vez. Estos eventos iniciales, o puntos de entrada en la red, a menudo aprovechan las vulnerabilidades que se han corregido, pero que no estaban. Las vulnerabilidades habituales son:  
  
    -   Vacíos en las implementaciones de antivirus y antimalware.  
  
    -   Aplicación de revisiones incompleta  
  
    -   Sistemas operativos y aplicaciones obsoletas  
  
    -   configuración incorrecta  
  
    -   Falta de prácticas de desarrollo de aplicaciones seguras  
  
-   **Cuentas atractivas para el robo de credenciales** -ataques de robo de credenciales son aquellos en los que un atacante inicialmente obtiene acceso con privilegios a un equipo en una red y, a continuación, usa las herramientas disponibles gratuitamente para extraer las credenciales de las sesiones de otros ha iniciado sesión las cuentas.   
    En esta sección son los siguientes:  
  
    -   **Las actividades que aumentan la probabilidad de peligro** : porque el destino de robo de credenciales es normalmente las cuentas de dominio con privilegios elevados y "person muy importante" cuentas (VIP), es importante ser consciente de los administradores de actividades que aumentan la probabilidad de éxito de un ataque de robo de credenciales. Estas actividades son:  
  
        -   Iniciar sesión en equipos no protegidos con las cuentas con privilegios  
  
        -   Explorar Internet con una cuenta con privilegios elevados  
  
        -   Configuración de las cuentas con privilegios locales con las mismas credenciales en todos los sistemas  
  
        -   Overpopulation y el uso excesivo de grupos de dominio con privilegios  
  
        -   Administración insuficiente de la seguridad de los controladores de dominio.  
  
    -   **Propagación y elevación de privilegios** -cuentas específicas, servidores y componentes de infraestructura suelen ser los objetivos principales de ataques contra Active Directory. Estas cuentas son:  
  
        -   Cuentas con privilegios de forma permanente  
  
        -   Cuentas de VIP  
  
        -   "Conexión con privilegios" cuentas de Active Directory  
  
        -   Controladores de dominio  
  
        -   Otros servicios de infraestructura que afectan a la administración de identidades, acceso y la configuración, como servidores de infraestructura de clave pública (PKI) y servidores de administración de sistemas  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Reducción de la superficie de ataque de Active Directory  
En esta sección se centra en los controles técnicos para reducir la superficie de ataque de una instalación de Active Directory. En esta sección son los siguientes temas:  
  
-   El **cuentas privilegiadas y grupos en Active Directory** sección describen las cuentas con privilegios más altas y grupos en Active Directory y los mecanismos que protegen las cuentas con privilegios. Dentro de Active Directory, tres grupos integrados son los grupos de privilegio más altos en el directorio (administradores de organización, Admins. del dominio y administradores), aunque un número de cuentas y grupos adicionales también debe protegerse.  
  
-   El **implementar modelos administrativos de menor privilegio** sección se centra en la identificación de los riesgos que presenta el uso de cuentas con privilegios elevados para la administración diaria, además de proporcionar recomendaciones para reducir el riesgo.  
  
No sólo se encuentran en Active Directory privilegios excesivos en entornos en peligro. Cuando una organización ha desarrollado el hábito de conceder más privilegios del que es necesario, normalmente se encuentra en toda la infraestructura:  
  
-   En Active Directory  
  
-   En los servidores miembro  
  
-   En las estaciones de trabajo  
  
-   En las aplicaciones  
  
-   En los repositorios de datos  
  
-   El **implementar Hosts administrativos seguros** sección describen los hosts administrativos seguros, que son equipos que están configurados para admitir la administración de Active Directory y de sistemas conectados. Estos hosts dedicados a las funciones administrativas y no se ejecutan software como aplicaciones de correo electrónico, exploradores web o software de productividad (como Microsoft Office).  
  
En esta sección son los siguientes:  
  
-   **Principios para la creación de Hosts administrativos seguros** -los principios generales que tener en cuenta son:  
  
    -   No administrar nunca un sistema de confianza de un host de menor confianza.  
  
    -   No confíe en un único factor de autenticación al realizar actividades con privilegios.  
  
    -   No olvide la seguridad física al diseñar e implementar los hosts administrativos seguros.  
  
-   **Protección de controladores de dominio frente a ataque** -si un usuario malintencionado obtiene acceso con privilegios a un controlador de dominio, ese usuario puede modificar, alterar y destruir la base de datos de Active Directory y por extensión, todos los sistemas y las cuentas que son administrada por Active Directory.  
  
En esta sección son los siguientes temas:  
  
-   **Seguridad física de los controladores de dominio** -contiene recomendaciones para proporcionar seguridad física de los controladores de dominio en ubicaciones remotas, sucursales y centros de datos.  
  
-   **Sistemas operativos de controlador de dominio** -contiene recomendaciones para proteger sistemas operativos de controlador de dominio.  
  
-   **Proteger la configuración de controladores de dominio** -herramientas de configuración disponible de forma gratuita y nativo y la configuración puede utilizarse para crear líneas base de configuración para los controladores de dominio que posteriormente se pueden aplicar por (objetos de directiva de grupo de seguridad GPO).  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory en busca de indicios de riesgo  
Esta sección proporciona información acerca de las categorías de auditoría heredadas y las subcategorías de directiva de auditoría (que se introdujeron en Windows Vista y Windows Server 2008) y directiva de auditoría avanzada (que se introdujo en Windows Server 2008 R2). También proporciona información acerca de los eventos y objetos de supervisión pueden indicar los intentos de poner en peligro el entorno y algunas referencias adicionales que pueden usarse para construir una directiva de auditoría integral para Active Directory.  
  
En esta sección son los siguientes temas:  
  
-   **Directiva de auditoría de Windows** : registros de eventos de seguridad de Windows tienen categorías y subcategorías que determinan los eventos de seguridad se realiza el seguimiento y registra.  
  
-   **Recomendaciones de directiva de auditoría** -esta sección describe la configuración de directiva de auditoría de Windows de forma predeterminada, la configuración de directivas recomendadas por Microsoft y las recomendaciones más exigentes para las organizaciones a utilizar para auditar servidores críticos de auditoría y estaciones de trabajo.  
  
## <a name="planning-for-compromise"></a>Planeación de compromiso  
Esta sección contiene recomendaciones que le ayudará a las organizaciones a prepararse para un peligro antes de que suceda, implemente controles que pueden detectar un evento de riesgo antes de que se ha producido una infracción completa y ofrece instrucciones de respuesta y la recuperación para casos en los que se consigue un riesgo importante para el directorio por los atacantes. En esta sección son los siguientes temas:  
  
-   **Replanteamiento el enfoque** -contiene los principios y directrices para crear entornos seguros en los que una organización puede colocar sus activos más importantes. Estas instrucciones son las siguientes:  
  
    -   Identificación de los principios para separar y proteger los activos críticos  
  
    -   Definir un plan de migración limitada, en función del riesgo  
  
    -   Aprovechamiento de las migraciones "nonmigratory" cuando sea necesario  
  
    -   Implementación de "destrucción creative"  
  
    -   Aislar aplicaciones y sistemas heredados  
  
    -   Simplificación de seguridad para los usuarios finales  
  
-   **Mantener un entorno seguro más** -contiene recomendaciones de alto nivel diseñadas para utilizarse como directrices para usar en el desarrollo de seguridad eficaces, pero no solo administración efectiva del ciclo de vida. En esta sección son los siguientes temas:  
  
    -   **Crear centrado en los negocios prácticas de seguridad para Active Directory** : para administrar el ciclo de vida de los usuarios, datos, aplicaciones y sistemas administrados por Active Directory, siga estos principios de forma eficaz.  
  
        -   **Asignar una propiedad de negocio a los datos de Active Directory** -asignar la propiedad de componentes de infraestructura TI; para los datos que se agregan a Active Directory Domain Services (AD DS) para satisfacer las necesidades empresariales, por ejemplo, los nuevos empleados, las nuevas aplicaciones, y nuevos repositorios de información, una unidad de negocio designado o el usuario se debe asociar con los datos.  
  
        -   **Implementar la administración del ciclo de vida de Business-Driven** -administración del ciclo de vida debe implementarse para los datos en Active Directory.  
  
        -   **Clasificar todos los datos de Active Directory** -propietarios de negocios deben proporcionar la clasificación de datos de Active Directory. Dentro del modelo de clasificación de datos, se debe incluir la clasificación para los datos de Active Directory siguientes:  
  
            -   **Sistemas** -clasificar las poblaciones de servidor, su sistema operativo su rol, las aplicaciones que se ejecutan en ellos y la TI y los propietarios de empresas de registro.  
  
            -   **Aplicaciones** -clasificar las aplicaciones mediante la funcionalidad de base de usuarios y su sistema operativo.  
  
            -   **Los usuarios** -deben etiquetar y supervisar las cuentas en las instalaciones de Active Directory que suelen ser objetivo de los atacantes.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Resumen de procedimientos recomendados para proteger los servicios de dominio de Active Directory  
En la tabla siguiente proporciona un resumen de las recomendaciones proporcionadas en este documento para proteger la instalación de AD DS. Algunas prácticas recomendadas son estratégicas por naturaleza y requieren una planeación completa y proyectos de implementación. otras son tácticas y centradas en componentes específicos de Active Directory y la infraestructura relacionada.  
  
Procedimientos recomendados se muestran en orden de prioridad, que lo sea., los números más bajos indican la prioridad más alta. Donde procedas, mejores prácticas se identifican como preventivas o detective por naturaleza. Todas estas recomendaciones deben ser minuciosamente probados y modificar según sea necesario para las características y los requisitos de su organización.  
  
  
||**Procedimiento recomendado**|**Estratégica o táctica**|**Preventivas o Detective**|  
|-|-|-|-|  
|1|Revisión de aplicaciones.|Táctica|Preventivas|  
|2|Revisiones en los sistemas operativos.|Táctica|Preventivas|  
|3|Implementar y actualizar rápidamente el software antivirus y antimalware en todos los sistemas y el monitor para los intentos para quitar o deshabilitarlo.|Táctica|Ambos|  
|4|Supervisar confidenciales objetos de Active Directory para los intentos de modificación y Windows para los eventos que puedan indicar un riesgo de intento.|Táctica|Detective|  
|5|Proteger y supervisar las cuentas para los usuarios que tienen acceso a datos confidenciales|Táctica|Ambos|  
|6|Impedir que se utilice en sistemas no autorizados cuentas eficaces.|Táctica|Preventivas|  
|7|Eliminar la pertenencia permanente a grupos con muchos privilegios.|Táctica|Preventivas|  
|8|Implementar controles para conceder la pertenencia temporal a grupos con privilegios cuando sea necesario.|Táctica|Preventivas|  
|9|Implementar hosts administrativos seguros.|Táctica|Preventivas|  
|10|Utilice la lista blanca de la aplicación en los controladores de dominio, hosts administrativos y otros sistemas confidenciales.|Táctica|Preventivas|  
|11|Identificar los recursos críticos y dar prioridad a su seguridad y supervisión.|Táctica|Ambos|  
|12|Implementar controles de acceso basado en rol con privilegios mínimos para la administración del directorio, su infraestructura de soporte y los sistemas unidos a un dominio.|Estratégica|Preventivas|  
|13|Aislar las aplicaciones y sistemas heredados.|Táctica|Preventivas|  
|14|Retirar aplicaciones y sistemas heredados.|Estratégica|Preventivas|  
|15|Implementar programas de ciclo de vida de desarrollo seguro para aplicaciones personalizadas.|Estratégica|Preventivas|  
|16|Implementar la administración de configuración, revise el cumplimiento con regularidad y evaluar opciones con cada nueva versión de hardware o software.|Estratégica|Preventivas|  
|17|Migrar los recursos críticos a bosques puro con estrictas de seguridad y requisitos de supervisión.|Estratégica|Ambos|  
|18|Simplifique la seguridad para los usuarios finales.|Estratégica|Preventivas|  
|19|Usar firewalls basados en host para el control y las comunicaciones seguras.|Táctica|Preventivas|  
|20|Dispositivos de la revisión.|Táctica|Preventivas|  
|21|Implementar la administración centrada en el negocio del ciclo de vida para los activos de TI.|Estratégica|N/D|  
|22|Crear o actualizar los planes de recuperación de incidentes.|Estratégica|N/D|  
  


