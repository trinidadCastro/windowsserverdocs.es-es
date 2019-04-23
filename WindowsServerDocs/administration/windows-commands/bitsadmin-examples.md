---
title: ejemplos de Bitsadmin
description: Los ejemplos siguientes muestran cómo usar la herramienta bitsadmin para realizar las tareas más comunes.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: 4b9deb4fc7fbfccf569250e965274009764054f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849336"
---
# <a name="bitsadmin-examples"></a>ejemplos de Bitsadmin

Los ejemplos siguientes muestran cómo usar el `bitsadmin` herramienta para realizar las tareas más comunes.

## <a name="transfer-a-file"></a>Transferir un archivo

El **/transferencia** conmutador es un acceso directo para llevar a cabo las tareas enumeradas a continuación. Este modificador crea el trabajo, agrega los archivos al trabajo, activa el trabajo en la cola de transferencia y completa el trabajo. BITSAdmin seguirá mostrando información de progreso en la ventana de MS-DOS hasta que se complete la transferencia o se produce un error.

**/transfer de Bitsadmin myDownloadJob /download/normal Priority https://downloadsrv/10mb.zip c:\\10mb.zip**

## <a name="create-a-download-job"></a>Crear un trabajo de descarga

Use la **/ crear** modificador para crear un trabajo de descarga denominado myDownloadJob.

**bitsadmin /create myDownloadJob**

BITSAdmin devuelve un GUID que identifica el trabajo. Use el nombre de trabajo o el GUID en las llamadas subsiguientes. El siguiente texto es la salida de ejemplo.

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

A continuación, use el **/addfile** switch para agregar uno o varios archivos para el trabajo de descarga.

## <a name="add-files-to-the-download-job"></a>Agregar archivos a la tarea de descarga

Use la **/addfile** switch para agregar un archivo para el trabajo. Repita esta llamada para cada archivo que desea agregar. Si varios trabajos usa myDownloadJob como su nombre, debe reemplazar myDownloadJob con el GUID del trabajo para identificar de forma única el trabajo.

**Bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip**

Para activar el trabajo en la cola de transferencia, utilice el **/reanudar** cambie.

## <a name="activate-the-download-job"></a>Activar el trabajo de descarga

Cuando se crea un nuevo trabajo, BITS suspende el trabajo. Para activar el trabajo en la cola de transferencia, utilice el **/reanudar** cambie. Si varios trabajos usa myDownloadJob como su nombre, debe reemplazar myDownloadJob con el GUID del trabajo para identificar de forma única el trabajo.

**bitsadmin /resume myDownloadJob**

Para determinar el progreso del trabajo, use el **/lista**, **/info**, o **/monitor** cambie.

## <a name="determine-the-progress-of-the-download-job"></a>Determinar el progreso del trabajo de descarga

Use la **/info** modificador para determinar el progreso de un trabajo. Si varios trabajos usa myDownloadJob como su nombre, debe reemplazar myDownloadJob con el GUID del trabajo para identificar de forma única el trabajo.

**bitsadmin /info myDownloadJob /verbose**

El **/info** switch devuelve el estado del trabajo y el número de bytes transferidos y de archivos. Cuando se TRANSFIERE el estado, BITS le ha transferido correctamente todos los archivos en el trabajo. El **/ verbose** argumento proporciona detalles completos del trabajo. El siguiente texto es la salida de ejemplo.

``` syntax
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

Para recibir información de todos los trabajos en la cola de transferencia, utilice el **/lista** o **/monitor** cambie.

## <a name="completing-the-download-job"></a>Completar el trabajo de descarga

Cuando se TRANSFIERE el estado del trabajo, BITS le ha transferido correctamente todos los archivos en el trabajo. Sin embargo, los archivos no están disponibles hasta que se utiliza el **/ completa** cambie. Si varios trabajos usa myDownloadJob como su nombre, debe reemplazar myDownloadJob con el GUID del trabajo para identificar de forma única el trabajo.

**bitsadmin /complete myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Supervisión de trabajos en la cola de transferencia

Use la **/lista**, **/monitor**, o **/info** conmutador para supervisar trabajos en la cola de transferencia. El **/lista** conmutador proporciona información para todos los trabajos en la cola.

**bitsadmin /list**

El **/lista** switch devuelve el estado del trabajo y el número de bytes transferidos por todos los trabajos en la cola de transferencia y de archivos. El siguiente texto es la salida de ejemplo.

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

Use la **/monitor** conmutador para supervisar todos los trabajos en la cola. El **/monitor** conmutador actualiza los datos cada 5 segundos. Para detener la actualización, escriba CTRL+C.

**bitsadmin /monitor**

El **/monitor** switch devuelve el estado del trabajo y el número de bytes transferidos por todos los trabajos en la cola de transferencia y de archivos. El siguiente texto es la salida de ejemplo.

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Eliminación de la cola de transferencia de trabajos

Use la **/restablecer** conmutador para quitar todos los trabajos de la cola de transferencia.

**bitsadmin /reset**

El siguiente texto es la salida de ejemplo.

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
