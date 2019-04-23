---
title: Ksetup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0194cf81d069d7a5c1223f0a514d593e4870d397
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868846"
---
# <a name="ksetup"></a>Ksetup



Realiza tareas que están relacionados con la configuración y mantenimiento de protocolo de Kerberos y el centro de distribución de claves (KDC) para admitir dominios Kerberos, que no son dominios de Windows. Para obtener ejemplos de cómo se puede usar este comando, vea la sección de ejemplos de cada uno de los temas secundarios relacionados.

## <a name="syntax"></a>Sintaxis

```
ksetup 
[/setrealm <DNSDomainName>] 
[/mapuser <Principal> <Account>] 
[/addkdc <RealmName> <KDCName>] 
[/delkdc <RealmName> <KDCName>]
[/addkpasswd <RealmName> <KDCPasswordName>] 
[/delkpasswd <RealmName> <KDCPasswordName>]
[/server <ServerName>] 
[/setcomputerpassword <Password>]
[/removerealm <RealmName>]  
[/domain <DomainName>] 
[/changepassword <OldPassword> <NewPassword>] 
[/listrealmflags] 
[/setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/dumpstate]
[/addhosttorealmmap] <HostName> <RealmName>]  
[/delhosttorealmmap] <HostName> <RealmName>]  
[/setenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <DomainName>
[/addenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <DomainName>

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|Convierte un miembro de un dominio Kerberos de este equipo.|
|[Ksetup:mapuser](ksetup-mapuser.md)|Una entidad de seguridad de Kerberos se asigna a una cuenta.|
|[Ksetup:addkdc](ksetup-addkdc.md)|Define una entrada KDC para el dominio Kerberos determinado.|
|[Ksetup:delkdc](ksetup-delkdc.md)|Elimina una entrada KDC para el dominio Kerberos.|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|Agrega una dirección de servidor Kpasswd para un dominio Kerberos.|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|Elimina una dirección de servidor Kpasswd para un dominio Kerberos.|
|[Ksetup:server](ksetup-server.md)|Permite especificar el nombre de un equipo de Windows en el que se va a aplicar los cambios.|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|Establece la contraseña de cuenta de dominio del equipo (o entidad de seguridad de host).|
|[Ksetup:removerealm](ksetup-removerealm.md)|Elimina toda la información para el dominio Kerberos especificado desde el registro.|
|[Ksetup:domain](ksetup-domain.md)|Le permite especificar un dominio (si \<DomainName > no se ha establecido mediante **/Domain**).|
|[Ksetup:changepassword](ksetup-changepassword.md)|Le permite utilizar el Kpasswd para cambiar la contraseña del usuario con sesión iniciada.|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|Enumera las marcas de dominio Kerberos que **ksetup** puede detectar.|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|Establece las marcas del dominio para un dominio Kerberos específico.|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|Agrega marcas de dominio adicionales a un dominio Kerberos.|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|Elimina las marcas de dominio Kerberos de un dominio Kerberos.|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|Analiza la configuración de Kerberos en el equipo dado. Agrega un host a la asignación del dominio en el registro.|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|Agrega un valor del registro para asignar el host al dominio Kerberos.|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|Elimina el valor del registro que asigna el equipo host al dominio Kerberos.|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|Establece los atributos de confianza para el dominio de uno o varios tipos de cifrado.|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|Obtiene el atributo de confianza de los tipos de cifrado para el dominio.|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|Agrega tipos de cifrado para el atributo de confianza de los tipos de cifrado para el dominio.|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|Elimina el atributo de confianza de los tipos de cifrado para el dominio.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

**Ksetup** se utiliza para cambiar la configuración del equipo para la localización de dominios Kerberos. En las implementaciones no basados en Microsoft Kerberos, esta información normalmente se mantiene en el archivo Krb5.conf. En sistemas operativos Windows Server, se mantiene en el registro. Puede usar esta herramienta para modificar esta configuración. Esta configuración se usa por las estaciones de trabajo para buscar dominios Kerberos y los controladores de dominio para localizar los dominios Kerberos para las relaciones de confianza entre territorios.

**Ksetup** inicializa las claves del registro que el proveedor de soporte técnico de seguridad (SSP) de Kerberos se utiliza para localizar un KDC para el dominio Kerberos si el equipo se está ejecutando Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 y no es un miembro de un Windows dominio. Después de la configuración, cuentas de usuario de un equipo cliente que se está ejecutando el sistema operativo puede iniciar sesión en Windows en el dominio Kerberos.

El protocolo Kerberos versión 5 es el valor predeterminado para la autenticación de red en equipos que ejecutan Windows XP Professional, Windows Vista y Windows 7. El SSP de Kerberos busca en el registro para el nombre de dominio del dominio Kerberos del usuario y, a continuación, resuelve el nombre en una dirección IP mediante una consulta de un servidor DNS. El protocolo Kerberos puede utilizar DNS para localizar los KDC utilizando únicamente el nombre de dominio Kerberos, pero debe ser especialmente configurado para ello.

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)