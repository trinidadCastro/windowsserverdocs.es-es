---
title: pubprn
description: Artículo de referencia del comando Pubprn, que publica una impresora en el Active Directory Domain Services.
ms.topic: reference
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 969c8ab91e954db869560e4d5e4fb6fc4345b26f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032385"
---
# <a name="pubprn"></a>pubprn

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Publica una impresora en el Active Directory Domain Services. Este comando es un script de Visual Basic ubicado en el `%WINdir%\System32\printing_Admin_Scripts\<language>` directorio. Para usar este comando en un símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo Pubprn o cambie los directorios a la carpeta correspondiente. Por ejemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn`.

## <a name="syntax"></a>Sintaxis

```
cscript pubprn {<servername> | <UNCprinterpath>} LDAP://CN=<container>,DC=<container>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<servername>` | Especifica el nombre del servidor de Windows que hospeda la impresora que desea publicar. Si no especifica un equipo, se usa el equipo local. |
| `<UNCprinterpath>` | La ruta de acceso de la Convención de nomenclatura universal (UNC) a la impresora compartida que desea publicar. |
| `LDAP://CN=<Container>,DC=<Container>` | Especifica la ruta de acceso al contenedor en Active Directory Domain Services donde desea publicar la impresora. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, "nombre del equipo").

### <a name="examples"></a>Ejemplos

Para publicar todas las impresoras del \\ equipo Servidor1 en el contenedor de conMyDomain.company.com en el dominio, escriba:

```
cscript pubprn Server1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

Para publicar la impresora Laserprinter1 en el \\ servidor de \Server1 en el contenedor de contención del dominio mydomain.Company.com, escriba:

```
cscript pubprn \\Server1\Laserprinter1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
