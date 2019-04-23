---
title: pubprn
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 499ff2ade7ffc6c608791ba3da0ede0c3282c13d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831706"
---
# <a name="pubprn"></a>pubprn

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publica una impresora en los servicios de dominio de active directory.

## <a name="syntax"></a>Sintaxis
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|\<ServerName>|Especifica el nombre del servidor de Windows que hospeda la impresora que desea publicar. Si no especifica un equipo, se usa el equipo local.|
|\<UNCprinterpath>|La ruta de acceso de convención de nomenclatura Universal (UNC) a una impresora compartida que desee publicar.|
|"LDAP://CN=<Container>, DC =<Container>"|Especifica la ruta de acceso al contenedor de servicios de dominio de Active Directory donde desea publicar la impresora.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   El **pubprn** comando es un script de Visual Basic que se encuentra en la %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando, en un símbolo del sistema, escriba **cscript** seguido por la ruta de acceso completa al archivo pubprn o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Ejemplos
Para publicar todas las impresoras en el \\\Server1 equipo en el contenedor MyContainer en el dominio MyDomain.company.Com, escriba:
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
Para publicar la impresora Laserprinter1 en el \\\Server1 server en el contenedor MyContainer en el dominio MyDomain.company.Com, tipo:
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[referencia de comandos de impresión](print-command-reference.md)
