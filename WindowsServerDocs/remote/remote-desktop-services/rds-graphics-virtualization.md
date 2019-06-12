---
title: ¿RDS - qué tecnología de virtualización de gráficos es adecuada para usted?
description: Información de planeación para ayudarle a elegir la opción de virtualización para la implementación de RDS de los gráficos adecuados.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/16/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: af5d5ce89561c89d8468627e20dfdb6f35eca5ef
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447113"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>¿La tecnología de virtualización de gráficos es adecuada para usted?

Tiene una gama de opciones en cuanto a habilitar la representación de gráficos en servicios de escritorio remoto. Cuando planee el entorno de virtualización, las siguientes consideraciones de unidad que la tecnología elegida la representación de gráficos:

![Consideraciones sobre la representación de gráficos - compara usuario escalado y los requisitos de GPU para determinar la mejor tecnología GPU para su entorno](media/rds-gpu.png)

En Windows Server 2016, tienen dos tecnologías de virtualización de gráficos disponibles con Hyper-V que le permite aprovechar el hardware GPU:

- [Asignación de dispositivo discretos (DDA)](#discrete-device-assignment) : para el máximo rendimiento de uso de uno o más GPU dedicada a una máquina virtual que proporciona compatibilidad nativa del controlador GPU dentro de la máquina virtual. La densidad es baja, ya que está limitado por el número de GPU físicas disponibles en el servidor. 
- [Remoto vGPU FX](#remotefx-vgpu) : como trabajador del conocimiento y escenarios de ráfaga de alta GPU donde varias máquinas virtuales aprovechan uno o más GPU a través de paravirtualización. Esta solución proporciona una mayor densidad de usuario por servidor.

La ilustración siguiente muestra los gráficos las opciones de virtualización en Windows Server 2016.

![Las opciones de virtualización de gráficos en Windows Server 2016 con RDS: muestra las tres tecnologías disponibles y cómo se diferencian en escala y rendimiento](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>Asignación de dispositivos discreta
Asignación de dispositivo discretos (DDA) es una solución de acceso directo de hardware que proporciona el mejor rendimiento, dado que la máquina virtual tiene acceso completo a la GPU con el controlador nativo. El usuario de la máquina virtual puede tener acceso a todas las capacidades de su dispositivo como bien controlador nativo del dispositivo. Esto significa que las características y capacidades de dispositivo en ejecución en un reflejo de la máquina virtual con el mismo dispositivo sin sistema operativo.

Para obtener más información acerca de DDA, visite [planear la implementación de la asignación discreta de dispositivos](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
VGPU de RemoteFX es una tecnología de virtualización de gráficos que permite la capacidad de procesamiento de una GPU va a dividir entre varios sistemas operativos de invitado para habilitar escenarios de trabajador del conocimiento (vea el primer gráfico anterior). Los avances en Windows Server 2016 permiten mejoras adicionales para escenarios de ráfaga GPU, por ejemplo, para las aplicaciones y datos de la visualización de diseñador. Otras mejoras de características se incluyen:

- Compatibilidad con invitados de generación 2 máquinas virtuales, Invitado VM de Windows Server 2016 y host de Hyper-V de Windows Client.
  >[!NOTE] 
  > Host de sesión de escritorio remoto no se admite en un máquina virtual; de invitado de Windows Server 2016 1 solo sesión se puede hospedar por máquina virtual invitada de Windows Server 2016.

- Compatibilidad de aplicaciones mejorada y la estabilidad.
- Máquina virtual conectarse modo de sesión mejorada, lo que permite la redirección de USB y Portapapeles a través de máquina virtual se conecte a una máquina virtual que está habilitada para RemoteFX vGPU.

Para obtener más información, consulte [establecer instalación y configuración vGPU de RemoteFX para servicios de escritorio remoto](rds-remotefx-vgpu.md).

## <a name="which-should-you-use"></a>¿Cuál debe usar?

Consideraciones clave en que la virtualización tecnología podría dependen de las especificaciones de hardware o requisitos de la aplicación en su entorno. Esta es una tabla breve con respecto a las capacidades de vGPU DDA y RemoteFX:

| Característica               | RemoteFX vGPU                                                                       | Asignación de dispositivos discreta                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Asignación de dispositivo de GPU | Paravirtualizada (muchas máquinas virtuales para GPU de uno o más)                                     | 1 o más GPU para 1 máquina virtual                                                  |
| Escala                 | Mejorar la escalabilidad / 1 GPU para muchas máquinas virtuales                                                      | Escala de baja / 1 o más GPU para 1 máquina virtual                                     |
| Compatibilidad de aplicaciones     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Todas las funcionalidades GPU proporcionadas por el proveedor (12 de DX, OpenGL, CUDA)          |
| AVC444                | Habilitado de forma predeterminada (Windows 10 y Windows Server 2016)                             | Disponible a través de la directiva de grupo (Windows 10 y Windows Server 2016)    |
| VRAM DE GPU              | Hasta 1 GB dedicado VRAM                                                           | Hasta VRAM compatible con la GPU                                        |
| Velocidad de fotogramas            | Hasta 30fps                                                                         | Hasta 60fps                                                            |
| Controlador de GPU en invitado   | Controlador de pantalla de RemoteFX 3D de adaptador (Microsoft)                                      | Controlador del proveedor de GPU (Nvidia, AMD, Intel)                                 |
| Compatibilidad con el SO invitado      |  Windows Server 2012 R2 Windows Server 2016 Windows 7 SP1, Windows 8.1, Windows 10 |  Windows Server 2012 R2  Windows Server 2016  Windows 10 Linux         |
| Hipervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| Disponibilidad del sistema operativo del host  |  Windows Server 2012 R2  Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| Hardware de GPU          | GPU de la empresa (como Nvidia Quadro/CUADRÍCULA o AMD FirePro)                         | GPU de la empresa (como Nvidia Quadro/CUADRÍCULA o AMD FirePro)            |
| Hardware de servidor       | Ningún requisito especial                                                             | Servidor moderno, expone IOMMU al sistema operativo (normalmente SR-IOV hardware compatible con) |

Una regla general es utilizar DDA la mejor compatibilidad de aplicaciones, ya que la máquina virtual tendrá acceso directo a la GPU. Si sus aplicaciones o cargas de trabajo no tiene como requisitos estrictos de GPU y desea al servidor una base de usuarios más amplia, RemoteFX vGPU podría funcionar mejor para usted.