---
title: Caso práctico de Windows Admin Center SDK - cuadrado
description: Caso práctico de Windows Admin Center SDK - cuadrado
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab0a7bdcf2388ffc867763c04e183b7388fd13e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863946"
---
# <a name="squared-up-extension"></a>Cuadrado de extensión

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>Usar supervisión basada en SCOM, visibilidad de la dependencia de servidor y perspectivas sobre los datos externos en Windows Admin Center

Cuadrado de fue fundada con la visión del uso de visualización de datos para ayudar a resolver los desafíos de la complejidad de TI empresarial. Único, ligero del cuadrado copia, software de sólo interfaz de usuario que se basa en Microsoft eficaz plataforma de System Center Operations Manager, así como la integración con orígenes de datos adicionales: de Azure Log Analytics de Microsoft, Application Insights y del sistema Centro de Service Manager para productos de terceros, como ServiceNow, Splunk y muchos más - para proporcionar visibilidad de la empresa a gran escala parques de infraestructura y las aplicaciones, tanto en el entorno local y en entornos de nube híbrida.

> <cite>"Nos hemos estado muy utilizando Windows Admin Center a lo largo de su vista previa técnica y ha sido un impacto enorme ya, realmente ayudar a resolver desafíos como nuestros ingenieros de obtener acceso fácil a nuestros laboratorios de configuración, pero tenemos previsto facilitar la administración principal una vez que llega a una versión completa de la consola. Nos encanta el potencial de la integración con el cuadrado de seguridad y la capacidad de exponer todos nuestros datos en un solo lugar."</cite>
>
> --David Acevedo, puedo / S especialista en energía NuStar L.P.

De clientes cuadrado administran cientos, miles a menudo, de servidores de Windows y la diversas carteras entregadas por ellos y al cuadrado de y Microsoft se encuentran en una misión de reunir los equipos de TI de la aplicación lo mejor en la web moderna, de rápida, la interfaz de usuario para proporcionar la información que necesitan. Como resultado, el equipo en el cuadrado de vio inmediatamente una alineación emocionante con Windows Admin Center que ofrece los mismos valores y las entidades de seguridad en la próxima generación de administración de Windows Server. En concreto, el equipo considera que los datos de rendimiento a largo plazo, información de dependencia de servidor en tiempo real y obtenidas por el cuadrado de contexto de la aplicación complementan perfectamente los datos elegantes, en tiempo real y proporcionadas capacidades de administración de servidor Windows Admin Center.

![Cuadrado de extensión](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>"Como una organización que administra un estado del servidor a gran escala, en el cuadrado Up / integración de Windows Admin Center es el matrimonio perfecta de nuestras herramientas traducida y centralizada y cosas como la que se va a lanzar un servidor directamente en modo de mantenimiento desde Windows Admin Center son excelentes wins poco para nosotros"</cite>
>
> -– Omitir Granson, administrador del sistema de virtualización en la Universidad Purdue

Contar con una visión clara de que desea presentar esos datos a la perfección en Windows Admin Center, al cuadrado de había trabajado con la versión de vista previa privada temprana de los SDK de Windows Admin Center y resultó sencillo, flexible y bien documentado.

Con el SDK de Windows Admin Center, al cuadrado de fue capaz de crear una extensión que incrusta dinámicamente pertinentes al cuadrado de experimentan las vistas dentro de la Windows Admin Center. Por ejemplo, dentro del contexto de un servidor específico o un clúster, al cuadrado de vistas se incrustan automáticamente en proporciona mayor visibilidad. Vistas incluyen las tendencias históricas de clave de rendimiento y las métricas de capacidad (por ejemplo, CPU, memoria y disco), hospedaje de pila (plataforma o centro de datos de virtualización de nube), componentes de la aplicación, como bases de datos SQL y servicios e incluso el análisis de registro en la nube y los datos ITSM.

![Cuadrado de extensión](../../media/extend-case-study-squared-up/squared-up-2.png)

Cuadrado de seguridad y Windows Admin Center comparten una web moderna arquitectura y diseño ethos, que se ha habilitado una integración sencilla técnica y una experiencia de usuario sin problemas. Con la administración basada en web que se está volviendo cada vez más a la norma, creemos que este método de integración entre distintos sistemas es la clave para desbloquear una experiencia de administración moderna y unificada.

> <cite>"Consideramos que Windows Admin Center la vanguardia de la administración moderna de Windows Server, por lo que ha sido una gran experiencia para que podamos trabajar estrechamente con el equipo y el hecho de que funcionan con dicha velocidad, entusiasmo, flexibilidad y dentro de tales fundamentalmente paradigmas de desarrollo modernos hizo una elección excelente con la forma, como una compañía de desarrollo de software lean, ágil y rápido, trabajamos nosotros mismos. "</cite>
>
> --Richard Benwell, arquitecto de producto en el cuadrado

Desde esta alineación natural, el equipo de desarrollo en el cuadrado de fue capaz de avanzar rápidamente a una integración de prototipo mostrar al cuadrado de forma nativa dentro de la experiencia de Windows Admin Center y poner eso en manos de sus primeras, técnica obtener una vista previa de los clientes. Desde las reacciones de los clientes, era que la historia era ganador claro inmediatamente.

> <cite>"Uno de los principales retos de mantenimiento de un servicio pendiente a través de nuestro entorno de más de 3.500 servidores es unificar nuestro diverso panorama de administración y supervisión de las herramientas de modo que la integración entre cuadrado copias de seguridad y Windows Admin Center - que aporta juntos tantos datos, desde muchos orígenes dispares, en una única consola – están masivos para nosotros".</cite>
>
> --Ehrnst Martin, director técnico de Azure en Intility A/S

Con ese tipo de entusiasmo de cuadrado de clientes ya y con numerosas características nuevas muy útiles todavía para llegar a Windows Admin Center, cuadrado copia se videoclip complace acerca del futuro de esta integración y las increíbles posibilidades lo abre para sus clientes y sus viaje hacia una true solo de-consola de administración de sus operaciones de TI.

El cuadrado Up / integración de Windows Admin Center está actualmente en versión Beta; Si desea que el acceso, consulte [la página dedicada de cuadrado copia](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu) para obtener más detalles. Si su organización usa Microsoft System Center Operations Manager y no tiene todavía al cuadrado hacia arriba (que es esencial para que funcione la extensión), también puede obtener sus manos en una con características completas, 30 días gratuita desde la misma ubicación. 
