---
title: Centro de administración de Windows SDK caso práctico - cuadrado copia de seguridad
description: Centro de administración de Windows SDK caso práctico - cuadrado copia de seguridad
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab0a7bdcf2388ffc867763c04e183b7388fd13e9
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2052549"
---
# <a name="squared-up-extension"></a>Cuadrado de extensión

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>Incorporación de supervisión basada en SCOM, visibilidad de dependencia del servidor y perspectivas de datos externos en el centro de administración de Windows

Cuadrado seguridad fue fundada con la visión del uso de visualización de datos para ayudar a resolver los desafíos de la complejidad de TI de empresa. Único, ligero del cuadrado copia de seguridad, software sólo la interfaz de usuario se basa en la parte superior de Microsoft potente plataforma de System Center Operations Manager, así como integración con orígenes de datos adicionales - de análisis de registro de Azure de Microsoft, perspectivas de aplicación y del sistema Centro de administrador de servicios a los productos de terceros como ServiceNow, Splunk y muchas más - para proporcionar visibilidad de empresa a gran escala estates de infraestructura y la aplicación, tanto local y a través de entornos híbridos de la nube.

> <cite>"Nos hemos sido muy utilizando centro de administración de Windows en toda su vista previa técnica y ha sido un éxito enorme ya, realmente ayudar a resolver desafíos como nuestros ingenieros obtener acceso fácil a nuestros laboratorios de configuración, y pensadas para facilitar la administración principal de nuestro una vez que llega a la versión completa de la consola. Nos gusta el potencial de la integración con cuadrado copia de seguridad y la capacidad para exponer todos nuestros datos en un solo lugar."</cite>
>
> --David Acevedo, I / S especialista en NuStar energía L.P.

Clientes de cuadrado copia de seguridad administración cientos, a menudo miles, de servidores de Windows y la aplicación diverso carteras entregadas por ellos y tanto cuadrado de seguridad y Microsoft se encuentran en una misión para incorporar los equipos de TI lo mejor en sitio web moderna, fast, la interfaz de usuario para proporcionar toda la información que necesitan. Como resultado, el equipo de seguridad de cuadrado inmediatamente han visto una alineación interesante con el centro de administración de Windows que aporta la funcionalidad de esos mismos valores y entidades de seguridad para la próxima generación de administración de Windows Server. En concreto, el equipo considera que los datos de rendimiento a largo plazo, perspectivas de dependencia de servidor en tiempo real y expuestos en cuadrado de contexto de la aplicación podrían complementar perfectamente el elegantes datos en tiempo real y las capacidades de administración de servidor proporcionadas por Centro de administración de Windows.

![Cuadrado de extensión](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>"Como una organización administrar un estado del servidor a gran escala, en el cuadrado Up / gracias a la unión perfecta de nuestras herramientas localizado y centralizado y cosas como la que se va a capaz de iniciar un servidor directamente en el modo de mantenimiento desde dentro de la integración del centro de administración de Windows está Centro de administración de Windows son satisfactorias wins poco para nosotros"</cite>
>
> -– Kip Granson, Administrador de sistemas de virtualización en la Universidad de Purdue

Contar con una visión clara de que deseen presentar los datos sin ningún problema en el centro de administración de Windows, cuadrado de seguridad se ha completado correctamente con la versión preview privada anticipado del SDK de centro de administración de Windows y resulta sencillo, bien documentado y flexibles.

Con el SDK de centro de administración de Windows, cuadrado seguridad pudo generar una extensión que incrusta dinámicamente relevantes cuadrado una experimentan de las vistas en el centro de administración de Windows. Por ejemplo, dentro del contexto de un servidor específico o en clúster, cuadrado seguridad vistas se incrustan automáticamente a proporciona visibilidad extendida. Vistas incluyen las tendencias históricas de clave métricas de rendimiento y capacidad (por ejemplo, CPU, memoria y disco), que hospeda la pila (en la nube la virtualización de plataforma o centro de datos), los componentes de la aplicación, como bases de datos de SQL y servicios e incluso el análisis de registro basados en la nube y datos ITSM.

![Cuadrado de extensión](../../media/extend-case-study-squared-up/squared-up-2.png)

Cuadrado de seguridad y centro de administración de Windows compartan un web moderna arquitectura y diseño actitud, que se ha habilitado una integración técnica simple y una experiencia de usuario más libre. Con la administración basada en web convirtiendo en la norma, creemos que este método de la integración entre los distintos sistemas de es la clave para desbloquear una experiencia de administración unificada moderno.

> <cite>"Se vea centro de administración de Windows como la última tecnología de administración de Windows Server moderno, por lo que ha sido una gran experiencia para trabajar estrechamente con el equipo y el hecho de que está trabajando con dicha velocidad, entusiasmo, flexibilidad y dentro de tales fundamentalmente paradigmas de desarrollo moderno hizo muy apropiado con la forma, como una compañía de desarrollo de software lean, ágil, rápido, trabajamos decir nosotros. "</cite>
>
> --Cuadrado Richard Benwell, arquitecto de producto en copia de seguridad

Desde esta alineación natural, el equipo de desarrollo de cuadrado seguridad pudo para avanzar rápidamente a una integración prototipo mostrar cuadrado de forma nativa dentro de la experiencia del centro de administración de Windows y para obtener en manos de su propios-temprano, técnica obtener una vista previa de los clientes. Desde las reacciones de los clientes, era inmediatamente evidente que el artículo fue ganador.

> <cite>"Uno de los desafíos claves del mantenimiento de servicio pendiente a través de nuestro entorno de servidores más de 3.500 es unificación nuestro diverso horizontal de la administración y supervisión de herramientas y lo la integración entre cuadrado de seguridad y centro de administración de Windows - que aporta juntos muchos datos, de diferentes orígenes tantos, en una única consola – están masivas para nosotros."</cite>
>
> --Martin Ehrnst, director técnico de Azure en Intility A/S

Con ese tipo de entusiasmo de ya cuadrado de clientes y con gran cantidad de características nuevas aún para llegar a centro de administración de Windows, cuadrado seguridad es enormemente interesado en el futuro de esta integración y las posibilidades de maravillas abre para sus clientes y sus viaje a una true solo-de consola para la administración de sus operaciones de TI.

El cuadrado Up / integración del centro de administración de Windows está actualmente en Beta; Si le gustaría acceso, desproteja [cuadrado copia de seguridad de la página dedicada](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu) para obtener más detalles. Si su organización usa Microsoft System Center Operations Manager y aún no tiene cuadrado hacia arriba (que es esencial para que funcione la extensión), también puede obtener las manos en un completas, 30 días versión de prueba gratuita desde la misma ubicación. 
