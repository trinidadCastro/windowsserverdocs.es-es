---
title: finger
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42622fdf19cdd50b76d32989769874cbd05e9f4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826946"
---
# <a name="finger"></a>finger

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de los usuarios en un equipo remoto especificado (normalmente un equipo con UNIX) que se está ejecutando el servicio o demonio finger. El equipo remoto especifica el formato y salida de la pantalla de información de usuario. Se utiliza sin parámetros, **dedo** muestra la Ayuda. 
## <a name="syntax"></a>Sintaxis
```
finger [-l] [<User>] [@<Host>] [...]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|-l|Muestra información de usuario en formato de lista larga.|
|<User>|Especifica el usuario sobre el que desea información. Si se omite el *usuario* parámetro, **dedo** muestra información sobre todos los usuarios en el equipo especificado.|
|@<Host>|Especifica el equipo remoto que ejecuta el servicio finger donde desea obtener información de usuario. Puede especificar una dirección IP o nombre de equipo.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
Varios User@Host se pueden especificar parámetros.
Se debe anteponer **dedo** parámetros con un guión (-) en lugar de una barra diagonal (/).
Este comando está disponible sólo si el protocolo del protocolo de Internet (TCP/IP) se instala como un componente en las propiedades de un adaptador de red en las conexiones de red.
Windows Server 2003 no proporciona un servicio del dedo.
## <a name="BKMK_Examples"></a>Ejemplos
Para mostrar información de Usuario1 en el equipo usuarios.Microsoft.com, escriba:
```
finger user1@users.microsoft.com
```
Para mostrar información para todos los usuarios en el equipo usuarios.Microsoft.com, escriba:
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
