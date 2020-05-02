---
title: nslookup set type
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5248e314-fac1-413e-81dc-bbe0a0873ba5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a9ccc7dde40b93db5f331930bc405c98764483d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723533"
---
# <a name="nslookup-set-type"></a>nslookup set type

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el tipo de registro de recursos de la consulta.
## <a name="syntax"></a>Sintaxis
```
set type=<ResourceRecordtype>
```
### <a name="parameters"></a>Parámetros
<ResourceRecordtype>Especifica un tipo de registro de recursos DNS. El tipo de registro de recursos predeterminado es. En la tabla siguiente se enumeran los valores válidos para este comando.

| Value |                                                   Descripción                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   Un   |                                      Especifica la dirección IP de un equipo&#39;s                                      |
|  ANY  |                                     Especifica la dirección IP de un equipo&#39;s.                                      |
| CNAME |                                    Especifica un nombre canónico para un alias.                                     |
|  GID  |                                  Especifica un identificador de grupo de un nombre de grupo.                                  |
| HINFO |                          Especifica el equipo&#39;s CPU y el tipo de sistema operativo.                           |
|  MB   |                                        Especifica un nombre de dominio de buzón.                                         |
|  MG   |                                         Especifica un miembro del grupo de correo.                                          |
| MINFO |                                   Especifica la información de buzón o de lista de correo.                                   |
|  MR   |                                     Especifica el nombre de dominio del cambio de nombre de correo.                                      |
|  MX   |                                          Especifica el intercambiador de correo.                                          |
|  NS   |                                 Especifica un servidor de nombres DNS para la zona con nombre.                                 |
|  PTR  | Especifica un nombre de equipo si la consulta es una dirección IP; de lo contrario, especifica el puntero a otra información. |
|  SOA  |                                Especifica el inicio de autoridad de una zona DNS.                                 |
|  TXT  |                                         Especifica la información de texto.                                         |
|  UID  |                                         Especifica el identificador de usuario.                                          |
| UINFO |                                         Especifica la información del usuario.                                         |
|  WKS  |                                         Describe un servicio bien conocido.                                         |
| {ayuda |                                                       ?}                                                        |

Muestra un breve resumen de los subcomandos de <strong>nslookup</strong> .
## <a name="remarks"></a>Observaciones
- El comando <strong>set Type</strong> realiza la misma función que el comando <strong>set QueryType</strong> .
- Para obtener más información acerca de los tipos de registro de recursos, consulte la solicitud de comentarios (RFC) 1035.
  ## <a name="additional-references"></a>Referencias adicionales
  <una clave de sintaxis de línea de comandos href = Command-key.md-Data-RAW-source =- [command-line](command-line-syntax-key.md)>clave</a> de sintaxis de línea de comandos <a href = nslookup-Set-QueryType.MD Data-RAW-source =[nslookup Set QueryType](nslookup-set-querytype.md)>nslookup Set QueryType</a>
