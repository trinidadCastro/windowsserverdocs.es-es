---
title: reset session
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: b83aa67e0d90be9ddc679b32c8ffa4270efec41f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835798"
---
# <a name="reset-session"></a>reset session

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite restablecer (eliminar) una sesión en un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto).  
para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).  

> [!NOTE]  
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.  

## <a name="syntax"></a>Sintaxis  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

### <a name="parameters"></a>Parámetros  

|Parámetro|Descripción|  
|-------|--------|  
|\<nombresesión >|Especifica el nombre de la sesión que desea restablecer. Para determinar el nombre de la sesión, use el comando **query Session** .|  
|\<SessionID >|Especifica el identificador de la sesión que se va a restablecer.|  
|/Server:\<ServerName >|Especifica el servidor de Terminal Server que contiene la sesión que desea restablecer. De lo contrario, se usa el servidor host de sesión de escritorio remoto actual.|  
|/v|Muestra información acerca de las acciones que se llevan a cabo.|  
|/?|Muestra la Ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   Siempre puede restablecer sus propias sesiones, pero debe tener permiso de acceso de control total para restablecer la sesión de otro usuario.  
-   Tenga en cuenta que el restablecimiento de una sesión de usuario sin advertir al usuario puede provocar la pérdida de datos en la sesión.  
-   Debe restablecer una sesión sólo si no funciona correctamente o ha dejado de responder.  
-   El parámetro **/Server** solo es necesario si usa **restablecer sesión** desde un servidor remoto.  

## <a name="examples"></a><a name=BKMK_examples></a>Example  
- Para restablecer la sesión designada como RDP-TCP # 6, escriba:  
  ```  
  reset session rdp-tcp#6  
  ```  
- Para restablecer la sesión que utiliza el ID. de sesión 3, escriba:  
  ```  
  reset session 3  
  ```  

## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)  
