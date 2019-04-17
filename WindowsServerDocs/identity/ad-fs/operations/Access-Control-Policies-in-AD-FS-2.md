---
ms.assetid: 
title: "Directivas de Control de acceso de cliente de servicios de federación de Active Directory 2.0"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00b9cd17c7a5c206e06bea12fd90762adb336715
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Directivas de Control de acceso de cliente de AD FS 2.0
Un directivas de acceso de cliente de servicios de federación de Active Directory 2.0 permiten restringir o conceder a los usuarios acceso a los recursos.  Este documento describe cómo habilitar las directivas de acceso de cliente de AD FS 2.0 y cómo configurar los escenarios más habituales.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Habilitar la directiva de acceso de cliente de AD FS 2.0

Para habilitar la directiva de acceso de cliente, sigue los pasos siguientes.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Paso 1: Instalar el paquete acumulativo de actualizaciones de actualización 2 de AD FS 2.0 empaquetar en los servidores de AD FS

Descargar el [paquete acumulativo de actualizaciones de la actualización 2 para los servicios de federación de Active Directory (AD FS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) empaquetar e instalarlo en todos los servidor de federación y los proxies de servidor de federación.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Paso 2: Agregar que cinco reclamar reglas para la confianza del proveedor de notificaciones de Active Directory

Una vez que el paquete acumulativo de actualizaciones de la actualización 2 se ha instalado en todos los servidores de AD FS y servidores proxy, usa el siguiente procedimiento para agregar un conjunto de reglas de notificaciones que hace que los nuevos tipos de notificación disponibles para el motor de directiva.

Para ello, va a agregar cinco reglas de transformación de aceptación para cada uno de los nuevos tipos de notificación de contexto de solicitud mediante el siguiente procedimiento.

En la confianza de proveedor de Active Directory reclamaciones, crear una nueva regla de transformación de aceptación pasa por cada uno de los nuevos tipos de notificación de contexto de la solicitud.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Para agregar una regla de notificación a Active Directory reclamaciones de confianza de proveedor para cada contexto de cinco de los tipos de notificación:


1. Haz clic en Inicio, apunta a programas, herramientas administrativas y, a continuación, haz clic en AD FS 2.0 administración.
2. En el árbol de consola, AD FS 2.0\Trust relaciones, haz clic en confianzas de proveedor de reclamaciones, haz clic en Active Directory y, a continuación, haz clic en Editar reglas de notificación.
3. En el cuadro de diálogo Editar reglas de notificación, selecciona la pestaña Rules de aceptación de transformación y, a continuación, haz clic en Agregar regla para iniciar al Asistente para la regla.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, selecciona paso a través o filtrar una notificación entrante de la lista y, a continuación, haz clic en siguiente.
5. En la página de configurar la regla, en nombre de regla de notificación, escribe el nombre para mostrar para esta regla; en entrante tipo de notificación, escribe la siguiente dirección URL de tipo de notificación y, a continuación, seleccione la fase de todos los valores de notificación.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Para comprobar la regla, selecciónalo en la lista y haga clic en Editar regla, haz clic en el idioma de la regla de vista. El idioma de la regla de notificación debería aparecer como sigue: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Haz clic en Finalizar.
8. En el cuadro de diálogo Editar reglas de notificación, haz clic en Aceptar para guardar las reglas.
9. Repite los pasos 2a 6 para crear una regla de notificación adicionales para cada uno de los tipos de notificación de cuatro restantes que se muestra a continuación, hasta que se crearon todas las reglas de cinco.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Paso 3: Actualizar la plataforma de identidad de Microsoft Office 365 confiar confianza de terceros

Elige uno de los escenarios de ejemplo a continuación para configurar las reglas de notificación en la plataforma de identidad de Microsoft Office 365 confiar confianza de terceros que mejor se adapte a las necesidades de la organización.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Escenarios de directiva de acceso de cliente de AD FS 2.0
Las siguientes secciones describen los escenarios que existen para AD FS 2.0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Escenario 1: Bloquear todos los accesos externos a Office 365

Este escenario de directiva de acceso de cliente permite el acceso de todos los clientes internos y bloques de todos los clientes externos en función de la dirección IP del cliente externo. El conjunto de reglas se basa en la regla de autorización de emisión predeterminada permitir acceso a todos los usuarios. Puedes usar el siguiente procedimiento para agregar una regla de autorización de emisión a Office 365 confiar confianza de terceros.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para crear una regla para bloquear todos los accesos externos a Office 365



1. Haz clic en Inicio, apunta a programas, herramientas administrativas y, a continuación, haz clic en AD FS 2.0 administración.
2. En el árbol de consola, AD FS 2.0\Trust relaciones, haz clic en confiar confía en parte, haz clic en la confianza de la plataforma de identidad de Microsoft Office 365 y, a continuación, haz clic en Editar reglas de notificación. 
3. En el cuadro de diálogo Editar reglas de notificación, selecciona la pestaña Rules de autorización de emisión y, a continuación, haz clic en Agregar regla para iniciar al Asistente para la regla de notificación.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione Enviar notificaciones mediante una regla personalizada y, a continuación, haz clic en siguiente.
5. En la página de configurar la regla, en nombre de regla de notificación, escribe el nombre para mostrar para esta regla. En la regla personalizada, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Haz clic en Finalizar. Comprueba que la nueva regla aparecerá inmediatamente debajo el acceso de permitir a la regla de todos los usuarios en la lista de reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo Editar reglas de notificación, haz clic en Aceptar.

>[!NOTE]
>Tendrás que reemplazar el valor por encima de "regex de dirección ip pública" con una expresión de IP válida; consulta el tema Creación de la expresión del intervalo de direcciones IP para obtener más información.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Escenario 2: Bloquear todos los accesos externos a Office 365 excepto Exchange ActiveSync

El siguiente ejemplo permite el acceso a todas las aplicaciones de Office 365, como Exchange Online, de los clientes internos incluido Outlook. Bloquea el acceso de los clientes que residen fuera de la red corporativa, como se indica en la dirección IP del cliente, excepto para los clientes de Exchange ActiveSync, como los smartphones. El conjunto de reglas se basa en la regla de autorización de emisión predeterminada titulada permitir acceso a todos los usuarios. Usa los siguientes pasos para agregar una regla de autorización de emisión a Office 365 confiar mediante el Asistente para la regla de Reclamación de confianza de terceros:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para crear una regla para bloquear todos los accesos externos a Office 365



1. Haz clic en Inicio, apunta a programas, herramientas administrativas y, a continuación, haz clic en AD FS 2.0 administración.
2. En el árbol de consola, AD FS 2.0\Trust relaciones, haz clic en confiar confía en parte, haz clic en la confianza de la plataforma de identidad de Microsoft Office 365 y, a continuación, haz clic en Editar reglas de notificación. 
3. En el cuadro de diálogo Editar reglas de notificación, selecciona la pestaña Rules de autorización de emisión y, a continuación, haz clic en Agregar regla para iniciar al Asistente para la regla de notificación.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione Enviar notificaciones mediante una regla personalizada y, a continuación, haz clic en siguiente.
5. En la página de configurar la regla, en nombre de regla de notificación, escribe el nombre para mostrar para esta regla. En la regla personalizada, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haz clic en Finalizar. Comprueba que la nueva regla aparecerá inmediatamente debajo el acceso de permitir a la regla de todos los usuarios en la lista de reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo Editar reglas de notificación, haz clic en Aceptar.

>[!NOTE]
>Tendrás que reemplazar el valor por encima de "regex de dirección ip pública" con una expresión de IP válida; consulta el tema Creación de la expresión del intervalo de direcciones IP para obtener más información.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Escenario 3: Bloquear todos los accesos externos a Office 365 excepto aplicaciones basadas en explorador

El conjunto de reglas se basa en la regla de autorización de emisión predeterminada titulada permitir acceso a todos los usuarios. Usar los siguientes pasos para agregar una regla de autorización de emisión a la plataforma de identidad de Microsoft Office 365 confiar mediante el Asistente para la regla de Reclamación de confianza de terceros:

>[!NOTE]
>Este escenario no es compatible con un proxy de terceros debido a las limitaciones en los encabezados de directiva de acceso de cliente con solicitudes pasivas (basada en Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para crear una regla para bloquear todos los accesos externos a Office 365 excepto aplicaciones basadas en explorador



1. Haz clic en Inicio, apunta a programas, herramientas administrativas y, a continuación, haz clic en AD FS 2.0 administración.
2. En el árbol de consola, AD FS 2.0\Trust relaciones, haz clic en confiar confía en parte, haz clic en la confianza de la plataforma de identidad de Microsoft Office 365 y, a continuación, haz clic en Editar reglas de notificación. 
3. En el cuadro de diálogo Editar reglas de notificación, selecciona la pestaña Rules de autorización de emisión y, a continuación, haz clic en Agregar regla para iniciar al Asistente para la regla de notificación.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione Enviar notificaciones mediante una regla personalizada y, a continuación, haz clic en siguiente.
5. En la página de configurar la regla, en nombre de regla de notificación, escribe el nombre para mostrar para esta regla. En la regla personalizada, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haz clic en Finalizar. Comprueba que la nueva regla aparecerá inmediatamente debajo el acceso de permitir a la regla de todos los usuarios en la lista de reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo Editar reglas de notificación, haz clic en Aceptar.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Escenario 4: Bloquear todos los accesos externos a Office 365 para grupos específicos de Active Directory

El siguiente ejemplo permite el acceso de los clientes internos en función de la dirección IP. Bloquea el acceso de los clientes que residen fuera de la red corporativa que tienen una dirección IP de cliente externo, excepto las personas de una regla de Active Directory Group.The especificada conjunto se basa en la regla de autorización de emisión predeterminada titulada permitir acceso a todos los usuarios. Usar los siguientes pasos para agregar una regla de autorización de emisión a la plataforma de identidad de Microsoft Office 365 confiar mediante el Asistente para la regla de Reclamación de confianza de terceros:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Para crear una regla para bloquear el acceso externo a Office 365a grupos específicos de Active Directory



1. Haz clic en Inicio, apunta a programas, herramientas administrativas y, a continuación, haz clic en AD FS 2.0 administración.
2. En el árbol de consola, AD FS 2.0\Trust relaciones, haz clic en confiar confía en parte, haz clic en la confianza de la plataforma de identidad de Microsoft Office 365 y, a continuación, haz clic en Editar reglas de notificación. 
3. En el cuadro de diálogo Editar reglas de notificación, selecciona la pestaña Rules de autorización de emisión y, a continuación, haz clic en Agregar regla para iniciar al Asistente para la regla de notificación.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione Enviar notificaciones mediante una regla personalizada y, a continuación, haz clic en siguiente.
5. En la página de configurar la regla, en nombre de regla de notificación, escribe el nombre para mostrar para esta regla. En la regla personalizada, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haz clic en Finalizar. Comprueba que la nueva regla aparecerá inmediatamente debajo el acceso de permitir a la regla de todos los usuarios en la lista de reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo Editar reglas de notificación, haz clic en Aceptar.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descripciones de la notificación de regla de sintaxis de lenguaje que se usan en los escenarios anteriores

|Descripción|La sintaxis de lenguaje de regla de notificación|
|-----|-----| 
|Regla de FS de anuncios de forma predeterminada para permitir el acceso a todos los usuarios. Esta regla debe existir en la plataforma de identidad de Microsoft Office 365 confiar confianza lista de reglas de autorización de emisión de terceros.|= > problema (tipo = "https://schemas.microsoft.com/authorization/claims/permit", valor = "true");| 
|Agrega esta cláusula a una nueva regla personalizada especifica que la solicitud proviene de proxy del servidor de federación (es decir, tiene el encabezado x-ms-proxy)
Se recomienda que todas las reglas de incluyen este.|Existe ([tipo == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"])| 
|Se usan para establecer que la solicitud proviene de un cliente con una dirección IP en el intervalo aceptable definido.|NO existe ([tipo == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", valor = ~ "regex de dirección ip pública proporcionado por el cliente"])| 
|Esta cláusula se usa para especificar que si la aplicación que se tiene acceso no es Microsoft.Exchange.ActiveSync debe deniega la solicitud.|NO existe ([tipo == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value=="Microsoft.Exchange.ActiveSync"])| 
|Esta regla te permite determinar si la llamada se realizó a través de un explorador Web y no se deniegan.|NO existe ([tipo == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path" valor == "/ adfs/ls /"])| 
|Esta regla indica que deben denegar los únicos usuarios en un determinado grupo de Active Directory (basado en el valor del SID). Agregar no a esta declaración significa que se permitirá un grupo de usuarios, independientemente de la ubicación.|Existe ([tipo == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", valor = ~ "{grupo valor del SID del grupo de AD permitida}"])| 
|Esto es una cláusula necesaria para emitir una denegación cuando se cumplen todas las condiciones anteriores.|= > problema (tipo = "https://schemas.microsoft.com/authorization/claims/deny", valor = "true");|

### <a name="building-the-ip-address-range-expression"></a>Creación de la expresión del intervalo de direcciones IP

La notificación de x-ms-reenvía-ip del cliente se rellena con un encabezado HTTP que está establecido actualmente solo por Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación de AD FS. El valor de la notificación puede ser uno de los siguientes:

>[!Note] 
>Exchange Online solo las direcciones IPV4 e IPV6 no admite actualmente.

Una sola dirección IP: la dirección IP del cliente que está conectada directamente al Exchange Online

>[!Note] 
>La dirección IP de un cliente en la red corporativa aparecerá como la dirección IP de interfaz externa del servidor proxy de salida de la organización o puerta de enlace.

Los clientes que se conectan a la red corporativa a una VPN o por Microsoft DirectAccess (DA) pueden aparecer como clientes corporativos internos o externos clientes según la configuración de VPN o DA.

Una o más direcciones IP: cuando Exchange en línea no puede determinar la dirección IP del cliente, establezca el valor en función del valor del x reenvía de encabezado, un encabezado estándar que se pueden incluir en las solicitudes HTTP y es compatible con muchos de los clientes, equilibradores de carga y servidores proxy en el mercado.

>[!Note]
>Varias direcciones IP, que indica la dirección IP del cliente y la dirección de cada servidor proxy que pasa la solicitud, se le separadas por comas.

No aparecerá en la lista de direcciones IP relacionadas con la infraestructura de Exchange Online.


#### <a name="regular-expressions"></a>Expresiones regulares

Una vez para que coincida con un intervalo de direcciones IP, es necesario construir una expresión regular para realizar la comparación. En la siguiente serie de pasos, se proporciona ejemplos de cómo crear este tipo de expresión para que coincida con los siguientes intervalos de direcciones (Ten en cuenta que tendrás que cambiar estos ejemplos para que coincida con el intervalo de IP público):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

En primer lugar, el patrón básico que coinciden con una sola dirección IP es la siguiente: \b###\.###\.###\.###\b

Extender esto, podemos encontrar dos direcciones IP diferentes con una expresión o como sigue: \b###\.###\.###\.###\b|\b###\.###\.###\.###\b

Por este motivo, sería un ejemplo para que coincida con tan solo dos direcciones (por ejemplo, 192.168.1.1 o 10.0.0.1): \b192\.168\.1\.1\b|\b10\.0\.0\.1\b

Esto le da la técnica que puede especificar cualquier número de direcciones. Cuando un intervalo de direcciones que debas permite, por ejemplo 192.168.1.1 – 192.168.1.25, establecer la coincidencia debe realizarse carácter por carácter: \b192\.168\.1\. ([1-9] | 1 [0-9] | 2 [0-5]) \b

>[!Note] 
>La dirección IP se trata como la cadena y no es un número.


La regla se desglosa como sigue: \b192\.168\.1\.

Esto coincide con cualquier valor que empieza por 192.168.1.

El siguiente coincide con los intervalos necesarios para la parte de la dirección después del último punto decimal:


- ([1-9] coincide con las direcciones que terminan en 1-9
- | 1 [0-9] coincide con direcciones que terminen en 10 19
- Direcciones de coincidencias |2[0-5]) que terminen en 20-25

>[!Note]
>Los paréntesis se deben colocar correctamente, para que no se inician coincidencia otras partes de direcciones IP.

Con el bloque 192 coinciden, podemos escribir una expresión similar para el bloque de 10: \b10\.0\.0\. ([1-9] | 1 [0-4]) \b

Y agrupamiento, la siguiente expresión debe coincidir con todas las direcciones de "192.168.1.1~25" y "10.0.0.1~14": \b192\.168\.1\. ([1-9]|1[0-9]|2[0-5])\b|\b10\.0\.0\. ([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>Pruebas de la expresión

Regex expresiones pueden tornarse muy complicadas, por lo que es muy recomendable mediante una herramienta de comprobación de la expresión regular. Si haces una búsqueda de "generador de expresiones regex en línea" internet, encontrarás varias utilidades buena en línea que te permitirá probar las expresiones de los datos de ejemplo.

Al probar la expresión, es importante comprenderlo que esperar a deben coincidir. El sistema de Exchange en línea puede enviar varias direcciones IP separadas por comas. Las expresiones proporcionadas anteriormente funcionará para esto. Sin embargo, es importante pensar esto cuando pruebes las expresiones de expresión regular. Por ejemplo, uno podría usar la siguiente muestra de entrada para comprobar los ejemplos anteriores: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>Validar la implementación

### <a name="security-audit-logs"></a>Registros de auditoría de seguridad

Para comprobar que el nuevo contexto de solicitud notificaciones se envían y están disponibles para la AD FS dice la canalización de procesamiento, habilita el registro en el servidor de AD FS de auditoría. A continuación, enviar algunas solicitudes de autenticación y comprueba si los valores de las notificaciones en las entradas del registro de auditoría de seguridad estándar. 

Para habilitar el registro de auditoría de eventos a la seguridad de sesión de un servidor de AD FS, sigue los pasos al configurar la auditoría de AD FS 2.0.

### <a name="event-logging"></a>Registro de eventos

De manera predeterminada, las solicitudes de errores se registran en el registro de eventos en registros de aplicaciones y servicios \ AD FS 2.0 \ Admin.For obtener más información sobre el registro de eventos de AD FS, consulte [configurar AD FS 2.0 registro de eventos](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configurar ADFS detallado registros de seguimiento

Eventos de seguimiento de AD FS se registran en el registro de depuración 2.0 de AD FS. Para habilitar el seguimiento, consulta [configurar el seguimiento de depuración de AD FS 2.0](https://technet.microsoft.com/en-us/library/adfs2-troubleshooting-configuring-computers.aspx).

Una vez habilitado el seguimiento, usa la siguiente sintaxis de línea de comandos para permitir que el nivel de registro detallado: wevtutil.exe sl "AD FS 2.0 seguimiento y depuración" /l: 5  

## <a name="related"></a>Relacionados
Para obtener más información sobre los tipos de notificación nueva consulta [tipos de notificaciones de AD FS](AD-FS-Claims-Types.md).
 
