---
title: ksetup
description: Artículo de referencia para el comando ksetup, que realiza tareas relacionadas con la configuración y el mantenimiento del protocolo Kerberos y el Centro de distribución de claves (KDC) para admitir dominios Kerberos.
ms.topic: reference
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8745b095b097935661bd5d45190c4060d75261ce
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037703"
---
# <a name="ksetup"></a>ksetup

Realiza tareas relacionadas con la configuración y el mantenimiento del protocolo Kerberos y el Centro de distribución de claves (KDC) para admitir dominios Kerberos. En concreto, este comando se usa para:

- Cambiar la configuración del equipo para buscar dominios Kerberos. En implementaciones basadas en Kerberos que no son de Microsoft, esta información normalmente se mantiene en el archivo Krb5. conf. En los sistemas operativos Windows Server, se mantiene en el registro. Puede usar esta herramienta para modificar esta configuración. Las estaciones de trabajo usan estas opciones para localizar los dominios Kerberos y los controladores de dominio para localizar los dominios Kerberos para las relaciones de confianza entre dominios.

- Inicialice las claves del registro que utiliza el proveedor de compatibilidad para seguridad (SSP) de Kerberos para buscar un KDC para el dominio Kerberos, si el equipo no es miembro de un dominio de Windows. Después de la configuración, el usuario de un equipo cliente que ejecuta el sistema operativo Windows puede iniciar sesión en las cuentas del dominio Kerberos.

- Busque en el registro el nombre de dominio del dominio Kerberos del usuario y, a continuación, resuelva el nombre en una dirección IP consultando un servidor DNS. El protocolo Kerberos puede usar DNS para buscar KDC usando solo el nombre de dominio Kerberos, pero debe configurarse especialmente para ello.

## <a name="syntax"></a>Sintaxis

```
ksetup
[/setrealm <DNSdomainname>]
[/mapuser <principal> <account>]
[/addkdc <realmname> <KDCname>]
[/delkdc <realmname> <KDCname>]
[/addkpasswd <realmname> <KDCPasswordName>]
[/delkpasswd <realmname> <KDCPasswordName>]
[/server <servername>]
[/setcomputerpassword <password>]
[/removerealm <realmname>]
[/domain <domainname>]
[/changepassword <oldpassword> <newpassword>]
[/listrealmflags]
[/setrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/addrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/dumpstate]
[/addhosttorealmmap] <hostname> <realmname>]
[/delhosttorealmmap] <hostname> <realmname>]
[/setenctypeattr] <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <domainname>
[/addenctypeattr] <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <domainname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [ksetup setrealm](ksetup-setrealm.md) | Convierte este equipo en miembro de un dominio Kerberos. |
| [ksetup addkdc](ksetup-addkdc.md) | Define una entrada KDC para el dominio Kerberos especificado. |
| [ksetup delkdc](ksetup-delkdc.md) | Elimina una entrada KDC para el dominio Kerberos. |
| [ksetup addkpasswd](ksetup-addkpasswd.md) | Agrega una dirección de servidor Kpasswd para un dominio Kerberos. |
| [ksetup delkpasswd](ksetup-delkpasswd.md) | Elimina una dirección de servidor Kpasswd para un dominio Kerberos. |
| [ksetup server](ksetup-server.md) | Permite especificar el nombre de un equipo Windows en el que se van a aplicar los cambios. |
| [ksetup setcomputerpassword](ksetup-setcomputerpassword.md) | Establece la contraseña de la cuenta de dominio del equipo (o la entidad de seguridad del host). |
| [ksetup removerealm](ksetup-removerealm.md) | Elimina toda la información del dominio Kerberos especificado del registro. |
| [ksetup domain](ksetup-domain.md) | Permite especificar un dominio (si aún no `<domainname>` se ha establecido mediante el parámetro **/Domain** ). |
| [ksetup changepassword](ksetup-changepassword.md) | Permite usar Kpasswd para cambiar la contraseña del usuario que ha iniciado sesión. |
| [ksetup listrealmflags](ksetup-listrealmflags.md) | Muestra las marcas de dominio Kerberos disponibles que **ksetup** puede detectar. |
| [ksetup setrealmflags](ksetup-setrealmflags.md) | Establece marcas de dominio Kerberos para un dominio Kerberos específico. |
| [ksetup addrealmflags](ksetup-addrealmflags.md) | Agrega marcas de dominio Kerberos adicionales a un dominio Kerberos. |
| [ksetup delrealmflags](ksetup-delrealmflags.md) | Elimina marcas de dominio Kerberos de un dominio Kerberos. |
| [ksetup dumpstate](ksetup-dumpstate.md) | Analiza la configuración de Kerberos en el equipo especificado. Agrega un host a la asignación de dominio Kerberos al registro. |
| [ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md) | Agrega un valor del registro para asignar el host al dominio Kerberos. |
| [ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md) | Elimina el valor del registro que asignó el equipo host al dominio Kerberos. |
| [ksetup setenctypeattr](ksetup-setenctypeattr.md) | Establece uno o varios tipos de cifrado de atributos de confianza para el dominio. |
| [ksetup getenctypeattr](ksetup-getenctypeattr.md) | Obtiene el atributo de confianza de tipos de cifrado para el dominio. |
| [ksetup addenctypeattr](ksetup-addenctypeattr.md) | Agrega tipos de cifrado al atributo de confianza de tipos de cifrado para el dominio. |
| [ksetup delenctypeattr](ksetup-delenctypeattr.md) | Elimina el atributo de confianza tipos de cifrado del dominio. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)