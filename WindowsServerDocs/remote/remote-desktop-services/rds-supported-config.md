---
title: Configuraciones admitidas para servicios de escritorio remoto
description: Proporciona información sobre las configuraciones admitidas para RDS en Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 894ea8b134ae5b871a2978e3f72e683c12346fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850206"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Configuraciones admitidas para los servicios de escritorio remoto en Windows Server 2016

> Se aplica a: Windows Server 2016

Cuando se trata de las configuraciones admitidas para entornos de servicios de escritorio remoto, la mayor preocupación tiende a ser interoperabilidad de versión. Mayoría de los entornos incluye varias versiones de Windows Server: por ejemplo, es posible que tiene una implementación existente de Windows Server 2012 R2 RDS pero desea actualizar a Windows Server 2016 para aprovechar las ventajas de las nuevas características (por ejemplo, la compatibilidad con OpenGL\OpenCL, discreto Asignación del dispositivo, o espacios de almacenamiento directo). ¿La cuestión entonces es, qué componentes RDS pueden trabajar con distintas versiones y que deba ser la misma?

Por lo que con eso en mente, estas son directrices básicas para las configuraciones compatibles de los servicios de escritorio remoto en Windows Server 2016.

> [!NOTE]
> No olvide revisar la [requisitos del sistema para Windows Server 2016](../../get-started/system-requirements.md).

## <a name="best-practices"></a>Procedimiento recomendado
- Usar Windows Server 2016 para su infraestructura de escritorio remoto, el acceso Web, puerta de enlace, agente de conexión y servidor de licencias. Windows Server 2016 es compatible con versiones anteriores de estos componentes: por lo que un Host de sesión de escritorio remoto de 2012 R2 puede conectarse a un agente de conexión a Escritorio remoto de 2016, pero de lo contrario no es cierto.

- Para Hosts de sesión de escritorio remoto - todos los Hosts de sesión en una colección deben ser del mismo nivel, pero puede tener varias colecciones. Puede tener una colección con Hosts de sesión de Windows Server 2012 R2 y otro con Hosts de sesión de Windows Server 2016.

- Si actualiza al Host de sesión de escritorio remoto a Windows Server 2016, actualice también el servidor de licencias. Recuerde que un servidor de licencias de 2016 puede procesar las CAL de todas las versiones anteriores de Windows Server, hasta Windows Server 2003.

- Seguir el orden de actualización recomendado en [actualizar su entorno de servicios de escritorio remoto](upgrade-to-rds.md#flow-for-deployment-upgrades). 

- Si va a crear un entorno de alta disponibilidad, todos los agentes de conexión deben ser del mismo nivel del sistema operativo.

## <a name="rd-connection-brokers"></a>Agentes de conexión a Escritorio remoto

Windows Server 2016 se quita la restricción del número de agentes de conexión puede tener en una implementación cuando se usan los Hosts de sesión de escritorio remoto (RDSH) y remoto Hosts de virtualización de escritorio (RDVH) que también se ejecuta Windows Server 2016. La siguiente tabla muestra qué versiones de trabajo de componentes RDS con las versiones R2 de 2016 y 2012 del agente de conexión en una implementación de alta disponibilidad con tres o más agentes de conexión.

| 3 + agentes de conexión de alta disponibilidad              | 2016 RDSH | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Agente de conexión de Windows Server 2016    | Se admite | Se admite | Se admite     | Se admite     |
| Agente de conexión de Windows Server 2012 R2 | N/D       | N/D       | Se admite     | Se admite     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Soporte técnico para la aceleración de GPU con Hyper-V
La tabla siguiente detalla la compatibilidad para la aceleración de GPU en máquinas virtuales. Consulte [qué tecnología de virtualización de gráficos es adecuada para usted?](rds-graphics-virtualization.md) para averiguar lo que necesita obtener ayuda. Para obtener información específica sobre DDA, visite [planear la implementación de la asignación discreta de dispositivos](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

|Sistema operativo invitado de máquina virtual  |Windows Server 2012 R2 o Windows Server 2016<br> VGPU de RemoteFX de Hyper-V (VM de generación 1) |  Windows Server 2016 Hyper-V RemoteFX vGPU (máquina virtual Gen 2) |  Asignación discreta de dispositivos de Windows Server 2016 Hyper-V (gen. 2 máquinas virtuales) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | Sí                                                        | No                                                     | No                                                                  |
| Windows 8.1                 | Sí                                                        | No                                                     | No                                                                  |
| Windows 10 1511 Update      | Sí                                                        | Sí                                                    | Sí                                                                 |
| Windows Server 2012 R2      | Sí                                                        | No                                                     | Sí (requiere 3133690 KB)                                           |
| Windows Server 2016         | Sí                                                        | Sí                                                    | Sí                                                                 |
| Windows Server 2012 R2 RDSH | No                                                         | No                                                     | Sí (requiere 3133690 KB)                                           |
| Windows Server 2016 RDSH    | No                                                         | No                                                     | Sí                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>Implementación de VDI, sistemas operativos invitados compatibles 
Servidores Host de virtualización de escritorio remoto de Windows Server 2016 admiten los siguientes sistemas operativos:

- Windows 10 Enterprise
- Windows 8.1 Enterprise 
- Windows 8 Enterprise 
- Windows 7 SP1 Enterprise 

La siguiente tabla muestra los sistemas operativos de los Hosts de virtualización de escritorio remoto y combinaciones de sistemas operativos invitados:

| Versión del sistema operativo RDVH        | Versión del SO invitado           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012      | Windows 7 SP1, Windows 8, Windows 8.1 |

> [!NOTE]  
> - Servicios de escritorio remoto de Windows Server 2016 no admite colecciones heterogéneas. Todas las máquinas virtuales en una colección deben ser la misma versión del SO. 
> - Puede tener colecciones homogéneas independientes con versiones del SO invitado diferentes en el mismo host. 
> - Plantillas de máquina virtual deben crearse en un host de Windows Server 2016 Hyper-V que va a usar como sistema operativo invitado en un host de Windows Server 2016 Hyper-V.

## <a name="single-sign-on-sso"></a>Inicio de sesión único (SSO)
Windows Server 2016 RDS admite dos experiencias de inicio de sesión único principales:

 - En la aplicación (aplicación de escritorio remoto en Windows, iOS, Android y Mac)
 - Web SSO
 
Con la aplicación de escritorio remoto, puede almacenar las credenciales como parte de la información de conexión ([Mac](clients\remote-desktop-mac.md)) o como parte de las cuentas administradas ([iOS](clients\remote-desktop-ios.md#manage-your-user-accounts), [Android](clients\remote-desktop-android.md#manage-your-user-accounts), Windows) forma segura a través de mecanismos únicos para cada sistema operativo.

Para conectarse a escritorios y RemoteApps con inicio de sesión único a través del cliente de conexión a Escritorio remoto de la Bandeja de entrada en Windows, debe conectarse a la página Web de escritorio remoto a través de Internet Explorer. Las siguientes opciones de configuración son necesarios en el lado del servidor. No hay otras configuraciones son compatibles con SSO Web:

 - Web de escritorio remoto se establece en autenticación basada en formularios (valor predeterminado)
 - Puerta de enlace de escritorio remoto se establece en autenticación de contraseña (valor predeterminado)
 - Implementación de RDS se establece en "Credenciales de puerta de enlace de escritorio remoto de uso para los equipos remotos" (predeterminado) en las propiedades de la puerta de enlace de escritorio remoto

> [!NOTE]
> Debido a las opciones de configuración necesarias, SSO Web no es compatible con tarjetas inteligentes. Usuarios iniciar sesión mediante tarjetas inteligentes puede enfrentarse varias indicaciones para iniciar sesión.

Para obtener más información sobre la creación de la implementación de VDI de servicios de escritorio remoto, consulte [admiten Windows 10 configuraciones de seguridad para servicios de escritorio remoto VDI](rds-vdi-supported-config.md).

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>Uso de servicios de escritorio remoto con los servicios de proxy de aplicación

Puede usar servicios de escritorio remoto, excepto el cliente web, con [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop). Servicios de escritorio remoto no admite el uso [Proxy de aplicación Web](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server), que se incluye en Windows Server 2016 y versiones anteriores.
