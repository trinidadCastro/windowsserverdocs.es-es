---
title: 'RDS: Instalación y configuración de RemoteFX vGPU'
description: Información de planeación para configurar la virtualización de gráficos de RemoteFX vGPU.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/23/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0263fa6b-2185-4cc3-99ef-3588e2f4ada5
author: lizap
manager: scottman
ms.openlocfilehash: 3e189d9ac059136b40d8ee5d93a4eea5b788cdd1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870851"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>Instalación y configuración de RemoteFX vGPU para Servicios de Escritorio remoto


La característica vGPU de RemoteFX permite que varias máquinas virtuales compartan una tarjeta gráfica física. Las máquinas virtuales pueden descargar la representación de la información gráfica desde el procesador a la tarjeta gráfica dedicada. Esto disminuye la carga de la CPU y mejora la escalabilidad de las cargas de trabajo con uso intensivo de gráficos que se ejecutan en las máquinas virtuales de VDI. 

## <a name="remotefx-vgpu-requirements"></a>Requisitos de RemoteFX vGPU

Requisitos para los sistemas host: 

- Windows Server 2016 o Windows 10
- GPU compatible con DX 11.0 con controlador compatible con WDDM 1.2 
- Rol de host de virtualización de Escritorio remoto de Windows Server habilitado (habilita el rol de Hyper-V) 
- Servidor con una CPU que admita SLAT (traducción de direcciones de segundo nivel). 

Requisitos de la máquina virtual invitada:

- Máquina virtual que ejecuta un cliente de Windows Enterprise (Windows 7 con Service Pack 1, Windows 8.1, Windows 10) o Windows Server (Windows Server 2012 R2 o Windows Server 2016). Para compatibilidad con sistemas operativos adicionales, consulta [Configuración admitida para los Servicios de Escritorio remoto](rds-supported-config.md).

Consideraciones adicionales de las máquinas virtuales invitadas:

- Las funcionalidades OpenGL y OpenCL solo están disponibles en Windows 10 o Windows Server 2016.  
- DirectX 11.0 solo está disponible con Windows 8 o máquinas virtuales más recientes. 
- El host de sesión de Escritorio remoto solo es compatible con RemoteFX vGPU si se está ejecutando como un [escritorio de sesión personal](rds-personal-session-desktops.md).

Para las máquinas virtuales invitadas, no olvides revisar [Implementación de VDI: Sistemas operativos invitados compatibles](rds-supported-config.md#vdi-deployment--supported-guest-oss).

## <a name="install-remotefx-vgpu"></a>Instalación de RemoteFX vGPU

Usa los pasos siguientes para instalar y configurar RemoteFX en el host de Windows Server 2016 y Windows 10:

1. Instale el sistema operativo.
2. Instala los controladores de GPU más recientes de Windows 10 y Windows Server 2016 que estén disponibles en el sitio web del proveedor de la tarjeta gráfica.
3. Instala RemoteFX vGPU en el host de Windows 10 o Windows Server 2016:
   1. En un host de Windows 10, habilita la característica Hyper-V en el Panel de control (ve a Panel de control/Programas y características/Activar o desactivar las características de Windows):

      ![Ventana de características de Windows que permite habilitar la característica Hyper-V](media/rds-hyperv-settings.png)

   2. En un host de Windows Server 2016, instala el rol de host de virtualización de Escritorio remoto (RDVH).
   

4. Ahora, crea y configura una máquina virtual invitada:
   1. Crea una máquina virtual con Windows 10 Enterprise o Windows Server 2016.
   2. Agrega la tarjeta gráfica 3D de RemoteFX. Consulta [Configuración de la tarjeta gráfica 3D de RemoteFX vGPU](#configure-the-remotefx-vgpu-3d-adapter) para obtener información sobre cómo hacerlo con los cmdlets del Administrador de Hyper-V o de PowerShell. 

RemoteFX vGPU utilizará todas las GPU cuando haya más de una disponible. Sin embargo, en algunos casos es posible que quieras restringir qué GPU se usan con RemoteFX. En el entorno de Hyper-V, puedes controlar esto seleccionando específicamente que GPU *no* debería utilizar RemoteFX. Use los pasos siguientes: 

   1. Ve a la configuración de Hyper-V en el Administrador de Hyper-V.
   2. Haz clic en **GPU físicas** en la configuración de Hyper-V.
   3. Selecciona la GPU que no quieres usar y, a continuación, desactiva **Usar esta GPU con RemoteFX**.


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configuración de la tarjeta gráfica 3D de RemoteFX vGPU
Puedes usar cmdlets de la interfaz de usuario del Administrador de Hyper-V o de PowerShell para configurar la tarjeta gráfica 3D de RemoteFX vGPU. 

#### <a name="through-hyper-v-manager"></a>Mediante el Administrador de Hyper-V:

1. Asegúrate de que el sistema se ha configurado con Hyper-V y que tiene una máquina virtual configurada.  
2. Detén la máquina virtual, si se está ejecutando. 
3. En el Administrador de Hyper-V, ve a **Configuración de la VM** y, a continuación, haz clic en **Agregar hardware**.
4. Selecciona la **tarjeta gráfica 3D de RemoteFX** y haz clic en **Agregar**. 
5. Establece el número máximo de monitores, la resolución máxima de monitor y la memoria de vídeo dedicada, o bien, deja los valores predeterminados.

   > [!NOTE]
   > - Si configuras valores más elevados para cualquiera de estas opciones, esto afectará a la escalabilidad, por lo que solo deberías configurar lo que sea absolutamente necesario.
   >
   > - Si necesitas usar 1 GB de VRAM dedicada, usa una máquina virtual invitada de 64 bits en lugar de 32 bits (x86) para obtener los mejores resultados.
6. Haz clic en **Aceptar** para finalizar la configuración.

#### <a name="with-powershell-cmdlets"></a>Con los cmdlets de PowerShell:

Ejecuta los siguientes cmdlets para agregar, revisar y configurar la tarjeta: 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Para más información, consulta [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter).

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

Para más información, consulta [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter).

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Para más información, consulta [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter).

Ejecuta el siguiente cmdlet para revisar las GPU físicas:

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

Para más información, consulta [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter).

## <a name="monitor-performance"></a>Supervisión del rendimiento

El rendimiento y el escalado de un sistema VDI se determinan mediante una serie de factores como la memoria total de la GPU, la cantidad de memoria del sistema y la velocidad de la memoria, el número de núcleos de la CPU y la frecuencia del reloj de la CPU, la velocidad de almacenamiento y la implementación de NUMA.

La compatibilidad remota de vGPU en RDS incluye los siguientes contadores de rendimiento que puedes ver en Supervisión del rendimiento (perfmon.exe) para reunir información acerca del rendimiento de fotogramas por segundo.

- Gráficos de RemoteFX: Contadores para la compresión de gráficos de Protocolo de escritorio remoto. Por ejemplo, si deseas mirar el número de fotogramas que se presentan a RDP para la compresión, examina el contador **Input Frames/Second** (fotogramas de entrada por segundo).
- Red de RemoteFX: Contadores para el tráfico de red de Protocolo de escritorio remoto. Por ejemplo, **Round Trip Time (RTT)** (Tiempo de ida y vuelta).
- Administración de GPU raíz de RemoteFX: Medidas de VRAM reservada y disponible.
- Software de RemoteFX: Proporciona contadores para la frecuencia de captura, el tiempo de respuesta de la GPU, etc.

Para una mayor supervisión del rendimiento de bajo nivel, especialmente para solución de problemas, puedes usar los siguientes contadores de rendimiento adicionales:

- Dispositivo de VM de VSC Synth3D de RemoteFX 
- Canal de transporte de VM de VSC Synth3D de RemoteFX 
- VSP Synth3D de RemoteFX 
- Máquina virtual de VSP Synth3D de RemoteFX 
- Canal de transporte de máquina virtual de VSP Synth3D de RemoteFX
