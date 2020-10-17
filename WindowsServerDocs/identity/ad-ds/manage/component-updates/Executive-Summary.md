---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Resumen ejecutivo
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/08/2018
ms.topic: article
ms.openlocfilehash: ffd3cf45d276445bca36f9e01651b74468446f61
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155930"
---
# <a name="executive-summary"></a>Resumen ejecutivo

>Se aplica a: Windows Server 2012

>[!IMPORTANT]
>La siguiente documentación se escribió en 2013 y se proporciona solo con fines históricos.  Actualmente se está revisando esta documentación y está sujeta a cambios.  Puede que no refleje las prácticas recomendadas actuales.

Ninguna organización con una infraestructura de tecnología de la información (TI) es inmune a los ataques, pero si se implementan las directivas, los procesos y los controles adecuados para proteger los segmentos clave de la infraestructura informática de una organización, es posible evitar que un evento de infracción crezca hasta un riesgo mayorista del entorno informático.

Este Resumen Ejecutivo está pensado para ser útil como un documento independiente que resume el contenido del documento, que contiene recomendaciones que ayudarán a las organizaciones a mejorar la seguridad de sus instalaciones Active Directory. Mediante la implementación de estas recomendaciones, las organizaciones podrán identificar y priorizar las actividades de seguridad, proteger los segmentos clave de la infraestructura informática de su organización y crear controles que disminuyan significativamente la probabilidad de ataques exitosos contra componentes críticos del entorno de ti.

Aunque este documento describe los ataques más comunes contra Active Directory y contramedidas para reducir la superficie expuesta a ataques, también contiene recomendaciones para la recuperación en caso de que se produzca un riesgo completo. La única forma de garantizar la recuperación en caso de que se produzca un riesgo completo de Active Directory se debe preparar para el riesgo antes de que ocurra.

Las secciones principales de este documento son:

- Vías de compromiso

- Reducción de la superficie de ataque de Active Directory

- Supervisión de Active Directory en busca de indicios de riesgo

- Planeación de compromiso

## <a name="avenues-to-compromise"></a>Vías de compromiso
En esta sección se proporciona información sobre algunas de las vulnerabilidades de uso más frecuente utilizadas por los atacantes para poner en peligro las infraestructuras de los clientes. Contiene categorías generales de vulnerabilidades y cómo se usan para penetrar inicialmente las infraestructuras de los clientes, propagar el compromiso entre sistemas adicionales y, finalmente, dirigirse a Active Directory y controladores de dominio para obtener el control completo de los bosques de la organización. No proporciona recomendaciones detalladas sobre cómo abordar cada tipo de vulnerabilidad, especialmente en las áreas en las que las vulnerabilidades no se usan para dirigirse directamente a Active Directory. Sin embargo, para cada tipo de vulnerabilidad, se proporcionan vínculos a información adicional que se utiliza para desarrollar contramedidas y reducir la superficie expuesta a ataques de la organización.

Se incluyen los siguientes temas:

- **Objetivos de infracción iniciales** : la mayoría de las infracciones de seguridad de la información comienzan con el riesgo de pequeñas partes de la infraestructura de una organización, a menudo uno o dos sistemas a la vez. Estos eventos iniciales, o puntos de entrada en la red, a menudo aprovechan vulnerabilidades que podrían haberse corregido, pero no. Las vulnerabilidades más comunes son las siguientes:
  - Brechas en las implementaciones de antivirus y antimalware
  - Revisión incompleta
  - Aplicaciones y sistemas operativos obsoletos
  - Error de configuración
  - Falta de prácticas de desarrollo de aplicaciones seguras

- **Cuentas atractivas para el robo de credenciales** : los ataques de robo de credenciales son aquellos en los que un atacante gana inicialmente el acceso con privilegios a un equipo de una red y, a continuación, usa herramientas disponibles gratuitamente para extraer las credenciales de las sesiones de otras cuentas que han iniciado sesión. En esta sección se incluyen los siguientes:
  - **Actividades que aumentan la probabilidad de poner en peligro** : dado que el objetivo del robo de credenciales suele ser cuentas de dominio con privilegios elevados y cuentas de "persona muy importante" (VIP), es importante que los administradores sean conscientes de las actividades que aumentan la probabilidad de que se produzca un ataque de robo de credenciales. Estas actividades son las siguientes:

    - Inicio de sesión en equipos no seguros con cuentas con privilegios

    - Explorar Internet con una cuenta con privilegios elevados

    - Configuración de cuentas con privilegios locales con las mismas credenciales en varios sistemas

    - Sobrellenado y uso excesivo de grupos de dominio con privilegios

    - Administración insuficiente de la seguridad de los controladores de dominio.

  - La **elevación de privilegios y** las cuentas, los servidores y los componentes de infraestructura específicos de la propagación son normalmente los objetivos principales de los ataques contra Active Directory. Estas cuentas son:

    - Cuentas con privilegios permanentes

    - Cuentas de VIP

    - Cuentas de Active Directory con privilegios adjuntos

    - Controladores de dominios

    - Otros servicios de infraestructura que afectan a la administración de identidades, acceso y configuración, como servidores de infraestructura de clave pública (PKI) y servidores de administración de sistemas

## <a name="reducing-the-active-directory-attack-surface"></a>Reducción de la superficie de ataque de Active Directory
Esta sección se centra en los controles técnicos para reducir la superficie expuesta a ataques de una instalación de Active Directory. En esta sección se incluyen los siguientes temas:

- En la sección **cuentas y grupos con privilegios de Active Directory** se describen las cuentas y los grupos con privilegios más altos de Active Directory y los mecanismos por los que se protegen las cuentas con privilegios. Dentro de Active Directory, tres grupos integrados son los grupos de privilegios más altos del directorio (administradores de organización, Admins. del dominio y administradores), aunque también se debe proteger un número de grupos y cuentas adicionales.

- La sección **implementación de modelos administrativos Least-Privilege** se centra en identificar el riesgo de que se presente el uso de cuentas con privilegios elevados para la administración diaria, además de proporcionar recomendaciones para reducir el riesgo.

Los privilegios excesivos no se encuentran solo en Active Directory en entornos en peligro. Cuando una organización ha desarrollado el hábito de conceder más privilegios de los necesarios, normalmente se encuentra en toda la infraestructura:

- En Active Directory

- En servidores miembro

- En las estaciones de trabajo

- En aplicaciones

- En repositorios de datos

- La sección **implementación de hosts administrativos seguros** describe hosts administrativos seguros, que son equipos que están configurados para admitir la administración de Active Directory y sistemas conectados. Estos hosts están dedicados a la funcionalidad administrativa y no ejecutan software como aplicaciones de correo electrónico, exploradores Web o software de productividad (como Microsoft Office).

En esta sección se incluyen los siguientes:

- **Principios para la creación de hosts administrativos seguros** : los principios generales que se deben tener en cuenta son los siguientes:
  - No administre nunca un sistema de confianza desde un host de menos confianza.
  - No se base en un único factor de autenticación al realizar actividades con privilegios.
  - No olvide la seguridad física al diseñar e implementar hosts administrativos seguros.

- **Protección de los controladores de dominio contra ataques** : Si un usuario malintencionado obtiene acceso con privilegios a un controlador de dominio, dicho usuario puede modificar, dañar y destruir el Active Directory base de datos y, por extensión, todos los sistemas y cuentas administrados por Active Directory.

En esta sección se incluyen los siguientes temas:

- **Seguridad física de los controladores de dominio** : contiene recomendaciones para proporcionar seguridad física para los controladores de dominio en centros de recursos, sucursales y ubicaciones remotas.

- **Sistemas operativos de controlador de dominio** : contiene recomendaciones para proteger los sistemas operativos de controlador de dominio.

- **Configuración segura de controladores de dominio** : las herramientas de configuración y las opciones de configuración disponibles de forma nativa y gratuita se pueden usar para crear líneas de base de configuración de seguridad para controladores de dominio que posteriormente se pueden aplicar a través de objetos de directiva de grupo (GPO).

## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory en busca de indicios de riesgo
En esta sección se proporciona información sobre las categorías de auditoría heredadas y las subcategorías de directivas de auditoría (que se introdujeron en Windows Vista y Windows Server 2008) y la Directiva de auditoría avanzada (que se presentó en Windows Server 2008 R2). También se proporciona información sobre los eventos y objetos que se van a supervisar que pueden indicar intentos de poner en peligro el entorno y algunas referencias adicionales que se pueden usar para construir una directiva de auditoría completa para Active Directory.

En esta sección se incluyen los siguientes temas:

- **Directiva de auditoría de Windows** : los registros de eventos de seguridad de Windows tienen categorías y subcategorías que determinan los eventos de seguridad de los que se realiza un seguimiento y se registran

- **Recomendaciones** de la Directiva de auditoría: en esta sección se describe la configuración de directiva de auditoría predeterminada de Windows, la configuración de directiva de auditoría recomendada por Microsoft y recomendaciones más agresivas para que las organizaciones las usen para auditar servidores y estaciones de trabajo críticos.

## <a name="planning-for-compromise"></a>Planeación de compromiso
Esta sección contiene recomendaciones que ayudarán a las organizaciones a prepararse para un riesgo antes de que se produzca, a implementar controles que pueden detectar un evento de riesgo antes de que se produzca una infracción de la totalidad, y proporcionar directrices de respuesta y recuperación para los casos en los que los atacantes consiguen un riesgo completo del directorio. En esta sección se incluyen los siguientes temas:

- **Replanteamiento del enfoque** : contiene principios e instrucciones para crear entornos seguros en los que una organización puede colocar sus activos más importantes. Estas instrucciones son las siguientes:

    - Identificación de los principios para separar y proteger los recursos críticos

    - Definición de un plan de migración limitado y basado en riesgos

    - Aprovechar las migraciones "no migratorias" cuando sea necesario

    - Implementación de "destrucción creativa"

    - Aislar sistemas y aplicaciones heredados

    - Simplificación de la seguridad para los usuarios finales

- **Mantenimiento de un entorno más seguro** : contiene recomendaciones de alto nivel diseñadas para usarse como directrices para el desarrollo no solo de seguridad efectiva, sino también para una administración de ciclo de vida efectiva. En esta sección se incluyen los siguientes temas:

    - La **creación de Business-Centric prácticas de seguridad para Active Directory** para administrar de forma eficaz el ciclo de vida de los usuarios, los datos, las aplicaciones y los sistemas administrados por Active Directory, siga estos principios.

        - **Asignar la propiedad de un negocio a Active Directory datos** : asignar la propiedad de los componentes de la infraestructura. en el caso de los datos que se agregan a Active Directory Domain Services (AD DS) para admitir la empresa, por ejemplo, nuevos empleados, nuevas aplicaciones y nuevos repositorios de información, se debe asociar a los datos una unidad de negocio o un usuario designados.

        - **Implementación de la administración del ciclo de vida de Business-Driven** : la administración del ciclo de vida debe implementarse para los datos en Active Directory.

        - **Clasifique todos los Active Directory datos: los** propietarios empresariales deben proporcionar una clasificación de los datos en Active Directory. Dentro del modelo de clasificación de datos, se debe incluir la clasificación de los siguientes datos de Active Directory:

            - **Sistemas** : clasifique los rellenados de servidores, su sistema operativo, su rol, las aplicaciones que se ejecutan en ellos y los propietarios de ti y empresariales del registro.

            - **Aplicaciones** : clasifique las aplicaciones por funcionalidad, base de usuarios y su sistema operativo.

            - **Usuarios** : se deben etiquetar y supervisar las cuentas de las instalaciones Active Directory que tengan más probabilidades de ser destinadas a los atacantes.

## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Resumen de los procedimientos recomendados para proteger Active Directory Domain Services
En la tabla siguiente se proporciona un resumen de las recomendaciones proporcionadas en este documento para proteger una instalación de AD DS. Algunos procedimientos recomendados son estratégicos por naturaleza y requieren proyectos completos de planeación e implementación. otras son tácticas y centradas en componentes específicos de Active Directory e infraestructura relacionada.

Las prácticas se muestran en orden aproximado de prioridad, es decir, los números más bajos indican una prioridad más alta. Cuando proceda, los procedimientos recomendados se identifican como preventivos o detectives por naturaleza. Todas estas recomendaciones se deben probar y modificar exhaustivamente según sea necesario para las características y los requisitos de su organización.

| **Procedimiento recomendado** | **Estratégica o táctica** | **Preventivo o detective** |
|--|--|--|
| Aplicaciones de revisión. | Táctica | Prevención |
| Sistemas operativos de revisión. | Táctica | Prevención |
| Implemente y actualice rápidamente el software antivirus y antimalware en todos los sistemas y supervise los intentos de quitarlos o deshabilitarlos. | Táctica | Ambos |
| Supervise los objetos de Active Directory confidenciales para los intentos de modificación y las ventanas de eventos que pueden indicar que se ha intentado poner en peligro. | Táctica | Detección |
| Proteger y supervisar las cuentas de los usuarios que tienen acceso a datos confidenciales | Táctica | Ambos |
| Evitar que se utilicen cuentas eficaces en sistemas no autorizados. | Táctica | Prevención |
| Elimine la pertenencia permanente a grupos con privilegios elevados. | Táctica | Prevención |
| Implemente controles para conceder la pertenencia temporal a grupos con privilegios cuando sea necesario. | Táctica | Prevención |
| Implementar hosts administrativos seguros. | Táctica | Prevención |
| Use la aplicación allowslists en controladores de dominio, hosts administrativos y otros sistemas confidenciales. | Táctica | Prevención |
| Identifique los recursos críticos y priorice su seguridad y supervisión. | Táctica | Ambos |
| Implemente controles de acceso basados en roles con privilegios mínimos para la administración del directorio, su infraestructura auxiliar y sistemas Unidos a un dominio. | Estratégico | Prevención |
| Aísle las aplicaciones y los sistemas heredados. | Táctica | Prevención |
| Retirar sistemas y aplicaciones heredados. | Estratégico | Prevención |
| Implemente programas de ciclo de vida de desarrollo seguros para aplicaciones personalizadas. | Estratégico | Prevención |
| Implemente la administración de la configuración, revise el cumplimiento con regularidad y evalúe la configuración con cada nueva versión de hardware o software. | Estratégico | Prevención |
| Migre recursos críticos a bosques puros con estrictos requisitos de seguridad y supervisión. | Estratégico | Ambos |
| Simplifique la seguridad para los usuarios finales. | Estratégico | Prevención |
| Utilice firewalls basados en host para controlar y proteger las comunicaciones. | Táctica | Prevención |
| Dispositivos de revisión. | Táctica | Prevención |
| Implemente la administración del ciclo de vida centrada en el negocio para los recursos de ti. | Estratégico | No aplicable |
| Crear o actualizar planes de recuperación de incidentes. | Estratégico | N/D |
