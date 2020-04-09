---
title: ejemplos de bitsadmin
description: En los siguientes ejemplos se muestra cómo usar la herramienta bitsadmin para realizar las tareas más comunes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: 96447410f76e4402c456b5ec402cc730480aedaf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850808"
---
# <a name="bitsadmin-examples"></a>ejemplos de bitsadmin

En los siguientes ejemplos se muestra cómo usar la herramienta de `bitsadmin` para realizar las tareas más comunes.

## <a name="transfer-a-file"></a>Transferir un archivo

El modificador **/Transfer** es un acceso directo para realizar las tareas que se enumeran a continuación. Este modificador crea el trabajo, agrega los archivos al trabajo, activa el trabajo en la cola de transferencia y completa el trabajo. BITSAdmin continúa mostrando información de progreso en la ventana de MS-DOS hasta que se complete la transferencia o se produzca un error.

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

## <a name="create-a-download-job"></a>Crear un trabajo de descarga

Use el modificador **/Create** para crear un trabajo de descarga denominado myDownloadJob.

### <a name="syntax"></a>Sintaxis

```
bitsadmin /create myDownloadJob
```

BITSAdmin devuelve un GUID que identifica de forma única el trabajo. Use el GUID o el nombre del trabajo en las llamadas posteriores. El siguiente texto es un resultado de ejemplo.

#### <a name="sample-output"></a>Resultados de ejemplo

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>Agregar archivos al trabajo de descarga

Use el modificador **/AddFile** para agregar un archivo al trabajo. Repita esta llamada para cada archivo que desee agregar.

Si varios trabajos usan myDownloadJob como su nombre, debe reemplazar myDownloadJob por el GUID del trabajo para identificar el trabajo de forma única.

### <a name="syntax"></a>Sintaxis

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

## <a name="activate-the-download-job"></a>Activar el trabajo de descarga

Cuando se crea un nuevo trabajo, BITS suspende el trabajo. Para activar el trabajo en la cola de transferencia, use el modificador **/resume**

Si varios trabajos usan myDownloadJob como su nombre, debe reemplazar myDownloadJob por el GUID del trabajo para identificar el trabajo de forma única.

### <a name="syntax"></a>Sintaxis

`bitsadmin /resume myDownloadJob`

## <a name="determine-the-progress-of-the-download-job"></a>Determinar el progreso del trabajo de descarga

Utilice el modificador **/info** para devolver el estado del trabajo y el número de archivos y bytes transferidos. Cuando se transfiere el estado, BITS ha transferido correctamente todos los archivos del trabajo.

- Use el argumento **/verbose** para obtener los detalles completos del trabajo.

- Use el modificador **/List** o **/monitor** para obtener todos los trabajos de la cola de transferencia.

Si varios trabajos usan myDownloadJob como su nombre, debe reemplazar myDownloadJob por el GUID del trabajo para identificar el trabajo de forma única.

### <a name="syntax"></a>Sintaxis

`bitsadmin /info myDownloadJob /verbose`

#### <a name="sample-output"></a>Resultados de ejemplo

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

## <a name="completing-the-download-job"></a>Finalización del trabajo de descarga

Cuando se transfiere el estado del trabajo, BITS ha transferido correctamente todos los archivos del trabajo. Sin embargo, los archivos no estarán disponibles hasta que use el modificador **/Complete**

Si varios trabajos usan myDownloadJob como su nombre, debe reemplazar myDownloadJob por el GUID del trabajo para identificar el trabajo de forma única.

### <a name="syntax"></a>Sintaxis

`bitsadmin /complete myDownloadJob`

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Supervisión de trabajos en la cola de transferencia

Use el modificador **/List**, **/monitor**o **/info** para supervisar los trabajos de la cola de transferencia. El modificador **/List** proporciona información para todos los trabajos de la cola.

## <a name="list-switch"></a>Switch/List

El modificador **/List** devuelve el estado del trabajo y el número de archivos y bytes transferidos para todos los trabajos de la cola de transferencia.

### <a name="syntax"></a>Sintaxis

`bitsadmin /list`

#### <a name="sample-output-for-the-list-switch"></a>Salida de ejemplo para el modificador/List

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-switch"></a>modificador/monitor

El modificador **/monitor** devuelve el estado del trabajo y el número de archivos y bytes transferidos para todos los trabajos de la cola de transferencia, actualizando los datos cada 5 segundos. Para detener la actualización, presione CTRL + C.

### <a name="syntax"></a>Sintaxis

`bitsadmin /monitor`

#### <a name="sample-output"></a>Resultados de ejemplo

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Eliminar trabajos de la cola de transferencia

Use el modificador **/RESET** para quitar todos los trabajos de la cola de transferencia.

### <a name="syntax"></a>Sintaxis

`bitsadmin /reset`

#### <a name="sample-output"></a>Resultados de ejemplo

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
