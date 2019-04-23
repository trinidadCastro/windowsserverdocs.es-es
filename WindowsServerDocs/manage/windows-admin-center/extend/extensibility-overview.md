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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885006"
---
# <a name="extensions-for-windows-admin-center"></a>Extensiones para Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Windows Admin Center se ha creado como una plataforma extensible para permitir a partners y desarrolladores sacar provecho de las capacidades existentes en Windows Admin Center, integrarse sin ningún problema con otros productos y soluciones de administración de TI y proporcionar valor adicional a los clientes. Cada solución y herramienta de Windows Admin Center se ha creado como una extensión usando las mismas características de extensibilidad disponibles para que partners y desarrolladores puedan crear herramientas potentes como las que están disponibles actualmente en Windows Admin Center.

Las extensiones de Windows Admin Center se han creado utilizando tecnologías web modernas que incluyen HTML5, CSS, Angular, TypeScript y jQuery y pueden administrar servidores de destino a través de PowerShell o WMI. También puede administrar servidores de destino, servicios o dispositivos a través de protocolos diferentes, como REST mediante la creación de un complemento de la puerta de enlace de Windows Admin Center.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Por qué debería considerar el desarrollo de una extensión para Windows Admin Center

Este es el valor que puede aportar a tu producto y a los clientes mediante el desarrollo de extensiones para Windows Admin Center:

- **Integración con herramientas de Windows Admin Center:** Integre sus productos y servicios con las herramientas de administración de servidor y clúster en Windows Admin Center y entregar unificado y sin problemas,-to-end, administración de supervisión, solución de problemas de experiencias a sus clientes.
- **Aproveche las capacidades de seguridad, identidad y administración de la plataforma:** Habilitar compatibilidad con Azure Active Directory (AAD), autenticación multifactor, Control de acceso basado en roles (RBAC), registro, auditoría de sus productos y servicios mediante funcionalidades de plataforma Windows Admin Center para cumplir los requisitos complejos de hoy en día Organizaciones de TI.
- **Desarrollar con las últimas tecnologías web:** Cree rápidamente sorprendentes experiencias de usuario mediante tecnologías web modernas como HTML5, CSS, Angular, jQuery y TypeScript y completas y eficaces controles de la interfaz de usuario incluidos en el SDK de Windows Admin Center.
- **Ampliar el alcance del producto:** Se convierten en una parte del ecosistema Windows Admin Center nueva con outreach a nuestro rápidamente creciente base de clientes y el momento de inicio de aprovechar el servidor de Windows 2019 más adelante este año.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Empiece a desarrollar con el SDK de Windows Admin Center

Introducción al desarrollo de Windows Admin Center es fácil.  Puede encontrar el código de ejemplo para [herramienta](develop-tool.md), [solución](develop-solution.md), y [complemento de puerta de enlace](develop-gateway-plugin.md) tipos de extensión en la documentación del SDK. No existe, aprovechará Windows Admin Center CLI para crear un nuevo proyecto de extensión, a continuación, siga las guías individuales para personalizar el proyecto para satisfacer sus necesidades.

Hemos realizado una de Windows Admin Center [Kit de herramientas de diseño SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) disponibles para ayudarle a simular rápidamente las extensiones de PowerPoint a partir de plantillas, controles y estilos Windows Admin Center. ¡Vea la extensión puede aspecto en Windows Admin Center antes de comenzar a codificar!

También contamos con código de ejemplo que se hospeda en GitHub: [Herramientas de desarrollo](https://aka.ms/wacsdk) es una extensión de la solución de ejemplo que contiene una amplia colección de controles que puede examinar y usar en su propia extensión. Herramientas de desarrollo es una extensión totalmente funcional que puede cargarse en Windows Admin Center en modo de desarrollador.

Consulta los temas siguientes para obtener más información sobre el SDK y empezar a trabajar:

- [Comprender cómo funcionan las extensiones](understand-extensions.md)
- [Desarrollar una extensión](developing-extensions.md)
- [Guías](guides.md)
- [Publicar la extensión](publish-extensions.md)

## <a name="partner-spotlight"></a>Aspectos destacados del partner

Consulta el increíble valor que nuestros socios han comenzado a incorporar en Windows Admin Center y prueba ya estas extensiones. Conoce más sobre [cómo instalar las extensiones](../configure/using-extensions.md) desde Windows Admin Center.

### <a name="dataon"></a>DataON

Extensión de datos deben ofrece supervisión, administración y to-end visión general de almacenamiento y la infraestructura de sistemas hiperconvergidos del datos basada en Windows Server. La extensión debe aporta un valor único, como los informes de datos históricos, asignación de disco, las alertas del sistema y servicio local llamada similar a SAN, como complemento para el servidor de Windows Admin Center y capacidades de administración de infraestructuras hiperconvergidas, a través de una conexión directa experiencia unificada. [Obtén más información acerca de la extensión DataON MUST y su experiencia de desarrollo](case-studies/dataon.md).

![Extensión DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

Fujitsu ServerView estado y mantenimiento de RAID extensiones para Windows Admin Center proporciona detallada de supervisión y administración de componentes de hardware críticos como procesadores, memoria, los subsistemas de almacenamiento y potencia para servidores de Fujitsu PRIMERGY. Mediante el uso de los controles de la interfaz de usuario y patrones de diseño UX de Windows Admin Center, Fujitsu nos ha permitido dar un enorme paso hacia nuestra visión de profundización integral en roles y servicios de servidor, a sistema operativo y a la administración de hardware a través de la plataforma de Windows Admin Center. [Obtén más información sobre las extensiones de Fujitsu y su experiencia de desarrollo](case-studies/fujitsu.md).

![Extensión ServerView de Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

Extensión de XClarity integrador de Lenovo lleva la administración de hardware al siguiente nivel al integrar sin problemas en varias experiencias de Windows Admin Center. La solución de integración de XClarity proporciona una visión general de todos los servidores de Lenovo y extensiones de herramientas diferentes proporcionan los detalles de hardware si está conectado a un solo servidor, clúster de conmutación por error o un clúster hiperconvergido. [Más información sobre la extensión de integración de Lenovo XClarity](case-studies/lenovo.md).

![Extensión de Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

Almacenamiento puro proporciona enterprise, soluciones de almacenamiento de datos de memoria flash que entregar arquitectura centradas en datos para acelerar su negocio para una ventaja competitiva. La extensión de almacenamiento puro para Windows Admin Center proporciona una vista de panel único en productos FlashArray puro y permite a los usuarios realizar tareas de supervisión, ver las métricas de rendimiento en tiempo real y administrar volúmenes de almacenamiento y los iniciadores a través de una interfaz de usuario único experiencia. [Más información sobre las extensiones del puro y su experiencia de desarrollo](case-studies/purestorage.md).

![Extensión de almacenamiento puro](../media/extensibility-overview/purestorage-extension.png)

### <a name="squared-up"></a>Squared Up

Squared Up proporciona la experiencia de supervisión mejor de su clase basada en System Center Operations Manager y la integración con Azure Log Analytics, Application Insights y otras soluciones de supervisión. La [extensión de Squared Up](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) aporta datos históricos de rendimiento así como dependencias y topologías de aplicaciones en directo que existen en el contexto administración de servidor y clúster que Windows Admin Center proporciona, y los primeros clientes han reconocido el valor de incorporar un gran número de datos en una única experiencia. [Obtén más información sobre la extensión de Squared Up y su experiencia de desarrollo](case-studies/squared-up.md).

![Extensión de Squared Up](../media/extensibility-overview/squaredup-extension.png)