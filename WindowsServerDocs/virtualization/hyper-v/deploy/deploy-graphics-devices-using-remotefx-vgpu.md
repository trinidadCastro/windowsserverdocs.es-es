---
title: Implementación de dispositivos de gráficos mediante vGPU de RemoteFX
description: Obtenga información sobre cómo implementar y configurar vGPU de RemoteFX en Windows Server
ms.prod: windows-server-threshold
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: 7111899557279d825191948e737d83d7467ce786
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923917"
---
# <a name="deploy-graphics-devices-using-remotefx-vgpu"></a>Implementación de dispositivos de gráficos mediante vGPU de RemoteFX

> Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016

La característica vGPU para RemoteFX permite que varias máquinas virtuales compartan una GPU física. Los recursos de representación y proceso se comparten dinámicamente entre las máquinas virtuales, lo que hace que el vGPU de RemoteFX sea adecuado para cargas de trabajo de alta ráfaga en las que no se requieren recursos de GPU dedicados. Por ejemplo, en un servicio VDI, el vGPU de RemoteFX se puede usar para descargar los costos de representación de la aplicación en la GPU, con el efecto de reducir la carga de la CPU y mejorar la escalabilidad del servicio.

## <a name="remotefx-vgpu-requirements"></a>Requisitos de RemoteFX vGPU

Requisitos del sistema host:

- Windows Server 2016
- Una GPU compatible con DirectX 11,0 con un controlador compatible con WDDM 1,2
- Una CPU con compatibilidad con la traducción de direcciones de segundo nivel (SLAT)

Requisitos de la máquina virtual invitada:

- SO invitado compatible. Para obtener más información, consulte [compatibilidad con el adaptador de vídeo RemoteFX 3D (vGPU)](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support).

Consideraciones adicionales de las máquinas virtuales invitadas:

- La funcionalidad de OpenGL y OpenCL solo está disponible en los invitados que ejecutan Windows 10 o Windows Server 2016.  
- DirectX 11,0 solo está disponible para invitados que ejecuten Windows 8 o posterior.

## <a name="enable-remotefx-vgpu"></a>Habilitar vGPU de RemoteFX

Para configurar vGPU de RemoteFX en el host de Windows Server 2016:

1. Instale los controladores de gráficos recomendados por el proveedor de GPU para Windows Server 2016.
2. Cree una máquina virtual que ejecute un sistema operativo invitado compatible con vGPU de RemoteFX. Para más información, consulte [compatibilidad con el adaptador de vídeo RemoteFX 3D (vGPU)](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support).
3. Agregue el adaptador de gráficos RemoteFX 3D a la máquina virtual. Para obtener más información, consulte [configuración del adaptador de VGPU 3D de RemoteFX](#configure-the-remotefx-vgpu-3d-adapter).

De forma predeterminada, vGPU de RemoteFX usará todas las GPU disponibles y admitidas. Para limitar qué GPU usa la vGPU de RemoteFX, siga estos pasos:

1. Ve a la configuración de Hyper-V en el Administrador de Hyper-V.
2. Seleccione **GPU físicas** en configuración de Hyper-V.
3. Selecciona la GPU que no quieres usar y, a continuación, desactiva **Usar esta GPU con RemoteFX**.

### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configuración de la tarjeta gráfica 3D de RemoteFX vGPU

Puedes usar cmdlets de la interfaz de usuario del Administrador de Hyper-V o de PowerShell para configurar la tarjeta gráfica 3D de RemoteFX vGPU.

#### <a name="configure-remotefx-vgpu-with-hyper-v-manager"></a>Configuración de vGPU de RemoteFX con el administrador de Hyper-V

1. Detenga la máquina virtual si se está ejecutando actualmente.
2. Abra el administrador de Hyper-V, vaya a **configuración de VM**y, luego, seleccione **agregar hardware**.
3. Seleccione **adaptador de gráficos RemoteFX 3D**y, a continuación, seleccione **Agregar**.
4. Establece el número máximo de monitores, la resolución máxima de monitor y la memoria de vídeo dedicada, o bien, deja los valores predeterminados.

   > [!NOTE]
   > - El establecimiento de valores más altos para cualquiera de estas opciones afectará a la escala de servicio, por lo que solo debe establecer lo que sea necesario.
   > - Cuando necesite usar 1 GB de VRAM dedicada, use una máquina virtual invitada de 64 bits en lugar de 32 bits (x86) para obtener los mejores resultados.

5. Seleccione **Aceptar** para finalizar la configuración.

#### <a name="configure-remotefx-vgpu-with-powershell-cmdlets"></a>Configuración de vGPU de RemoteFX con cmdlets de PowerShell

Use los siguientes cmdlets de PowerShell para agregar, revisar y configurar el adaptador:

- [Add-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/add-vmremotefx3dvideoadapter?view=win10-ps)
- [Get-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefx3dvideoadapter?view=win10-ps)
- [Set-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/set-vmremotefx3dvideoadapter?view=win10-ps)
- [Get-VMRemoteFXPhysicalVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefxphysicalvideoadapter?view=win10-ps)

## <a name="monitor-performance"></a>Supervisión del rendimiento

El rendimiento y la escala de un servicio habilitado para vGPU de RemoteFX vienen determinados por diversos factores, como el número de GPU del sistema, la memoria total de la GPU, la cantidad de memoria del sistema y la velocidad de memoria, el número de núcleos de CPU y la frecuencia del reloj de la CPU, la velocidad de almacenamiento y NUMA aplicación.

### <a name="host-system-memory"></a>Memoria del sistema host

Para cada máquina virtual habilitada con vGPU, RemoteFX usa la memoria del sistema tanto en el sistema operativo invitado como en el servidor host. El hipervisor garantiza la disponibilidad de la memoria del sistema para un sistema operativo invitado. En el host, cada escritorio virtual habilitado para vGPU debe anunciar su requisito de memoria del sistema al hipervisor. Cuando se inicia el escritorio virtual habilitado para vGPU, el hipervisor reserva más memoria del sistema en el host.

Los requisitos de memoria para el servidor habilitado para RemoteFX es dinámico porque la cantidad de memoria consumida en el servidor habilitado para RemoteFX depende del número de monitores asociados a los escritorios virtuales habilitados para vGPU y la resolución máxima de esos monitores.

### <a name="host-gpu-video-memory"></a>Memoria de vídeo de GPU de host

Cada escritorio virtual habilitado para vGPU usa la memoria de vídeo de hardware de GPU en el servidor host para representar el escritorio. Además, un códec usa la memoria de vídeo para comprimir la pantalla representada. La cantidad de memoria necesaria para la representación y compresión se basa directamente en el número de monitores aprovisionados a la máquina virtual. La cantidad de memoria de vídeo reservada varía en función de la resolución de la pantalla del sistema y del número de monitores que haya. Algunos usuarios requieren una resolución de pantalla más alta para tareas específicas, pero hay mayor escalabilidad con una configuración de resolución inferior si el resto de opciones de configuración permanecen constantes.

### <a name="host-cpu"></a>CPU del host

El hipervisor programa el host y las máquinas virtuales en la CPU. La sobrecarga se aumenta en un host habilitado para RemoteFX porque el sistema ejecuta un proceso adicional (rdvgm. exe) por escritorio virtual habilitado para vGPU. Este proceso usa el controlador de dispositivo gráfico para ejecutar comandos en la GPU. El códec también utiliza la CPU para comprimir los datos de pantalla que se deben devolver al cliente.

Un mayor número de procesadores virtuales supone una mejor experiencia de usuario. Se recomienda asignar al menos dos CPU virtuales por escritorio virtual habilitado para vGPU. También se recomienda usar la arquitectura x64 para escritorios virtuales habilitados para vGPU porque el rendimiento de las máquinas virtuales x64 es mejor en comparación con las máquinas virtuales x86.

### <a name="gpu-processing-power"></a>Potencia de procesamiento de GPU

Cada escritorio virtual habilitado para vGPU tiene un proceso de DirectX correspondiente que se ejecuta en el servidor host. Este proceso reproduce todos los comandos de gráficos que recibe del escritorio virtual de RemoteFX en la GPU física. Esto es como ejecutar varias aplicaciones de DirectX al mismo tiempo en la misma GPU física.

Normalmente, los dispositivos y controladores de gráficos están optimizados para ejecutar solo algunas aplicaciones en el escritorio a la vez, pero RemoteFX amplía las GPU para continuar. vGPUs incluye contadores de rendimiento que miden la respuesta de GPU a las solicitudes de RemoteFX y ayudan a asegurarse de que las GPU no se extienden demasiado lejos.

Cuando una GPU tiene pocos recursos, las operaciones de lectura y escritura tardan mucho tiempo en completarse. Los administradores pueden utilizar contadores de rendimiento para saber cuándo ajustar recursos y evitar el tiempo de inactividad de los usuarios.

Obtenga más información sobre los contadores de rendimiento para supervisar el comportamiento de vGPU de RemoteFX en [diagnóstico de problemas de rendimiento de gráficos en escritorio remoto](https://docs.microsoft.com/azure/virtual-desktop/remotefx-graphics-performance-counters).
