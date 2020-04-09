---
title: nslookup
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15062d81992ee1b6e55d47cb9e49822350e4f2bc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838098"
---
# <a name="nslookup"></a>nslookup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información que puede usar para diagnosticar la infraestructura del sistema de nombres de dominio (DNS). Antes de usar esta herramienta, debe estar familiarizado con el funcionamiento de DNS. La herramienta de línea de comandos nslookup solo está disponible si se ha instalado el protocolo TCP/IP.
## <a name="syntax"></a>Sintaxis

```
nslookup [<-SubCommand ...>] [{<computerTofind> | -<Server>}]
nslookup /exit
nslookup /finger [<UserName>] [{[>] <FileName>|[>>] <FileName>}]
nslookup /{help | ?}
nslookup /ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
nslookup /lserver <DNSDomain> 
nslookup /root 
nslookup /server <DNSDomain>
nslookup /set <KeyWord>[=<Value>]
nslookup /set all 
nslookup /set class=<Class>
nslookup /set [no]d2
nslookup /set [no]debug
nslookup /set [no]defname
nslookup /set domain=<DomainName>
nslookup /set [no]ignore
nslookup /set port=<Port>
nslookup /set querytype=<ResourceRecordtype>
nslookup /set [no]recurse
nslookup /set retry=<Number>
nslookup /set root=<RootServer>
nslookup /set [no]search
nslookup /set srchlist=<DomainName>[/...]
nslookup /set timeout=<Number>
nslookup /set type=<ResourceRecordtype>
nslookup /set [no]vc
nslookup /view <FileName>
```

### <a name="parameters"></a>Parámetros

|                       Parámetro                       |                                                                                                         Descripción                                                                                                         |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [Comando nslookup exit](nslookup-exit-command.md)   |                                                                                                     Sale de **nslookup**.                                                                                                     |
| [Comando nslookup finger](nslookup-finger-command.md) |                                                                                  Se conecta con el servidor Finger en el equipo actual.                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    Muestra un breve resumen de los subcomandos de **nslookup** .                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             Muestra información de un dominio DNS.                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   Cambia el servidor predeterminado al dominio DNS especificado.                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     Cambia el servidor predeterminado al servidor para la raíz del espacio de nombres de dominio DNS.                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   Cambia el servidor predeterminado al dominio DNS especificado.                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              Cambia los valores de configuración que afectan a cómo funcionan las búsquedas.                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  Imprime los valores actuales de las opciones de configuración.                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     Cambia la clase de consulta. La clase especifica el grupo de protocolos de la información.                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     Activa o desactiva el modo de depuración exhaustiva. Se imprimen todos los campos de cada paquete.                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               Activa o desactiva el modo de depuración.                                                                                               |
|                 nslookup/Set defname                 |                                            Anexa el nombre de dominio DNS predeterminado a una solicitud de búsqueda de componente única. Un componente único es un componente que no contiene puntos.                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 Cambia el nombre de dominio DNS predeterminado por el nombre especificado.                                                                                  |
|                 nslookup/Set ignore                  |                                                                                              Omite los errores de truncamiento de paquetes.                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          Cambia el puerto del servidor de nombres DNS TCP/UDP predeterminado al valor especificado.                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       Cambia el tipo de registro de recursos de la consulta.                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    Indica al servidor de nombres DNS que consulte a otros servidores si no tiene la información.                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 Establece el número de reintentos.                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    Cambia el nombre del servidor raíz que se usa para las consultas.                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | Anexa los nombres de dominio DNS de la lista de búsqueda de dominios DNS a la solicitud hasta que se reciba una respuesta. Esto se aplica cuando el conjunto y la solicitud de búsqueda contienen al menos un punto, pero no finalizan con un punto final. |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    Cambia el nombre de dominio DNS y la lista de búsqueda predeterminados.                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           Cambia el número inicial de segundos de espera para una respuesta a una solicitud.                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       Cambia el tipo de registro de recursos de la consulta.                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     Especifica que se use o no un circuito virtual al enviar solicitudes al servidor.                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          Ordena y muestra la salida del subcomando o los comandos **LS** anteriores.                                                                          |

## <a name="remarks"></a>Comentarios
- Si *computerTofind* es una dirección IP y la consulta es para un tipo de registro de recursos A o PTR, se devuelve el nombre del equipo. Si *computerTofind* es un nombre y no tiene un punto final, el nombre de dominio DNS predeterminado se anexa al nombre. Este comportamiento depende del estado de los siguientes subcomandos **set** : **Domain**, **srchlist**, **defname**y **Search**.
- Si escribe un guión (-) en lugar de *computerTofind*, el símbolo del sistema cambia al modo interactivo de **nslookup** .
- La longitud de la línea de comandos debe ser inferior a 256 caracteres.
- **nslookup** tiene dos modos: interactivo y no interactivo.
  Si solo necesita buscar una parte de los datos, use el modo no interactivo. En el primer parámetro, escriba el nombre o la dirección IP del equipo que desea buscar. En el segundo parámetro, escriba el nombre o la dirección IP de un servidor de nombres DNS. Si omite el segundo argumento, **nslookup** usa el servidor de nombres DNS predeterminado.
  Si necesita buscar más de un fragmento de datos, puede usar el modo interactivo. Escriba un guión (-) para el primer parámetro y el nombre o la dirección IP de un servidor de nombres DNS para el segundo parámetro. O bien, omita ambos parámetros y **nslookup** usará el servidor de nombres DNS predeterminado. A continuación se muestran algunas sugerencias sobre cómo trabajar en modo interactivo:
  -   Para interrumpir los comandos interactivos en cualquier momento, presione CTRL + B.
  -   Para salir, escriba **Exit**.
  -   Para tratar un comando integrado como un nombre de equipo, debe ir precedido del carácter de escape (\\).
  -   Un comando desconocido se interpreta como un nombre de equipo.
- Si se produce un error en la solicitud de búsqueda, **nslookup** imprime un mensaje de error. En la tabla siguiente se enumeran los posibles mensajes de error.
  |**Mensaje de error**|**Descripción**|
  |-----------|----------|
  |`timed out`|El servidor no respondió a una solicitud después de un determinado período de tiempo y cierto número de reintentos. Puede establecer el período de tiempo de espera con el subcomando **establecer tiempo de espera** . Puede establecer el número de reintentos con el subcomando **set retry** .|
  |`No response from server`|No se está ejecutando ningún servidor de nombres DNS en el equipo servidor.|
  |`No records`|El servidor de nombres DNS no tiene registros de recursos del tipo de consulta actual para el equipo, aunque el nombre del equipo es válido. El tipo de consulta se especifica con el comando **set QueryType** .|
  |`Nonexistent domain`|El nombre del equipo o dominio DNS no existe.|
  |`Connection refused`<p>O bien,<p>`Network is unreachable`|No se pudo establecer la conexión con el servidor de nombres DNS o el servidor Finger. Este error suele producirse con solicitudes **LS** y **Finger** .|
  |`Server failure`|El servidor de nombres DNS encontró una incoherencia interna en su base de datos y no pudo devolver una respuesta válida.|
  |`Refused`|El servidor de nombres DNS rechazó el servicio de la solicitud.|
  |`format error`|El servidor de nombres DNS detectó que el paquete de solicitud no tenía el formato correcto. Puede indicar un error en **nslookup**.|
- Para obtener más información acerca del comando **nslookup** y DNS, consulte los siguientes recursos:
  - Lee, T., Davies, J. 2000. *Referencia técnica de servicios y protocolos TCP/IP de Microsoft Windows 2000*. Redmond, Washington: Microsoft Press.
  - Albitz, P., Loukides, M. y C. Liu. 2001. *DNS y bind, cuarta edición*. Sebastopol, California: O'Reilly y Associates, Inc.
  - Larson, M. y C. Liu. 2001. *DNS en Windows 2000*. Sebastopol, California: O'Reilly y Associates, Inc.
    #### <a name="examples"></a>Ejemplos
    Cada opción de línea de comandos está formada por un guión (-) seguido inmediatamente del nombre del comando y, en algunos casos, un signo igual (=) y, a continuación, un valor. Por ejemplo, para cambiar el tipo de consulta predeterminado a información de host (equipo) y el tiempo de espera inicial a 10 segundos, escriba: **nslookup-QueryType = HINFO-timeout = 10**
    ## <a name="see-also"></a>Consulta también
    - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
