---
ms.assetid: ''
title: Directivas de Control de acceso de cliente en servicios de federación de Active Directory 2.0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 216af933aee643ee56feff71c59d9ecc2e62998c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842996"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Directivas de Control de acceso de cliente de AD FS 2.0
Una directivas de acceso de cliente en Active Directory Federation Services 2.0 permiten restringir o conceder acceso a los usuarios a los recursos.  Este documento describe cómo habilitar las directivas de acceso de cliente de AD FS 2.0 y cómo configurar los escenarios más comunes.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Habilitar directiva de acceso de cliente de AD FS 2.0

Para habilitar la directiva de acceso de cliente, siga estos pasos.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Paso 1: Instalar la actualización acumulativa 2 para AD FS 2.0 del paquete en los servidores de AD FS

Descargue el [actualización acumulativa 2 para servicios de federación de Active Directory (AD FS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) paquete e instalarlo en todos los servidores de federación y servidores proxy de federación.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Paso 2: Agregue que cinco reglas de notificación a la confianza de proveedor de notificaciones de Active Directory

Una vez que la actualización acumulativa 2 se ha instalado en todos los servidores de AD FS y servidores proxy, utilice el procedimiento siguiente para agregar un conjunto de reglas de notificaciones que hace que los nuevos tipos de notificación esté disponible para el motor de directiva.

Para ello, va a agregar cinco reglas de transformación de aceptación para cada uno de los nuevos tipos de notificación de contexto de solicitud con el siguiente procedimiento.

En la confianza de proveedor de notificaciones de Active Directory, cree una nueva regla de transformación de aceptación para pasar a través de cada uno de los nuevos tipos de notificación de contexto de solicitud.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Para agregar una regla de notificación a Active Directory confianza de proveedor de notificaciones para cada uno de los cinco contexto tipos de notificación:


1. Haga clic en Inicio, elija programas, herramientas administrativas y, a continuación, haga clic en AD FS 2.0 administración.
2. En el árbol de consola, en AD FS 2.0 \ relaciones, haga clic en confianzas de proveedor de notificaciones, haga clic en Active Directory y, a continuación, haga clic en Editar reglas de notificación.
3. En el cuadro de diálogo Editar reglas de notificación, seleccione la ficha reglas de transformación de aceptación y, a continuación, haga clic en Agregar regla para iniciar al Asistente para la regla.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione paso a través o filtrar una notificación entrante en la lista y, a continuación, haga clic en siguiente.
5. En la página Configurar regla, bajo el nombre de la regla de notificación, escriba el nombre para mostrar para esta regla; en entrante tipo de notificación, escriba la dirección URL siguiente tipo de notificación y, a continuación, seleccione el paso a través de todos los valores de notificación.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Para comprobar la regla, selecciónelo en la lista y haga clic en Editar regla, haga clic en Ver lenguaje de reglas. El lenguaje de reglas de notificación debe aparecer como sigue: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Haga clic en Finalizar.
8. En el cuadro de diálogo Editar reglas de notificación, haga clic en Aceptar para guardar las reglas.
9. Repita los pasos del 2 al 6 para crear una regla de notificación adicionales para cada uno de los tipos de cuatro notificación restantes que se muestra a continuación, hasta que se han creado todos los cinco reglas.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Paso 3: Actualizar la plataforma de identidad de Microsoft Office 365 confianza

Elija uno de los escenarios de ejemplo siguientes para configurar las reglas de notificación en la plataforma de identidad de Microsoft Office 365 confianza que mejor satisfaga las necesidades de su organización.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Escenarios de directiva de acceso de cliente de AD FS 2.0
Las secciones siguientes describen los escenarios que existen para AD FS 2.0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Escenario 1: Bloquear todo acceso externo a Office 365

Este escenario de directiva de acceso de cliente permite el acceso de todos los clientes internos y bloques de todos los clientes externos según la dirección IP del cliente externo. El conjunto de reglas se basa en la regla de autorización de emisión predeterminada permitir el acceso a todos los usuarios. Puede usar el siguiente procedimiento para agregar una regla de autorización de emisión a la confianza de Office 365.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para crear una regla para bloquear todo acceso externo a Office 365



1. Haga clic en Inicio, elija programas, herramientas administrativas y, a continuación, haga clic en AD FS 2.0 administración.
2. En el árbol de consola, en AD FS 2.0 \ relaciones, haga clic en autenticado, haga clic en la confianza de la plataforma de identidad de Microsoft Office 365 y, a continuación, haga clic en Editar reglas de notificación. 
3. En el cuadro de diálogo Editar reglas de notificación, seleccione la ficha reglas de autorización de emisión y, a continuación, haga clic en Agregar regla para iniciar al Asistente para la regla de notificación.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione Enviar notificaciones mediante regla personalizada y, a continuación, haga clic en siguiente.
5. En la página Configurar regla, bajo el nombre de la regla de notificación, escriba el nombre para mostrar para esta regla. En regla personalizada, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Haga clic en Finalizar. Compruebe que la nueva regla aparece inmediatamente debajo de la opción Permitir el acceso a la regla de todos los usuarios en la lista de reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo Editar reglas de notificación, haga clic en Aceptar.

>[!NOTE]
>Tendrá que reemplazar el valor anterior de "regex de dirección ip pública" con una expresión de IP válida; vea Creación de la expresión de intervalo de direcciones IP para obtener más información.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Escenario 2: Bloquear todo acceso externo a Office 365 excepto Exchange ActiveSync

El ejemplo siguiente permite el acceso a todas las aplicaciones de Office 365, incluidos Exchange Online, de los clientes internos, incluido Outlook. Bloquea el acceso de los clientes que se encuentran fuera de la red corporativa, tal y como indica la dirección IP del cliente, excepto para los clientes de Exchange ActiveSync, como teléfonos inteligentes. El conjunto de reglas se basa en la regla de autorización de emisión predeterminada denominada Permitir el acceso a todos los usuarios. Para agregar una regla de autorización de emisión a la relación de confianza con el Asistente para la regla de notificación de usuario de confianza de Office 365, siga estos pasos:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para crear una regla para bloquear todo acceso externo a Office 365



1. Haga clic en Inicio, elija programas, herramientas administrativas y, a continuación, haga clic en AD FS 2.0 administración.
2. En el árbol de consola, en AD FS 2.0 \ relaciones, haga clic en autenticado, haga clic en la confianza de la plataforma de identidad de Microsoft Office 365 y, a continuación, haga clic en Editar reglas de notificación. 
3. En el cuadro de diálogo Editar reglas de notificación, seleccione la ficha reglas de autorización de emisión y, a continuación, haga clic en Agregar regla para iniciar al Asistente para la regla de notificación.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione Enviar notificaciones mediante regla personalizada y, a continuación, haga clic en siguiente.
5. En la página Configurar regla, bajo el nombre de la regla de notificación, escriba el nombre para mostrar para esta regla. En regla personalizada, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haga clic en Finalizar. Compruebe que la nueva regla aparece inmediatamente debajo de la opción Permitir el acceso a la regla de todos los usuarios en la lista de reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo Editar reglas de notificación, haga clic en Aceptar.

>[!NOTE]
>Tendrá que reemplazar el valor anterior de "regex de dirección ip pública" con una expresión de IP válida; vea Creación de la expresión de intervalo de direcciones IP para obtener más información.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Escenario 3: Bloquear todo acceso externo a Office 365 excepto las aplicaciones basadas en explorador

El conjunto de reglas se basa en la regla de autorización de emisión predeterminada denominada Permitir el acceso a todos los usuarios. Siga estos pasos para agregar una regla de autorización de emisión a la plataforma de identidad de Microsoft Office 365 confianza mediante el Asistente para la regla de notificación:

>[!NOTE]
>Este escenario no es compatible con un proxy de terceros debido a limitaciones en los encabezados de directiva de acceso de cliente con solicitudes pasivas (basada en Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para crear una regla para bloquear todo acceso externo a Office 365 excepto las aplicaciones basadas en explorador



1. Haga clic en Inicio, elija programas, herramientas administrativas y, a continuación, haga clic en AD FS 2.0 administración.
2. En el árbol de consola, en AD FS 2.0 \ relaciones, haga clic en autenticado, haga clic en la confianza de la plataforma de identidad de Microsoft Office 365 y, a continuación, haga clic en Editar reglas de notificación. 
3. En el cuadro de diálogo Editar reglas de notificación, seleccione la ficha reglas de autorización de emisión y, a continuación, haga clic en Agregar regla para iniciar al Asistente para la regla de notificación.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione Enviar notificaciones mediante regla personalizada y, a continuación, haga clic en siguiente.
5. En la página Configurar regla, bajo el nombre de la regla de notificación, escriba el nombre para mostrar para esta regla. En regla personalizada, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haga clic en Finalizar. Compruebe que la nueva regla aparece inmediatamente debajo de la opción Permitir el acceso a la regla de todos los usuarios en la lista de reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo Editar reglas de notificación, haga clic en Aceptar.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Escenario 4: Bloquear todo acceso externo a Office 365 para los grupos de Active Directory designados

El ejemplo siguiente habilita el acceso de clientes internos en función de la dirección IP. Bloquea el acceso de los clientes que se encuentran fuera de la red corporativa que tengan una dirección IP de cliente externo, excepto para las personas de una regla de grupo de Active Directory especificada conjunto se basa en la regla de autorización de emisión predeterminada denominada Permitir el acceso a Todos los usuarios. Siga estos pasos para agregar una regla de autorización de emisión a la plataforma de identidad de Microsoft Office 365 confianza mediante el Asistente para la regla de notificación:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Para crear una regla para bloquear todo acceso externo a Office 365 para los grupos de Active Directory designados



1. Haga clic en Inicio, elija programas, herramientas administrativas y, a continuación, haga clic en AD FS 2.0 administración.
2. En el árbol de consola, en AD FS 2.0 \ relaciones, haga clic en autenticado, haga clic en la confianza de la plataforma de identidad de Microsoft Office 365 y, a continuación, haga clic en Editar reglas de notificación. 
3. En el cuadro de diálogo Editar reglas de notificación, seleccione la ficha reglas de autorización de emisión y, a continuación, haga clic en Agregar regla para iniciar al Asistente para la regla de notificación.
4. En la página Seleccionar plantilla de regla, en la plantilla de regla de notificación, seleccione Enviar notificaciones mediante regla personalizada y, a continuación, haga clic en siguiente.
5. En la página Configurar regla, bajo el nombre de la regla de notificación, escriba el nombre para mostrar para esta regla. En regla personalizada, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haga clic en Finalizar. Compruebe que la nueva regla aparece inmediatamente debajo de la opción Permitir el acceso a la regla de todos los usuarios en la lista de reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo Editar reglas de notificación, haga clic en Aceptar.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descripciones de la sintaxis de lenguaje de reglas de notificación utilizado en los escenarios anteriores

|Descripción|Sintaxis de lenguaje de reglas de notificación|
|-----|-----| 
|Regla de FS AD de forma predeterminada para permitir el acceso a todos los usuarios. Esta regla ya debe existir en la plataforma de identidad de Microsoft Office 365 confianza lista de reglas de autorización de emisión.|= > problema (tipo = "https://schemas.microsoft.com/authorization/claims/permit", valor = "true");| 
|Agregue esta cláusula a una nueva regla personalizada especifica que la solicitud procede de servidor proxy de federación (es decir, tiene el encabezado x-ms-proxy)
Se recomienda que todas las reglas de incluyen.|exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"])| 
|Se usa para establecer que la solicitud proviene de un cliente con una dirección IP del intervalo aceptable definido.|NO existe ([tipo == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", valor = ~ "regex de dirección ip pública proporcionado por el cliente"])| 
|Esta cláusula se utiliza para especificar que si la aplicación que se obtiene acceso no es Microsoft.Exchange.ActiveSync se debe denegar la solicitud.|NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value=="Microsoft.Exchange.ActiveSync"])| 
|Esta regla permite determinar si la llamada se realizó utilizando un explorador Web y no se denegará.|NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])| 
|Esta regla indica que deben denegarse los únicos usuarios de un determinado grupo de Active Directory (según el valor de SID). Agregar en la declaración no significa que se permitirá un grupo de usuarios, independientemente de la ubicación.|exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "{Group SID value of allowed AD group}"])| 
|Se trata de una cláusula necesaria para emitir un permiso deny cuando se cumplen todas las condiciones anteriores.|= > problema (tipo = "https://schemas.microsoft.com/authorization/claims/deny", valor = "true");|

### <a name="building-the-ip-address-range-expression"></a>Creación de la expresión de intervalo de direcciones IP

La notificación de x-ms-reenviados-ip del cliente se rellena con un encabezado HTTP que está establecido actualmente solo mediante Exchange Online, que llena el encabezado al pasar la solicitud de autenticación para AD FS. El valor de la notificación puede ser uno de los siguientes:

>[!Note] 
>Exchange Online solo direcciones IPV4 e IPV6 no admite actualmente.

Una única dirección IP: La dirección IP del cliente que se conecta directamente a Exchange Online

>[!Note] 
>La dirección IP de un cliente en la red corporativa aparecerá como la dirección IP de interfaz externa de la puerta de enlace o proxy de salida de la organización.

Los clientes que están conectados a la red corporativa mediante una VPN o por Microsoft DirectAccess (DA) pueden aparecer como clientes corporativos internos o clientes externos según la configuración de VPN o DA.

Una o varias direcciones IP: Cuando Exchange Online no puede determinar la dirección IP del cliente que se conecta, establecerá el valor en función del valor del encabezado x-forwarded-for, un encabezado no estándar que puede incluirse en las solicitudes basadas en HTTP y es compatible con muchos clientes, los equilibradores de carga y servidores proxy en el mercado.

>[!Note]
>Varias direcciones IP, que indica la dirección IP del cliente y la dirección de cada servidor proxy que pasa la solicitud, estarán separadas por una coma.

Las direcciones IP relacionadas con la infraestructura de Exchange Online no aparecerá en la lista.


#### <a name="regular-expressions"></a>Expresiones regulares

Una vez para que coincida con un intervalo de direcciones IP, es necesario construir una expresión regular para llevar a cabo la comparación. En la siguiente serie de pasos, proporcionamos ejemplos sobre cómo construir una expresión para que coincida con los siguientes intervalos de direcciones (tenga en cuenta que tendrá que cambiar estos ejemplos para que coincida con el intervalo IP público):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

En primer lugar, el patrón básico que coincidirá con una única dirección IP es como sigue: \b###\.###\.###\.\b ###

Al ampliar esto, podemos hacer coincidir dos direcciones IP diferentes con una expresión OR como sigue: \b###\.###\.###\.### \b|\b###\. ### \. ### \.\b ###

Por lo tanto, sería un ejemplo para que coincida con solo dos direcciones (por ejemplo, 192.168.1.1 o 10.0.0.1): \b192\.168\.1\.1\b | \b10\.0\.0\.1\b

Esto le ofrece la técnica que puede escribir cualquier número de direcciones. Cuando un intervalo de direcciones necesita permite, por ejemplo, 192.168.1.1 – 192.168.1.25, la coincidencia debe realizarse carácter por carácter: \b192\.168\.1\.([1-9] | [0-9] 1 | 2 [0-5]) \b

>[!Note] 
>La dirección IP se trata como cadena y no un número.


La regla se desglosa como sigue: \b192\.168\.1\.

Esto coincide con cualquier valor que empieza por 192.168.1.

El siguiente coincide con los intervalos necesarios para la parte de la dirección después del separador decimal final:


- ([1-9] coincide con las direcciones que terminen en 1-9
- | 1 [0-9] coincide con las direcciones que terminan en 10-19
- Direcciones de coincidencias de |2[0-5]) terminen en 20 a 25

>[!Note]
>Los paréntesis se deben colocar correctamente, por lo que no se inician otras partes de direcciones IP coincidentes.

Con el bloque 192 coincidente, podemos escribir una expresión similar para el bloque de 10: \b10\.0\.0\.([1-9] | [0-4] 1) \b

Y colocarlos juntos, la siguiente expresión debe coincidir todas las direcciones de "192.168.1.1~25" y "10.0.0.1~14": \b192\.168\.1\.([1-9] | [0-9] 1 | 2 [0-5]) \b|\b10\.0\.0\. ([1-9] | [0-4] 1) \b

#### <a name="testing-the-expression"></a>Probar la expresión

Las expresiones de Regex pueden ser bastante complicadas, por lo que se recomienda usar una herramienta de comprobación de regex. Si lo hace una búsqueda de "generador de expresiones de regex en línea" internet, encontrará varias utilidades buena en línea que le permitirá probar las expresiones con datos de ejemplo.

Al probar la expresión, es importante que comprenda lo que puede esperar que deben coincidir. El sistema de Exchange online puede enviar muchas direcciones IP separadas por comas. Las expresiones proporcionadas anteriormente funcionará para esto. Sin embargo, es importante pensar en esto al probar las expresiones de regex. Por ejemplo, se podría usar el siguiente ejemplo de entrada para comprobar los ejemplos anteriores: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>Validar la implementación

### <a name="security-audit-logs"></a>Registros de auditoría de seguridad

Para comprobar que el nuevo contexto de solicitud notificaciones se envían y están disponibles para la instancia de AD FS de notificaciones de canalización de procesamiento, habilite la auditoría de registro en el servidor de AD FS. A continuación, enviar algunas solicitudes de autenticación y comprobación de los valores de notificación en las entradas del registro de auditoría de seguridad estándar. 

Para habilitar el registro de auditoría de eventos a la seguridad de registro en un servidor de AD FS, siga los pasos de configuración de la auditoría de AD FS 2.0.

### <a name="event-logging"></a>Registro de eventos

De forma predeterminada, las solicitudes con error se registran en el registro de eventos de aplicación ubicado en registros de aplicaciones y servicios \ AD FS 2.0 \ Admin.For obtener más información sobre el registro de eventos de AD FS, consulte [configurar AD FS 2.0 el registro de eventos](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configuración de detallado AD FS registros de seguimiento

Los eventos de seguimiento de AD FS se registran en el registro de AD FS 2.0 de depuración. Para habilitar el seguimiento, vea [configurar el seguimiento de depuración de AD FS 2.0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

Después de haber habilitado el seguimiento, use la siguiente sintaxis de línea de comandos para habilitar el nivel de registro detallado: wevtutil.exe sl "AD FS 2.0 seguimiento y depuración" /l:5  

## <a name="related"></a>Relacionados
Para obtener más información sobre los nuevos tipos de notificación vea [tipos de notificaciones de AD FS](AD-FS-Claims-Types.md).
 
