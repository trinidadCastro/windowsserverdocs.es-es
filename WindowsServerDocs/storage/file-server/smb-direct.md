---
title: Mejorar el rendimiento de un servidor de archivos con SMB directo
description: Describe la característica de SMB directo en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 41126aa0d054607449d57928c1777679e5087e73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394461"
---
# <a name="smb-direct"></a>SMB directo

>Se aplica a: Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016

Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016 incluyen una característica llamada SMB directo, que admite el uso de adaptadores de red con capacidad de acceso directo a memoria remota (RDMA). Los adaptadores de red que cuentan con RDMA pueden funcionar a toda velocidad con una latencia muy baja y, al mismo tiempo, minimizan el uso de CPU. Para las cargas de trabajo como Hyper-V o Microsoft SQL Server, esto permite que un servidor de archivo remoto se parezca al almacenamiento local. SMB directo incluye:

- Aumento de rendimiento: aprovecha el rendimiento total de las redes de alta velocidad donde los adaptadores de red coordinan la transferencia de grandes cantidades de datos a velocidad de línea.
- Latencia baja: ofrece respuestas extremadamente rápidas a las solicitudes de red y, como resultado, hace que parezca que el almacenamiento de archivos remoto es en realidad almacenamiento en bloque conectado directamente.
- Uso bajo de CPU: usa menos ciclos de CPU al transferir datos por la red, lo que deja más potencia disponible a las aplicaciones de servidor.

Windows Server 2012 R2 y Windows Server 2012 configuran automáticamente SMB directo.

## <a name="smb-multichannel-and-smb-direct"></a>SMB multicanal y SMB directo

SMB multicanal es la característica responsable de detectar las funcionalidades RDMA de los adaptadores de red para habilitar SMB directo. Sin SMB multicanal, SMB usa TCP/IP regular con los adaptadores de red aptos para RDMA (todos los adaptadores de red proporcionan una pila TCP/IP junto con la nueva pila RDMA).

Con SMB multicanal, SMB detecta si un adaptador de red tiene una funcionalidad RDMA y después crea varias conexiones RDMA para esa sola sesión (dos por interfaz). Esto permite que SMB utilice el alto rendimiento, la baja latencia y el uso reducido de CPU que ofrecen los adaptadores de red compatibles con RDMA. También ofrece tolerancia a errores si usa varias interfaces de RDMA.

>[!NOTE]
>No debe agrupar adaptadores de red compatibles con RDMA si planea usar la funcionalidad RDMA de los adaptadores de red. Al agruparlos, los adaptadores de red no admitirán RDMA.
>Después de crear al menos una conexión de red RDMA, ya no se utiliza la conexión TCP/IP que se usó para la negociación del protocolo original. Sin embargo, se conserva la conexión TCP/IP en caso de que falle la conexión de red RDMA.

## <a name="requirements"></a>Requisitos

SMB directo tiene los siguientes requisitos:

- Al menos dos equipos que ejecuten Windows Server 2012 R2 o Windows Server 2012
- Uno o más adaptadores de red con la funcionalidad RDMA.

### <a name="considerations-when-using-smb-direct"></a>Consideraciones sobre el uso de SMB directo

- Puede usar SMB directo en un clúster de conmutación por error; sin embargo, necesita asegurarse de que las redes utilizadas para el acceso del cliente sean las adecuadas para SMB directo. La conmutación por error admite el uso de varias redes para acceso de cliente, junto con los adaptadores de red que admiten RSS (Ajuste de escala en lado de recepción) y RDMA.
- Puede usar SMB directo en el sistema operativo de administración de Hyper-V de manera que admita el uso de Hyper-V a través de SMB y para proporcionar almacenamiento en una máquina virtual que use la pila de almacenamiento de Hyper-V. Sin embargo, los adaptadores de red compatibles con RDMA no están directamente expuestos a un cliente Hyper-V. Si conecta un adaptador de red compatible con RDMA a un conmutador virtual, los adaptadores de redes virtuales del conmutador no serán compatibles con RDMA.
- Si deshabilita SMB multicanal, también se deshabilita SMB directo. Dado que SMB multicanal detecta funcionalidades de adaptadores de red y determina si un adaptador de red es compatible con RDMA, el cliente no puede usar SMB directo si SMB multicanal está deshabilitado.
- SMB directo no se admite en Windows RT. SMB directo requiere compatibilidad con los adaptadores de red compatibles con RDMA, que solo está disponible en Windows Server 2012 R2 y Windows Server 2012.
- SMB directo no se admite en versiones inferiores de Windows Server. Solo se admite en Windows Server 2012 R2 y Windows Server 2012.

## <a name="enabling-and-disabling-smb-direct"></a>Habilitar y deshabilitar SMB directo

SMB directo está habilitado de forma predeterminada cuando está instalado Windows Server 2012 R2 o Windows Server 2012. El cliente SMB detecta y usa automáticamente varias conexiones de red si se identifica una configuración apropiada.

### <a name="disable-smb-direct"></a>Deshabilitar SMB directo

Generalmente, no deberá deshabilitar SMB directo; sin embargo, puede deshabilitarlo al ejecutar uno de los siguientes scripts de Windows PowerShell.

Para deshabilitar RDMA para una interfaz específica, escriba:

```PowerShell
Disable-NetAdapterRdma <name>
```

Para deshabilitar RDMA para todas las interfaces, escriba:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

Si deshabilita RDMA en el cliente o en el servidor, los sistemas no pueden usarlo. *Network Direct* es el nombre interno de la compatibilidad con redes básicas de windows Server 2012 R2 y windows Server 2012 para las interfaces RDMA.

### <a name="re-enable-smb-direct"></a>Volver a habilitar SMB directo

Después de deshabilitar RDMA, puede volver a habilitarlo al ejecutar uno de los siguientes scripts de Windows PowerShell.

Para volver a habilitar RDMA para una interfaz específica, escriba:

```PowerShell
Enable-NetAdapterRDMA <name>
```

Para volver a habilitar RDMA para todas las interfaces, escriba:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

Debe habilitar RDMA en el cliente y en el servidor para comenzar a usarlo nuevamente.

## <a name="test-performance-of-smb-direct"></a>Probar el rendimiento de SMB directo

Puede probar cómo funciona el rendimiento al emplear uno de los siguientes procedimientos.

### <a name="compare-a-file-copy-with-and-without-using-smb-direct"></a>Comparar una copia de archivo utilizando y no utilizando SMB directo

Aquí se muestra cómo medir el mayor rendimiento de SMB directo:

1. Configurar SMB directo
2. Mida la cantidad de tiempo necesaria para ejecutar una copia grande de archivos con SMB directo.
3. Deshabilite RDMA en el adaptador de red, consulte [Enabling and disabling SMB Direct](#enabling-and-disabling-smb-direct).
4. Mida la cantidad de tiempo necesaria para ejecutar una copia grande de archivos sin utilizar SMB directo.
5. Vuelva a habilitar RDMA en el adaptador de red y después, compare ambos resultados.
6. Para evitar el impacto del almacenamiento en caché, debe hacer lo siguiente:
    1. Copie una cantidad grande de datos (más datos de los que la memoria es capaz de manejar).
    2. Copie los datos dos veces, realice la primera copia como práctica y después cronometre la segunda copia.
    3. Reinicie ambos servidores y el cliente antes de cada prueba para asegurarse de que funcionen bajo condiciones similares.

### <a name="fail-one-of-multiple-network-adapters-during-a-file-copy-with-smb-direct"></a>Falla de uno de los varios adaptadores de red durante la copia de archivos con SMB directo

A continuación se indica cómo confirmar la capacidad de conmutación por error de SMB directo:

1. Asegúrese de que SMB directo funcione en una configuración de varios adaptadores de red.
2. Ejecute una copia grande de archivos. Mientras se ejecuta la copia, simule un error de una de las rutas de red al desconectar uno de los cables (o al deshabilitar uno de los adaptadores de red).
3. Confirme que la copia de archivos continúe usando uno de los adaptadores de red restantes y que no haya ningún error en la copia de archivos.

>[!NOTE]
>Para evitar errores en la carga de trabajo que no usa SMB directo, asegúrese de que no haya otras cargas de trabajo que usen la ruta de red desconectada.

## <a name="more-information"></a>Más información

- [Información general de bloque de mensajes del servidor](file-server-smb-overview.md)
- [Aumento de la disponibilidad del servidor, el almacenamiento y la red: información general del escenario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Implementación de Hyper-V en SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
