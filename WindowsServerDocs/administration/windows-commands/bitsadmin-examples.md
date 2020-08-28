---
title: Ejemplos de bitsadmin
description: Ejemplos que muestran cómo usar la herramienta bitsadmin para realizar las tareas más comunes.
ms.topic: reference
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: e1fcf545efee765b5130c616ddee53b25f03d0d3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030493"
---
# <a name="bitsadmin-examples"></a>Ejemplos de bitsadmin

En los siguientes ejemplos se muestra cómo usar la `bitsadmin` herramienta para realizar las tareas más comunes.

## <a name="transfer-a-file"></a>Transferir un archivo

Para crear un trabajo, agregue archivos, active el trabajo en la cola de transferencia y complete el trabajo:

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

BITSAdmin continúa mostrando información de progreso en la ventana de MS-DOS hasta que se complete la transferencia o se produzca un error.

## <a name="create-a-download-job"></a>Crear un trabajo de descarga

Para crear un trabajo de descarga denominado *myDownloadJob*:

```
bitsadmin /create myDownloadJob
```

BITSAdmin devuelve un GUID que identifica de forma única el trabajo. Use el GUID o el nombre del trabajo en las llamadas posteriores. El siguiente texto es un resultado de ejemplo.

### <a name="sample-output"></a>Salida de ejemplo

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>Agregar archivos al trabajo de descarga

Para agregar un archivo al trabajo:

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

Repita esta llamada para cada archivo que desee agregar. Si varios trabajos usan *myDownloadJob* como nombre, debe usar el GUID del trabajo para identificarlo de forma exclusiva para su finalización.

## <a name="activate-the-download-job"></a>Activar el trabajo de descarga

Después de crear un nuevo trabajo, BITS suspende automáticamente el trabajo. Para activar el trabajo en la cola de transferencia:

```
bitsadmin /resume myDownloadJob
```

Si varios trabajos usan *myDownloadJob* como nombre, debe usar el GUID del trabajo para identificarlo de forma exclusiva para su finalización.

## <a name="determine-the-progress-of-the-download-job"></a>Determinar el progreso del trabajo de descarga

El modificador **/info** devuelve el estado del trabajo y el número de archivos y bytes transferidos. Cuando el estado se muestra como `TRANSFERRED` , significa que bits ha transferido correctamente todos los archivos del trabajo. También puede Agregar el argumento **/verbose** para obtener los detalles completos del trabajo y **/List** o **/monitor** para obtener todos los trabajos de la cola de transferencia.

Para devolver el estado del trabajo:

```
bitsadmin /info myDownloadJob /verbose
```

Si varios trabajos usan *myDownloadJob* como nombre, debe usar el GUID del trabajo para identificarlo de forma exclusiva para su finalización.

## <a name="complete-the-download-job"></a>Completar el trabajo de descarga

Para completar el trabajo después de que el estado cambie a `TRANSFERRED` :

```
bitsadmin /complete myDownloadJob
```

Debe ejecutar el `/complete` modificador antes de que los archivos del trabajo estén disponibles. Si varios trabajos usan *myDownloadJob* como nombre, debe usar el GUID del trabajo para identificarlo de forma exclusiva para su finalización.

## <a name="monitor-jobs-in-the-transfer-queue-using-the-list-switch"></a>Supervisar trabajos en la cola de transferencia mediante el modificador/List

Para devolver el estado del trabajo y el número de archivos y bytes transferidos para todos los trabajos de la cola de transferencia:

```
bitsadmin /list
```

### <a name="sample-output"></a>Salida de ejemplo

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-monitor-switch"></a>Supervisión de trabajos en la cola de transferencia mediante el modificador/monitor

Para devolver el estado del trabajo y el número de archivos y bytes transferidos para todos los trabajos de la cola de transferencia, actualice los datos cada 5 segundos:

```
bitsadmin /monitor
```

> [!NOTE]
> Para detener la actualización, presione CTRL + C.

### <a name="sample-output"></a>Salida de ejemplo

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-info-switch"></a>Supervisar trabajos en la cola de transferencia mediante el modificador/info

Para devolver el estado del trabajo y el número de archivos y bytes transferidos:

```
bitsadmin /info
```

### <a name="sample-output"></a>Salida de ejemplo

```
GUID: {482FCAF0-74BF-469B-8929-5CCD028C9499} DISPLAY: myDownloadJob
TYPE: DOWNLOAD STATE: TRANSIENT_ERROR OWNER: domain\user
PRIORITY: NORMAL FILES: 0 / 1 BYTES: 0 / UNKNOWN
CREATION TIME: 12/17/2002 1:21:17 PM MODIFICATION TIME: 12/17/2002 1:21:30 PM
COMPLETION TIME: UNKNOWN
NOTIFY INTERFACE: UNREGISTERED NOTIFICATION FLAGS: 3
RETRY DELAY: 600 NO PROGRESS TIMEOUT: 1209600 ERROR COUNT: 0
PROXY USAGE: PRECONFIG PROXY LIST: NULL PROXY BYPASS LIST: NULL
ERROR FILE:    https://downloadsrv/10mb.zip -> c:\10mb.zip
ERROR CODE:    0x80072ee7 - The server name or address could not be resolved
ERROR CONTEXT: 0x00000005 - The error occurred while the remote file was being
processed.
DESCRIPTION:
JOB FILES:
0 / UNKNOWN WORKING https://downloadsrv/10mb.zip -> c:\10mb.zip
NOTIFICATION COMMAND LINE: none
```

## <a name="delete-jobs-from-the-transfer-queue"></a>Eliminar trabajos de la cola de transferencia

Para quitar todos los trabajos de la cola de transferencia, use el modificador/RESET:

```
bitsadmin /reset
```

### <a name="sample-output"></a>Salida de ejemplo

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
