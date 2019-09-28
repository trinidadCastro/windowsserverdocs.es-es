---
title: Configuraciones admitidas para Servicios de Escritorio remoto
description: Proporciona información sobre las configuraciones admitidas para RDS en Windows Server 2016.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 7363fe3eee33a5345a25c8df9e4216b9eda7e3b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403806"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Configuraciones admitidas para los Servicios de Escritorio remoto en Windows Server 2016

> Se aplica a: Windows Server 2016

Cuando se trata de las configuraciones admitidas para entornos de Servicios de Escritorio remoto, la mayor preocupación tiende suele ser la interoperabilidad de las versiones. La mayoría de los entornos incluyen múltiples versiones de Windows Server; por ejemplo, puedes tener una implementación existente de Windows Server 2012 R2 RDS pero quieres actualizar a Windows Server 2016 para aprovechar las nuevas características (como soporte para OpenGL\OpenCL, asignación de dispositivos discretos o espacios de almacenamiento directo). La pregunta entonces es, ¿qué componentes de RDS pueden funcionar con diferentes versiones y cuáles deben ser los mismos?

Con esto en mente, estas son las directrices básicas para las configuraciones compatibles de Servicios de Escritorio remoto en Windows Server 2016.

> [!NOTE]
> No te olvides revisar los [requisitos del sistema para Windows Server 2016](../../get-started/system-requirements.md).

## <a name="best-practices"></a>Procedimiento recomendado
- Usa Windows Server 2016 para tu infraestructura de Escritorio remoto: acceso web, puerta de enlace, agente de conexión y servidor de licencias. Windows Server 2016 es compatible con versiones anteriores para estos componentes, por lo que un host de sesión de Escritorio remoto 2012 R2 puede conectarse a un Agente de conexión a Escritorio remoto 2016, pero lo contrario no se aplica.

- Para los hosts de sesión de Escritorio remoto: todos los hosts de sesión de una colección deben estar al mismo nivel, pero puedes tener varias colecciones. Puedes tener una colección con hosts de sesión de Windows Server 2012 R2 y otro con hosts de sesión de Windows Server 2016.

- Si actualizas el host de sesión de Escritorio remoto a Windows Server 2016, actualiza también el servidor de licencias. Recuerda que un servidor de licencias de la versión 2016 puede procesar las CAL de todas las versiones anteriores de Windows Server, hasta Windows Server 2003.

- Sigue el orden de actualización recomendado en [Actualización del entorno de Servicios de Escritorio remoto](upgrade-to-rds.md#flow-for-deployment-upgrades). 

- Si estás creando un entorno de alta disponibilidad, todos tus agentes de conexión deben tener el mismo nivel de sistema operativo.

## <a name="rd-connection-brokers"></a>Agentes de conexión a Escritorio remoto

Windows Server 2016 elimina la restricción del número de agentes de conexión que puedes tener en una implementación al usar hosts de sesión de Escritorio remoto (RDSH) y hosts de virtualización de Escritorio remoto (RDVH) que también ejecutan Windows Server 2016. En la tabla siguiente se muestra qué versiones de los componentes de RDS funcionan con las versiones 2016 y 2012 R2 del agente de conexión en una implementación altamente disponible con tres o más agentes de conexión.

| Más de tres agentes de conexión de alta disponibilidad              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Agente de conexión de Windows Server 2016    | Se admite | Se admite | Se admite     | Se admite     |
| Agente de conexión de Windows Server 2012 R2 | N/D       | N/D       | Se admite     | Se admite     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Compatibilidad con la aceleración de GPU con Hyper-V
En la tabla siguiente se detalla la compatibilidad con la aceleración de GPU en máquinas virtuales. Consulta [Which graphics virtualization technology is right for you?](rds-graphics-virtualization.md) (¿Qué tecnología de virtualización de gráficos es adecuada para usted?) para obtener ayuda para determinar lo que necesitas. Para más información sobre DDA, consulta [Planear la implementación de la asignación de dispositivos discreta](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

|Sistema operativo invitado de máquina virtual  |Windows Server 2012 R2 o Windows Server 2016<br> VGPU de Hyper-V RemoteFX (máquina virtual de primera generación) |  vGPU Hyper-V RemoteFX de Windows Server 2016 (máquina virtual de segunda generación) |  Asignación discreta de dispositivos Hyper-V de Windows Server 2016 (máquinas virtuales de segunda generación) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | Sí                                                        | No                                                     | No                                                                  |
| Windows 8.1                 | Sí                                                        | No                                                     | No                                                                  |
| Actualización 1511 de Windows 10      | Sí                                                        | Sí                                                    | Sí                                                                 |
| Windows Server 2012 R2      | Sí                                                        | No                                                     | Sí (requiere KB 3133690)                                           |
| Windows Server 2016         | Sí                                                        | Sí                                                    | Sí                                                                 |
| Windows Server 2012 R2 RDSH | No                                                         | No                                                     | Sí (requiere KB 3133690)                                           |
| Windows Server 2016 RDSH    | No                                                         | No                                                     | Sí                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>Implementación de VDI: sistemas operativos invitados compatibles 
Los servidores host de virtualización de Escritorio remoto de Windows Server 2016 son compatibles con los siguientes sistemas operativos invitados:

- Windows 10 Enterprise
- Windows 8.1 Enterprise 
- Windows 8 Enterprise 
- Windows 7 SP1 Enterprise 

En la tabla siguiente se muestran las combinaciones de sistemas operativos invitados y sistemas operativos de hosts de virtualización de Escritorio remoto:

| Versión del sistema operativo RDVH        | Versión del sistema operativo invitado           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012      | Windows 7 SP1, Windows 8, Windows 8.1 |

> [!NOTE]  
> - Servicios de Escritorio remoto de Windows Server 2016 no admite colecciones heterogéneas. Todas las máquinas virtuales de una colección deben tener la misma versión del sistema operativo. 
> - Puedes tener colecciones homogéneas independientes con versiones de sistemas operativos invitados diferentes en el mismo host. 
> - Las plantillas de máquina virtual deben crearse en un host de Hyper-V de Windows Server 2016 que se van a usar como un sistema operativo invitado en un host de Hyper-V de Windows Server 2016.

## <a name="single-sign-on-sso"></a>Inicio de sesión único (SSO)
RDS de Windows Server 2016 admite dos experiencias de inicio de sesión único principales:

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

Puedes utilizar los Servicios de Escritorio remoto, excepto el cliente web, con [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop). Servicios de Escritorio remoto no admite el uso de [Proxy de aplicación web](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server), que se incluye en Windows Server 2016 y versiones anteriores.
