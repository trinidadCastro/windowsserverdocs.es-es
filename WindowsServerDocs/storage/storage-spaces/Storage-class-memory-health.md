---
ms.assetid: 2bab6bf6-90e7-46a7-b917-14a7a8f55366
title: Administración de estado de la memoria de clase de almacenamiento (NVDIMM-N) en Windows
ms.author: jgerend
manager: dongill
ms.topic: article
author: JasonGerend
ms.date: 06/25/2019
ms.localizationpriority: medium
ms.openlocfilehash: b97263c0cc1fefd71eebd6eb4d7b66ca66741a04
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865934"
---
# <a name="storage-class-memory-nvdimm-n-health-management-in-windows"></a>Administración de estado de la memoria de clase de almacenamiento (NVDIMM-N) en Windows

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows 10

En este artículo se ofrece información sobre el control de errores y la administración de estados específicos de dispositivos de memoria de clase de almacenamiento (NVDIMM-N) en Windows destinada a administradores de sistemas y profesionales de TI, destacando las diferencias entre la memoria de clase de almacenamiento y los dispositivos de almacenamiento tradicionales.

Si no está familiarizado con la compatibilidad de Windows con dispositivos de memoria de clase de almacenamiento, estos breves vídeos proporcionan información general:
- [Uso de memoria no volátil (NVDIMM-N) como almacenamiento en bloque en Windows Server 2016](https://channel9.msdn.com/Events/Build/2016/P466)
- [Uso de memoria no volátil (NVDIMM-N) como almacenamiento Byte-Addressable en Windows Server 2016](https://channel9.msdn.com/Events/Build/2016/P470)
- [Aceleración del rendimiento de SQL Server 2016 con memoria persistente en Windows Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-Windows-Server-2016-SCM--FAST)

Vea también [comprender e implementar la memoria persistente en espacios de almacenamiento directo](deploy-pmem.md).

Los dispositivos de memoria de clase de almacenamiento NVDIMM-N compatibles con JEDEC se admiten en Windows con controladores nativos, a partir de Windows Server 2016 y Windows 10 (versión 1607). Aunque estos dispositivos se comportan de manera similar a otros discos (HDD y SSD), existen algunas diferencias.

Todas las condiciones que se muestran aquí se esperan que ocurran muy rara vez, aunque esto depende de las condiciones en las que se utilice el hardware.

Los distintos casos siguientes pueden hacer referencia a las configuraciones de Espacios de almacenamiento. La configuración específica de interés es aquella en la que se utilizan dos dispositivos NVDIMM-N como una caché con reescritura reflejada en un espacio de almacenamiento. Para establecer este tipo de configuración, consulte [Configuración de espacios de almacenamiento con una caché con reescritura de NVDIMM-N](/sql/relational-databases/performance/configuring-storage-spaces-with-a-nvdimm-n-write-back-cache).

En Windows Server 2016, la GUI de espacios de almacenamiento muestra el tipo de bus NVDIMM-N como desconocido. No tiene ninguna pérdida fuctionality o imposibilidad de crear el grupo, el almacenamiento VD. Puede comprobar el tipo de bus ejecutando el comando siguiente:

```powershell
PS C:\>Get-PhysicalDisk | fl
```
El parámetro BusType en la salida del cmdlet mostrará correctamente el tipo de bus como "SCM"

## <a name="checking-the-health-of-storage-class-memory"></a>Comprobación del estado de la memoria de la clase de almacenamiento
Para consultar el estado de la memoria de la clase de almacenamiento, utilice los siguientes comandos en una sesión de Windows PowerShell.

```powershell
PS C:\> Get-PhysicalDisk | where BusType -eq "SCM" | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails
```

Al hacerlo, se obtiene este resultado de ejemplo:

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | Healthy | Aceptar | |
| 802c-01-1602-117cb64f | Advertencia | Error predictivo | {Umbral superado,Error NVDIMM\_N} |

> [!NOTE]
> Para buscar la ubicación física de un dispositivo NVDIMM-N especificado en un evento, en la pestaña **detalles** del evento en visor de eventos, vaya a ubicación de **EventData**  >  **Location**. Tenga en cuenta que Windows Server 2016 enumera los dispositivos de ubicación NVDIMM-N incorrectos, pero esto se corrigió en Windows Server, versión 1709.

Para obtener ayuda sobre las distintas condiciones de estado, vea las secciones siguientes.

## <a name="warning-health-status"></a>Estado de mantenimiento de "ADVERTENCIA"

Esta situación se produce cuando comprueba el estado de un dispositivo de memoria de clase de almacenamiento y ve que su estado de mantenimiento aparece como **Advertencia**, como se muestra en este resultado de ejemplo:

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | Healthy | Aceptar | |
| 802c-01-1602-117cb64f | Advertencia | Error predictivo | {Umbral superado,Error NVDIMM\_N} |

La tabla siguiente muestra información acerca de esta condición.

| Dirección | Descripción |
| --- | --- |
| Condición probable | Infracción del umbral de advertencia de NVDIMM-N |
| Causa principal | Los dispositivos NVDIMM-N supervisan varios umbrales, como la temperatura, la duración de NVM y la duración de la fuente de energía. Cuando se supera uno de estos umbrales, se envía una notificación al sistema operativo. |
| Comportamiento general | El dispositivo sigue siendo totalmente operativo. Se trata de una advertencia, no de un error. |
| Comportamiento de Espacios de almacenamiento | El dispositivo sigue siendo totalmente operativo. Se trata de una advertencia, no de un error. |
| Más información | Campo OperationalStatus del objeto PhysicalDisk. Registro de eventos: Microsoft-Windows-ScmDisk0101/Operational |
| Qué hacer | Dependiendo de la infracción del umbral de advertencia, puede ser recomendable considerar el reemplazo de la totalidad de NVDIMM-N o de determinadas partes. Por ejemplo, si se supera el umbral de duración NVM, puede ser buena idea el reemplazo de NVDIMM-N. |

## <a name="writes-to-an-nvdimm-n-fail"></a>Error en la escritura de NVDIMM-N

Esta situación se da al comprobar el estado de un dispositivo de memoria de clase de almacenamiento y ver que el estado de mantenimiento aparece como **Incorrecto**, y el estado operativo es de tipo **Error de E/S**, como se muestra en este resultado de ejemplo:

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | Healthy | Aceptar | |
| 802c-01-1602-117cb64f | Unhealthy (Incorrecto) | {Metadatos obsoletas, Error de E/S, Error transitorio} | {Pérdida de persistencia de datos, Pérdida de datos, NV...} |

La tabla siguiente muestra información acerca de esta condición.

| Dirección | Descripción |
| --- | --- |
| Condición probable | Pérdida de persistencia / alimentación de copia de seguridad |
|Causa principal|Los dispositivos NVDIMM-N dependen de una fuente de alimentación de copia de seguridad para su persistencia (generalmente una batería o supercondensador). Si esta fuente de alimentación de copia de seguridad no está disponible o el dispositivo no puede realizar una copia de seguridad por algún motivo (error de controlador/flash), los datos están en peligro y Windows impedirá escrituras adicionales en los dispositivos afectados. Las lecturas siguen siendo posibles para evacuar datos.|
|Comportamiento general|El volumen NTFS se desmontará.<br>El campo Estado de mantenimiento de DiscoFísico mostrará "incorrecto" en todos los dispositivos NVDIMM-N afectados.|
|Comportamiento de Espacios de almacenamiento|Espacios de almacenamiento seguirá funcionando si solo hay un NVDIMM-N afectado. Si se ven afectados varios dispositivos, se producirá un error de escritura en el Espacio de almacenamiento. <br>El campo Estado de mantenimiento de DiscoFísico mostrará "incorrecto" en todos los dispositivos NVDIMM-N afectados.|
|Más información|Campo OperationalStatus del objeto PhysicalDisk.<br>Registro de eventos: Microsoft-Windows-ScmDisk0101/Operational|
|Qué hacer|Se recomienda realizar copias de seguridad de los datos de NVDIMM-N afectados. Para obtener acceso de lectura, puede conectar manualmente el disco (aparecerá como un volumen NTFS de solo lectura).<br><br>Para borrar completamente esta condición, se debe resolver la causa principal (es decir, reparar la fuente de alimentación o sustituir NVDIMM-N, según el problema) y el volumen de NVDIMM-N debe desconectarse y conectarse de nuevo, o se debe reiniciar el sistema.<br><br>Para permitir el uso de NVDIMM-N en Espacios de almacenamiento de nuevo, use el cmdlet **Reset-PhysicalDisk**, que vuelve a integrar el dispositivo e inicia el proceso de reparación.|

## <a name="nvdimm-n-is-shown-with-a-capacity-of-0-bytes-or-as-a-generic-physical-disk"></a>NVDIMM-N se muestra con una capacidad de ' 0 ' bytes o como un "disco físico genérico"

Esta condición se da cuando un dispositivo de memoria de clase de almacenamiento se muestra con una capacidad de 0 bytes y no se puede inicializar, o se expone como un objeto de "Disco físico genérico" con un estado operativo de **Pérdida de comunicación**, como se muestra en este resultado de ejemplo:

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
|802c-01-1602-117cb5fc|Healthy|Aceptar||
||Advertencia|Pérdida de comunicación||

La tabla siguiente muestra información acerca de esta condición.

|Dirección|Descripción|
|---|---|
|Condición probable|BIOS no expuso NVDIMM-N al sistema operativo|
|Causa principal|Los dispositivos NVDIMM-N se basan en DRAM. Cuando se hace referencia a una dirección DRAM dañada, la mayoría de CPU se iniciarán en una comprobación de la máquina y reiniciarán el servidor. Algunas plataformas de servidor entonces eliminan la asignación de NVDIMM, evitando que el sistema operativo tenga acceso a ella y causando potencialmente otra comprobación de máquina. Esto también puede ocurrir si el BIOS detecta que se ha producido un error en NVDIMM-N y necesita reemplazarse.|
|Comportamiento general|NVDIMM-N se muestra como no inicializado, con una capacidad de 0 bytes y no se puede leer o escribir.|
|Comportamiento de Espacios de almacenamiento|Espacios de almacenamiento permanece en funcionamiento (siempre que se vea afectado solo un NVDIMM-N).<br>El objeto PhysicalDisk de NVDIMM-N aparece con un estado de mantenimiento de Advertencia y como "Disco físico general"|
|Más información|Campo OperationalStatus del objeto PhysicalDisk. <br>Registro de eventos: Microsoft-Windows-ScmDisk0101/Operational|
|Qué hacer|El dispositivo NVDIMM-N debe reemplazarse o sanearse, de modo que la plataforma de servidor lo exponga al sistema operativo host de nuevo. Se recomienda la sustitución del dispositivo, ya que pueden producirse otros errores que no se pueden corregir. Se puede conseguir la incorporación de un dispositivo de reemplazo para una configuración de Espacios de almacenamiento con el cmdlet **Add-Physicaldisk**.|

## <a name="nvdimm-n-is-shown-as-a-raw-or-empty-disk-after-a-reboot"></a>NVDIMM-N aparece como disco sin procesar o vacío tras un reinicio

Esta situación se da al comprobar el estado de un dispositivo de memoria de clase de almacenamiento y ve que el estado de mantenimiento aparece como **Incorrecto**, y el estado operativo es de tipo **Metadatos no reconocidos**, como se muestra en este resultado de ejemplo:

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
|802c-01-1602-117cb5fc|Healthy|Aceptar|{Desconocido}|
|802c-01-1602-117cb64f|Unhealthy (Incorrecto)|{Metadatos no reconocidos, metadatos obsoletos}|{Desconocido}|

La tabla siguiente muestra información acerca de esta condición.

|Dirección|Descripción|
|---|---|
|Condición probable|Error de copia de seguridad y restauración|
|Causa principal|Un error en el procedimiento de copia de seguridad o restauración probablemente tendrá como resultado que se pierdan todos los datos en NVDIMM-N. Cuando se carga el sistema operativo, aparecerá como un nuevo NVDIMM-N sin una partición o sistema de archivos y la superficie como sin formato, lo que significa que no tiene un sistema de archivos.|
|Comportamiento general|NVDIMM-N estará en modo de solo lectura. Se requiere una acción explícita del usuario para empezar a utilizarlo de nuevo.|
|Comportamiento de Espacios de almacenamiento|Espacios de almacenamiento permanece operativo si solo se ve afectado un NVDIMM.<br>El objeto de disco físico NVDIMM-N se mostrará con el estado de mantenimiento "incorrecto" y no lo usan los espacios de almacenamiento.|
|Más información|Campo OperationalStatus del objeto PhysicalDisk.<br>Registro de eventos: Microsoft-Windows-ScmDisk0101/Operational|
|Qué hacer|Si el usuario no desea reemplazar el dispositivo afectado, puede utilizar el cmdlet **Reset-PhysicalDisk** para borrar la condición de solo lectura en el NVDIMM-N afectado. En entornos de Espacios de almacenamiento, también intentará volver a integrar el NVDIMM-N en el Espacio de almacenamiento e iniciar el proceso de reparación.|

## <a name="interleaved-sets"></a>Conjuntos de intercalado

A menudo pueden crearse conjuntos de intercalado en el BIOS de una plataforma para que varios dispositivos NVDIMM-N aparezcan como uno solo en el sistema operativo host.

Windows Server 2016 y Windows 10 Anniversary Edition no admiten conjuntos de intercalados de NVDIMM-N.

En el momento de redactar este artículo, no hay ningún mecanismo para el sistema operativo de host que permita identificar correctamente los NVDIMM-N individuales en un conjunto de este tipo y comunicar claramente al usuario qué dispositivo concreto ha provocado un error o necesita mantenimiento.
