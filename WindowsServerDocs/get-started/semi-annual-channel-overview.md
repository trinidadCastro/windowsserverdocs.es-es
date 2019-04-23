---
title: Introducción al Canal semianual de Windows Server
description: Microsoft ha simplificado el mantenimiento de Windows Server para que las actualizaciones de sistema operativo sean más fáciles de probar, administrar e implementar.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 05/07/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 2995fca3085d6611ecce083685dca0e587913f17
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841246"
---
# <a name="windows-server-semi-annual-channel-overview"></a>Introducción al Canal semianual de Windows Server

>Se aplica a: Windows Server (Canal semianual)

El modelo de versión de Windows Server ofrece una nueva opción para alinearse con modelos de mantenimiento y lanzamiento similares para [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) y [Office 365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). Si has estado trabajando con Windows 10 u Office 365 ProPlus, es posible que ya estés familiarizado con estas mejoras.

**Hay dos canales de versión principal disponible para los clientes de Windows Server, el canal de mantenimiento a largo plazo y el canal semianual.** Puedes mantener los servidores en el Canal de mantenimiento a largo plazo (LTSC), moverlos al Canal semianual o tener algunos servidores en cada pista, en función de lo que funcione mejor para tus necesidades.


## <a name="long-term-servicing-channel-ltsc"></a>Canal de mantenimiento a largo plazo (LTSC)
Este es el modelo de distribución con el que ya estás familiarizado (anteriormente denominado "*Rama* de mantenimiento a largo plazo"), donde se lanza una nueva versión principal de Windows Server cada 2 o 3 años. Los usuarios tienen derecho a 5 años de soporte estándar y 5 años de soporte extendido. Este canal es adecuado para los sistemas que requieren una opción de mantenimiento prolongado y una estabilidad funcional. Las implementaciones de Windows Server 2016 y las versiones anteriores de Windows Server no se verán afectadas por las nuevas versiones del canal semianual. El canal de mantenimiento a largo plazo seguirá recibiendo actualizaciones de seguridad y no relacionadas con la seguridad, pero no recibirá las nuevas funciones y funcionalidades.

> [!Note]  
> **El producto LTSC actual es Windows Server 2016**. Si quieres mantenerte en este canal, debes instalar (o seguir usando) Windows Server 2016, que puede instalarse en la opción de instalación básica de servidor o en la opción de instalación de Experiencia de escritorio. Consulta [Introducción a Windows Server 2016](server-basics.md) para obtener más información. 



## <a name="semi-annual-channel"></a>Canal semianual 
El canal semianual es ideal para los clientes que innovan de forma rápida para sacar partido de nuevas funciones del sistema operativo a un ritmo más rápido, tanto en aplicaciones, especialmente las integradas en contenedores y microservicios, como en el centro de datos híbrido definido por software. Los productos de Windows Server del canal semianual tendrán novedades disponibles dos veces al año, en primavera y en otoño. Cada lanzamiento de este canal tendrá soporte técnico durante 18 meses desde el lanzamiento inicial.

La mayoría de las funciones presentadas en el canal semianual se acumularán en la próxima versión del canal de mantenimiento a largo plazo de Windows Server. Las ediciones, la funcionalidad y el contenido de soporte pueden variar entre las distintas versiones, en función de los comentarios de los clientes.

El Canal semianual estará disponible para los clientes de licencias por volumen con [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx), así como a través de Azure Marketplace u otros proveedores de servicios de hosting/nube y programas de fidelidad, como por ejemplo, Suscripciones de Visual Studio.

> [!Note]  
> **La versión actual del Canal semianual es Windows Server, versión 1803**. Si quieres poner servidores en este canal, debes instalar Windows Server, versión 1803, que puede instalarse en modo Server Core o en modo Nano Server ejecutado en un contenedor. Consulta [Introducción a Windows Server, versión 1803](get-started-with-1803.md) para obtener información sobre cómo obtener y activar Windows Server, versión 1803. No se admiten las actualizaciones en contexto de Windows Server 2016 para Windows Server, versión 1803, porque están en **diferentes canales de distribución**. Windows Server, versión 1803 no es una actualización de Windows Server 2016, es la siguiente versión de Windows Server en el canal semianual.



En este modelo, se identifican las versiones de Windows Server mediante el año y el mes de la versión: por ejemplo, en 2017, una versión en el mes 9 (septiembre) se identificaría como **versión 1709**. Las versiones nuevas de Windows Server en el canal semianual aparecerán dos veces al año. El ciclo de vida de soporte técnico de cada versión es de 18 meses.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>¿Debes mantener los servidores en el LTSC o moverlos al canal semianual?
Estas son las diferencias clave a tener en cuenta:

- ¿Necesitas innovar rápidamente? ¿Necesitas acceso anticipado a las funciones más recientes de Windows Server? ¿Necesitas trabajar con aplicaciones híbridas de cadencia rápida, dev-ops y tejidos Hyper-V? Si es así, debes plantearte **unirte al canal semianual** instalando [Windows Server, versión 1803](get-started-with-1803.md). Como se describe en este tema, recibirás nuevas versiones dos veces al año, con 18 meses de soporte de producción estándar por versión. Se obtienen a través de licencias por volumen, Azure o servicios de suscripción de Visual Studio. Actualmente, las versiones del canal semianual requieren licencias por volumen y Software Assurance, si vas a ejecutar el producto en producción.
- ¿Necesitas estabilidad y previsibilidad? ¿Necesitas ejecutar máquinas virtuales y cargas de trabajo tradicionales en servidores físicos? Si es así, debes plantearte **mantener esos servidores en el canal de mantenimiento a largo plazo**. La versión LTSC actual es [Windows Server 2016](server-basics.md). Como se describe en este tema, tendrás acceso a las nuevas versiones cada 2-3 años, con 5 años de soporte estándar, seguidos de 5 años de soporte extendido por versión. Las versiones de la LTSC están disponibles a través de todos los mecanismos de distribución. Las versiones de la LTSC están disponibles para cualquiera, independientemente del modelo de licencias que estén usando. 

En la siguiente tabla se resumen las principales diferencias entre los canales, empezando por Windows Server, versión 1803:

|  | Canal de mantenimiento a largo plazo (Windows Server 2016) |Canal semianual (Windows Server) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|Escenarios recomendados | Servidores de archivos de uso general, cargas de trabajo de Microsoft y que no sean de Microsoft, aplicaciones tradicionales, roles de infraestructura, centro de datos definido mediante software e infraestructura hiperconvergida | Aplicaciones en contenedor, hosts de contenedor y escenarios de aplicaciones que se benefician de una innovación más rápida |
| Nuevas versiones | Cada 2–3 años |Cada 6 meses |
| Soporte |5 años de soporte estándar, más 5 años de soporte ampliado | 18 meses |
| Ediciones | Todas las ediciones de Windows Server disponibles | Ediciones Standard y Datacenter |
| Quién puede usarlas | Todos los clientes a través de todos los canales | Solo clientes de Software Assurance y de la nube |
| Opciones de instalación | Server Core y Server con Experiencia de escritorio | Server Core para host de contenedor e imagen e imagen de contenedor de Nano Server |                |


## <a name="device-compatibility"></a>Compatibilidad de dispositivos
A menos que se indique lo contrario, los requisitos mínimos de hardware para ejecutar los lanzamientos del canal semianual serán los mismos que para la versión más reciente del canal de mantenimiento a largo plazo de Windows Server. Por ejemplo, **la versión actual del canal de mantenimiento a largo plazo es Windows Server 2016**. La mayoría de los controladores de hardware seguirán funcionando en estas versiones.

## <a name="servicing"></a>Mantenimiento
Tanto los lanzamientos del canal de mantenimiento a largo plazo como los del canal semianual serán compatibles con las actualizaciones de seguridad y las actualizaciones no relacionadas con la seguridad. La diferencia es el período de tiempo que el lanzamiento es compatible, tal y como se ha indicado anteriormente.

### <a name="servicing-tools"></a>Herramienta de mantenimiento
Existen muchas herramientas con las que los profesionales de TI pueden realizar el mantenimiento de Windows Server. Cada opción tiene sus ventajas y desventajas, que van desde las funcionalidades y el control hasta la simplicidad y los escasos requisitos administrativos. Los siguientes son ejemplos de las herramientas de mantenimiento disponibles para administrar las actualizaciones de mantenimiento:

- **Actualización de Windows (independiente)**: Esta opción solo está disponible para los servidores que están conectados a Internet y tienen habilitada la actualización de Windows.
- **Windows Server Update Services (WSUS)** ofrece un amplio control sobre las actualizaciones de Windows 10 y Windows Server, y se encuentra disponible de forma nativa en el sistema operativo Windows Server. Además de la posibilidad de aplazar actualizaciones, las organizaciones pueden agregar una capa de aprobación para las actualizaciones y optar por implementarlas en equipos específicos o grupos de equipos cuando estén preparados.
- **System Center Configuration Manager** ofrece un control más amplio sobre el mantenimiento. Los profesionales de TI pueden aplazar y aprobar actualizaciones, y tienen varias opciones para seleccionar el destino de las implementaciones y administrar el uso del ancho de banda y los tiempos de implementación.

Es probable que ya hayas elegido usar al menos una de estas opciones en función de los recursos, el personal y la experiencia. Puedes seguir usando el mismo proceso para los lanzamientos del Canal semianual: por ejemplo, si ya usas System Center Configuration Manager para administrar las actualizaciones, puedes seguir usándolo. De manera similar, si usas WSUS, puedes seguir usándolo.

## <a name="obtain-preview-releases-through-the-windows-insider-program"></a>Obtener las versiones preliminares mediante el Programa Windows Insider
Las pruebas en las primeras compilaciones de Windows Server ayudan a Microsoft y a sus clientes, ya que tienen la oportunidad de detectar posibles problemas incluso antes del lanzamiento. También ofrece a los clientes una oportunidad única para influir directamente en la funcionalidad del producto. 

Microsoft depende de la recepción de comentarios durante todo el proceso de desarrollo para poder realizar ajustes tan rápido como sea posible. Las primeras pruebas y los comentarios son esenciales para un modelo de lanzamiento rápido.

Para obtener más información sobre cómo involucrarse en el Programa Windows Insider, consulta la sección [Programa Windows Insider para documentos de servidor](https://docs.microsoft.com/windows-insider/at-work/).
# <a name="related-topics"></a>Temas relacionados
[Canales de servicio de Windows Server 2019: LTSC y SAC](https://docs.microsoft.com/windows-server/get-started-19/servicing-channels-19)

[Cambios realizados en Nano Server en canal semianual de Windows Server](nano-in-semi-annual-channel.md)

[Ciclo de vida de soporte técnico de Windows Server](https://support.microsoft.com/en-us/lifecycle)

[Requisitos del sistema de Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/system-requirements) 




