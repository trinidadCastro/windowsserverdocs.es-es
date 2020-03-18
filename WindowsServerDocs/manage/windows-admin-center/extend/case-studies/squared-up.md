---
title: 'Caso práctico de SDK del centro de administración de Windows: cuadrado arriba'
description: 'Caso práctico de SDK del centro de administración de Windows: cuadrado arriba'
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 620d3d9f4b5c3638d49fe9141e83ebdcb9eb245c
ms.sourcegitcommit: 5197a87e659589bcc8d2a32069803ae736b02892
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2020
ms.locfileid: "79429309"
---
# <a name="squared-up-extension"></a>Extensión cuadrada

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>Incorporación de supervisión basada en SCOM, visibilidad de dependencia del servidor e información externa sobre datos en el centro de administración de Windows

En el cuadrado se fundó la visión del uso de la visualización de datos para ayudar a resolver los desafíos de la complejidad de TI de la empresa. Las compilaciones de software únicas, ligeras y exclusivas de la interfaz de usuario de Microsoft, además de la potente plataforma de System Center Operations Manager de Microsoft, así como la integración con orígenes de datos adicionales, desde Azure Log Analytics, Application Insights y System de Microsoft. Centre Service Manager en productos de terceros, como ServiceNow, Splunk y muchos más, para proporcionar visibilidad en infraestructura empresarial a gran escala y en las inmuebles de aplicaciones, tanto locales como en entornos de nube híbrida.

> <cite>"Hemos usado ampliamente el centro de administración de Windows en su versión Technical Preview y ya ha sido un gran éxito, ayudando a la solución de desafíos como nuestros ingenieros a obtener un acceso fácil a nuestros laboratorios de configuración y nos gustaría convertirlo en la administración principal. consola una vez que llega a la versión completa. Nos encanta el potencial de la integración con un cuadrado y la capacidad de exponer todos los datos en un único lugar ".</cite>
>
> --David Acevedo, especialista de I/S en NuStar Energy L.P.

Los clientes del cuadrado hacia arriba administran cientos, a menudo miles, de servidores de Windows y las diversas carteras de aplicaciones que ofrecen, y ambos se han cuadrado y Microsoft está en una misión para ofrecer a los equipos de ti la mejor interfaz de usuario Web rápida y moderna para proporcionar la información que necesitan. Como resultado, el equipo con un cuadrado de arriba vio una excelente alineación con el centro de administración de Windows, que proporciona los mismos valores y entidades de seguridad a la próxima generación de administración de Windows Server. En concreto, el equipo consideró que los datos de rendimiento a largo plazo, la información de dependencia de los servidores en tiempo real y el contexto de la aplicación que se muestran por el cuadrado se complementaban perfectamente con las capacidades de administración de datos y servidores en tiempo real proporcionadas por Centro de administración de Windows.

![Extensión cuadrada](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>"Como organización que administra un servidor a gran escala, la integración del centro de administración de Windows y el cuadrado es el matrimonio perfecto de nuestras herramientas localizadas y centralizadas, y otros aspectos como poder iniciar un servidor directamente en modo de mantenimiento desde el centro de administración de Windows son muy poco buenos para nosotros"</cite>
>
> --Kip Granson, administrador de sistemas de virtualización en Purdue University

Gracias a una visión clara de la idea de presentar los datos sin problemas en el centro de administración de Windows, con un cuadrado de trabajo con la versión preliminar de la versión preliminar privada del SDK del centro de administración de Windows, y lo encontramos sencillo, bien documentado y flexible.

Al usar el SDK del centro de administración de Windows, con el cuadrado, se podía crear una extensión que incrustara dinámicamente las vistas cuadradas correspondientes dentro de la experiencia del centro de administración de Windows. Por ejemplo, en el contexto de un servidor o clúster específico, las vistas cuadradas se incrustan automáticamente en la visibilidad extendida proporcionada. Las vistas incluyen tendencias históricas de métricas clave de rendimiento y capacidad (como CPU, memoria y disco), pila de hospedaje (virtualización de centros de datos o plataforma en la nube), componentes de aplicación como bases de datos y servicios SQL e incluso análisis de registros basados en la nube. y datos de ITSM.

![Extensión cuadrada](../../media/extend-case-study-squared-up/squared-up-2.png)

Centrado y el centro de administración de Windows comparten una arquitectura web moderna y un diseño Ethos, que ha habilitado tanto una integración técnica sencilla como una experiencia de usuario sin problemas. Con la administración basada en web cada vez que se convierte en la norma, creemos que este método de integración entre distintos sistemas es la clave para desbloquear una experiencia de administración unificada y moderna.

> <cite>"Vemos el centro de administración de Windows como la vanguardia de la administración moderna de Windows Server, por lo que ha sido una excelente experiencia para que podamos trabajar tan cerca con el equipo y el hecho de que están trabajando con esa velocidad, entusiasmo, flexibilidad y, en esencia. los paradigmas de desarrollo modernos les han hecho un gran ajuste con la forma en que somos una compañía de desarrollo de software eficiente, ágil y rápida, trabajaremos ".</cite>
>
> --Richard Benwell, arquitecto de productos en un cuadrado

A partir de esta alineación natural, el equipo de desarrollo en un cuadrado hacia arriba podía progresar rápidamente a una integración de prototipos que se muestra de forma nativa en la experiencia del centro de administración de Windows y para que llegue a su propio pionero, técnico obtener una vista previa de los clientes. A partir de las reacciones de los clientes, se deshizo inmediatamente que la historia era ganador.

> <cite>"Uno de los principales desafíos de mantener un servicio extraordinario en nuestro entorno de más de 3.500 servidores es unificar nuestro diverso panorama de herramientas de administración y supervisión, así como la integración entre el centro de administración de Windows y el cuadrado. juntos, tantos datos, desde muchos orígenes dispares, en una sola consola, son enormes para nosotros ".</cite>
>
> --Martin Ehrnst, responsable técnico de Azure en Intility A/S

Con ese tipo de entusiasmo de los clientes con un cuadrado en vertical y con una gran cantidad de características nuevas todavía para el centro de administración de Windows, el cuadrado se encuentra muy entusiasmado sobre el futuro de esta integración y las posibilidades formidables que se abre para sus clientes y su viaje a una verdadera sección de vidrio para su administración de operaciones de ti.

La integración del centro de administración de Windows o el cuadrado está actualmente en versión beta. Si desea obtener acceso, consulte [la página dedicada del cuadrado arriba](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu) para obtener más detalles. Si su organización usa Microsoft System Center Operations Manager y aún no se ha puesto en marcha (lo que es esencial para que la extensión funcione), también puede obtener una evaluación gratuita de 30 días de la misma ubicación. 
