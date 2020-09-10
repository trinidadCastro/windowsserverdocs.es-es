---
title: rpcping
description: Artículo de referencia para el comando RPCPing, que confirma la conectividad RPC entre el equipo que ejecuta Microsoft Exchange Server y cualquiera de las estaciones de trabajo de cliente de Microsoft Exchange compatibles en la red.
ms.topic: reference
ms.assetid: 7382aa0d-90fc-47c0-84b3-15f52dd656d0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7351195d8206cb13b334ca3e06ffb794e4a13461
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641181"
---
# <a name="rpcping"></a>rpcping

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Confirma la conectividad RPC entre el equipo que ejecuta Microsoft Exchange Server y cualquiera de las estaciones de trabajo de cliente de Microsoft Exchange compatibles en la red. Esta utilidad se puede usar para comprobar si los servicios de Microsoft Exchange Server responden a las solicitudes RPC de las estaciones de trabajo cliente a través de la red.

## <a name="syntax"></a>Sintaxis

```
rpcping [/t <protseq>] [/s <server_addr>] [/e <endpoint>
        |/f <interface UUID>[,majorver]] [/O <interface object UUID]
        [/i <#_iterations>] [/u <security_package_id>] [/a <authn_level>]
        [/N <server_princ_name>] [/I <auth_identity>] [/C <capabilities>]
        [/T <identity_tracking>] [/M <impersonation_type>]
        [/S <server_sid>] [/P <proxy_auth_identity>] [/F <RPCHTTP_flags>]
        [/H <RPC/HTTP_authn_schemes>] [/o <binding_options>]
        [/B <server_certificate_subject>] [/b] [/E] [/q] [/c]
        [/A <http_proxy_auth_identity>] [/U <HTTP_proxy_authn_schemes>]
        [/r <report_results_interval>] [/v <verbose_level>] [/d]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /t `<protseq>` | Especifica la secuencia de protocolo que se va a usar. Puede ser una de las secuencias de protocolo RPC estándar: ncacn_ip_tcp, ncacn_np o ncacn_http.<p>Si no se especifica, el valor predeterminado es ncacn_ip_tcp. |
| modificado `<server_addr>` | Especifica la dirección del servidor. Si no se especifica, se hará ping en el equipo local. |
| /e `<endpoint>` | Especifica el extremo en el que se va a hacer ping. Si no se especifica ninguno, se hará ping en el asignador de extremos del equipo de destino.<p>Esta opción es mutuamente excluyente con la opción Interface (**/f**). |
| /o `<binding_options>` | Especifica las opciones de enlace para el ping de RPC. |
| /f `<interface UUID>[,Majorver]` | Especifica la interfaz a la que se va a hacer ping. Esta opción es mutuamente excluyente con la opción de extremo. La interfaz se especifica como UUID.<p>Si no se especifica *majorver* , se buscará la versión 1 de la interfaz.<p>Cuando se especifica interface, **RPCPing** consultará el asignador de extremos en el equipo de destino para recuperar el extremo de la interfaz especificada. El asignador de extremos se consultará con las opciones especificadas en la línea de comandos. |
| /O `<object UUID>` | Especifica el UUID del objeto si la interfaz registró uno. |
| /i `<#_iterations>` | Especifica el número de llamadas que se van a realizar. El valor predeterminado es 1. Esta opción es útil para medir la latencia de conexión si se especifican varias iteraciones. |
| /u `<security_package_id>` | Especifica el paquete de seguridad (proveedor de seguridad) que RPC usará para hacer la llamada. El paquete de seguridad se identifica como un número o un nombre. Si se usa un número, es el mismo número que en la API de RpcBindingSetAuthInfoEx. Si especifica esta opción, debe especificar un nivel de autenticación distinto de *ninguno*. No hay ningún valor predeterminado para esta opción. Si no se especifica, RPC no usará seguridad para el ping. La siguiente lista muestra los nombres y los números. Los nombres no distinguen mayúsculas de minúsculas:<ul><li>Negotiate/9 o uno de nego, snego o Negotiate</li><li>NTLM/10 o NTLM</li><li>SChannel/14 o SChannel</li><li>Kerberos/16 o Kerberos</li><li>Kernel/20 o kernel</li></ul> |
| /a `<authn_level>` | Especifica el nivel de autenticación que se va a usar. Si se especifica esta opción, también se debe especificar el ID. de paquete de seguridad (**/u**). Si no se especifica esta opción, RPC no usará seguridad para el ping. No hay ningún valor predeterminado para esta opción. Los valores posibles son:<ul><li>conectar</li><li>llamada</li><li>PKT</li><li>integridad</li><li>privacy</li></ul> |
| /N `<server_princ_name>` | Especifica un nombre de entidad de seguridad de servidor.<p>Este campo solo se puede usar cuando se seleccionan el nivel de autenticación y el paquete de seguridad. |
| /I `<auth_identity>` | Permite especificar una identidad alternativa para conectarse al servidor. La identidad tiene el formato usuario, dominio, contraseña. Si el nombre de usuario, el dominio o la contraseña tienen caracteres especiales que el shell puede interpretar, incluya la identidad entre comillas dobles. Puede especificar en `\*` lugar de la contraseña y RPC le pedirá que escriba la contraseña sin repetirla en la pantalla. Si no se especifica este campo, se usará la identidad del usuario que ha iniciado sesión.<p>Este campo solo se puede usar cuando se seleccionan el nivel de autenticación y el paquete de seguridad. |
| /C `<capabilities>` | Especifica una máscara de máscara hexadecimal de marcas. Este campo solo se puede usar cuando se seleccionan el nivel de autenticación y el paquete de seguridad. |
| /T `<identity_tracking>` | Especifica estático o dinámico. Si no se especifica, el valor predeterminado es Dynamic.<p>Este campo solo se puede usar cuando se seleccionan el nivel de autenticación y el paquete de seguridad. |
| /M `<impersonation_type>` | Especifica Anonymous, identify, Impersonate o Delegate. El valor predeterminado es Impersonate.<p>Este campo solo se puede usar cuando se seleccionan el nivel de autenticación y el paquete de seguridad. |
| Modificado `<server_sid>` | Especifica el SID esperado del servidor.<p>Este campo solo se puede usar cuando se seleccionan el nivel de autenticación y el paquete de seguridad. |
| /P `<proxy_auth_identity>` | Especifica la identidad con la que se va a autenticar en el proxy RPC/HTTP. Tiene el mismo formato que para la opción **/i** . Debe especificar el paquete de seguridad (**/u**), el nivel de autenticación (**/a**) y los esquemas de autenticación (**/h**) para poder usar esta opción. |
| /F `<RPCHTTP_flags>` | Especifica las marcas que se van a pasar para la autenticación de front-end de RPC/HTTP. Las marcas se pueden especificar como números o nombres los marcadores reconocidos actualmente:<ul><li>Usar SSL/1 o SSL o use_ssl</li><li>Usar primer esquema de autenticación/2 o primero o use_first</li></ul>Debe especificar el paquete de seguridad (**/u**) y el nivel de autenticación (**/a**) para utilizar esta opción. |
| /H `<RPC/HTTP_authn_schemes>` | Especifica los esquemas de autenticación que se usarán para la autenticación de front-end de RPC/HTTP. Esta opción es una lista de valores numéricos o nombres separados por comas. Ejemplo: Basic, NTLM. Los valores reconocidos son (los nombres no distinguen mayúsculas de minúsculas):<ul><li>Basic/1 o Basic</li><li>NTLM/2 o NTLM</li><li>Certificado/65536 o CERT</li></ul><p>Debe especificar el paquete de seguridad (**/u**) y el nivel de autenticación (**/a**) para poder usar esta opción. |
| B `<server_certificate_subject>` | Especifica el sujeto del certificado de servidor. Debe usar SSL para que esta opción funcione.<p>Debe especificar el paquete de seguridad (**/u**) y el nivel de autenticación (**/a**) para poder usar esta opción. |
| /b | Recupera el sujeto del certificado de servidor del certificado enviado por el servidor y lo imprime en una pantalla o un archivo de registro. Solo es válido cuando se especifican la opción de solo eco de proxy (/E) y las opciones de uso de SSL.<p>Debe especificar el paquete de seguridad (**/u**) y el nivel de autenticación (**/a**) para poder usar esta opción. |
| /R | Especifica el proxy HTTP. Si no hay *ninguno*, se usa el proxy RPC. El valor *predeterminado* significa que se usa la configuración de Internet Explorer en el equipo cliente. Cualquier otro valor se tratará como el proxy HTTP explícito. Si no se especifica esta marca, se asume el valor predeterminado, es decir, se comprueba la configuración de Internet Explorer. Esta marca solo es válida cuando está habilitada la marca **/e** (solo eco). |
| /E | Restringe el ping solo al proxy RPC/HTTP. El ping no alcanza el servidor. Resulta útil cuando se intenta establecer si el proxy RPC/HTTP es accesible. Para especificar un proxy HTTP, use la marca/R. Si se especifica un proxy HTTP en la marca/o, se omitirá esta opción.<p>Debe especificar el paquete de seguridad (**/u**) y el nivel de autenticación (**/a**) para poder usar esta opción. |
| /q | Especifica el modo silencioso. No emite ningún mensaje, excepto para las contraseñas. Supone una respuesta *y* a todas las consultas. Use esta opción con cuidado. |
| /C | Usar certificado de tarjeta inteligente. RPCPing solicitará al usuario que elija tarjeta inteligente. |
| /A | Especifica la identidad con la que se va a autenticar en el proxy HTTP. Tiene el mismo formato que para la opción/I.<p>Debe especificar los esquemas de autenticación (/U), el paquete de seguridad (**/u**) y el nivel de autenticación (**/a**) para poder usar esta opción. |
| /U | Especifica los esquemas de autenticación que se usarán para la autenticación del proxy HTTP. Esta opción es una lista de valores numéricos o nombres separados por comas. Ejemplo: Basic, NTLM. Los valores reconocidos son (los nombres no distinguen mayúsculas de minúsculas):<ul><li>Basic/1 o Basic</li><li>NTLM/2 o NTLM</li></ul>Debe especificar el paquete de seguridad (**/u**) y el nivel de autenticación (**/a**) para poder usar esta opción. |
| /r | Si se especifican varias iteraciones, esta opción hará que **RPCPing** muestre las estadísticas de ejecución actuales periódicamente en lugar de la última llamada. El intervalo del informe se indica en segundos. El valor predeterminado es 15. |
| /v | Indica a **RPCPing** el grado de detalle de la salida. El valor predeterminado es 1. 2 y 3 proporcionan más resultados de **RPCPing**. |
| /d | Inicia la interfaz de usuario de diagnóstico de red RPC. |
| /p | Especifica que se soliciten las credenciales si se produce un error de autenticación. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para averiguar si el servidor de Exchange al que se conecta a través de RPC/HTTP es accesible, escriba:

```
rpcping /t ncacn_http /s exchange_server /o RpcProxy=front_end_proxy /P username,domain,* /H Basic /u NTLM /a connect /F 3
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
