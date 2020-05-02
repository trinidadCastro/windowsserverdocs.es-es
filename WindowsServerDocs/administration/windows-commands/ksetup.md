---
title: ksetup
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f3fde0ada4ab8bcbe52eccf22b959f99f91319f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724548"
---
# <a name="ksetup"></a>ksetup



Realiza tareas relacionadas con la configuración y el mantenimiento del protocolo Kerberos y el Centro de distribución de claves (KDC) para admitir dominios Kerberos, que no son también dominios de Windows. Para obtener ejemplos de cómo se puede usar este comando, consulte la sección ejemplos de cada uno de los subtemas relacionados.

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

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|Convierte este equipo en miembro de un dominio Kerberos.|
|[Ksetup:mapuser](ksetup-mapuser.md)|Asigna una entidad de seguridad de Kerberos a una cuenta.|
|[Ksetup:addkdc](ksetup-addkdc.md)|Define una entrada KDC para el dominio Kerberos especificado.|
|[Ksetup:delkdc](ksetup-delkdc.md)|Elimina una entrada KDC para el dominio Kerberos.|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|Agrega una dirección de servidor Kpasswd para un dominio Kerberos.|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|Elimina una dirección de servidor Kpasswd para un dominio Kerberos.|
|[Ksetup:server](ksetup-server.md)|Permite especificar el nombre de un equipo Windows en el que se van a aplicar los cambios.|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|Establece la contraseña de la cuenta de dominio del equipo (o la entidad de seguridad del host).|
|[Ksetup:removerealm](ksetup-removerealm.md)|Elimina toda la información del dominio Kerberos especificado del registro.|
|[Ksetup:domain](ksetup-domain.md)|Permite especificar un dominio (si \<domainname> no se ha establecido mediante **/Domain**).|
|[Ksetup:changepassword](ksetup-changepassword.md)|Permite usar Kpasswd para cambiar la contraseña del usuario que ha iniciado sesión.|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|Muestra las marcas de dominio Kerberos disponibles que **ksetup** puede detectar.|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|Establece marcas de dominio Kerberos para un dominio Kerberos específico.|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|Agrega marcas de dominio Kerberos adicionales a un dominio Kerberos.|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|Elimina marcas de dominio Kerberos de un dominio Kerberos.|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|Analiza la configuración de Kerberos en el equipo especificado. Agrega un host a la asignación de dominio Kerberos al registro.|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|Agrega un valor del registro para asignar el host al dominio Kerberos.|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|Elimina el valor del registro que asignó el equipo host al dominio Kerberos.|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|Establece uno o varios tipos de cifrado de atributos de confianza para el dominio.|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|Obtiene el atributo de confianza de tipos de cifrado para el dominio.|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|Agrega tipos de cifrado al atributo de confianza de tipos de cifrado para el dominio.|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|Elimina el atributo de confianza tipos de cifrado del dominio.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

**Ksetup** se usa para cambiar la configuración del equipo para buscar dominios Kerberos. En implementaciones que no son de Microsoft, esta información normalmente se mantiene en el archivo Krb5. conf. En los sistemas operativos Windows Server, se mantiene en el registro. Puede usar esta herramienta para modificar esta configuración. Las estaciones de trabajo usan estas opciones para localizar los dominios Kerberos y los controladores de dominio para localizar los dominios Kerberos para las relaciones de confianza entre dominios.

**Ksetup** inicializa las claves del registro que utiliza el proveedor de compatibilidad para seguridad (SSP) de Kerberos para buscar un KDC para el dominio Kerberos si el equipo ejecuta windows Server 2003, windows Server 2008 o windows Server 2008 R2 y no es miembro de un dominio de Windows. Después de la configuración, el usuario de un equipo cliente que ejecuta el sistema operativo Windows puede iniciar sesión en las cuentas del dominio Kerberos.

El protocolo Kerberos versión 5 es el valor predeterminado para la autenticación de red en equipos que ejecutan Windows XP Professional, Windows Vista y Windows 7. El SSP de Kerberos busca el nombre de dominio del dominio de usuario en el registro y, a continuación, resuelve el nombre en una dirección IP mediante una consulta a un servidor DNS. El protocolo Kerberos puede usar DNS para buscar KDC usando solo el nombre de dominio Kerberos, pero debe configurarse especialmente para ello.

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)