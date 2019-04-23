---
title: prnqctl
description: Imprimir una página de prueba, pausar o reanudar una impresora.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 26af9527b7b16b42fd9d389f3409143dfc3e9aa9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858086"
---
# <a name="prnqctl"></a>prnqctl

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imprime una página de prueba, se detiene o reanuda una impresora y borra una cola de impresión.  
  
## <a name="syntax"></a>Sintaxis  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|-z|detiene la impresión en la impresora especificada con el **-p** parámetro.|  
|-m|Reanuda la impresión en la impresora especificada con el **-p** parámetro.|  
|-e|imprime una página de prueba en la impresora especificada con el **-p** parámetro.|  
|-x|Cancela todos los trabajos de impresión de la impresora especificada con el **-p** parámetro.|  
|-s \<ServerName >|Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.|  
|-p \<nombreImpresora >|Especifica el nombre de la impresora que desea administrar. Obligatorio.|  
|-u \<UserName > -w \<contraseña >|Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores local del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe haber iniciado sesión con una cuenta con estos permisos para que funcione el comando.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   El **prnqctl** comando es un script de Visual Basic que se encuentra en la %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando, en un símbolo del sistema, escriba **cscript** seguido por la ruta de acceso completa al archivo prnqctl o cambie los directorios a la carpeta correspondiente. Por ejemplo:  
    ```  
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
    ```  
-   Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).  

## <a name="BKMK_examples"></a>Ejemplos  
Para imprimir una página de prueba en la impresora Laserprinter1 compartida por la \\\Server1 equipo, escriba:  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
Para pausar la impresión de la impresora Laserprinter1 en el equipo local, escriba:  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
Para cancelar todos los trabajos de impresión de la impresora Laserprinter1 en el equipo local, escriba:  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Referencia de comandos de impresión](print-command-reference.md)  
