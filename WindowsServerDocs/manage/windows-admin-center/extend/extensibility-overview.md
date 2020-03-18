---
title: Extensiones para Windows Admin Center
description: Extensiones para el SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 90f5b670744b812769164a7a2c70fc673fe4089f
ms.sourcegitcommit: 3cb84bc0bd4be0f9333b7c85cda858c38730cb3a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/17/2020
ms.locfileid: "79432454"
---
# <a name="extensions-for-windows-admin-center"></a>Extensiones para Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Windows Admin Center se ha creado como una plataforma extensible para permitir a partners y desarrolladores sacar provecho de las capacidades existentes en Windows Admin Center, integrarse sin ningún problema con otros productos y soluciones de administración de TI y proporcionar valor adicional a los clientes. Cada solución y herramienta de Windows Admin Center se ha creado como una extensión usando las mismas características de extensibilidad disponibles para que partners y desarrolladores puedan crear herramientas potentes como las que están disponibles actualmente en Windows Admin Center.

Las extensiones de Windows Admin Center se han creado utilizando tecnologías web modernas que incluyen HTML5, CSS, Angular, TypeScript y jQuery y pueden administrar servidores de destino a través de PowerShell o WMI. También puede administrar servidores de destino, servicios o dispositivos a través de protocolos diferentes, como REST mediante la creación de un complemento de la puerta de enlace de Windows Admin Center.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Por qué debería considerar el desarrollo de una extensión para Windows Admin Center

Este es el valor que puede aportar a su producto y a los clientes mediante el desarrollo de extensiones para el centro de administración de Windows:

- **Integrar con las herramientas de Windows Admin Center:** Integra tus productos y servicios con herramientas de administración de servidor y clúster en Windows Admin Center y ofrece experiencias integrales de supervisión, administración y solución de problemas a tus clientes.
- **Aproveche las capacidades de seguridad, identidad y administración de la plataforma:** Habilite la compatibilidad con Azure Active Directory (AAD), la Multi-Factor Authentication, el Access Control basado en roles (RBAC), el registro, la auditoría de sus productos y servicios aprovechando las capacidades de la plataforma del centro de administración de Windows para satisfacer los complejos requisitos de las organizaciones de TI actuales.
- **Desarrollar usando las tecnologías web más recientes:** Crea rápidamente experiencias de usuario atractivas usando tecnologías web modernas que incluyen HTML5, CSS, Angular, TypeScript and jQuery y controles de interfaz de usuario enriquecidos y potentes incluidos en el SDK de Windows Admin Center.
- **Extender el alcance del producto:** Conviértete en parte del nuevo ecosistema de Windows Admin Center con un alcance a nuestra base de clientes de rápido y aprovecha el impulso del lanzamiento de Windows Server 2019 más adelante este año.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Empezar a desarrollar con el SDK del centro de administración de Windows

Empezar a trabajar con el desarrollo del centro de administración de Windows es fácil.  Se puede encontrar código de ejemplo para los tipos de extensión de [Complementos](develop-gateway-plugin.md) de [herramientas](develop-tool.md), [soluciones](develop-solution.md)y puertas de enlace en nuestra documentación del SDK. Allí se aprovecha la CLI del centro de administración de Windows para crear un nuevo proyecto de extensión y, a continuación, seguir las guías individuales para personalizar el proyecto de acuerdo con sus necesidades.

Hemos creado un kit de herramientas de [diseño de SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) para el centro de administración de Windows que le ayuda a simular rápidamente extensiones en PowerPoint mediante los estilos, los controles y las plantillas de página del centro de administración de Windows. Vea Cuál puede ser su extensión en el centro de administración de Windows antes de comenzar a codificar.

También tenemos código de ejemplo hospedado en GitHub: [herramientas de desarrollo](https://aka.ms/wacsdk) es una extensión de solución de ejemplo que contiene una colección enriquecida de controles que puede examinar y usar en su propia extensión. Herramientas de desarrollo es una extensión totalmente funcional que puede cargarse en Windows Admin Center en modo de desarrollador.

Consulta los temas siguientes para obtener más información sobre el SDK y empezar a trabajar:

- [Comprender cómo funcionan las extensiones](understand-extensions.md)
- [Desarrollar una extensión](developing-extensions.md)
- [Guías](guides.md)
- [Publicación de la extensión](publish-extensions.md)

## <a name="partner-spotlight"></a>Aspectos destacados del partner

Consulta el increíble valor que nuestros socios han comenzado a incorporar en Windows Admin Center y prueba ya estas extensiones. Conoce más sobre [cómo instalar las extensiones](../configure/using-extensions.md) desde Windows Admin Center.

### <a name="biitops"></a>BiitOps
La extensión BiitOps Changes proporciona el seguimiento de cambios para el hardware, el software y la configuración de las máquinas virtuales o físicas de Windows Server. La extensión BiitOps Changes mostrará exactamente las novedades, lo que ha cambiado y lo que se ha eliminado en un solo panel de cristal para ayudar a realizar un seguimiento de los problemas relacionados con el cumplimiento, la confiabilidad y la seguridad. [Más información sobre la extensión BiitOps Changes](case-studies/biitops.md).

![Extensión BiitOps](../media/extensibility-overview/biitops-1.png)

### <a name="dataon"></a>DataON

La extensión DataOn debe ser una extensión de supervisión, administración y de un extremo a otro de la infraestructura hiperconvergente y los sistemas de almacenamiento de DataOn basados en Windows Server. La extensión debe agregar un valor único, como la creación de informes de datos históricos, la asignación de discos, las alertas del sistema y el servicio de inicio de llamadas a SAN, que complementa el servidor del centro de administración de Windows y las capacidades de administración de la infraestructura hiperconvergida, a través de un experiencia unificada. [Obtén más información acerca de la extensión DataON MUST y su experiencia de desarrollo](case-studies/dataon.md).

![Extensión DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

Las extensiones de mantenimiento de RAID y de estado de ServerView para el centro de administración de Windows proporcionan supervisión y administración detalladas de componentes de hardware críticos, como procesadores, memoria, energía y subsistemas de almacenamiento para los servidores de Fujitsu PRIMERGY. Mediante el uso de los controles de la interfaz de usuario y patrones de diseño UX de Windows Admin Center, Fujitsu nos ha permitido dar un enorme paso hacia nuestra visión de profundización integral en roles y servicios de servidor, a sistema operativo y a la administración de hardware a través de la plataforma de Windows Admin Center. [Obtén más información sobre las extensiones de Fujitsu y su experiencia de desarrollo](case-studies/fujitsu.md).

![Extensión ServerView de Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

La extensión de integrador de Lenovo XClarity lleva la administración de hardware al siguiente nivel al integrarse sin problemas en varias experiencias del centro de administración de Windows. La solución XClarity Integrator proporciona una visión general de todos los servidores de Lenovo y diferentes extensiones de herramientas proporcionan detalles de hardware tanto si está conectado a un solo servidor, un clúster de conmutación por error como un clúster hiperconvergido. [Obtenga más información sobre la extensión de integrador de Lenovo XClarity](case-studies/lenovo.md).

![Extensión de Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

El almacenamiento puro proporciona soluciones de almacenamiento de datos empresariales, todo el flash que proporcionan arquitectura centrada en los datos para acelerar su negocio con el fin de obtener una ventaja competitiva. La extensión de almacenamiento pura para el centro de administración de Windows proporciona una vista de un solo panel en productos FlashArray puros y permite a los usuarios realizar tareas de supervisión, ver las métricas de rendimiento en tiempo real y administrar los volúmenes de almacenamiento y los iniciadores a través de una única interfaz de usuario. evaluación. [Obtenga más información sobre las extensiones de Pure y su experiencia de desarrollo](case-studies/purestorage.md).

![Extensión de almacenamiento puro](../media/extensibility-overview/purestorage-extension.png)

### <a name="qct"></a>QCT

La extensión de QCT Management Suite complementa el centro de administración de Windows al proporcionar supervisión y administración de servidores físicos para sistemas certificados de QCT Azure Stack HCl. La extensión QCT Management Suite muestra información de hardware del servidor y proporciona una interfaz de usuario de asistente intuitiva para ayudar a reemplazar los discos físicos de forma eficaz, las herramientas del registro de eventos de hardware y S.M.A.R.T. Administración de discos predictivo basada en. [Obtenga más información sobre la extensión de QCT Management Suite](case-studies/qct.md).

![Extensión QCT](../media/extensibility-overview/qct-extension.png)