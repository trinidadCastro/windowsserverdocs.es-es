---
title: pubprn
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 844a13c1a650ebedcc0d5b4fbf65b9de671b2180
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372015"
---
# <a name="pubprn"></a>pubprn

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publica una impresora en Active Directory Domain Services.

## <a name="syntax"></a>Sintaxis
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|\<ServerName >|Especifica el nombre del servidor de Windows que hospeda la impresora que desea publicar. Si no especifica un equipo, se usa el equipo local.|
|\<UNCprinterpath >|La ruta de acceso de la Convención de nomenclatura universal (UNC) a la impresora compartida que desea publicar.|
|"LDAP://CN =<Container>, DC =<Container>"|Especifica la ruta de acceso al contenedor en servicios de dominio de Active Directory donde desea publicar la impresora.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
-   El comando **Pubprn** es un script de Visual Basic ubicado en el printing_Admin_Scripts%windir%\system32\\\<language> directorio. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo Pubprn o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Example
Para publicar todas las impresoras del equipo \\\Server1 en el contenedor de contención del dominio MyDomain.company.Com, escriba:
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
Para publicar la impresora Laserprinter1 en el servidor de la \\\Server1 en el contenedor de contención del dominio MyDomain.company.Com, escriba:
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[Referencia del comando Print](print-command-reference.md)
