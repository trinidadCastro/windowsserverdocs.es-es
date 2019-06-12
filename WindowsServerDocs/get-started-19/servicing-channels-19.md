---
title: Ramas de mantenimiento
description: 'Explicación de los canales de servicio de Windows Server: LTSC y SAC'
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 05/21/2019
ms.openlocfilehash: dee19cd5a30b7d913a7faeeaa38368cee8a91895
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442294"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Canales de servicio de Windows Server: LTSC y SAC

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Existen dos canales de versión principal disponibles para clientes de Windows Server, el Canal de mantenimiento a largo plazo y el Canal semianual.

Puedes mantener los servidores en el Canal de mantenimiento a largo plazo (LTSC), moverlos al Canal semianual o tener algunos servidores en cada pista, en función de lo que funcione mejor para tus necesidades.

## <a name="long-term-servicing-channel-ltsc"></a>Canal de mantenimiento a largo plazo (LTSC)

Este es el modelo de distribución con el que ya estás familiarizado (anteriormente denominado "*Rama* de mantenimiento a largo plazo"), donde se lanza una nueva versión principal de Windows Server cada 2 o 3 años. Los usuarios tienen derecho a 5 años de soporte estándar y 5 años de soporte extendido. Este canal es adecuado para los sistemas que requieren una opción de mantenimiento prolongado y una estabilidad funcional. Las implementaciones de Windows Server 2016 y las versiones anteriores de Windows Server no se verán afectadas por las nuevas versiones del canal semianual. El canal de mantenimiento a largo plazo seguirá recibiendo actualizaciones de seguridad y no relacionadas con la seguridad, pero no recibirá las nuevas funciones y funcionalidades.

> [!Note]  
> **El producto LTSC actual es Windows Server 2019**. Si quieres mantenerte en este canal, debes instalar (o seguir usando) Windows Server 2019, que puede instalarse en la opción de instalación básica de servidor o en la opción de instalación de Experiencia de escritorio.

## <a name="semi-annual-channel"></a>Canal semianual

El canal semianual es perfecto para los clientes que están a innovar rápidamente para aprovechar las nuevas capacidades de sistema operativo a un ritmo más rápido, tanto en aplicaciones, especialmente aquellos que creó en contenedores y microservicios, así como en definido por el software Centro de datos híbrida. Los productos de Windows Server del canal semianual tendrán novedades disponibles dos veces al año, en primavera y en otoño. Cada lanzamiento de este canal tendrá soporte técnico durante 18 meses desde el lanzamiento inicial.

La mayoría de las funciones presentadas en el canal semianual se acumularán en la próxima versión del canal de mantenimiento a largo plazo de Windows Server. Las ediciones, la funcionalidad y el contenido de soporte pueden variar entre las distintas versiones, en función de los comentarios de los clientes.

Está disponible para los clientes de licencias por volumen con el canal semianual [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx), así como a través de Azure Marketplace o de otro en la nube y hospedaje proveedores de servicios y la fidelidad programas tales como suscripciones de Visual Studio.

> [!Note]  
> **La versión actual del canal semianual es Windows Server, versión 1903**. Si desea colocar los servidores en este canal, debe instalar a Windows Server, versión 1903, que se puede instalar en modo Server Core o Nano Server que se ejecute en un contenedor. No se admiten las actualizaciones en contexto desde desde una versión de canal de mantenimiento a largo plazo porque se encuentran en **los canales de lanzamiento distintos**. Versiones de canal semianual no son las actualizaciones: es la próxima versión de Windows Server en el canal semianual.

En este modelo, se identifican las versiones de Windows Server mediante el año y el mes de la versión: por ejemplo, en 2017, una versión en el mes 9 (septiembre) se identificaría como **versión 1709**. Las versiones nuevas de Windows Server en el canal semianual aparecerán dos veces al año. El ciclo de vida de soporte técnico de cada versión es de 18 meses.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>¿Debes mantener los servidores en el LTSC o moverlos al canal semianual?

Estas son las diferencias clave a tener en cuenta:

- ¿Necesitas innovar rápidamente? ¿Necesitas acceso anticipado a las funciones más recientes de Windows Server? ¿Necesitas trabajar con aplicaciones híbridas de cadencia rápida, dev-ops y tejidos Hyper-V? Si es así, debe considerar **al canal semianual** instalando **Windows Server, versión 1903**. Como se describe en este tema, recibirás nuevas versiones dos veces al año, con 18 meses de soporte de producción estándar por versión. Se obtienen a través de licencias por volumen, Azure o servicios de suscripción de Visual Studio. Actualmente, las versiones del canal semianual requieren licencias por volumen y Software Assurance, si vas a ejecutar el producto en producción.
- ¿Necesitas estabilidad y previsibilidad? ¿Necesitas ejecutar máquinas virtuales y cargas de trabajo tradicionales en servidores físicos? Si es así, debes plantearte **mantener esos servidores en el canal de mantenimiento a largo plazo**. La versión LTSC actual es **Windows Server 2019**. Como se describe en este tema, tendrás acceso a las nuevas versiones cada 2-3 años, con 5 años de soporte estándar, seguidos de 5 años de soporte extendido por versión. Las versiones de la LTSC están disponibles a través de todos los mecanismos de distribución. Las versiones de la LTSC están disponibles para cualquiera, independientemente del modelo de licencias que estén usando. 

La siguiente tabla resume las principales diferencias entre los canales:


|                       |                                                              Canal de mantenimiento a largo plazo (Windows Server 2019)                                                               |                                   Canal semianual (Windows Server)                                   |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Escenarios recomendados | Servidores de archivos de uso general, cargas de trabajo de Microsoft y que no sean de Microsoft, aplicaciones tradicionales, roles de infraestructura, centro de datos definido mediante software e infraestructura hiperconvergida | Aplicaciones en contenedor, hosts de contenedor y escenarios de aplicaciones que se benefician de una innovación más rápida |
|     Nuevas versiones      |                                                                               Cada 2–3 años                                                                                |                                              Cada 6 meses                                              |
|        Soporte        |                                                       5 años de soporte estándar, más 5 años de soporte ampliado                                                        |                                                18 meses                                                 |
|       Ediciones        |                                                                    Todas las ediciones de Windows Server disponibles                                                                     |                                     Ediciones Standard y Datacenter                                     |
|      Quién puede usarlas      |                                                                      Todos los clientes a través de todos los canales                                                                      |                               Solo clientes de Software Assurance y de la nube                                |
| Opciones de instalación  |                                                                Server Core y Server con Experiencia de escritorio                                                                |                 Server Core para host de contenedor e imagen e imagen de contenedor de Nano Server                 |

## <a name="device-compatibility"></a>Compatibilidad de dispositivos

A menos que se indique lo contrario, los requisitos mínimos de hardware para ejecutar los lanzamientos del canal semianual serán los mismos que para la versión más reciente del canal de mantenimiento a largo plazo de Windows Server. Por ejemplo, **la versión actual del canal de mantenimiento a largo plazo es Windows Server 2019**. La mayoría de los controladores de hardware seguirán funcionando en estas versiones.

## <a name="servicing"></a>Mantenimiento

Tanto los lanzamientos del canal de mantenimiento a largo plazo como los del canal semianual serán compatibles con las actualizaciones de seguridad y las actualizaciones no relacionadas con la seguridad. La diferencia es el período de tiempo que el lanzamiento es compatible, tal y como se ha indicado anteriormente.

### <a name="servicing-tools"></a>Herramienta de mantenimiento

Existen muchas herramientas con las que los profesionales de TI pueden realizar el mantenimiento de Windows Server. Cada opción tiene sus ventajas y desventajas, que van desde las funcionalidades y el control hasta la simplicidad y los escasos requisitos administrativos. Los siguientes son ejemplos de las herramientas de mantenimiento disponibles para administrar las actualizaciones de mantenimiento:

- **Actualización de Windows (independiente)** : Esta opción solo está disponible para los servidores que están conectados a Internet y tienen habilitada la actualización de Windows.
- **Windows Server Update Services (WSUS)** ofrece un amplio control sobre las actualizaciones de Windows 10 y Windows Server, y se encuentra disponible de forma nativa en el sistema operativo Windows Server. Además de la posibilidad de aplazar actualizaciones, las organizaciones pueden agregar una capa de aprobación para las actualizaciones y optar por implementarlas en equipos específicos o grupos de equipos cuando estén preparados.
- **System Center Configuration Manager** ofrece un control más amplio sobre el mantenimiento. Los profesionales de TI pueden aplazar y aprobar actualizaciones, y tienen varias opciones para seleccionar el destino de las implementaciones y administrar el uso del ancho de banda y los tiempos de implementación.

Es probable que ya hayas elegido usar al menos una de estas opciones en función de los recursos, el personal y la experiencia. Puedes seguir usando el mismo proceso para los lanzamientos del Canal semianual: por ejemplo, si ya usas System Center Configuration Manager para administrar las actualizaciones, puedes seguir usándolo. De manera similar, si usas WSUS, puedes seguir usándolo.

## <a name="where-to-obtain-semi-annual-channel-releases"></a>Libera dónde obtener el canal semianual

Versiones de canal semianual deben instalarse como una instalación limpia.

- Volume Licensing Service Center (VLSC): Los clientes de licencias por volumen con [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) puede obtener esta versión, vaya a la [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) y haga clic en **sesión**. Luego haz clic en **Descargas y claves** y busca esta versión. 

- También están disponibles en versiones de canal semianual [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Suscripciones de Visual Studio: Suscriptores de Visual Studio puede obtener versiones de canal semianual descargándolos desde el [página de descarga de suscriptor de Visual Studio](https://my.visualstudio.com/downloads?pid=2347). Si ya no eres un suscriptor, dirígete a [Suscripciones de Visual Studio](https://www.visualstudio.com/subscriptions/) para registrarte y luego visita la [página de descarga de suscriptor de Visual Studio](https://my.visualstudio.com/downloads?pid=2347) tal y como se indica anteriormente. Las versiones obtenidas a través de suscripciones de Visual Studio son solo para desarrollo y pruebas.

- Obtenga las versiones preliminares a través del programa de Windows Insider: Las pruebas en las primeras compilaciones de Windows Server ayudan a Microsoft y a sus clientes, ya que tienen la oportunidad de detectar posibles problemas incluso antes del lanzamiento. También ofrece a los clientes una oportunidad única para influir directamente en la funcionalidad del producto.   
Microsoft depende de la recepción de comentarios durante todo el proceso de desarrollo para poder realizar ajustes tan rápido como sea posible. Las primeras pruebas y los comentarios son esenciales para un modelo de lanzamiento rápido. Para obtener relacionados con el programa de Windows Insider, consulte el [Windows Insider programa para Server](https://docs.microsoft.com/windows-insider/at-work/).

## <a name="activating-semi-annual-channel-releases"></a>Libera la activación de canal semianual

- Si usa Microsoft Azure, automáticamente se debe activar esta versión.
- Si ha obtenido esta versión desde el centro de servicios de licencias por volumen o suscripciones de Visual Studio, puede activarla mediante el uso de su CSVLK de 2019 de Windows Server con el entorno de sistema de administración de claves (KMS). Para obtener más información, consulte [claves de configuración de cliente KMS](../get-started/kmsclientkeys.md).

Las versiones de canal semianual que se publicaron antes de Windows Server 2019 utilizan el CSVLK de Windows Server 2016.

## <a name="why-do-semi-annual-channel-releases-offer-only-the-server-core-installation-option"></a>¿Por qué versiones de canal semianual ofrecen sólo la opción de instalación Server Core?

Uno de los pasos más importantes que tomamos en la planificación de todas las versiones de Windows Server es escuchar las opiniones de los clientes: ¿cómo utilizas Windows Server? ¿Las nuevas características tendrán el mayor impacto en las implementaciones de Windows Server y, por extensión, en tus actividades cotidianas? Tus comentarios nos informan que ofrecer innovación de la forma más rápida y eficaz posible es una prioridad clave. Al mismo tiempo, para los clientes a innovar más rápidamente, se nos ha dicho lo que principalmente usa secuencias de comandos de línea de comandos de PowerShell para administrar sus centros de datos y, por lo tanto no tiene una fuerte necesidad de la GUI del escritorio disponible en la instalación de Windows Server con experiencia de escritorio, especialmente ahora que [Windows Admin Center](../manage/windows-admin-center/overview.md) está disponible para administrar los servidores de forma remota.

Al centrarse en la opción de instalación de Server Core, podemos dedicar más recursos a estas nuevas innovaciones, a la vez que mantenemos la funcionalidad de la plataforma tradicional Windows Server y la compatibilidad de aplicaciones. Si tienes comentarios sobre este u otros problemas relativos a Windows Server y nuestras versiones futuras, puedes hacer sugerencias y comentarios a través del [Centro de opiniones](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

## <a name="what-about-nano-server"></a>¿Qué sucede con Nano Server?

Nano Server está disponible como un sistema operativo de contenedor en el canal semianual. Consulta [Cambios de Nano Server en el canal semianual de Windows Server](../get-started/nano-in-semi-annual-channel.md) para obtener más información.

## <a name="how-to-tell-whether-a-server-is-running-an-ltsc-or-sac-release"></a>Cómo saber si un servidor se está ejecutando una versión LTSC o SAC

Por lo general, canal de servicio a largo plazo de las versiones como Windows Server 2019 se liberan al mismo tiempo como una nueva versión del canal semianual, por ejemplo, Windows Server, versión 1809. Esto puede hacer un poco complicado determinar si está ejecutando un servidor de publicación en el canal semianual. En lugar de examinar el número de compilación, debe mirar el nombre del producto: Versiones de canal semianual de usar el nombre de producto "Windows Server Standard" o "Windows Server Datacenter", sin un número de versión, aunque las versiones de canal de servicio a largo plazo se incluyen el número de versión, por ejemplo, "Windows Server Datacenter de 2019".

>[!Note]  
> La siguiente guía tiene como objetivo ayudar a identificar y diferenciar entre LTSC y SAC únicamente con fines de ciclo de vida y de inventario general.  No tiene como fin la compatibilidad de aplicaciones o la representación de una superficie de API específica.  Los desarrolladores de aplicaciones deben utilizar otras instrucciones para garantizar la compatibilidad correcta, ya que se pueden agregar componentes, API y funcionalidades durante el transcurso de vida de un sistema o puede que aún no se agreguen. La [versión del sistema operativo](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) es un mejor punto de partida para los desarrolladores de aplicaciones.

Abre Powershell y usa el cmdlet Get-ItemProperty o el cmdlet Get-ComputerInfo para comprobar estas propiedades en el registro.  Junto con el número de compilación, sabrás si se trata de LTSC o SAC por la presencia o ausencia del año de publicación; es decir, 2019.  LTSC lo tiene, SAC no.  También se devolverá el momento del lanzamiento en ReleaseId o WindowsVersion (es decir, 1809), así como si la instalación es Server Core o Server con Experiencia de escritorio. 

**Windows Server 2019 Datacenter Edition (LTSC) con el ejemplo de la experiencia de escritorio:**

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

**Windows Server, ejemplo de Standard Edition Server Core de versión 1809 (SAC):**

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

**Ejemplo de Windows Server 2019 Standard Edition (LTSC) Server Core:**


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

Para consultar si en un servidor está presente la nueva [característica de compatibilidad de aplicaciones de Server Core a petición](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) , usa el Cmdlet [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) y busca:
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

## <a name="see-also"></a>Vea también

[Cambios realizados en Nano Server en canal semianual de Windows Server](../get-started/nano-in-semi-annual-channel.md)

[Ciclo de vida de soporte técnico de Windows Server](https://support.microsoft.com/lifecycle)

[Determinar si se está ejecutando Server Core](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[Función GetProductInfo](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Cmdlets de registro de inventario de software](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
