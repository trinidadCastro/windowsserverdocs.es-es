---
title: Mejorar el rendimiento de un servidor de archivos con SMB directo
description: Describe la característica de SMB directo en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: ed8fd5b4114fc9fd9c7dc278a98cea8cc67a8749
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239232"
---
# SMB directo

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016 incluyen una característica denominada SMB directo, que admite el uso de adaptadores de red que tienen la capacidad de memoria de acceso directo remoto (RDMA). Los adaptadores de red que tienen RDMA puedan funcionar a la máxima velocidad con una latencia muy baja, mientras usan la CPU muy poco. Para cargas de trabajo, como Hyper-V o Microsoft SQL Server, esto permite que un servidor de archivos remoto similar a la de almacenamiento local. SMB directo incluye:

- Un mayor rendimiento: aprovecha el rendimiento completo de redes de alta velocidad donde la transferencia de grandes cantidades de datos a velocidad de línea de coordenadas de los adaptadores de red.
- Baja latencia: proporciona respuestas rápidas a las solicitudes de red y, como resultado, hace que el almacenamiento de archivos remoto sentir como si es almacenamiento en bloque conectado directamente.
- Uso de la CPU de baja: usa menos ciclos de CPU cuando transfiriendo datos a través de la red, lo que deja más energía disponible para aplicaciones de servidor.

SMB directo se configuran automáticamente con Windows Server 2012 R2 y Windows Server 2012.

## SMB multicanal y SMB directo

SMB multicanal es la característica responsable de detección de las funcionalidades RDMA de adaptadores de red para habilitar SMB directo. Sin SMB multicanal, SMB usa normal TCP/IP con los adaptadores de red compatibles con RDMA (todos los adaptadores de red proporcionan una pila TCP/IP junto con la nueva pila RDMA).

Con SMB multicanal, SMB detecta si un adaptador de red tiene la capacidad de RDMA y, a continuación, crea varias conexiones de RDMA para esa sesión única (dos por interfaz). Esto permite SMB usar el alto rendimiento, baja latencia y bajo uso de CPU que ofrece compatibilidad con RDMA adaptadores de red. También ofrece tolerancia a errores si utilizas varias interfaces RDMA.

>[!NOTE]
>No debe team compatibilidad con RDMA adaptadores de red si vas a usar la funcionalidad RDMA los adaptadores de red. Cuando se asoció, los adaptadores de red no admitirá RDMA.
>Después de crea al menos una conexión de red RDMA, ya no se usa la conexión TCP/IP utilizada para la negociación del protocolo original. Sin embargo, se conserva la conexión TCP/IP en el caso de las conexiones de red RDMA producirá un error.

## Requisitos

SMB directo requiere lo siguiente:

- Al menos dos equipos que ejecutan Windows Server 2012 R2 o Windows Server 2012
- Uno o más adaptadores de red con la funcionalidad RDMA.

### Consideraciones sobre el uso de SMB directo

- Puedes usar SMB directo en un clúster de conmutación por error. Sin embargo, debes asegurarte de que las redes de clúster usa para el acceso de cliente son adecuadas para SMB directo. Failover clustering supports using multiple networks for client access, along with network adapters that are RSS (Receive Side Scaling)-capable and RDMA-capable.
- Puedes usar SMB directo en el sistema operativo de administración de Hyper-V para admitir el uso de Hyper-V en SMB y para proporcionar almacenamiento a una máquina virtual que use la pila de almacenamiento de Hyper-V. Sin embargo, compatibilidad con RDMA adaptadores de red no se exponen directamente a un cliente de Hyper-V. Si se conecta un adaptador de red compatibilidad con RDMA a un conmutador virtual, los adaptadores de red virtual desde el conmutador no estará compatibilidad con RDMA.
- Si deshabilitas SMB multicanal, SMB directo también está deshabilitado. Dado que SMB multicanal detecta las funcionalidades del adaptador de red y determina si un adaptador de red es compatible con RDMA, SMB directo no puede usarse por el cliente si se deshabilita SMB multicanal.
- SMB directo no se admite en Windows RT SMB directo necesita compatibilidad con compatibilidad con RDMA adaptadores de red, que está disponible solo en Windows Server 2012 R2 y Windows Server 2012.
- SMB directo no se admite en versiones de nivel inferior de Windows Server. Se admite solo en Windows Server 2012 R2 y Windows Server 2012.

## Habilitación y deshabilitación de SMB directo

SMB directo está habilitado de manera predeterminada cuando se instala Windows Server 2012 R2 o Windows Server 2012. El cliente SMB automáticamente detecta y usa varias conexiones de red si se identifica una configuración adecuada.

### Deshabilitar SMB directo

Por lo general, no debes deshabilitar SMB directo, sin embargo, puedes deshabilitar mediante la ejecución de uno de los siguientes scripts de Windows PowerShell.

Para deshabilitar RDMA para una interfaz específica, escribe:

```PowerShell
Disable-NetAdapterRdma <name>
```

Para deshabilitar RDMA para todas las interfaces, escribe:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

Al deshabilitar RDMA en el cliente o el servidor, los sistemas no usan. *Red directa* es el nombre interno para Windows Server 2012 R2 y Windows Server 2012 compatibilidad básica de red para interfaces RDMA.

### Volver a habilitar SMB directo

Después de deshabilitar RDMA, se puede volver a habilitarlo mediante la ejecución de uno de los siguientes scripts de Windows PowerShell.

Para volver a habilitar RDMA para una interfaz específica, escribe:

```PowerShell
Enable-NetAdapterRDMA <name>
```

Para volver a habilitar RDMA para todas las interfaces, escribe:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

Debes habilitar RDMA en el cliente y el servidor para empezar a utilizarlo de nuevo.

## Probar el rendimiento de SMB directo

Puedes probar cómo funciona el rendimiento mediante uno de los siguientes procedimientos.

### Comparar una copia del archivo con y sin usar SMB directo

Aquí te mostramos cómo medir el rendimiento de una mayor de SMB directo:

1. Configurar SMB directo
2. Medir la cantidad de tiempo para ejecutar una copia de archivos de gran tamaño con SMB directo.
3. Deshabilitar RDMA en el adaptador de red, vea [Habilitar y deshabilitar SMB directo](#enabling-and-disabling-smb-direct).
4. Medir la cantidad de tiempo para ejecutar una copia de archivos de gran tamaño sin usar SMB directo.
5. Vuelve a habilitar RDMA en el adaptador de red y, a continuación, compara los dos resultados.
6. Para evitar el impacto de almacenamiento en caché, debe hacer lo siguiente:
    1. Copia una gran cantidad de datos (más datos de memoria es capaz de control).
    2. Copiar los datos de dos veces, con la primera copia como práctica y, a continuación, la segunda copia de tiempo.
    3. Reinicia el servidor y el cliente antes de cada prueba para asegurarte de que trabajen en condiciones similares.

### Producirá un error en uno de varios adaptadores de red durante una copia de archivos con SMB directo

Aquí te mostramos cómo confirmar la capacidad de conmutación por error de SMB directo:

1. Asegúrate de que está funcionando SMB directo en una configuración de adaptador de red múltiples.
2. Ejecutar una copia de archivos de gran tamaño. Mientras se ejecuta la copia, simular un error de una de las rutas de acceso de red desconectando uno de los cables (o si se deshabilita uno de los adaptadores de red).
3. Confirma que la copia de archivos continúa usando uno de los adaptadores de red restante, y que no hay ningún error de copia de archivos.

>[!NOTE]
>Para evitar errores de carga de trabajo que no use SMB directo, asegúrate de que no hay ningún otras cargas de trabajo con la ruta de acceso de red desconectado.

## Más información

- [Información general de bloque de mensajes del servidor](file-server-smb-overview.md)
- [Aumentar la disponibilidad de la red, almacenamiento y servidores: información general del escenario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Implementar Hyper-V en SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
