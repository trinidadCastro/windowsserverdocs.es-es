---
title: Problema de uso intensivo de la CPU en el servidor SMB
description: Presenta la forma de solucionar el problema de uso intensivo de la CPU en el servidor SMB.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 9634ca5b0eb07beff8d8213f88e771d76734e6fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815439"
---
# <a name="high-cpu-usage-issue-on-the-smb-server"></a>Problema de uso intensivo de la CPU en el servidor SMB

En este artículo se describe cómo solucionar el problema de uso intensivo de la CPU en el servidor SMB.

## <a name="high-cpu-usage-because-of-storage-performance-issues"></a>Uso intensivo de la CPU debido a problemas de rendimiento de almacenamiento

Los problemas de rendimiento del almacenamiento pueden provocar un uso intensivo de la CPU en los servidores SMB. Antes de solucionar los problemas, asegúrese de que el paquete acumulativo de actualizaciones más reciente está instalado en el servidor SMB para eliminar cualquier problema conocido en srv2. sys.

En la mayoría de los casos, observará el problema de uso intensivo de la CPU en el proceso del sistema. Antes de continuar, use el explorador de procesos para asegurarse de que srv2. sys o NTFS. sys consumen demasiados recursos de CPU.

### <a name="storage-area-network-san-scenario"></a>Escenario de red de área de almacenamiento (SAN)

En los niveles agregados, es posible que el rendimiento general de SAN parezca correcto. Sin embargo, cuando se trabaja con problemas de SMB, el tiempo de respuesta de solicitud individual es lo que más importa.

Por lo general, este problema puede deberse a algún tipo de cola de comandos en la red SAN. Puede usar **Perfmon** para capturar un seguimiento de **Microsoft-Windows-StorPort** y analizarlo para determinar con precisión la capacidad de respuesta del almacenamiento.

### <a name="disk-io-latency"></a>Latencia de e/s de disco

La latencia de e/s de disco es una medida del retraso entre el momento en que se crea y se completa una solicitud de e/s de disco.

La latencia de e/s que se mide en Perfmon incluye todo el tiempo que se emplea en las capas de hardware y el tiempo dedicado a la cola de controladores de puertos de Microsoft (Storport. sys para SCSI). Si los procesos en ejecución generan una cola de StorPort grande, aumenta la latencia medida. Esto se debe a que la e/s debe esperar antes de que se envíe a los niveles de hardware.

En Perfmon, los contadores siguientes muestran la latencia del disco físico:

- "Objeto de rendimiento de disco físico"-\> "promedio de segundos de disco/lectura": muestra el promedio de latencia de lectura.

- "Objeto de rendimiento de disco físico"-\> "promedio de segundos de disco/escritura": muestra la latencia media de escritura.

- "Objeto de rendimiento de disco físico"-\> "promedio de segundos de disco/recuento de transferencia": muestra los promedios combinados para lecturas y escrituras.

La instancia de "\_total" es el promedio de las latencias de todos los discos físicos del equipo. Cada una de las demás instancias representa un disco físico individual.

> [!NOTE]
> No confunda estos contadores con promedio de transferencias de disco por segundo. Se trata de contadores completamente diferentes.

### <a name="windows-storage-stack-follows"></a>Pila de almacenamiento de Windows sigue

En esta sección se proporciona una breve explicación sobre la pila de almacenamiento de Windows.

Cuando una aplicación crea una solicitud de e/s, envía la solicitud al subsistema de e/s de Windows en la parte superior de la pila. A continuación, la e/s viaja hacia abajo en la pila hasta el subsistema de "disco" de hardware. A continuación, la respuesta recorre todo el proceso de copia de seguridad. Durante este proceso, cada capa realiza su función y, a continuación, entrega la e/s a la siguiente capa.

![Flujo de pila](media/high-cpu-usage-issue-on-smb-server-1.png)

Perfmon no crea ningún dato de rendimiento por segundo. En su lugar, consume datos proporcionados por otros subsistemas dentro de Windows.

En "objeto de rendimiento de disco físico", los datos se capturan en el nivel de "Administrador de particiones" de la pila de almacenamiento.

Cuando se miden los contadores que se mencionan en la sección anterior, se mide todo el tiempo que se emplea en la solicitud bajo el nivel de "Administrador de particiones". Cuando la solicitud de e/s se envía por el administrador de particiones hacia abajo en la pila, se marca la hora. Cuando devuelve, se marca de nuevo el tiempo y se calcula la diferencia horaria. La diferencia de tiempo es la latencia.

Al hacerlo, contamos el tiempo dedicado a los siguientes componentes:

- Controlador de clase: administra el tipo de dispositivo, como discos, cintas, etc.

- Controlador de puerto: administra el protocolo de transporte, como SCSI, FC, SATA, etc.

- Controlador de minipuerto de dispositivo: es el controlador de dispositivo para el adaptador de almacenamiento. Lo proporciona el fabricante de los dispositivos, como el controlador RAID y el HBA de FC.

- Subsistema de disco: incluye todo lo que se encuentra debajo del controlador de minipuerto de dispositivo. Esto podría ser tan simple como un cable conectado a un solo disco duro físico o tan complejo como una red de área de almacenamiento. Si se determina que el problema se debe a este componente, puede ponerse en contacto con el proveedor de hardware para obtener más información acerca de la solución de problemas.

### <a name="disk-queuing"></a>Puesta en cola de discos

Existe una cantidad limitada de e/s que un subsistema de disco puede aceptar en un momento dado. El exceso de e/s se pone en cola hasta que el disco puede aceptar la e/s de nuevo. El tiempo que la e/s emplea en las colas bajo el nivel de "Administrador de particiones" se cuenta en las mediciones de latencia de disco físico de Perfmon. A medida que las colas crecen más y la e/s deben esperar más, la latencia medida también crece.

Hay varias colas por debajo del nivel de "Administrador de particiones", como se indica a continuación:

- Cola de controladores de puertos de Microsoft-puerto SCSIport o Storport

- Cola de controladores de dispositivos suministrados por el fabricante-controlador de dispositivo OEM

- Colas de hardware, como la cola del controlador de disco, la cola de conmutadores SAN, la cola del controlador de la matriz y la cola del disco duro

También se tiene en cuenta el tiempo que el disco duro invierte en atender activamente la e/s y el tiempo de viaje que se lleva a cabo para que la solicitud vuelva al nivel "Administrador de particiones" que se va a marcar como completada.

Por último, tenemos que prestar especial atención a la cola del controlador de puerto (para SCSI Storport. sys). El controlador de puerto es el último componente de Microsoft para tocar una e/s antes de entregarnos al controlador de minipuerto de dispositivo proporcionado por el fabricante.

Si el controlador de minipuerto de dispositivo no puede aceptar más e/s porque la cola o las colas de hardware que hay debajo de ella están saturadas, comenzaremos a acumular e/s en la cola del controlador de puerto. El tamaño de la cola del controlador de puerto de Microsoft está limitado únicamente por la memoria del sistema (RAM) disponible y puede aumentar considerablemente. Esto provoca una latencia grande medida.

## <a name="high-cpu-caused-by-enumerating-folders"></a>Gran CPU causada por enumerar carpetas 

Para solucionar este problema, deshabilite la característica de enumeración basada en acceso (ABE).

Para determinar qué recursos compartidos de SMB tienen habilitado ABE, ejecute el siguiente comando de PowerShell:

```PowerShell
Get-SmbShare | Select Name, FolderEnumerationMode
```

Unrestricted = ABE deshabilitado. <br />
AccessBase = ABE habilitado.


Puede habilitar ABE en **Administrador del servidor**. Desplácese a **servicios de archivos y almacenamiento** > **recursos compartidos**, haga clic con el botón derecho en el recurso compartido, seleccione **propiedades**, vaya a **configuración** y, después, seleccione **Habilitar enumeración basada en el acceso**.

![Opciones de la interfaz de usuario](media/high-cpu-usage-issue-on-smb-server-2.png)

Además, puede reducir **ABELevel** a un nivel inferior (1 o 2) para mejorar el rendimiento.

Puede comprobar el rendimiento del disco cuando la enumeración es lenta abriendo la carpeta localmente a través de una consola o una sesión de RDP.
