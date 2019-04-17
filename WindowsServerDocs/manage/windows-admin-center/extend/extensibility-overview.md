---
title: Extensiones para Windows Admin Center
description: Extensiones para el SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fa3d7e75b32f0195346e58db54b7932c8d2fd3b9
ms.sourcegitcommit: 659544db1e19d6eecc52c7de07116ae735280544
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "9001847"
---
# Extensiones para Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Windows Admin Center se ha creado como una plataforma extensible para permitir a partners y desarrolladores sacar provecho de las capacidades existentes en Windows Admin Center, integrarse sin ningún problema con otros productos y soluciones de administración de TI y proporcionar valor adicional a los clientes. Cada solución y herramienta de Windows Admin Center se ha creado como una extensión usando las mismas características de extensibilidad disponibles para que partners y desarrolladores puedan crear herramientas potentes como las que están disponibles actualmente en Windows Admin Center.

Las extensiones de Windows Admin Center se han creado utilizando tecnologías web modernas que incluyen HTML5, CSS, Angular, TypeScript y jQuery y pueden administrar servidores de destino a través de PowerShell o WMI. También puede administrar servidores de destino, servicios o dispositivos a través de protocolos diferentes, como REST mediante la creación de un complemento de la puerta de enlace de Windows Admin Center.

## Por qué debería considerar el desarrollo de una extensión para Windows Admin Center

Este es el valor que puede aportar a tu producto y a los clientes mediante el desarrollo de extensiones para Windows Admin Center:

- **Integrar con las herramientas de Windows Admin Center:** Integra tus productos y servicios con herramientas de administración de servidor y clúster en Windows Admin Center y ofrece experiencias integrales de supervisión, administración y solución de problemas a tus clientes.
- **Sacar provecho de las capacidades de seguridad, identidad y administración de la plataforma:** Habilita el soporte de Azure Active Directory (AAD), Multi-Factor Authentication, Control de acceso basado en roles (RBAC), registro, auditoría del producto y servicios aprovechando la plataforma Windows Admin Center para satisfacer los requisitos complejos de las organizaciones de TI de hoy en día.
- **Desarrollar usando las tecnologías web más recientes:** Crea rápidamente experiencias de usuario atractivas usando tecnologías web modernas que incluyen HTML5, CSS, Angular, TypeScript and jQuery y controles de interfaz de usuario enriquecidos y potentes incluidos en el SDK de Windows Admin Center.
- **Extender el alcance del producto:** Conviértete en parte del nuevo ecosistema de Windows Admin Center con un alcance a nuestra base de clientes de rápido y aprovecha el impulso del lanzamiento de Windows Server 2019 más adelante este año.

## Empezar a desarrollar con el SDK de Windows Admin Center

Introducción al desarrollo de Windows Admin Center es muy fácil.  Código de muestra se puede encontrar para tipos de extensión de [herramienta](develop-tool.md), [soluciones](develop-solution.md)y [complemento de puerta de enlace](develop-gateway-plugin.md) en nuestra documentación del SDK. Hay aprovechará la CLI de Windows Admin Center para generar un nuevo proyecto de extensión, a continuación, sigue las guías para personalizar el proyecto para satisfacer sus necesidades individuales.

Hemos facilitado un [Kit de herramientas de diseño SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) disponibles que te ayudarán a rápidamente simular extensiones en PowerPoint de Windows Admin Center con estilos de Windows Admin Center, controles y plantillas de página. ¡Vea la extensión aspecto que en Windows Admin Center antes de empezar a escribir código!

También disponemos de código de ejemplo hospedada en GitHub: [Herramientas de desarrollo](https://aka.ms/wacsdk) es una extensión de la solución de muestra que contiene una amplia colección de controles que puedes explorar y usar en su propia extensión. Herramientas de desarrollo es una extensión totalmente funcional que puede cargarse en Windows Admin Center en modo de desarrollador.

Consulta los temas siguientes para obtener más información sobre el SDK y empezar a trabajar:

- [Comprender el funcionamiento de las extensiones](understand-extensions.md)
- [Desarrollar una extensión](developing-extensions.md)
- [Guías](guides.md)
- [Publicar la extensión](publish-extensions.md)

## Aspectos destacados del partner

Consulta el increíble valor que nuestros socios han comenzado a incorporar en Windows Admin Center y prueba ya estas extensiones. Conoce más sobre [cómo instalar las extensiones](../configure/using-extensions.md) desde Windows Admin Center.

### DataON

La extensión DataON MUST ofrece supervisión, administración y principio a fin vistas previas hiperconvergida infraestructura y almacenamiento sistemas de DataON basados en Windows Server. La extensión debe aporta un valor único, como informes de datos históricos, asignación de disco, alertas del sistema y servicio principal de llamadas de SAN similar, que complementan al servidor de Windows Admin Center y las capacidades de administración de la infraestructura hiperconvergida, a través de una sencilla experiencia unificada. [Obtén más información acerca de la extensión DataON MUST y su experiencia de desarrollo](case-studies/dataon.md).

![Extensión DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### Fujitsu

Las extensiones ServerView Health y RAID Health de Fujitsu para Windows Admin Center proporcionan supervisión y administración exhaustivos de componentes de hardware críticos como procesadores, memoria, subsistemas de almacenamiento y alimentación para servidores PRIMERGY de Fujitsu. Mediante el uso de los controles de la interfaz de usuario y patrones de diseño UX de Windows Admin Center, Fujitsu nos ha permitido dar un enorme paso hacia nuestra visión de profundización integral en roles y servicios de servidor, a sistema operativo y a la administración de hardware a través de la plataforma de Windows Admin Center. [Obtén más información sobre las extensiones de Fujitsu y su experiencia de desarrollo](case-studies/fujitsu.md).

![Extensión ServerView de Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### Lenovo

Extensión de XClarity integrador de Lenovo lleva la administración de hardware al siguiente nivel al integrar sin problemas en diversas experiencias en Windows Admin Center. La solución de integrador de XClarity proporciona una visión general de todos los servidores de Lenovo y extensiones de herramienta diferentes proporcionan detalles de hardware si estás conectado a un único servidor, clúster de conmutación por error o un clúster hiperconvergido. [Obtenga más información acerca de la extensión de Lenovo XClarity integrador](case-studies/lenovo.md).

![Extensión de Lenovo](../media/extensibility-overview/lenovo-extension.png)

### Almacenamiento pura

Almacenamiento pura proporciona de empresa, soluciones de almacenamiento de datos de memoria flash que ofrecen arquitectura centradas en datos para acelerar el negocio para una ventaja competitiva. La extensión de almacenamiento pura para Windows Admin Center proporciona una vista de panel único en los productos de FlashArray pura y permite a los usuarios realizar tareas de supervisión, ver las métricas de rendimiento en tiempo real y administrar volúmenes de almacenamiento y los iniciadores a través de una interfaz de usuario único experiencia. [Más información sobre las extensiones del pura y su experiencia de desarrollo](case-studies/purestorage.md).

![Extensión de almacenamiento pura](../media/extensibility-overview/purestorage-extension.png)

### Squared Up

Squared Up proporciona la experiencia de supervisión mejor de su clase basada en System Center Operations Manager y la integración con Azure Log Analytics, Application Insights y otras soluciones de supervisión. La [extensión de Squared Up](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) aporta datos históricos de rendimiento así como dependencias y topologías de aplicaciones en directo que existen en el contexto administración de servidor y clúster que Windows Admin Center proporciona, y los primeros clientes han reconocido el valor de incorporar un gran número de datos en una única experiencia. [Obtén más información sobre la extensión de Squared Up y su experiencia de desarrollo](case-studies/squared-up.md).

![Extensión de Squared Up](../media/extensibility-overview/squaredup-extension.png)