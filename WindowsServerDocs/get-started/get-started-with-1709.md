---
title: Introducción a Windows Server, versión 1709
description: Cómo obtenerlo, instalarlo y activarlo
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 12/5/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 96f098457b9bac4541421c7889aefb030e3d8804
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868806"
---
# <a name="introducing-windows-server-version-1709"></a>Introducción a Windows Server, versión 1709

>Se aplica a: Windows Server (Canal semianual)

**Windows Server, versión 1709 es la primera versión en el nuevo canal semianual.** 

## <a name="what-the-semi-annual-channel-is--and-isnt"></a>Qué es el Canal semianual y qué no es
Al ser la primera versión en este nuevo canal, Windows Server, versión 1709 *no* es una "actualización" o "service pack" para Windows Server 2016. Es la primera de las dos versiones anuales de servidor en una **nueva pista de versiones** que está diseñada para los clientes que se mueven con una "cadencia de la nube", como por ejemplo, aquellos con ciclos de desarrollo rápidos o proveedores de servicios de alojamiento que se mantienen al día en las últimas inversiones en Hyper-V. Cada versión de esta pista tendrá soporte técnico durante 18 meses desde el lanzamiento inicial. Para más información sobre el canal semianual y **sugerencias para decidir a qué canal unirse** (o permanecer en el mismo) consulta [Información general del canal semianual](semi-annual-channel-overview.md).


**El producto de canal de mantenimiento a largo plazo (LTSC) actual es Windows Server 2016**. El LTSC es mejor si necesitas estabilidad a largo plazo y previsibilidad en el sistema operativo del servidor para admitir aplicaciones y cargas de trabajo tradicionales. Si quieres mantenerte en el LTSC, debes instalar (o seguir usando) Windows Server 2016, que puede instalarse en el modo Server Core o en el modo servidor con modo Experiencia de escritorio. Consulta [Introducción a Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics) para obtener más información.


## <a name="whats-different-about-1709"></a>¿Cuáles son las diferencias de la versión 1709?

Windows Server, versión 1709 se ejecuta en el modo Server Core. Esto significa que no hay interfaz de usuario gráfica, por lo que debes realizar la administración de forma remota. Sin embargo, ofrece grandes ventajas, como menores requisitos de hardware, superficie de ataque mucho más reducida y una reducción en la necesidad de actualizaciones. Si acabas de empezar a trabajar con Server Core, [Administrar un servidor Server Core](../administration/server-core/server-core-manage.md) te ayudará a acostumbrarte a este entorno. [Administrar Windows Server 2016](../administration/manage-windows-server.md) muestra las distintas opciones para administrar los servidores de forma remota.

[Novedades de Windows Server versión 1709](whats-new-in-windows-server-1709.md) es una introducción a las nuevas características y funciones añadidas a Windows Server, versión 1709.

### <a name="why-does-windows-server-version-1709-offer-only-the-server-core-installation-option"></a>¿Por qué Windows Server, versión 1709 solo ofrece la opción de instalación Server Core?
Uno de los pasos más importantes que tomamos en la planificación de todas las versiones de Windows Server es escuchar las opiniones de los clientes: ¿cómo utilizas Windows Server? ¿Las nuevas características tendrán el mayor impacto en las implementaciones de Windows Server y, por extensión, en tus actividades cotidianas? Tus comentarios nos informan que ofrecer innovación de la forma más rápida y eficaz posible es una prioridad clave. Al mismo tiempo, en el caso de los clientes que quieren innovar de forma más rápida, nos has comentado que principalmente usas un scripting de línea de comandos con PowerShell para administrar los centros de datos y, como tal, no necesitas que la GUI de escritorio esté disponible en la instalación de Windows Server con Experiencia de escritorio. Al centrarse en la opción de instalación de Server Core, podemos dedicar más recursos a estas nuevas innovaciones, a la vez que mantenemos la funcionalidad de la plataforma tradicional Windows Server y la compatibilidad de aplicaciones. Si tienes comentarios sobre este u otros problemas relativos a Windows Server y nuestras versiones futuras, puedes hacer sugerencias y comentarios a través del [Centro de opiniones](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).


### <a name="what-about-nano-server"></a>¿Qué sucede con Nano Server?
Nano Server está disponible como un sistema operativo de contenedor. Consulta [Cambios de Nano Server en el canal semianual de Windows Server](nano-in-semi-annual-channel.md) para obtener más información.

## <a name="additional-information-about-this-release"></a>Información adicional sobre este lanzamiento
Para obtener una vista completa de los principales datos sobre Windows Server, versión 1709, también debes revisar estos temas antes de instalarlo:

- ¿Qué hardware es necesario para ejecutarlo? Consulta [Requisitos del sistema](system-requirements.md); los requisitos del sistema para esta versión son los mismos que para Windows Server 2016.
- ¿Qué nuevas funciones y funcionalidades se han añadido? Consulta [Novedades de Windows Server, versión 1709](whats-new-in-windows-server-1709.md).
- ¿Qué se ha eliminado? Consulta [Funciones eliminadas o planificadas para su reemplazo a partir de Windows Server (versión 1709)](Removed-Features-1709.md)
- ¿En qué problemas exclusivos de esta versión es necesario trabajar? Consulta [Notas de la versión: problemas importantes de Windows Server, versión 1709](server-1709-relnotes.md)


## <a name="where-to-obtain-windows-server-version-1709"></a>Dónde obtener Windows Server, versión 1709

Esta versión debe instalarse como una instalación limpia.

- VLSC: Los clientes de licencias por volumen con [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) puede obtener esta versión, vaya a la [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) y haga clic en **sesión**. Luego haz clic en **Descargas y claves** y busca esta versión. 

- Windows Server, versión 1709, también está disponible en  [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Los participantes en **suscripciones de Visual Studio:** Si ya participa en suscripciones de Visual Studio, puede obtener Windows Server, versión 1709, vaya a la [página de descarga de suscriptor de Visual Studio](https://my.visualstudio.com/downloads?pid=2347) y completar la descarga disponible allí. Si ya no eres un suscriptor, dirígete a [Suscripciones de Visual Studio](https://www.visualstudio.com/subscriptions/) para registrarte y luego visita la [página de descarga de suscriptor de Visual Studio](https://my.visualstudio.com/downloads?pid=2347) tal y como se indica anteriormente. Las versiones obtenidas a través de suscripciones de Visual Studio son solo para desarrollo y pruebas.




## <a name="activating-windows-server-version-1709"></a>Activación de Windows Server, versión 1709

- Si has obtenido esta versión desde el Centro de servicios de licencias por volumen, puedes activarla con la clave de activación de Windows Server 2016.
- Si estás usando Microsoft Azure, esta versión debería activarse automáticamente.
- Si obtienes esta versión desde Suscripciones de Visual Studio, se debería activar automáticamente.