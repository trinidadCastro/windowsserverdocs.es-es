---
title: Configuraciones admitidas para Servicios de Escritorio remoto
description: Proporciona información sobre las configuraciones admitidas para RDS en Windows Server 2016 y Windows Server 2019.
ms.author: elizapo
ms.date: 07/14/2020
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 47aa9327e70d07ce46477024fb0c734ea1d64603
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954842"
---
# <a name="supported-configurations-for-remote-desktop-services"></a>Configuraciones admitidas para Servicios de Escritorio remoto

> Se aplica a: Windows Server 2016 y Windows Server 2019

Cuando se trata de las configuraciones admitidas para entornos de Servicios de Escritorio remoto, la mayor preocupación tiende suele ser la interoperabilidad de las versiones. La mayoría de los entornos incluyen múltiples versiones de Windows Server; por ejemplo, puedes tener una implementación existente de Windows Server 2012 R2 RDS pero quieres actualizar a Windows Server 2016 para aprovechar las nuevas características (como soporte para OpenGL\OpenCL, asignación de dispositivos discretos o espacios de almacenamiento directo). La pregunta entonces es, ¿qué componentes de RDS pueden funcionar con diferentes versiones y cuáles deben ser los mismos?

Con esto en mente, estas son las directrices básicas para las configuraciones compatibles de los Servicios de Escritorio remoto en Windows Server.

> [!NOTE]
> Asegúrate de revisar los [requisitos del sistema para Windows Server 2016](../../get-started/system-requirements.md) y los [requisitos del sistema para Windows Server 2019](../../get-started-19/sys-reqs-19.md).

## <a name="best-practices"></a>Procedimientos recomendados

- Usa Windows Server 2019 para tu infraestructura de Escritorio remoto (acceso web, puerta de enlace, agente de conexión y servidor de licencias). Windows Server 2019 es compatible con versiones anteriores de estos componentes, lo que significa que un host de sesión de Escritorio remoto de Windows Server 2016 o Windows Server 2012 R2 puede conectarse a un agente de conexión de Escritorio remoto de 2019, pero no al revés.

- Para los hosts de sesión de Escritorio remoto: todos los hosts de sesión de una colección deben estar al mismo nivel, pero puedes tener varias colecciones. Puedes tener una colección con hosts de sesión de Windows Server 2016 y otra con hosts de sesión de Windows Server 2019.

- Si actualizas el host de sesión de Escritorio remoto a Windows Server 2019, actualiza también el servidor de licencias. Recuerda que un servidor de licencias de la versión 2019 puede procesar las CAL de todas las versiones anteriores de Windows Server, hasta Windows Server 2003.

- Sigue el orden de actualización recomendado en [Actualización del entorno de Servicios de Escritorio remoto](upgrade-to-rds.md#flow-for-deployment-upgrades).

- Si estás creando un entorno de alta disponibilidad, todos tus agentes de conexión deben tener el mismo nivel de sistema operativo.

## <a name="rd-connection-brokers"></a>Agentes de conexión a Escritorio remoto

Windows Server 2016 elimina la restricción del número de agentes de conexión que puedes tener en una implementación al usar hosts de sesión de Escritorio remoto (RDSH) y hosts de virtualización de Escritorio remoto (RDVH) que también ejecutan Windows Server 2016. En la tabla siguiente se muestra qué versiones de los componentes de RDS funcionan con las versiones 2016 y 2012 R2 del agente de conexión en una implementación altamente disponible con tres o más agentes de conexión.

|Más de tres agentes de conexión de alta disponibilidad|RDSH o RDVH 2019|RDSH o RDVH 2016|RDSH o RDVH 2012 R2|
|---|---|---|---|
 |Agente de conexión de Windows Server 2019|Compatible|Compatible|Compatible|
 |Agente de conexión de Windows Server 2016|N/A|Compatible|Compatible|
 |Agente de conexión de Windows Server 2012 R2|N/A|N/A|No admitido|

## <a name="support-for-graphics-processing-unit-gpu-acceleration"></a>Compatibilidad con la aceleración de la unidad de procesamiento de gráficos (GPU)

Los Servicios de Escritorio remoto admiten sistemas equipados con GPU. Las aplicaciones que requieren una GPU se pueden usar a través de la conexión remota. Además, se puede habilitar la representación y la codificación acelerados por GPU para mejorar la escalabilidad y el rendimiento de las aplicaciones.

Los host de sesión de los Servicios de Escritorio remoto y los sistemas operativos de cliente de sesión única pueden aprovechar las ventajas de una GPU física o virtual que se presenta en el sistema operativo de muchas maneras, entre las que se incluyen los [tamaños de máquina virtual optimizada para GPU de Azure](/en-us/azure/virtual-machines/windows/sizes-gpu), las GPU disponibles para el servidor de RDSH físico y las GPU que se presentan en las VM mediante hipervisores admitidos.

Consulta [Which graphics virtualization technology is right for you?](rds-graphics-virtualization.md) (¿Qué tecnología de virtualización de gráficos es adecuada para usted?) para obtener ayuda para determinar lo que necesitas. Para más información sobre DDA, consulta [Planear la implementación de la asignación de dispositivos discreta](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

Los proveedores de GPU pueden tener un esquema de licencias independiente para escenarios de RDSH o restringir el uso de GPU en el sistema operativo del servidor, comprueba los requisitos con tu proveedor favorito.

Las GPU que se presentan mediante hipervisores o plataformas en la nube que no son de Microsoft deben tener controladores que haya firmados digitalmente WHQL y que haya proporcionado el proveedor de GPU.

### <a name="remote-desktop-session-host-support-for-gpus"></a>Compatibilidad del host de sesión de Escritorio remoto con las GPU

En la tabla siguiente se muestran los escenarios compatibles con diferentes versiones de hosts de RDSH.

|Característica|Windows Server 2008 R2|Windows Server 2012 R2|Windows Server 2016|Windows Server 2019|
|---|---|---|---|---|
|Uso de GPU de hardware para todas las sesiones de RDP|No|Sí|Sí|Sí|
|Codificación de hardware H.264/AVC (si es compatible con la GPU)|No|No|Sí|Sí|
|Equilibrio de carga entre varias GPU presentadas al sistema operativo|No|No|No|Sí|
|Optimizaciones de codificación H.264/AVC para minimizar el uso de ancho de banda|No|No|No|Sí|
|Compatibilidad con H.264/AVC para la resolución de 4 K|No|No|No|Sí|

### <a name="vdi-support-for-gpus"></a>Compatibilidad de VDI con las GPU

En la tabla siguiente se muestra la compatibilidad con escenarios de GPU en el sistema operativo del cliente.

|Característica|Windows 7 SP1|Windows 8.1|Windows 10|
|---|---|---|---|
|Uso de GPU de hardware para todas las sesiones de RDP|No|Sí|Sí|
|Codificación de hardware H.264/AVC (si es compatible con la GPU)|No|No|Windows 10 1703 y versiones posteriores|
|Equilibrio de carga entre varias GPU presentadas en el sistema operativo|No|No|Windows 10 1803 y versiones posteriores|
|Optimizaciones de codificación H.264/AVC para minimizar el uso de ancho de banda|No|No|Windows 10 1803 y versiones posteriores|
|Compatibilidad con H.264/AVC para la resolución de 4 K|No|No|Windows 10 1803 y versiones posteriores|

### <a name="remotefx-3d-video-adapter-vgpu-support"></a>Compatibilidad del adaptador de vídeo 3D RemoteFX (vGPU)

> [!NOTE]
> Debido a los problemas de seguridad, vGPU de RemoteFX está deshabilitado de manera predeterminada en todas las versiones de Windows a partir de la actualización de seguridad del 14 de julio de 2020. Para obtener más información, consulte [KB 4570006](https://support.microsoft.com/help/4570006).

Los Servicios de Escritorio remoto admiten vGPU de RemoteFX cuando la máquina virtual se ejecuta como invitado de Hyper-V en Windows Server 2012 R2 o Windows Server 2016. Los siguientes sistemas operativos invitados son compatibles con la vGPU de RemoteFX:

- Windows 7 SP1
- Windows 8.1
- Windows 10 1703 o posterior
- Windows Server 2016 solo en una implementación de sesión única

### <a name="discrete-device-assignment-support"></a>Compatibilidad de la asignación discreta de dispositivos

Los Servicios de Escritorio remoto admiten las GPU físicas que se presentan con la asignación discreta de dispositivos desde hosts de Hyper-V de Windows Server 2016 o Windows Server 2019. Consulta [Planeación de la implementación de dispositivos mediante la asignación discreta de dispositivos](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md) para obtener más información.


## <a name="vdi-deployment--supported-guest-oses"></a>Implementación de VDI: sistemas operativos invitados compatibles

Los servidores host de virtualización de Escritorio remoto de Windows Server 2016 y Windows Server 2019 son compatibles con los siguientes sistemas operativos invitados:

- Windows 10 Enterprise
- Windows 8.1 Enterprise
- Windows 7 SP1 Enterprise

> [!NOTE]
> - Los Servicios de Escritorio remoto no admiten colecciones de sesiones heterogéneas. Los sistemas operativos de todas las máquinas virtuales de una colección deben tener la misma versión.
> - Puedes tener colecciones homogéneas independientes con versiones de sistemas operativos invitados diferentes en el mismo host.
> - El host de Hyper-V que se usa para ejecutar las máquinas virtuales debe ser de la misma versión que el host de Hyper-V que se usa para crear las plantillas de máquina virtual originales.

## <a name="single-sign-on"></a>Inicio de sesión único

RDS de Windows Server 2016 y Windows Server 2019 admiten dos experiencias de inicio de sesión único principales:

- En la aplicación (aplicación de Escritorio remoto en Windows, iOS, Android y Mac)
- Web SSO

Mediante la aplicación de Escritorio remoto, puede almacenar las credenciales como parte de la información de conexión ([Mac](clients/remote-desktop-mac.md)) o como parte de cuentas administradas ([iOS](clients/remote-desktop-ios.md#manage-your-user-accounts), [Android](clients/remote-desktop-android.md#manage-your-user-accounts), Windows) de forma segura mediante los mecanismos exclusivos de cada sistema operativo.

Para conectarse a escritorios y RemoteApps con el inicio de sesión único incluido del cliente de Conexión a Escritorio remoto en Windows, debes conectarte a la página de acceso web de Escritorio Remoto a través de Internet Explorer. Las siguientes opciones de configuración son necesarias en el lado servidor. No se admiten otras configuraciones para el inicio de sesión único de web:

- Acceso web de Escritorio remoto establecida en la autenticación basada en formularios (valor predeterminado)
- Puerta de enlace de Escritorio remoto establecida en autenticación de contraseña (valor predeterminado)
- La implementación de RDS establecida en "Usar las credenciales de Puerta de enlace de Escritorio remoto para los equipos remotos" (valor predeterminado) en las propiedades de la puerta de enlace de Escritorio remoto

> [!NOTE]
> Debido a las opciones de configuración necesarias, el inicio de sesión único de web no es compatible con tarjetas inteligentes. Los usuarios que se conectan a través de tarjetas inteligentes pueden enfrentarse a múltiples avisos de inicio de sesión.

Para más información sobre la creación de la implementación de VDI de Servicios de Escritorio remoto, consulta [Configuraciones de seguridad admitidas de Windows 10 para VDI de Servicios de Escritorio remoto](rds-vdi-supported-config.md).

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>Uso de Servicios de Escritorio remoto con los servicios de proxy de la aplicación

Puede utilizar los Servicios de Escritorio remoto con [Azure AD Application Proxy](/azure/active-directory/application-proxy-publish-remote-desktop). Servicios de Escritorio remoto no admite el uso de [Proxy de aplicación web](../remote-access/web-application-proxy/web-application-proxy-windows-server.md), que se incluye en Windows Server 2016 y versiones anteriores.
