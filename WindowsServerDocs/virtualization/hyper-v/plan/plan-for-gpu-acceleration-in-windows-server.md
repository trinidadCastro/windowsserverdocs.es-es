---
title: Plan de aceleración de GPU en Windows Server
description: Obtenga información sobre las diferentes tecnologías de Hyper-V para la aceleración de GPU, incluido DDA y vGPU de RemoteFX
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 07/14/2020
ms.openlocfilehash: afdb856fc84bcee634381f04054a97f545056882
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938798"
---
# <a name="plan-for-gpu-acceleration-in-windows-server"></a>Plan de aceleración de GPU en Windows Server

> Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

En este artículo se presentan las capacidades de virtualización de gráficos disponibles en Windows Server.

## <a name="when-to-use-gpu-acceleration"></a>Cuándo usar la aceleración de GPU

En función de la carga de trabajo, es posible que desee considerar la aceleración de la GPU. Esto es lo que debe tener en cuenta antes de elegir la aceleración de GPU:

- **Cargas de trabajo de aplicaciones y de escritorio remoto (VDI/DaaS)**: Si va a compilar una aplicación o un servicio de comunicación remota de escritorio con Windows Server, tenga en cuenta el catálogo de aplicaciones que espera que los usuarios ejecuten. Algunos tipos de aplicaciones, como las aplicaciones CAD/CAM, las aplicaciones de simulación, los juegos y las aplicaciones de representación y visualización, se basan en gran medida en la representación 3D para ofrecer interactividad fluida y receptiva. La mayoría de los clientes consideran las GPU una necesidad de una experiencia de usuario razonable con estos tipos de aplicaciones.
- **Cargas de trabajo de representación, codificación y visualización remotas**: estas cargas de trabajo orientadas a gráficos tienden a basarse en gran medida en las funcionalidades especializadas de una GPU, como la representación 3D eficaz y la codificación/descodificación de fotogramas, con el fin de lograr objetivos de rentabilidad y rendimiento. Para este tipo de carga de trabajo, una sola máquina virtual habilitada para GPU puede coincidir con el rendimiento de muchas máquinas virtuales solo de CPU.
- **Cargas de trabajo de HPC y ml**: en el caso de cargas de trabajo informáticas en paralelo de gran nivel de datos, como el entrenamiento o la inferencia del modelo de proceso de alto rendimiento y de aprendizaje automático, las GPU pueden reducir drásticamente el tiempo de inferencia y el tiempo de entrenamiento. Como alternativa, pueden ofrecer una mejor rentabilidad que una arquitectura solo de CPU en un nivel de rendimiento comparable. Muchos marcos de trabajo de HPC y machine learning tienen una opción para habilitar la aceleración de GPU; considere si esto puede beneficiar a su carga de trabajo específica.

## <a name="gpu-virtualization-in-windows-server"></a>Virtualización de GPU en Windows Server

Las tecnologías de virtualización de GPU permiten la aceleración de GPU en un entorno virtualizado, normalmente dentro de máquinas virtuales. Si la carga de trabajo está virtualizada con Hyper-V, tendrá que emplear la virtualización de gráficos para proporcionar aceleración de GPU desde la GPU física a sus aplicaciones o servicios virtualizados. Sin embargo, si la carga de trabajo se ejecuta directamente en hosts de Windows Server físicos, no necesita la virtualización de gráficos. las aplicaciones y los servicios ya tienen acceso a las funcionalidades de GPU y API que se admiten de forma nativa en Windows Server.

Las siguientes tecnologías de virtualización de gráficos están disponibles para las máquinas virtuales de Hyper-V en Windows Server:

- [Asignación discreta de dispositivos (DDA)](#discrete-device-assignment-dda)
- [RemoteFX vGPU](#remotefx-vgpu)

Además de las cargas de trabajo de máquinas virtuales, Windows Server también admite la aceleración de GPU de cargas de trabajo en contenedor en contenedores de Windows. Para obtener más información, consulte [aceleración de GPU en contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/gpu-acceleration).

## <a name="discrete-device-assignment-dda"></a>Asignación discreta de dispositivos (DDA)

La asignación discreta de dispositivos (DDA), también conocida como paso a través de GPU, le permite dedicar una o más GPU físicas a una máquina virtual. En una implementación de DDA, las cargas de trabajo virtualizadas se ejecutan en el controlador nativo y normalmente tienen acceso completo a la funcionalidad de la GPU. DDA ofrece el máximo nivel de compatibilidad de aplicaciones y rendimiento potencial. DDA también puede proporcionar aceleración de GPU a máquinas virtuales Linux, sujeto a soporte técnico.

Una implementación de DDA puede acelerar solo un número limitado de máquinas virtuales, ya que cada GPU física puede proporcionar aceleración a una máquina virtual como máximo. Si está desarrollando un servicio cuya arquitectura es compatible con máquinas virtuales compartidas, considere la posibilidad de hospedar varias cargas de trabajo aceleradas por máquina virtual. Por ejemplo, si está creando un servicio de comunicación remota de escritorio con RDS, puede mejorar el escalado de usuario aprovechando las capacidades de varias sesiones de Windows Server para hospedar varios escritorios de usuario en cada máquina virtual. Estos usuarios compartirán las ventajas de la aceleración de GPU.

Para más información, consulte los temas siguientes:

- [Planear la implementación de la asignación discreta de dispositivos](plan-for-deploying-devices-using-discrete-device-assignment.md)
- [Implementación de dispositivos de gráficos mediante la asignación discreta de dispositivos](../deploy/Deploying-graphics-devices-using-dda.md)

## <a name="remotefx-vgpu"></a>RemoteFX vGPU

> [!NOTE]
> Debido a los problemas de seguridad, vGPU de RemoteFX está deshabilitado de manera predeterminada en todas las versiones de Windows a partir de la actualización de seguridad del 14 de julio de 2020. Para obtener más información, consulte [KB 4570006](https://support.microsoft.com/help/4570006).

VGPU de RemoteFX es una tecnología de virtualización de gráficos que permite compartir una sola GPU física entre varias máquinas virtuales. En una implementación de vGPU de RemoteFX, las cargas de trabajo virtualizadas se ejecutan en el adaptador 3D RemoteFX de Microsoft, que coordina las solicitudes de procesamiento de GPU entre el host y los invitados. La vGPU de RemoteFX es más adecuada para cargas de trabajo de nivel de conocimiento y de alta ráfaga, donde no se requieren recursos de GPU dedicados. VGPU de RemoteFX solo puede proporcionar aceleración de GPU a máquinas virtuales Windows.

Para más información, consulte los temas siguientes:

- [Implementar dispositivos gráficos con RemoteFX vGPU](../deploy/deploy-graphics-devices-using-remotefx-vgpu.md)
- [Compatibilidad del adaptador de vídeo 3D RemoteFX (vGPU)](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)

## <a name="comparing-dda-and-remotefx-vgpu"></a>Comparar DDA y vGPU de RemoteFX

Tenga en cuenta la siguiente funcionalidad y admita diferencias entre las tecnologías de virtualización de gráficos al planear la implementación:

| Descripción | RemoteFX vGPU | Asignación discreta de dispositivos |
|--|--|--|
| Modelo de recursos de GPU | Dedicada o compartida | Solo dedicado |
| Densidad de máquinas virtuales | Alta (una o más GPU en muchas máquinas virtuales) | Bajo (una o más GPU en una VM) |
| Compatibilidad de aplicaciones | DX 11.1, OpenGL 4.4, OpenCL 1.1 | Todas las funcionalidades de GPU proporcionadas por el proveedor (DX 12, OpenGL, CUDA) |
| AVC444 | Habilitado de forma predeterminada | Disponible a través de directiva de grupo |
| VRAM de GPU | Hasta 1 GB de VRAM dedicada | Hasta la VRAM que admita la GPU |
| Velocidad de fotogramas | Hasta 30 fps | Hasta 60 fps |
| Controlador de GPU en invitado | Controlador de pantalla de adaptador 3D de RemoteFX (Microsoft) | Controlador del proveedor de GPU (NVIDIA, AMD, Intel) |
| Compatibilidad del sistema operativo host | Windows Server 2016 | Windows Server 2016; Windows Server 2019 |
| Compatibilidad con sistema operativo de invitado | Windows Server 2012 R2; Windows Server 2016; Windows 7 SP1; Windows 8.1; Windows 10 | Windows Server 2012 R2; Windows Server 2016; Windows Server 2019; Windows 10; Linuxun |
| Hipervisor | Microsoft Hyper-V | Microsoft Hyper-V |
| Hardware de GPU | GPU de la empresa (como Nvidia Quadro/GRID o AMD FirePro) | GPU de la empresa (como Nvidia Quadro/GRID o AMD FirePro) |
| Hardware del servidor | No hay requisitos especiales | Servidor moderno, expone IOMMU al sistema operativo (normalmente hardware compatible con SR-IOV) |
