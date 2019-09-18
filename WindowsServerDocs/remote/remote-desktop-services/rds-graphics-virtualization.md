---
title: 'RDS: ¿qué tecnología de virtualización de gráficos es adecuada para usted?'
description: Información que le ayuda a elegir la opción de virtualización de gráficos adecuada para la implementación de RDS.
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
ms.openlocfilehash: ce10575d38bccc0b22dadf55bd89156c6ce5ea7b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871049"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>¿Qué tecnología de virtualización de gráficos es adecuada para ti?

Para habilitar la representación de gráficos en Servicios de Escritorio remoto tienes a tu disposición varias opciones. Al planear un entorno de virtualización, las siguientes consideraciones son el motor a la hora de elegir la tecnología de representación de gráficos:

![Consideraciones de representación de gráficos (compara la escala del usuario y los requisitos de GPU para determinar la mejor tecnología de GPU para el entorno)](media/rds-gpu.png)

En Windows Server 2016, hay dos tecnologías de virtualización de gráficos disponibles con Hyper-V que permiten aprovechar el hardware GPU:

- [Asignación de dispositivos discreta (DDA)](#discrete-device-assignment): para lograr el máximo rendimiento mediante el uso de una o varias GPU dedicadas a una máquina virtual que proporciona compatibilidad nativa del controlador GPU dentro de la máquina virtual. La densidad es baja, ya que está limitado por el número de GPU físicas disponibles en el servidor. 
- [vGPU de RemoteFX](#remotefx-vgpu): en escenarios de trabajador del conocimiento y de GPU de ráfaga alta en los que varias máquinas virtuales aprovechan una o varias GPU a través de la paravirtualización. Esta solución proporciona mayor densidad de usuarios por servidor.

La ilustración siguiente muestra las opciones de virtualización de gráficos en Windows Server 2016.

![Opciones de virtualización de gráficos de Windows Server 2016 con RDS: muestra las tres tecnologías disponibles y cómo se diferencian en cuanto a escala y rendimiento](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>Asignación discreta de dispositivos
Asignación discreta de dispositivos (DDA) es una solución de paso a través de hardware que proporciona el mejor rendimiento, dado que la máquina virtual tiene acceso total a la GPU mediante el controlador nativo. El usuario de la VM puede acceder a todas las funcionalidades de su dispositivo, así como al controlador nativo del dispositivo. Esto significa las características y funcionalidades de la ejecución del dispositivo en una máquina virtual espejo que ejecuta el mismo dispositivo sin sistema operativo.

Para más información acerca de DDA, consulta [Planear la implementación de la asignación de dispositivos discreta](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
RemoteFX vGPU es una tecnología de virtualización de gráficos que permite que la potencia de procesamiento de una GPU se divida entre varios sistemas operativos de invitado para habilitar escenarios de trabajador del conocimiento (vea el primer gráfico anterior). Los avances en Windows Server 2016 permiten mejoras adicionales para escenarios de ráfaga GPU, por ejemplo, para programas de diseñador y visualización de datos. Otras mejoras de características incluyen:

- Compatibilidad con máquinas virtuales invitadas de generación 2, máquinas virtuales invitadas con Windows Server 2016 y host Hyper-V de Windows Client.
  >[!NOTE] 
  > Host de sesión de Escritorio remoto no se admite en un máquina virtual invitada con de Windows Server 2016; solo se puede hospedar una sesión por máquina virtual invitada de Windows Server 2016.

- Mejora de la compatibilidad y estabilidad de las aplicaciones.
- Modo de sesión mejorada de VM Connect, lo que permite el redireccionamiento de USB y Portapapeles a través de VM Connect se conecte a una máquina virtual que está habilitada para RemoteFX vGPU.

Para más información, consulte [Instalación y configuración de RemoteFX vGPU para Servicios de Escritorio remoto](rds-remotefx-vgpu.md).

## <a name="which-should-you-use"></a>¿Cuál deberías usar?

Las consideraciones clave acerca de la tecnología de virtualización pueden depender de las especificaciones de hardware o de los requisitos de las aplicaciones de su entorno. Esta es una pequeña tabla con las funcionalidades de DDA y vGPU de RemoteFX:

| Característica               | RemoteFX vGPU                                                                       | Asignación de dispositivos discreta                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Asignación de GPU de dispositivo | Paravirtualizada (muchas máquinas virtuales para una o varias GPU)                                     | 1 o varias GPU para 1 máquina virtual                                                  |
| Escala                 | Mejor escala/1 GPU para muchas máquinas virtuales                                                      | Escala baja/1 o más GPU para 1 máquina virtual                                     |
| Compatibilidad de aplicaciones     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Todas las funcionalidades de GPU proporcionadas por el proveedor (DX 12, OpenGL, CUDA)          |
| AVC444                | Habilitado de manera predeterminada (Windows 10 y Windows Server 2016)                             | Disponible a través de la directiva de grupo (Windows 10 y Windows Server 2016)    |
| VRAM de GPU              | Hasta 1 GB de VRAM dedicada                                                           | Hasta la VRAM que admita la GPU                                        |
| Velocidad de fotogramas            | Hasta 30 fps                                                                         | Hasta 60 fps                                                            |
| Controlador de GPU en invitado   | Controlador de pantalla de adaptador 3D de RemoteFX (Microsoft)                                      | Controlador de proveedor de GPU (Nvidia, AMD, Intel)                                 |
| Compatibilidad con sistema operativo de invitado      |  Windows Server 2012 R2  Windows Server 2016  Windows 7 SP1  Windows 8.1 Windows 10 |  Windows Server 2012 R2  Windows Server 2016  Windows 10 Linux         |
| Hipervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| Disponibilidad del sistema operativo del host  |  Windows Server 2012 R2  Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| Hardware de GPU          | GPU de la empresa (como Nvidia Quadro/GRID o AMD FirePro)                         | GPU de la empresa (como Nvidia Quadro/GRID o AMD FirePro)            |
| Hardware del servidor       | No hay requisitos especiales                                                             | Servidor moderno, expone IOMMU al sistema operativo (normalmente hardware compatible con SR-IOV) |

Como regla general se utiliza DDA cuando se desea que la compatibilidad de aplicaciones sea óptima, ya que la máquina virtual tendrá acceso directo a la GPU. Si tus programas o cargas de trabajo no tienen un requisitos de GPU tan estrictos y quieres servir a una base de usuarios más amplia, es posible que RemoteFX vGPU fuera idónea para ti.