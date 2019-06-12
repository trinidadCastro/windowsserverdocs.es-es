---
title: reset session
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5a0991c76ba890bb94b0dcf258df6207ed228e72
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441797"
---
# <a name="reset-session"></a>reset session

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite restablecer (eliminar) una sesión en un servidor Host de sesión de escritorio remoto (Host de sesión de rd).  
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).  

> [!NOTE]  
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.  

## <a name="syntax"></a>Sintaxis  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>Parámetros  

|Parámetro|Descripción|  
|-------|--------|  
|\<SessionName>|Especifica el nombre de la sesión que desea restablecer. Para determinar el nombre de la sesión, utilice el **consultar sesión** comando.|  
|\<SessionID>|Especifica el identificador de la sesión para restablecer.|  
|/ Server:\<ServerName >|Especifica el servidor de terminal server que contiene la sesión que desea restablecer. En caso contrario, se usa el servidor Host de sesión de escritorio remoto actual.|  
|/v|Muestra información sobre las acciones que se va a realizar.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   Siempre puede restablecer sus propias sesiones, pero debe tener permiso de acceso de Control total para restablecer la sesión de otro usuario.  
-   Tenga en cuenta que restablecer una sesión de usuario sin advertir al usuario puede causar la pérdida de datos en la sesión.  
-   Debe restablecer una sesión solo cuando no funciona correctamente o ha dejado de responder.  
-   El **/Server** parámetro es obligatorio solo si usa **restablecer sesión** desde un servidor remoto.  

## <a name="BKMK_examples"></a>Ejemplos  
- Para restablecer la sesión designada rdp-tcp #6, escriba:  
  ```  
  reset session rdp-tcp#6  
  ```  
- Para restablecer la sesión que utiliza 3 Id. de sesión, escriba:  
  ```  
  reset session 3  
  ```  

#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)  
