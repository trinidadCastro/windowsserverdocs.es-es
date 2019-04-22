---
title: Novedades de Windows Server, versión 1803
description: ¿Cuáles son las nuevas características de proceso, identidad, administración, automatización, redes, seguridad y almacenamiento?
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.date: 05/07/2018
ms.openlocfilehash: c4f80b668b91e65b6c8bc528e14f52a1d117a3c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823096"
---
# <a name="whats-new-in-windows-server-version-1803"></a>Novedades de Windows Server, versión 1803

>Se aplica a: Windows Server (Canal semianual)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;El contenido de esta sección describe las novedades y los cambios de Windows Server, versión 1803. Las nuevas características y los cambios que se muestran aquí son los que probablemente tengan un mayor impacto al trabajar con esta versión. Consulta también [Actualización del Canal semianual de Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/).

## <a name="windows-admin-center"></a>Windows Admin Center

El Proyecto Honolulu ahora es **Windows Admin Center**.
<br>&nbsp;
> [!video https://www.youtube.com/embed/WCWxAp27ERk?autoplay=false]

[Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) consolida todos los aspectos de administración del servidor local y remoto. Windows Admin Center es una experiencia de administración basada en explorador, implementada localmente, que no requiere conexión a Internet y que otorga el control total de todos los aspectos de la implementación de Windows Server.

## <a name="windows-server-release-strategy"></a>Estrategia de versión de Windows Server

Windows Server versión 1709 se lanzó en septiembre de 2017 como la primera versión del Canal semianual. El Canal semianual tiene una cadencia de lanzamiento más rápida y se ocupa de los comentarios de aquellos que desean una innovación rápida cada pocos meses. Complementa al Canal de mantenimiento a largo plazo, donde la cadencia de lanzamiento es cada 2-3 años.

En función de la telemetría y los comentarios, estos canales han demostrado que se ajustan bien a la siguiente estrategia general:
- El Canal semianual es ideal para escenarios de innovación y aplicaciones modernas, como contenedores y microservicios.
- El Canal de mantenimiento a largo plazo es la versión preferida para escenarios de infraestructuras básicos, como el centro de datos definido por el software y la infraestructura hiperconvergida (HCI). 

Los escenarios específicos para el Canal semianual y el Canal de mantenimiento a largo plazo son los siguientes:

|   | Canal de mantenimiento a largo plazo |  Canal semianual |
| ------------- | ------------- | ------------ |
| Escenarios recomendados     | Servidores de archivos de uso general, cargas de trabajo propias y de terceros, aplicaciones tradicionales, roles de infraestructura, centro de datos definido mediante software e infraestructura hiperconvergida  | Aplicaciones en contenedor, hosts de contenedor y escenarios de aplicaciones que se benefician de una innovación más rápida |
| Nuevas versiones  | Cada 2–3 años  | Cada 6 meses |
| Soporte  | 5 años de soporte estándar + 5 años de soporte ampliado  | 18 meses |
| Ediciones  | Todas las ediciones de Windows Server disponibles  | Ediciones Standard y Datacenter |
| Quién puede usarlas  | Todos los clientes a través de todos los canales | Solo clientes de Software Assurance y de la nube |
| Opciones de instalación  | Server Core y Server con Experiencia de escritorio  | Server Core para host contenedor, imagen de contenedor y imagen de contenedor de Nano Server |

## <a name="application-platform-and-containers"></a>Contenedores y plataforma de aplicaciones

- Optimization
    - La imagen de contenedor base de Server Core se reduce en un 30 % con respecto a Windows Server, versión 1709. 
    - La compatibilidad de aplicaciones también se ha mejorado para ayudar con la introducción en contenedores de las aplicaciones tradicionales.
    - El rendimiento de inicio del contenedor y el rendimiento de tiempo de ejecución se han mejorado también gracias a varias revisiones y optimizaciones.
- Redes de contenedor: Se ha agregado compatibilidad con el proxy http y el host local y se ha mejorado el tiempo de inicio y la escalabilidad del contenedor.
- Herramientas: Se ha mejorado la compatibilidad con Curl.exe, Tar.exe y SSH a fin de complementar PowerShell para generar y depurar los escenarios.

### <a name="server-core-container-image"></a>Imagen de contenedor de Server Core

Ahora está disponible un contenedor más pequeño de Server Core con mejor compatibilidad con las aplicaciones. Información detallada [aquí](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/) disponible.

- Se han quitado los roles y las características opcionales no usados. Para obtener más información, consulta [Roles, servicios de roles y características no incluidas en los contenedores de Server Core](../administration/server-core/server-core-container-removed-roles.md).
    - Tamaño de descarga reducida a 1,58 GB, 30 % de reducción con respecto a Windows Server, versión 1709.
    - Reducción de tamaño reducido en disco a 3,61 GB, reducción del 20 % con respecto a Windows Server, versión 1709.
- Imagen de contenedor Nano Server por debajo de 100 MB

### <a name="windows-subsystem-for-linux-wsl"></a>Subsistema de Windows para Linux (WSL)

WSL permite a los administradores de servidores usar las herramientas existentes y los scripts de Linux en Windows Server. Muchas mejoras expuestas en el [blog de línea de comandos](https://blogs.msdn.microsoft.com/commandline/tag/wsl/) ahora forman parte de Windows Server, incluidas Tareas en segundo plano, DriveFS, WSLPath y mucho más.

### <a name="kubernetes"></a>Kubernetes 

Kubernetes (normalmente denominado K8s) es un sistema de código abierto para automatizar la implementación, el escalado y la administración de aplicaciones incluidas en contenedor desarrollados bajo la protección de la [Cloud Native Computing Foundation](https://www.cncf.io). 

En Windows Server, los usuarios de la versión 1709 podían aprovechar Kubernetes en características de redes de Windows, que incluye:
- Compartido compartimientos pod: Los pods de infraestructura y de trabajo comparten ahora un compartimento de red (de manera similar a un espacio de nombres de Linux).
- Optimización de punto de conexión: Gracias al uso compartido de compartimiento, servicios de contenedor deben realizar un seguimiento de al menos la mitad tantos puntos de conexión.
- Optimización de la ruta de acceso de datos: Mejoras en la plataforma de filtrado Virtual y el servicio de red de Host que basada en kernel-equilibrio de carga.

Con el lanzamiento de Windows Server, versión 1803, estará disponibles más características en las próximas versiones de kubernetes: 
- [Complementos de almacenamiento](https://github.com/Microsoft/K8s-Storage-Plugins) para los contenedores de Windows organizados por Kubernetes.
- Redes de escala de nube a través de iniciativas como nuestra asociación con soporte [Tigera on Project Calico](https://cloudblogs.microsoft.com/windowsserver/2017/12/07/securing-modernized-apps-and-simplified-networking-on-windows-with-calico/).
- Soporte de plataforma de Windows para pods aislados de Hyper-V con varios contenedores por Pod.

### <a name="application-compatibility-and-feature-parity-issues-fixed"></a>Problemas de paridad de características y compatibilidad de aplicaciones solucionados

- Microsoft Message Queuing (MSMQ) ahora se instala en un contenedor de Server Core.
- Se ha corregido un problema que interrumpe los contadores de rendimiento de ASP.net.
- Se ha corregido un problema donde los servicios que se ejecutan en los contenedores no recibían notificación de cierre.
    - En concreto, la notificación se ha cambiado a CTRL_SHUTDOWN_EVENT para las imágenes basadas en contenedor Server Core y Nano Server. Además, extiende la notificación en imágenes basadas en contenedor de Server Core para que afecten a todos los procesos que se ejecuten en el contenedor, incluido el envío de notificaciones de cierre de servicio a los servicios que se ejecuten en el contenedor.
- Se ha corregido una incompatibilidad de docker pull y docker load con la configuración de directiva que determina si la protección de BitLocker es necesaria para que las unidades de datos fijas sean escribibles (FDVDenyWriteAccess). 

## <a name="storage"></a>Almacenamiento

Con esta versión, es posible evitar que el servicio del Administrador de recursos del servidor de archivos cree un diario de cambios (también conocido como diario USN) en todos los volúmenes cuando se inicia el servicio. Esto puede ahorrar espacio en cada volumen, pero deshabilitará la clasificación de archivos en tiempo real. Para obtener más información, consulta [Información general sobre el Administrador de recursos del servidor de archivos](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview).

## <a name="features-added-to-server-core"></a>Características agregadas a Server Core

El rol de servidor de transporte en el rol de Servicios de implementación de Windows (WDS) se ha añadido a Server Core.

Servidor de transporte solo contiene los principales elementos de red de WDS. Puedes usar el Servidor de transporte para crear espacios de nombres de multidifusión que transmitan datos (incluidas las imágenes de sistema operativo) desde un servidor independiente. También puedes utilizarlo si deseas disponer de un servidor PXE que permita a los clientes arrancar basándose en PXE y descargar su propia aplicación personalizada de configuración. Debes usar esta opción si deseas usar cualquiera de estos escenarios.

Puedes usar el siguiente comando de Windows PowerShell para habilitar el servicio de Servidor de transporte en Server Core:

```
Install-WindowsFeature -Name WDS
```

## <a name="see-also"></a>Vea también

[Información de versión de Windows Server](https://docs.microsoft.com/windows-server/get-started/windows-server-release-info)<br>
[Novedades de Windows 10, el contenido para profesionales de TI de versión 1803](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1803)
