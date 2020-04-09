---
title: bitsadmin Transfer
description: Tema de comandos de Windows para la transferencia de bitsadmin, que transfiere uno o varios archivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e960b4d94416d57e6c42ec27057dafef5e44516
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849008"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Transfiere uno o varios archivos. Para transferir más de un archivo, especifique varios \<RemoteFileName\>-\<pares de\> LocalFileName. Los pares están delimitados por espacios.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Name|Nombre del trabajo. A diferencia de la mayoría de los comandos, **Name** solo puede ser un nombre y no un GUID.|
|Tipo|Opcional: especifique el tipo de trabajo. Use **/download** (valor predeterminado) para un trabajo de descarga o **/upload** para un trabajo de carga.|
|Priority|Opcional: establezca el job_priority en uno de los siguientes valores:</br>-FOREGROUND</br>-ALTO</br>-NORMAL</br>-LOW|
|ACLFlags|Opcional: indica que desea mantener la información de propietario y ACL con el archivo que se está descargando. Por ejemplo, para mantener el propietario y el grupo con el archivo, establezca marcas en `OG`. Especifique una o varias de las marcas siguientes:</br>-O: copiar información del propietario con el archivo.</br>-G: copiar información de grupo con el archivo.</br>-D: copiar información de DACL con el archivo.</br>-S: copiar información de SACL con el archivo.|
|/DYNAMIC|Configura el trabajo con [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), lo que relaja los requisitos del lado servidor.|
|RemoteFileName|Nombre del archivo cuando se transfiere al servidor.|
|LocalFileName|Nombre del archivo que reside localmente.|

## <a name="remarks"></a>Comentarios

De forma predeterminada, el servicio BITSAdmin crea un trabajo de descarga que se ejecuta con prioridad **normal** y actualiza la ventana de comandos con información de progreso hasta que se completa la transferencia o hasta que se produce un error crítico. El servicio completa el trabajo si transfiere correctamente todos los archivos y cancela el trabajo si se produce un error crítico. El servicio no crea el trabajo si no puede Agregar archivos al trabajo o si especifica un valor no válido para el *tipo* o *Job_Priority*. Para transferir más de un archivo, especifique varios pares de *RemoteFileName*-*LocalFileName* . Los pares están delimitados por espacios.

> [!NOTE]
> El comando BITSAdmin continúa ejecutándose si se produce un error transitorio. Para finalizar el comando, presione CTRL + C.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se inicia un trabajo de transferencia con el nombre *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)