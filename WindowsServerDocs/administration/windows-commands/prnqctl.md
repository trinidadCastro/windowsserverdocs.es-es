---
title: prnqctl
description: Imprimir una página de prueba, pausar o reanudar una impresora.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d07d8caa0568b26f5edc16258085a59ecdafcf4e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837208"
---
# <a name="prnqctl"></a>prnqctl

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

imprime una página de prueba, pausa o reanuda una impresora y borra una cola de impresión.  

## <a name="syntax"></a>Sintaxis  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
### <a name="parameters"></a>Parámetros  

|Parámetro|Descripción|  
|-------|--------|  
|-z|pausa la impresión en la impresora especificada con el parámetro **-p** .|  
|-m|Reanuda la impresión en la impresora especificada con el parámetro **-p** .|  
|-e|imprime una página de prueba en la impresora especificada con el parámetro **-p** .|  
|-x|Cancela todos los trabajos de impresión en la impresora especificada con el parámetro **-p** .|  
|-s \<ServerName >|Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.|  
|-p \<Nombredeimpresora >|Especifica el nombre de la impresora que desea administrar. Obligatorio.|  
|-u \<nombreDeUsuario >-w \<contraseña >|Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione.|  
|/?|Muestra la Ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
- El comando **prnqctl** es un script de Visual Basic ubicado en el printing_Admin_Scripts de%windir%\system32\\\<language> directorio. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prnqctl o cambie los directorios a la carpeta correspondiente. Por ejemplo:  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).  

## <a name="examples"></a><a name="BKMK_examples"></a>Example  
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

## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Referencia de comandos de impresión](print-command-reference.md)  
