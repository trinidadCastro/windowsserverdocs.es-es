---
title: Bitsadmin setsecurityflags
description: Tema de los comandos de Windows para **setsecurityflags bitsadmin** -conjuntos de indicadores para HTTP que determinan si BITS deben comprobar la lista de revocación de certificados, pasar por alto algunos errores de certificado y definir la directiva para usar cuando un servidor redirige la solicitud HTTP.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a7f74146a26314ddb4fa92f85e5a40267d0f0d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858626"
---
# <a name="bitsadmin-setsecurityflags"></a>Bitsadmin setsecurityflags



Establece las marcas de HTTP que determinan si BITS deben comprobar la lista de revocación de certificados, pasar por alto algunos errores de certificado y definir la directiva que se usará cuando un servidor redirige la solicitud HTTP. El valor es un entero sin signo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Valor|Vea la sección Comentarios|

## <a name="remarks"></a>Comentarios

El **valor** parámetro puede contener una o varias de las siguientes marcas de notificación.

|Acción|Representación binaria|
|------|---------------------|
|Habilitar la comprobación de CRL|Establecer el bit menos significativo|
|Omitir nombre común no válido en el certificado de servidor|Establecer el bit 2 de la derecha|
|Omitir fecha no válida en el certificado de servidor|Establezca el bit 3 de la derecha|
|Omitir la entidad de certificación no válido en el certificado de servidor|Establecer el bit 4 desde la derecha|
|Ignorar uso no válido del certificado|Establecer el bit 5 de la derecha|
|Directiva de redirección|Controla los bits del 9 al 11 de la derecha</br>0,0,0 - redirecciones se podrá automáticamente.</br>0,0,1 - nombre remoto con la interfaz IBackgroundCopyFile se actualizará si se produce una redirección.</br>0,1,0 - BITS generará un error del trabajo si se produce una redirección.|
|Permitir redireccionamiento de HTTPS a HTTP|Establecer el bit 12 desde la derecha|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente establece las marcas de seguridad para habilitar la comprobación CRL del trabajo denominado *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)