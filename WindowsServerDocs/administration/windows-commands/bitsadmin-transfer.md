---
title: bitsadmin Transfer
description: 'Temas de comandos de Windows para la **transferencia de bitsadmin** : transfiere uno o varios archivos.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a12e6e2023c979d5b0c095c1eddd77eb5155d1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380355"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Transfiere uno o varios archivos. Para transferir más de un archivo, especifique varios \<RemoteFileName @ no__t-1 @ no__t-2 @ no__t-3LocalFileName @ no__t-4 pares. Los pares están delimitados por espacios.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Nombre|Nombre del trabajo. A diferencia de la mayoría de los comandos, **Name** solo puede ser un nombre y no un GUID.|
|Tipo|Opcional: especifique el tipo de trabajo. Use **/download** (valor predeterminado) para un trabajo de descarga o **/upload** para un trabajo de carga.|
|Prioridad|Opcional: establezca job_priority en uno de los siguientes valores:</br>-FOREGROUND</br>-ALTO</br>-NORMAL</br>-LOW|
|ACLFlags|Opcional: indica que desea mantener la información de propietario y ACL con el archivo que se está descargando. Por ejemplo, para mantener el propietario y el grupo con el archivo, establezca Flags en `OG`. Especifique una o varias de las marcas siguientes:</br>ACEPTAR Copie la información del propietario con el archivo.</br>I Copiar información de grupo con el archivo.</br>D Copiar información de DACL con el archivo.</br>SEG Copie la información de SACL con el archivo.|
|@NO__T 0DYNAMIC|Configura el trabajo con [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), lo que relaja los requisitos del lado servidor.|
|RemoteFileName|Nombre del archivo cuando se transfiere al servidor.|
|LocalFileName|Nombre del archivo que reside localmente.|

## <a name="remarks"></a>Comentarios

De forma predeterminada, el servicio BITSAdmin crea un trabajo de descarga que se ejecuta con prioridad **normal** y actualiza la ventana de comandos con información de progreso hasta que se completa la transferencia o hasta que se produce un error crítico. El servicio completa el trabajo si transfiere correctamente todos los archivos y cancela el trabajo si se produce un error crítico. El servicio no crea el trabajo si no puede Agregar archivos al trabajo o si especifica un valor no válido para el *tipo* o *Job_Priority*. Para transferir más de un archivo, especifique varios pares *RemoteFileName*-*LocalFileName* . Los pares están delimitados por espacios.

> [!NOTE]
> El comando BITSAdmin continúa ejecutándose si se produce un error transitorio. Para finalizar el comando, presione CTRL + C.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se inicia un trabajo de transferencia con el nombre *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)