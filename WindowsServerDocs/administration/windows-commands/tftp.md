---
title: tftp
description: Transferir archivos a y desde un equipo remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad195409076840fda0e8d6bf5cd0c295a62cdede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845906"
---
# <a name="tftp"></a>tftp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfiere archivos a y desde un equipo remoto, normalmente un equipo con UNIX, que se está ejecutando el servicio de protocolo Trivial de transferencia de archivos (tftp) o demonio. TFTP se suelen usar los dispositivos incrustados o sistemas que recuperan firmware, información de configuración o una imagen de sistema durante el proceso de arranque desde un servidor tftp.   

## <a name="syntax"></a>Sintaxis  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|-i|Especifica el modo de transferencia de imagen binaria (también denominado modo de octeto). En modo binario, el archivo se transfiere en unidades de un byte. Utilice este modo al transferir los archivos binarios. Si **-i** es se omite, el archivo se transfiere en modo ASCII. Este es el modo de transferencia de forma predeterminada. Este modo convierte los caracteres de fin de línea (EOL) en un formato adecuado para el equipo especificado. Utilice este modo al transferir los archivos de texto. Si una transferencia de archivos se realiza correctamente, se muestra la velocidad de transferencia de datos.|  
|\<Host\>|Especifica el equipo local o remoto.|  
|PUT|Transfiere el archivo *origen* en el equipo local al archivo *destino* en el equipo remoto. Dado que el protocolo tftp no admite la autenticación de usuario, el usuario deberá iniciar sesión en el equipo remoto y los archivos deben escribirse en el equipo remoto.|  
|get|Transfiere el archivo *destino* en el equipo remoto en el archivo *origen* en el equipo local.|  
|\<Origen\>|Especifica el archivo para transferir.|  
|\<Destino\>|Especifica dónde se debe transferir el archivo.|  

## <a name="remarks"></a>Comentarios  
-   Puede instalar al cliente de tftp mediante el Asistente de características para agregar.  
-   El protocolo tftp no admite ningún mecanismo de autenticación ni cifrado y por lo tanto, puede suponer un riesgo de seguridad cuando está presente. No se recomienda instalar al cliente de tftp para sistemas conectados a Internet.  
-   El cliente de tftp es software opcional y está marcado como obsoleto en Windows Vista y versiones posteriores del sistema operativo Windows. Microsoft ya no proporciona un servicio de servidor tftp por motivos de seguridad.  

## <a name="BKMK_Examples"></a>Ejemplos  
Copie el archivo **boot.img** desde el equipo remoto **Host1**.  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
