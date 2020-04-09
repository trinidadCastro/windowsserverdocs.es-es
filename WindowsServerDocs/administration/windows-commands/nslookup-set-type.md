---
title: nslookup set type
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5248e314-fac1-413e-81dc-bbe0a0873ba5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37b6636d9bf457596fc070cdce4a02a023ffd263
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838268"
---
# <a name="nslookup-set-type"></a>nslookup set type

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el tipo de registro de recursos de la consulta.
## <a name="syntax"></a>Sintaxis
```
set type=<ResourceRecordtype>
```
### <a name="parameters"></a>Parámetros
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
|  SOA  |                                Especifica el inicio de autoridad de una zona DNS.                                 |
|  TXT  |                                         Especifica la información de texto.                                         |
|  UID  |                                         Especifica el identificador de usuario.                                          |
| UINFO |                                         Especifica la información del usuario.                                         |
|  WKS  |                                         Describe un servicio bien conocido.                                         |
| {ayuda |                                                       ?}                                                        |

Muestra un breve resumen de los subcomandos de <strong>nslookup</strong> .
## <a name="remarks"></a>Comentarios
- El comando <strong>set Type</strong> realiza la misma función que el comando <strong>set QueryType</strong> .
- Para obtener más información acerca de los tipos de registro de recursos, consulte la solicitud de comentarios (RFC) 1035.
  ## <a name="additional-references"></a>Referencias adicionales
  < a href = Command-Line-Syntax-key.md Data-RAW-source =- [Command-Line Syntax key](command-line-syntax-key.md)> clave de sintaxis de línea de comandos</a> < a href = nslookup-set-QueryType.MD Data-RAW-source =[nslookup Set QueryType](nslookup-set-querytype.md)> nslookup Set QueryType</a>
