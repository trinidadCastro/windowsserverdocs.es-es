---
title: nslookup set type
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5248e314-fac1-413e-81dc-bbe0a0873ba5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3f41cb6bc5117fdd26bba85c6cfd806414bbab4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372880"
---
# <a name="nslookup-set-type"></a>nslookup set type

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el tipo de registro de recursos de la consulta.
## <a name="syntax"></a>Sintaxis
```
set type=<ResourceRecordtype>
```
## <a name="parameters"></a>Parámetros
<ResourceRecordtype> especifica un tipo de registro de recursos DNS. El tipo de registro de recursos predeterminado es. En la tabla siguiente se enumeran los valores válidos para este comando.

| Valor |                                                   Descripción                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      Especifica la dirección&#39;IP de un equipo.                                      |
|  CUALQUIER  |                                     Especifica la dirección&#39;IP de un equipo.                                      |
| CNAME |                                    Especifica un nombre canónico para un alias.                                     |
|  GID  |                                  Especifica un identificador de grupo de un nombre de grupo.                                  |
| HINFO |                          Especifica la CPU&#39;y el tipo de sistema operativo de un equipo.                           |
|  MB   |                                        Especifica un nombre de dominio de buzón.                                         |
|  MG   |                                         Especifica un miembro del grupo de correo.                                          |
| MINFO |                                   Especifica la información de buzón o de lista de correo.                                   |
|  MR   |                                     Especifica el nombre de dominio del cambio de nombre de correo.                                      |
|  MX   |                                          Especifica el intercambiador de correo.                                          |
|  NS   |                                 Especifica un servidor de nombres DNS para la zona con nombre.                                 |
|  ANOTA  | Especifica un nombre de equipo si la consulta es una dirección IP; de lo contrario, especifica el puntero a otra información. |
|  Oriente  |                                Especifica el inicio de autoridad de una zona DNS.                                 |
|  TXT  |                                         Especifica la información de texto.                                         |
|  UID  |                                         Especifica el identificador de usuario.                                          |
| UINFO |                                         Especifica la información del usuario.                                         |
|  WKS  |                                         Describe un servicio bien conocido.                                         |
| {ayuda |                                                       ?}                                                        |

Muestra un breve resumen de los subcomandos de <strong>nslookup</strong> .
## <a name="remarks"></a>Observaciones
- El comando <strong>set Type</strong> realiza la misma función que el comando <strong>set QueryType</strong> .
- Para obtener más información acerca de los tipos de registro de recursos, consulte la solicitud de comentarios (RFC) 1035.
  ## <a name="additional-references"></a>referencias adicionales
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">Clave de sintaxis de línea de comandos</a>
  <a href="nslookup-set-querytype.md" data-raw-source="[nslookup set querytype](nslookup-set-querytype.md)">nslookup Set QueryType</a>
