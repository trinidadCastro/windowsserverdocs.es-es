---
title: prnqctl
description: Imprimir una página de prueba, pausar o reanudar una impresora.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 189b344dc0c4f587ba7a6382c481304242e22c74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372033"
---
# <a name="prnqctl"></a>prnqctl

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imprime una página de prueba, pausa o reanuda una impresora y borra una cola de impresión.  

## <a name="syntax"></a>Sintaxis  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>Parámetros  

|Parámetro|Descripción|  
|-------|--------|  
|-z|pausa la impresión en la impresora especificada con el parámetro **-p** .|  
|-m|Reanuda la impresión en la impresora especificada con el parámetro **-p** .|  
|-e|imprime una página de prueba en la impresora especificada con el parámetro **-p** .|  
|-x|Cancela todos los trabajos de impresión en la impresora especificada con el parámetro **-p** .|  
|-s \<ServerName >|Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.|  
|-p \<Nombredeimpresora >|Especifica el nombre de la impresora que desea administrar. Obligatorio.|  
|-u \<nombreDeUsuario >-w \<contraseña >|Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Observaciones  
- El comando **prnqctl** es un script de Visual Basic ubicado en el printing_Admin_Scripts de%windir%\system32\\\<language> directorio. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prnqctl o cambie los directorios a la carpeta correspondiente. Por ejemplo:  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).  

## <a name="BKMK_examples"></a>Example  
Para imprimir una página de prueba en la impresora Laserprinter1 compartida por el equipo \\\Server1, escriba:  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
Para pausar la impresión en la impresora Laserprinter1 en el equipo local, escriba:  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
Para cancelar todos los trabajos de impresión de la impresora Laserprinter1 en el equipo local, escriba:  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

#### <a name="additional-references"></a>referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Referencia de comandos de impresión](print-command-reference.md)  
