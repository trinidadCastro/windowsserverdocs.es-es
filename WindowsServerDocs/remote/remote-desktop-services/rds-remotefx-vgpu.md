---
title: RDS - RemoteFX vGPU instalación y configuración
description: Información de planeación para configurar la virtualización de gráficos de vGPU de RemoteFX.
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
ms.openlocfilehash: 3e7da1a70826dc720a96ceb3fe5d04868943f163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876846"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>Instalar y configurar RemoteFX vGPU para Servicios de Escritorio remoto


La característica de vGPU de RemoteFX permite varias máquinas virtuales compartir un adaptador de gráficos físicos. Las máquinas virtuales pueden descargar la representación de la información gráfica desde el procesador para el adaptador de gráficos dedicado. Esto disminuye la carga de CPU y mejorar la escalabilidad para cargas de trabajo intensas gráficos que se ejecutan en las máquinas virtuales VDI. 

## <a name="remotefx-vgpu-requirements"></a>Requisitos de la vGPU de RemoteFX

Requisitos para sistemas host: 

- Windows Server 2016 o Windows 10
- 11.0 DX GPU compatible con el controlador compatible con WDDM 1.2 
- Rol de Host de virtualización de escritorio remoto de Windows Server habilitado (habilita el rol Hyper-V) 
- Servidor con una CPU que admita SLAT (traducción de direcciones de segundo nivel) 

Requisitos de la máquina virtual invitada:

- Ejecución de un cliente de empresa de Windows (Windows 7 con Service Pack 1, Windows 8.1, Windows 10) o Windows Server (Windows Server 2012 R2 o Windows Server 2016) de máquina virtual invitada. Para sistemas operativos adicionales admiten vea [configuración admitidos para los servicios de escritorio remoto](rds-supported-config.md).

Consideraciones adicionales para las máquinas virtuales invitadas:

- OpenGL y OpenCL funcionalidad solo está disponible en Windows 10 o Windows Server 2016.  
- 11.0 de DirectX solo está disponible con Windows 8 o máquinas virtuales más recientes de invitado. 
- Host de sesión de escritorio remoto solo es compatible con RemoteFX vGPU si se está ejecutando como un [desktop sesión personal](rds-personal-session-desktops.md).

Para los Invitados VM, no olvide revisar [implementación de VDI: sistemas operativos invitados compatibles](rds-supported-config.md#vdi-deployment--supported-guest-oss).

## <a name="install-remotefx-vgpu"></a>Instalar la vGPU de RemoteFX

Use los pasos siguientes para instalar y configurar RemoteFX en el host de Windows Server 2016 y Windows 10:

1. Instale el sistema operativo.
2. Instalar el más reciente Windows 10 o Windows controladores de GPU de Server 2016 disponibles desde el sitio del proveedor de tarjeta de gráficos.
3. Instale RemoteFX vGPU en el host de Windows 10 o Windows Server 2016:
   1. En un host de Windows 10, habilitar la característica de Hyper-V en el Panel de Control (vaya a/programas y características de Windows de características/activar o desactivar):

      ![Ventana de las características de Windows para habilitar la característica de Hyper-V](media/rds-hyperv-settings.png)

   2. En un host de Windows Server 2016, instale el rol de Host de virtualización de escritorio remoto (RDVH).
   

4. Ahora, cree y configure una máquina virtual invitada:
   1. Cree una máquina virtual con Windows 10 Enterprise o Windows Server 2016.
   2. Agregar el adaptador de gráficos 3D de RemoteFX. Consulte [configurar el adaptador RemoteFX vGPU 3D](#configure-the-remotefx-vgpu-3d-adapter) para obtener información sobre cómo hacerlo con los cmdlets del Administrador de Hyper-V o PowerShell. 

RemoteFX vGPU utilizará todas las GPU cuando hay más de uno disponible. Sin embargo, en algunos casos es posible que desea limitar qué GPU se usan con RemoteFX. En el entorno de Hyper-V, controlarlo seleccionando específicamente que deberían GPU *no* utilizar RemoteFX. Use los pasos siguientes: 

   1. Vaya a la configuración de Hyper-V en el Administrador de Hyper-V.
   2. Haga clic en **GPU físicas** en configuración de Hyper-V.
   3. Seleccione la GPU que no desea usar y, a continuación, desactive **usar este GPU con RemoteFX**.


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurar el adaptador RemoteFX vGPU 3D
Puede usar cmdlets de la interfaz de usuario de administrador de Hyper-V o PowerShell para configurar el adaptador de gráficos 3D de vGPU de RemoteFX. 

#### <a name="through-hyper-v-manager"></a>A través del Administrador de Hyper-V:

1. Asegúrese del sistema se ha configurado con Hyper-V y ha configurado una máquina virtual.  
2. Detener la máquina virtual, si se está ejecutando. 
3. En el Administrador de Hyper-V, vaya a la **configuración de máquina virtual**y, a continuación, haga clic en **agregar Hardware**.
4. Seleccione **adaptador RemoteFX de gráficos 3D**y haga clic en **agregar**. 
5. Establecer el número máximo de monitores, la resolución máxima del monitor y la memoria de vídeo dedicada, o bien, deje los valores predeterminados.

   > [!NOTE]
   > - Los valores más altos para cualquiera de estas opciones de configuración afectará a escala, por lo que solo se debe establecer lo que sea absolutamente necesario.
   >
   > - Cuando necesite usar 1GB de VRAM dedicada, use una máquina virtual invitada de 64 bits en lugar de 32 bits (x86) para obtener mejores resultados.
6. Haga clic en **Aceptar** para finalizar la configuración.

#### <a name="with-powershell-cmdlets"></a>Los cmdlets de PowerShell:

Ejecute los siguientes cmdlets para agregar, revisar y configurar el adaptador: 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Para obtener información detallada, consulte [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter).

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

Para obtener información detallada, consulte [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Para obtener información detallada, consulte [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter).

Ejecute el siguiente cmdlet para revisar las GPU físicas:

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

Para obtener información detallada, consulte [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter).

## <a name="monitor-performance"></a>Supervisar el rendimiento

El rendimiento y la escala de un sistema VDI se determinan mediante una serie de factores como la memoria total de la GPU, cantidad de memoria del sistema y la velocidad de la memoria, número de núcleos de CPU y la frecuencia de reloj de CPU, velocidad de almacenamiento e implementación de NUMA.

Compatibilidad de la vGPU remoto en RDS incluye los siguientes contadores de rendimiento, que se pueden ver en el Monitor de rendimiento (perfmon.exe) para recopilar información sobre el rendimiento de velocidad de fotogramas.

- Gráficos de RemoteFX - contadores para la compresión de gráficos de protocolo de escritorio remoto. Por ejemplo, si desea observar el número de fotogramas que se presentan para RDP para la compresión, examine el **Frames/Second entrada** contador.
- Red de RemoteFX: los contadores para el tráfico de red de protocolo de escritorio remoto. Por ejemplo, **tiempo de ida y vuelta (RTT)**.
- Administración de GPU de RemoteFX Root - medidas VRAM reservada y disponible.
- RemoteFX Software - proporciona contadores para la frecuencia de captura, el tiempo de respuesta de la GPU y otros.

Para supervisar el rendimiento más bajo nivel, especialmente para solucionar el problema, puede usar los siguientes contadores de rendimiento adicionales:

- Dispositivo de máquina virtual VSC RemoteFX Synth3D 
- Canal de transporte de RemoteFX Synth3D VM VSC 
- VSP RemoteFX Synth3D 
- Dispositivo de máquina virtual de VSP RemoteFX Synth3D 
- Canal de máquina virtual de VSP RemoteFX Synth3D
