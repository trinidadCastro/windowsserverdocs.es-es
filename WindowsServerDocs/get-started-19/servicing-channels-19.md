---
title: Canales de servicio
description: 'Explicación de los canales de servicio de Windows Server: LTSC y SAC'
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 05/21/2019
ms.openlocfilehash: 0190fc05a7bf82e35339d93accae3a998babe166
ms.sourcegitcommit: 7116460855701eed4e09d615693efa4fffc40006
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/15/2020
ms.locfileid: "83433139"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Canales de servicio de Windows Server: LTSC y SAC

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Existen dos canales de versión principal disponibles para clientes de Windows Server, el Canal de mantenimiento a largo plazo y el Canal semianual.

Puedes mantener los servidores en el Canal de mantenimiento a largo plazo (LTSC), moverlos al Canal semianual o tener algunos servidores en cada pista, en función de lo que funcione mejor para tus necesidades.

## <a name="long-term-servicing-channel-ltsc"></a>Canal de mantenimiento a largo plazo (LTSC)

Este es el modelo de distribución con el que ya estás familiarizado (anteriormente denominado "*Rama* de mantenimiento a largo plazo"), en que se publica una nueva versión principal de Windows Server cada 2 o 3 años. Los usuarios tienen derecho a 5 años de soporte estándar y 5 años de soporte extendido. Este canal es adecuado para los sistemas que requieren una opción de mantenimiento prolongado y una estabilidad funcional. Las implementaciones de Windows Server 2016 y las versiones anteriores de Windows Server no se verán afectadas por las nuevas versiones del canal semianual. El Canal de mantenimiento a largo plazo seguirá recibiendo actualizaciones de seguridad y no relacionadas con la seguridad, pero no recibirá las nuevas funciones y funcionalidades.

> [!Note]
> **El producto LTSC actual es Windows Server 2019**. Si quieres mantenerte en este canal, debes instalar (o seguir usando) Windows Server 2019, que puede instalarse en la opción de instalación básica de servidor o en la opción de instalación de experiencia de escritorio.

## <a name="semi-annual-channel"></a>Canal semianual

El canal semianual es ideal para los clientes que innovan de forma rápida para sacar partido de nuevas funciones del sistema operativo a un ritmo más rápido, tanto en aplicaciones, especialmente las integradas en contenedores y microservicios, como en el centro de datos híbrido definido por software. Los productos de Windows Server del canal semianual tendrán novedades disponibles dos veces al año, en primavera y en otoño. Cada lanzamiento de este canal tendrá soporte técnico durante 18 meses desde el lanzamiento inicial.

La mayoría de las funciones presentadas en el Canal semianual se acumularán en la próxima versión del Canal de mantenimiento a largo plazo de Windows Server. Las ediciones, la funcionalidad y el contenido de soporte pueden variar entre las distintas versiones, en función de los comentarios de los clientes.

El Canal semianual estará disponible para los clientes de licencias por volumen con [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx), así como a través de Azure Marketplace u otros proveedores de servicios de hosting/nube y programas de fidelidad como, por ejemplo, Suscripciones de Visual Studio.

> [!Note]
> **La versión actual del Canal semianual es Windows Server, versión 1903**. Si quieres poner servidores en este canal, debes instalar Windows Server, versión 1903, que puede instalarse en modo Server Core o en modo Nano Server ejecutado en un contenedor. No se admiten actualizaciones en contexto desde una versión de Canal de mantenimiento a largo plazo porque se encuentran en **canales de lanzamiento distintos**. Los lanzamientos del Canal semianual no son actualizaciones: son la siguiente versión de Windows Server en este canal.

En este modelo, se identifican las versiones de Windows Server mediante el año y el mes de la versión: por ejemplo, en 2017, una versión en el mes 9 (septiembre) se identificaría como **versión 1709**. Las versiones nuevas de Windows Server en el Canal semianual aparecerán dos veces al año. El ciclo de vida de soporte técnico de cada versión es de 18 meses.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>¿Debes mantener los servidores en el LTSC o moverlos al Canal semianual?

Estas son las diferencias clave a tener en cuenta:

- ¿Necesitas innovar rápidamente? ¿Necesitas acceso anticipado a las funciones más recientes de Windows Server? ¿Necesitas trabajar con aplicaciones híbridas de cadencia rápida, dev-ops y tejidos Hyper-V? Si es así, debes plantearte **unirte al Canal semianual** instalando **Windows Server, versión 1903**. Como se describe en este tema, recibirás nuevas versiones dos veces al año, con 18 meses de soporte de producción estándar por versión. Se obtienen a través de licencias por volumen, Azure o servicios de suscripción de Visual Studio. Actualmente, las versiones del Canal semianual requieren licencias por volumen y Software Assurance, si vas a ejecutar el producto en producción.
- ¿Necesitas estabilidad y previsibilidad? ¿Necesitas ejecutar máquinas virtuales y cargas de trabajo tradicionales en servidores físicos? Si es así, debes plantearte **mantener esos servidores en el Canal de mantenimiento a largo plazo**. La versión del LTSC actual es **Windows Server 2019**. Como se describe en este tema, tendrás acceso a las nuevas versiones cada 2-3 años, con 5 años de soporte estándar, seguidos de 5 años de soporte extendido por versión. Las versiones del LTSC están disponibles a través de todos los mecanismos de distribución. Las versiones del LTSC están disponibles para cualquiera, independientemente del modelo de licencias que estén usando.

La siguiente tabla resume las principales diferencias entre los canales:


|                       |                                                              Canal de mantenimiento a largo plazo (Windows Server 2019)                                                               |                                   Canal semianual (Windows Server)                                   |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Escenarios recomendados | Servidores de archivos de uso general, cargas de trabajo de Microsoft y otras que no lo son, aplicaciones tradicionales, roles de infraestructura, centro de datos definido mediante software e infraestructura hiperconvergida | Aplicaciones en contenedor, hosts de contenedor y escenarios de aplicaciones que se benefician de una innovación más rápida |
|     Nuevas versiones      |                                                                               Cada 2–3 años                                                                                |                                              Cada 6 meses                                              |
|        Soporte técnico        |                                                       5 años de soporte estándar, más 5 años de soporte ampliado                                                        |                                                18 meses                                                 |
|       Ediciones        |                                                                    Todas las ediciones de Windows Server disponibles                                                                     |                                     Ediciones Standard y Datacenter                                     |
|      ¿Quién puede usarlas?      |                                                                      Todos los clientes a través de todos los canales                                                                      |                               Solo clientes de Software Assurance y de la nube                                |
| Opciones de instalación  |                                                                Server Core y Server con experiencia de escritorio                                                                |                 Server Core para host de contenedor e imagen de contenedor de Nano Server                 |

## <a name="device-compatibility"></a>Compatibilidad de dispositivos

A menos que se indique lo contrario, los requisitos mínimos de hardware para ejecutar los lanzamientos del Canal semianual serán los mismos que para la versión más reciente del Canal de mantenimiento a largo plazo de Windows Server. Por ejemplo, **la versión actual del Canal de mantenimiento a largo plazo es Windows Server 2019**. La mayoría de los controladores de hardware seguirán funcionando en estas versiones.

## <a name="servicing"></a>Mantenimiento

Tanto los lanzamientos del Canal de mantenimiento a largo plazo como los del Canal semianual serán compatibles con las actualizaciones de seguridad y las no relacionadas con la seguridad. La diferencia es el período de tiempo que el lanzamiento es compatible, tal y como se ha indicado anteriormente.

### <a name="servicing-tools"></a>Herramientas de mantenimiento

Existen muchas herramientas con las que los profesionales de TI pueden realizar el mantenimiento de Windows Server. Cada opción tiene sus ventajas y desventajas: desde las funcionalidades y el control hasta la simplicidad y los escasos requisitos administrativos. Los siguientes son ejemplos de las herramientas de mantenimiento disponibles para administrar las actualizaciones de mantenimiento:

- **Windows Update (independiente)** : esta opción solo está disponible para servidores que están conectados a Internet con Windows Update habilitado.
- **Windows Server Update Services (WSUS)** ofrece un amplio control sobre las actualizaciones de Windows 10 y Windows Server, y se encuentra disponible de forma nativa en el sistema operativo Windows Server. Además de la posibilidad de aplazar las actualizaciones, las organizaciones pueden agregar un nivel de aprobación para las actualizaciones y optar por implementarlas en equipos o grupos de equipos específicos cuando estén preparados.
- **Microsoft Endpoint Configuration Manager** ofrece un control más amplio sobre el mantenimiento. Los profesionales de TI pueden aplazar y aprobar las actualizaciones, y tienen varias opciones para seleccionar el destino de las implementaciones y administrar el uso del ancho de banda y las horas de implementación.

Es probable que ya hayas elegido usar al menos una de estas opciones en función de tus recursos, personal y experiencia. Puedes seguir usando el mismo proceso para los lanzamientos del Canal semianual: por ejemplo, si ya usas Configuration Manager para administrar las actualizaciones, puedes seguir usándolo. De manera similar, si usas WSUS, puedes seguir usándolo.

## <a name="where-to-obtain-semi-annual-channel-releases"></a>¿Dónde conseguir las versiones del Canal semianual?

Las versiones del Canal semianual deben instalarse como una instalación limpia.

- Centro de servicio de licencias por volumen (VLSC): los clientes de licencias por volumen con [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx) pueden obtener esta versión si se dirigen al [Centro de servicios de licencias por volumen](https://www.microsoft.com/Licensing/servicecenter/default.aspx) y haces clic en **Inicio de sesión**. Luego haz clic en **Descargas y claves** y busca esta versión.

- Las versiones del Canal semianual también están disponibles en [Microsoft Azure](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Suscripciones a Visual Studio: Los suscriptores de Visual Studio pueden obtener versiones del canal semianual descargándolas desde la [página de descargas del suscriptor de Visual Studio](https://my.visualstudio.com/downloads?pid=2347). Si ya no eres un suscriptor, dirígete a [Suscripciones de Visual Studio](https://www.visualstudio.com/subscriptions/) para registrarte y luego visita la [página de descarga de suscriptor de Visual Studio](https://my.visualstudio.com/downloads?pid=2347) tal y como se indica anteriormente. Las versiones obtenidas a través de suscripciones de Visual Studio son solo para desarrollo y pruebas.

- Obtén las versiones preliminares mediante el Programa Windows Insider: las pruebas en las primeras compilaciones de Windows Server ayudan a Microsoft y a sus clientes, ya que tienen la oportunidad de detectar posibles problemas incluso antes del lanzamiento. También ofrece a los clientes una oportunidad única para influir directamente en la funcionalidad del producto.
Microsoft depende de la recepción de comentarios durante todo el proceso de desarrollo para poder realizar ajustes tan rápido como sea posible. Las primeras pruebas y los comentarios son esenciales para un modelo de lanzamiento rápido. Para involucrarse en el Programa Windows Insider, consulta la sección [Programa Windows Insider para documentos de servidor](https://docs.microsoft.com/windows-insider/at-work/).

## <a name="activating-semi-annual-channel-releases"></a>Activación de las versiones del Canal semianual

- Si estás usando Microsoft Azure, esta versión debería activarse automáticamente.
- Si has obtenido esta versión desde el Centro de servicio de licencias por volumen o de suscripciones de Visual Studio, puedes activarla usando el CSVLK de Windows Server 2019 con el entorno de sistema de administración de claves (KMS). Para más información, consulte [Claves de configuración del cliente KMS](../get-started/kmsclientkeys.md).

Las versiones del Canal semianual que se publicaron antes de Windows Server 2019 utilizan clave de licencia por volumen específica de cliente de Windows Server 2016.

## <a name="why-do-semi-annual-channel-releases-offer-only-the-server-core-installation-option"></a>¿Por qué las versiones del Canal semianual ofrecen solo la opción de instalación básica?

Uno de los pasos más importantes que tomamos en la planificación de todas las versiones de Windows Server es escuchar las opiniones de los clientes: ¿cómo utilizas Windows Server? ¿Qué nuevas características tendrán el mayor impacto en las implementaciones de Windows Server y, por extensión, en tus actividades cotidianas? Los comentarios nos indican que ofrecer innovación de la forma más rápida y eficaz posible es una prioridad clave. Al mismo tiempo, en el caso de los clientes que quieren innovar más rápidamente, nos has comentado que, principalmente, usas un scripting de línea de comandos con PowerShell para administrar los centros de datos y, como tal, no necesitas que la GUI de escritorio esté disponible en la instalación de Windows Server con experiencia de escritorio, especialmente ahora que [Windows Admin Center](../manage/windows-admin-center/overview.md) está disponible para administrar los servidores de forma remota.

Al centrarnos en la opción de instalación básica de servidor, podemos dedicar más recursos a estas nuevas innovaciones, a la vez que mantenemos la funcionalidad de la plataforma tradicional de Windows Server y la compatibilidad de aplicaciones. Si tienes comentarios sobre este u otros problemas relativos a Windows Server y nuestras versiones futuras, puedes hacer sugerencias y comentarios a través del [Centro de opiniones](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

## <a name="what-about-nano-server"></a>¿Qué sucede con Nano Server?

Nano Server está disponible como un sistema operativo de contenedor en el Canal semianual. Consulte [Cambios de Nano Server en el Canal semianual de Windows Server](../get-started/nano-in-semi-annual-channel.md) para más información.

## <a name="how-to-tell-whether-a-server-is-running-an-ltsc-or-sac-release"></a>¿Cómo saber si un servidor está ejecutando una versión del Canal de mantenimiento a largo plazo o del Canal semianual?

Por lo general, las versiones del Canal de mantenimiento a largo plazo como Windows Server 2019 se lanzan al mismo tiempo que una nueva versión del Canal semianual, por ejemplo, Windows Server, versión 1809. Esto puede hacer un poco complicado determinar si un servidor está ejecutando una versión del Canal semianual. En lugar de mirar el número de compilación, debes mirar el nombre del producto: Las versiones del Canal semianual usan el nombre de producto Windows Server Standard o Windows Server Datacenter, sin ningún número de versión, mientras que las del Canal de mantenimiento a largo plazo incluyen un número de versión como, por ejemplo, Windows Server 2019 Datacenter.

> [!Note]
> La siguiente guía tiene como objetivo ayudar a identificar y diferenciar entre LTSC y SAC únicamente con fines de ciclo de vida y de inventario general.  No tiene como fin la compatibilidad de aplicaciones o la representación de una superficie de API específica.  Los desarrolladores de aplicaciones deben utilizar otras orientaciones para garantizar la compatibilidad correcta, ya que se pueden agregar componentes, API y funcionalidades durante el período de vida de un sistema o puede que aún no se agreguen. La [versión del sistema operativo](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) es un punto de partida mejor para los desarrolladores de aplicaciones.

Abre Powershell y usa el cmdlet Get-ItemProperty o el cmdlet Get-ComputerInfo para comprobar estas propiedades en el registro.  Junto con el número de compilación, sabrás si se trata de LTSC o SAC por la presencia o ausencia del año de publicación; es decir, 2019.  LTSC lo tiene, SAC no.  También se devolverá el momento del lanzamiento en ReleaseId o WindowsVersion (es decir, 1809), así como si la instalación es básica o con experiencia de escritorio.

**Ejemplo de Windows Server 2019 Datacenter Edition (LTSC) con experiencia de escritorio:**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server 2019 Datacenter
ReleaseId                 : 1809
InstallationType          : Server
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Ejemplo de Windows Server, versión 1809 (SAC) Standard Edition con instalación básica:**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server Standard
ReleaseId                 : 1809
InstallationType          : Server Core
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Ejemplo de Windows Server 2019 Standard Edition (LTSC) con instalación básica:**


````PowerShell
Get-ComputerInfo | Select WindowsProductName, WindowsVersion, WindowsInstallationType, OsServerLevel, OsVersion, OsHardwareAbstractionLayer
````

````
WindowsProductName            : Windows Server 2019 Standard
WindowsVersion                : 1809
WindowsInstallationType       : Server Core
OsServerLevel                 : ServerCore
OsVersion                     : 10.0.17763
OsHardwareAbstractionLayer    : 10.0.17763.107
````

Para consultar si en un servidor está presente la nueva [característica de compatibilidad de aplicaciones de instalación básica a petición](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19), usa el cmdlet [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) y busca:
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

## <a name="see-also"></a>Consulta también

[Cambios en Nano Server en la versión de Windows Server del Canal semianual](../get-started/nano-in-semi-annual-channel.md)

[Ciclo de vida de soporte técnico de Windows Server](https://support.microsoft.com/lifecycle)

[Determinar si se está ejecutando Server Core](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[Función GetProductInfo](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Cmdlets de registro de inventario de software](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
