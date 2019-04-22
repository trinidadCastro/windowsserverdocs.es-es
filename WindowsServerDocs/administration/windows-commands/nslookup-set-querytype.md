---
title: nslookup set querytype
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d698e6d4603afb332efeaf1cdc79eeeee37d66d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813126"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el tipo de registro de recursos para la consulta.
## <a name="syntax"></a>Sintaxis
```
set querytype=<ResourceRecordtype>
```
## <a name="parameters"></a>Parámetros
<ResourceRecordtype> Especifica un tipo de registro de recursos DNS. El tipo de registro de recursos predeterminado es r. En la tabla siguiente se enumera los valores válidos para este comando.
|Valor|Descripción|
|-----|--------|
|A|Especifica la dirección IP de un equipo|
|ANY|Especifica la dirección IP de un equipo.|
|CNAME|Especifica un nombre canónico de un alias.|
|GID|Especifica un identificador de grupo de un nombre de grupo.|
|HINFO|Especifica la CPU y el tipo de sistema operativo de un equipo.|
|MB|Especifica un nombre de dominio de buzón.|
|MG|Especifica a un miembro del grupo de correo electrónico.|
|MINFO|Especifica la información de buzón o de correo electrónico de la lista.|
|MR|Especifica el nombre de dominio de cambio de nombre de correo electrónico.|
|MX|Especifica al intercambiador de correo.|
|NS|Especifica un servidor de nombres DNS para la zona con nombre.|
|PTR|Especifica un equipo con el nombre si la consulta es una dirección IP. en caso contrario, especifica el puntero a otra información.|
|SOA|Especifica el inicio de autoridad para una zona DNS.|
|TXT|Especifica la información de texto.|
|UID|Especifica el identificador de usuario.|
|UINFO|Especifica la información de usuario.|
|WKS|Describe un servicio conocido.|
{Ayuda | ?}
Muestra un resumen breve de **nslookup** subcomandos
## <a name="remarks"></a>Comentarios
-   El **establecer tipo** comando realiza la misma función que el **establecer querytype** comando.
-   Para obtener más información acerca de los tipos de registros de recursos, vea la solicitud de comentarios (Rfc) 1035.
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[tipo de conjunto de nslookup](nslookup-set-type.md)
