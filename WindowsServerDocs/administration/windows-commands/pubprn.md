---
title: pubprn
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17ca9e98ef9e3423521b03c5c21be4b3f1538b62
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722779"
---
# <a name="pubprn"></a>pubprn

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Publica una impresora en Active Directory Domain Services.

## <a name="syntax"></a>Sintaxis
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
LDAP://CN=<Container>,DC=<Container>
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|\<NombreDeServidor>|Especifica el nombre del servidor de Windows que hospeda la impresora que desea publicar. Si no especifica un equipo, se usa el equipo local.|
|\<> UNCprinterpath|La ruta de acceso de la Convención de nomenclatura universal (UNC) a la impresora compartida que desea publicar.|
|LDAP://CN =<Container>, DC =<Container>|Especifica la ruta de acceso al contenedor en servicios de dominio de Active Directory donde desea publicar la impresora.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
-   El comando **Pubprn** es un script de Visual Basic ubicado en el directorio\\ <language> de printing_Admin_Scripts%WINDIR%\system32\. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo Pubprn o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `computer Name`).

## <a name="examples"></a>Ejemplos
Para publicar todas las impresoras del \\equipo \Server1 en el contenedor de contención del dominio mydomain.Company.com, escriba:
```
cscript Ppubprn Server1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```
Para publicar la impresora Laserprinter1 en el \\servidor de \Server1 en el contenedor de contención del dominio mydomain.Company.com, escriba:
```
cscript Ppubprn \\Server1\Laserprinter1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

## <a name="additional-references"></a>Referencias adicionales
- [Referencia de la clave de sintaxis de la línea de comandos referencia del](command-line-syntax-key.md)
[comando Print](print-command-reference.md)
