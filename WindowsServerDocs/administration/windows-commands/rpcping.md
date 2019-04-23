---
title: rpcping
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7382aa0d-90fc-47c0-84b3-15f52dd656d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfa1d08c81f8b26507a5cae5f688923a7b226e1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829996"
---
# <a name="rpcping"></a>rpcping

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Confirma la conectividad RPC entre el equipo que ejecuta Microsoft Exchange Server y cualquiera de las estaciones de trabajo de cliente de Microsoft Exchange admitidos en la red. Esta utilidad se puede usar para comprobar si los servicios de Microsoft Exchange Server responden a las solicitudes RPC de las estaciones de trabajo cliente a través de la red. 

## <a name="syntax"></a>Sintaxis
```
rpcping [/t <protseq>] [/s <server_addr>] [/e <endpoint>
        |/f <interface UUID>[,Majorver]] [/O <Interface Object UUID]
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

|Parámetro|Descripción|
|-------|--------|
|/t \<protseq>|Especifica que use la secuencia de protocolo. Puede ser una de las secuencias de protocolo RPC estándares, por ejemplo: ncacn_ip_tcp, ncacn_np, o ncacn_http.<br /><br />Si no se especifica, el valor predeterminado es ncacn_ip_tcp.|
|/s \<server_addr>|Especifica la dirección del servidor. Si no se especifica, se hará ping a la máquina local.|
|/e \<punto de conexión >|Especifica el punto de conexión para hacer ping. Si se especifica ninguno, el asignador de extremos en el equipo de destino hará ping a.<br /><br />Esta opción es mutuamente excluyente con la interfaz (**/f**) opción.|
|/o \<binding_options>|Especifica las opciones de enlace para el ping RPC.|
|/f \<UUID de interfaz > [, Majorver]|Especifica la interfaz para hacer ping. Esta opción es mutuamente excluyente con la opción de punto de conexión. La interfaz se especifica como un UUID.<br /><br />Si el *Majorver* no se especifica, se buscará la versión 1 de la interfaz.<br /><br />Cuando se especifica la interfaz, **rpcping** consultará el asignador de extremos en el equipo de destino para recuperar el punto de conexión para la interfaz especificada. Se va a consultar el asignador de extremos utilizando las opciones especificadas en la línea de comandos.|
|/O \<UUID del objeto >|Especifica el UUID del objeto si la interfaz de uno registrado.|
|/i \<_iterations # >|Especifica el número de llamadas para realizar. El valor predeterminado es 1. Esta opción es útil para medir la latencia de conexión si se especifican varias iteraciones.|
|/u \<security_package_id>|Especifica el paquete de seguridad (proveedor de seguridad) RPC usará para realizar la llamada. El paquete de seguridad se identifica como un número o un nombre. Si se usa un número es el mismo número como en la API RpcBindingSetAuthInfoEx. La siguiente lista muestra los nombres y números. Los nombres no distinguen mayúsculas de minúsculas:<br /><br />-Negotiate / 9 o una de nego, snego o negociar<br />-NTLM / 10 o NTLM<br />-SChannel / 14 o SChannel<br />-Kerberos / 16 o Kerberos<br />-Kernel / 20 o del núcleo<br />    Si especifica esta opción, debe especificar el nivel de autenticación distinto de none. No hay ningún valor predeterminado para esta opción. Si no se especifica, RPC no usará seguridad para el comando ping.|
|/a \<authn_level>|Especifica el nivel de autenticación que utilice. Los valores posibles son:<br /><br />-conectar<br />-llamar a<br />-pkt<br />-   integrity<br />-privacidad<br /><br />Si se especifica esta opción, el Id. de paquete de la seguridad (/ u) también debe especificarse. No hay ningún valor predeterminado para esta opción.<br /><br />Si no se especifica esta opción, RPC no usará seguridad para el comando ping.|
|/N \<server_princ_name>|Especifica un nombre principal del servidor.<br /><br />Este campo puede usarse solo cuando se selecciona el paquete de seguridad y el nivel de autenticación.|
|/I \<auth_identity>|Le permite especificar una identidad alternativa para conectarse al servidor. La identidad está en el usuario del formulario, el dominio, la contraseña. Si el nombre de usuario, el dominio o la contraseña tiene caracteres especiales que se pueden interpretar el shell, incluya la identidad entre comillas dobles. Puede especificar **\*** en lugar de la contraseña y RPC le pedirá que escriba la contraseña sin reproducirla en la pantalla. Si este campo no se especifica, se usará la identidad del usuario con sesión iniciada.<br /><br />Este campo puede usarse solo cuando se selecciona el paquete de seguridad y el nivel de autenticación.|
|/C \<capacidades >|Especifica una máscara de bits hexadecimal de marcas. Este campo puede usarse solo cuando se selecciona el paquete de seguridad y el nivel de autenticación.|
|/T \<identity_tracking >|Especifica estática o dinámica. Si no se especifica, dinámica es el valor predeterminado.<br /><br />Este campo puede usarse solo cuando se selecciona el paquete de seguridad y el nivel de autenticación.|
|/M \<impersonation_type>|Especifica anónimo, identificar, impersonate o delegate. Valor predeterminado es suplantar.<br /><br />Este campo puede usarse solo cuando se selecciona el paquete de seguridad y el nivel de autenticación.|
|/S \<server_sid>|Especifica al SID del servidor esperado.<br /><br />Este campo puede usarse solo cuando se selecciona el paquete de seguridad y el nivel de autenticación.|
|/P \<proxy_auth_identity>|Especifica la identidad para autenticar con el proxy RPC/HTTP. Tiene el mismo formato en cuanto a la opción/i. Debe especificar el paquete de seguridad (/ u), nivel de autenticación (/ a) y los esquemas de autenticación (/ H) para poder usar esta opción.|
|/F \<RPCHTTP_flags>|Especifica las marcas para pasar para la autenticación de front-end RPC/HTTP. Las marcas se pueden especificar como números o nombres de que las marcas reconocidas actualmente son:<br /><br />-Use SSL / 1 o ssl o use_ssl<br />-Usar primer esquema de autenticación / 2 o primero o use_first<br /><br />Debe especificar el paquete de seguridad (/ u) y el nivel de autenticación (/ a) para poder usar esta opción.|
|/H \<RPC/HTTP_authn_schemes>|Especifica los esquemas de autenticación que se usará para la autenticación de front-end RPC/HTTP. Esta opción es una lista de valores numéricos o de nombres separados por comas. Por ejemplo: Basic,NTLM. Los valores reconocidos son (los nombres no distinguen mayúsculas de minúsculas):<br /><br />-Básica o básica 1<br />-NTLM / 2 o NTLM<br />-Certificados / 65536 o certificado<br /><br />Debe especificar el paquete de seguridad (/ u) y el nivel de autenticación (/ a) para poder usar esta opción.|
|/B \<server_certificate_subject >|Especifica al asunto del certificado de servidor. Debe usar SSL para esta opción funcione.<br /><br />Debe especificar el paquete de seguridad (/ u) y el nivel de autenticación (/ a) para poder usar esta opción.|
|/b|Recupera al sujeto del certificado de servidor desde el certificado enviado por el servidor y lo imprime a una pantalla o un archivo de registro. Válido solo cuando el Proxy echo opción solo (/ E) y se especifican las opciones de uso SSL.<br /><br />Debe especificar el paquete de seguridad (/ u) y el nivel de autenticación (/ a) para poder usar esta opción.|
|/R|Especifica al proxy HTTP. Si *ninguno*, se utiliza el proxy RPC. El valor *predeterminada* significa que se usarán la configuración de Internet Explorer en el equipo cliente. Cualquier otro valor se tratará como un proxy HTTP explícita. Si no especifica este marcador, se supone que el valor predeterminado, es decir, se comprueba la configuración de Internet Explorer. Esta marca solo es válido cuando el **/E** marca (solo eco) está habilitada.|
|/E|Restringe el comando ping a solo el proxy RPC/HTTP. El comando ping no alcanza el servidor. Resulta útil cuando se intenta establecer si el proxy RPC/HTTP es accesible. Para especificar a un servidor proxy HTTP, use la marca/r. Si se especifica un proxy HTTP en la marca/o, esta opción se omitirá.<br /><br />Debe especificar el paquete de seguridad (/ u) y el nivel de autenticación (/ a) para poder usar esta opción.|
|/q|Especifica el modo silencioso. No emite los mensajes excepto por las contraseñas. Se da por supuesto *Y* respuesta a todas las consultas. Use esta opción con precaución.|
|/c|Usar certificado de tarjeta inteligente. RPCPing solicitará el usuario para elegir la tarjeta inteligente.|
|/A|Especifica la identidad con la que se va a autenticar el proxy HTTP. Tiene el mismo formato en cuanto a la opción/i.<br /><br />Debe especificar los esquemas de autenticación (/ U), paquete de seguridad (/ u) y el nivel de autenticación (/ a) para poder usar esta opción.|
|/U|Especifica los esquemas de autenticación que se usará para la autenticación del proxy HTTP. Esta opción es una lista de valores numéricos o de nombres separados por comas. Por ejemplo: Basic,NTLM. Los valores reconocidos son (los nombres no distinguen mayúsculas de minúsculas):<br /><br />-Básica o básica 1<br />-NTLM / 2 o NTLM<br /><br />Debe especificar el paquete de seguridad (/ u) y el nivel de autenticación (/ a) para poder usar esta opción.|
|/r|Si se especifican varias iteraciones, esta opción hará que **rpcping** mostrar las estadísticas de ejecución actuales periódicamente en su lugar después de la última llamada. El intervalo de informe se expresa en segundos. Valor predeterminado es 15.|
|/v|Indica a **rpcping** el nivel de detalle que el resultado. Valor predeterminado es 1. 2 y 3 proporcionar más resultados de **rpcping**.|
|/d|La interfaz de usuario de diagnóstico de red de lanzamientos de RPC.|
|/p|Especifica que se le pedirán las credenciales si se produce un error de autenticación.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos
Para averiguar si el servidor de Exchange que se conecta a través de RPC/HTTP es accesible, escriba:
```
rpcping /t ncacn_http /s exchange_server /o RpcProxy=front_end_proxy /P "username,domain,*" /H Basic /u NTLM /a connect /F 3
```

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
