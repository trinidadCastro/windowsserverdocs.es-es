---
title: nslookup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3c20855befbe71413fa8fdb44925b8b634c0c11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841656"
---
# <a name="nslookup"></a>nslookup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información que puede usar para diagnosticar la infraestructura de sistema de nombres de dominio (DNS). Antes de usar esta herramienta, debe estar familiarizado con cómo funciona DNS. La herramienta de línea de comandos nslookup está disponible solo si ha instalado el protocolo TCP/IP.
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
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[salir de nslookup comando](nslookup-exit-command.md)|se cierra **nslookup**.|
|[Nslookup dedo comando](nslookup-finger-command.md)|Se conecta con el servidor del dedo en el equipo actual.|
|[Ayuda de Nslookup](nslookup-help.md)|Muestra un resumen breve de **nslookup** subcomandos.|
|[nslookup ls](nslookup-ls.md)|Muestra información de un dominio DNS.|
|[nslookup lserver](nslookup-lserver.md)|cambia el servidor predeterminado para el dominio DNS especificado.|
|[nslookup root](nslookup-root.md)|cambia el servidor predeterminado en el servidor para la raíz del espacio de nombres de dominio DNS.|
|[nslookup server](nslookup-server.md)|cambia el servidor predeterminado para el dominio DNS especificado.|
|[nslookup set](nslookup-set.md)|cambia los valores de configuración que afectan a cómo las búsquedas.|
|[Nslookup establecer todo](nslookup-set-all.md)|Imprime los valores actuales de las opciones de configuración.|
|[nslookup set (clase)](nslookup-set-class.md)|cambia la clase de consulta. La clase especifica el grupo de protocolo de la información.|
|[nslookup set d2](nslookup-set-d2.md)|Desactiva el modo de depuración exhaustivo activado o desactivado. Se imprimen todos los campos de todos los paquetes.|
|[depuración de conjunto de Nslookup](nslookup-set-debug.md)|Desactiva el modo de depuración o desactivar.|
|nslookup /set defname|Anexa el nombre de dominio DNS predeterminado a una solicitud de búsqueda de componente único. Un único componente es un componente que no contiene puntos.|
|[dominio conjunto de Nslookup](nslookup-set-domain.md)|cambia el nombre de dominio DNS predeterminado para el nombre especificado.|
|nslookup /set ignore|Omite los errores de truncamiento de paquetes.|
|[Nslookup establece puerto](nslookup-set-port.md)|cambia el puerto de servidor de nombres de DNS de TCP/UDP predeterminada para el valor especificado.|
|[Nslookup conjunto querytype](nslookup-set-querytype.md)|cambia el tipo de registro de recursos para la consulta.|
|[recurse del conjunto de Nslookup](nslookup-set-recurse.md)|Indica que el servidor de nombres DNS para consultar otros servidores si no tiene la información.|
|[reintento de conjunto de Nslookup](nslookup-set-retry.md)|Establece el número de reintentos.|
|[raíz del conjunto de Nslookup](nslookup-set-root.md)|cambia el nombre del servidor raíz utilizado para consultas.|
|[nslookup set search](nslookup-set-search.md)|anexa los nombres de dominio DNS en la lista de búsqueda de dominio DNS a la solicitud hasta que se reciba una respuesta. Esto se aplica cuando el conjunto y la búsqueda de solicitan contener al menos un punto, pero no finalizan con un punto final.|
|[Nslookup conjunto srchlist](nslookup-set-srchlist.md)|cambia el nombre y la búsqueda de la lista de los dominios DNS predeterminado.|
|[Establecer tiempo de espera de Nslookup](nslookup-set-timeout.md)|cambia el número de segundos que espera una respuesta a una solicitud inicial.|
|[tipo de conjunto de Nslookup](nslookup-set-type.md)|cambia el tipo de registro de recursos para la consulta.|
|[nslookup set vc](nslookup-set-vc.md)|Especifica el uso o no utilizar un circuito virtual al enviar solicitudes al servidor.|
|[nslookup view](nslookup-view.md)|Ordena y se muestra el resultado de la anterior **ls** subcomando o comandos.|
## <a name="remarks"></a>Comentarios
-   Si *equipoBuscado* es una dirección IP y la consulta es para una A o se devuelve el tipo de registro de recursos PTR, el nombre del equipo. Si *equipoBuscado* es un nombre y no tienen un carácter final período, el valor predeterminado se anexa el nombre de dominio DNS para el nombre. Este comportamiento depende del estado de los siguientes **establecer** subcomandos: **dominio**, **srchlist**, **defname**, y  **búsqueda**.
-   Si escribe un guión (-) en lugar de *equipoBuscado*, el símbolo del sistema cambia a **nslookup** modo interactivo.
-   La longitud de línea de comandos debe ser inferior a 256 caracteres.
-   **nslookup** tiene dos modos: interactivas y.
    Si necesita buscar solo un único fragmento de datos, use el modo no interactivo. Para el primer parámetro, escriba el nombre o dirección IP del equipo que desea buscar. Para el segundo parámetro, escriba el nombre o dirección IP de un servidor de nombres DNS. Si se omite el segundo argumento, **nslookup** usa el servidor de nombres DNS de forma predeterminada.
    Si necesita buscar más de un fragmento de datos, puede usar el modo interactivo. Escriba un guión (-) para el primer parámetro y el nombre o dirección IP de un servidor de nombres DNS para el segundo parámetro. O bien, omita los dos parámetros y **nslookup** usa el servidor de nombres DNS de forma predeterminada. Estos son algunos consejos sobre cómo trabajar en modo interactivo:
    -   Para interrumpir los comandos interactivos en cualquier momento, presione CTRL + B.
    -   Para salir, escriba **salir**.
    -   Para tratar un comando integrado como un nombre de equipo, precedidos por el carácter de escape (\\).
    -   Un comando no reconocido se interpreta como un nombre de equipo.
-   Si se produce un error en la solicitud de búsqueda, **nslookup** imprime un mensaje de error. En la tabla siguiente se enumera los posibles mensajes de error.
    |**mensaje de error**|**Descripción**|
    |-----------|----------|
    |`timed out`|El servidor no respondió a una solicitud después de una cierta cantidad de tiempo y un cierto número de reintentos. Puede establecer el período de tiempo de espera con el **Establecer tiempo de espera** subcomando. Puede establecer el número de reintentos con el **conjunto reintento** subcomando.|
    |`No response from server`|Ningún servidor de nombres DNS se está ejecutando en el equipo servidor.|
    |`No records`|El servidor de nombres DNS no tiene registros de recursos del tipo de consulta actual para el equipo, aunque el nombre del equipo es válido. El tipo de consulta se especifica con el **establecer querytype** comando.|
    |`Nonexistent domain`|El equipo o el nombre de dominio DNS no existe.|
    |`Connection refused`<br /><br />-o bien-<br /><br />`Network is unreachable`|No se pudo establecer la conexión al servidor de nombres DNS o servidor del dedo. Este error suele ocurre con **ls** y **dedo** solicitudes.|
    |`Server failure`|El servidor de nombres DNS encontró una incoherencia interna en su base de datos y no pudo devolver una respuesta válida.|
    |`Refused`|El servidor de nombres DNS ha rechazado la solicitud de servicio.|
    |`format error`|El servidor de nombres DNS encontró que el paquete de solicitud no tenía el formato correcto. Esto puede indicar un error en **nslookup**.|
-   Para obtener más información sobre la **nslookup** comando y DNS, consulte los siguientes recursos:
    -   Lee, T., Davies, J. 2000. *Referencia técnica de Microsoft Windows 2000 TCP/IP Protocols and Services*. Redmond, Washington: Microsoft Press.
    -   Albitz, P., Loukides, M. y C. Liu. 2001. *DNS y BIND, cuarta edición*. Sebastopol, California: O ' Reilly and associates, Inc.
    -   Larson, M. y C. Liu. 2001. *DNS en Windows 2000*. Sebastopol, California: O ' Reilly and associates, Inc.
#### <a name="examples"></a>Ejemplos
Cada opción de línea de comandos consta de un guión (-) inmediatamente seguido por el nombre de comando y, en algunos casos, un signo igual (=) y, a continuación, en un valor. Por ejemplo, para cambiar el tipo de consulta predeterminado a la información de host (equipo) y el tiempo de espera inicial en 10 segundos, escriba: **nslookup - querytype = hinfo - timeout = 10**
## <a name="see-also"></a>Vea también
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
