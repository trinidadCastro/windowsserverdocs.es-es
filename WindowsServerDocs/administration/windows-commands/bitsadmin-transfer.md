---
title: bitsadmin Transfer
description: Tema de los comandos de Windows para **bitsadmin transferencia** -transfiere uno o varios archivos.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2ef29a242a834fae42d1de3994a82aedcf87ec2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852466"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Transfiere uno o varios archivos. Para transferir varios archivos, especificar varios \<RemoteFileName\>-\<LocalFileName\> pares. Los pares están delimitados por espacios.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Name|El nombre del trabajo. A diferencia de la mayoría de los comandos **nombre** sólo puede ser un nombre y no es un GUID.|
|Tipo|Opcional: especifique el tipo de trabajo. Use **/descargar** (valor predeterminado) para un trabajo de descarga o **/cargar** para un trabajo de carga.|
|Prioridad|Opcional: establecer el job_priority en uno de los siguientes valores:</br>: PRIMER PLANO</br>-ALTA</br>: NORMAL</br>-LOW|
|ACLFlags|Opcional: indica que desea mantener la información de la ACL y el propietario con el archivo que se va a descargar. Por ejemplo, para mantener el propietario y el grupo con el archivo, establecer marcas `OG`. Especifique uno o varios de los siguientes indicadores:</br>-   O: Copiar información de propietario con el archivo.</br>-   G: Copiar información de grupo con el archivo.</br>-D: Copiar información de la DACL con el archivo.</br>-S: Copiar información de la SACL con archivo.|
|\/DYNAMIC|Configura el trabajo con [ **BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), lo que reduce los requisitos de servidor.|
|RemoteFileName|El nombre del archivo cuando se transfieren al servidor.|
|LocalFileName|El nombre del archivo que reside localmente.|

## <a name="remarks"></a>Comentarios

De forma predeterminada, el servicio BITSAdmin crea un trabajo de descarga que se ejecuta en **NORMAL** prioridad y actualiza la ventana de comandos con la información de progreso hasta que se complete la transferencia o hasta que se produce un error crítico. El servicio completa el trabajo si transfiere todos los archivos correctamente y se cancela el trabajo si se produce un error crítico. El servicio no crea el trabajo si no puede agregar archivos al trabajo o si se especifica un valor no válido para *tipo* o *Job_Priority*. Para transferir varios archivos, especificar varios *RemoteFileName*-*LocalFileName* pares. Los pares están delimitados por espacios.

> [!NOTE]
> El comando BITSAdmin continúa ejecutándose si se produce un error transitorio. Para finalizar el comando, presione CTRL+C.

## <a name="BKMK_examples"></a>Ejemplos

El siguiente ejemplo se inician con una transferencia del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)