---
ms.assetid: ''
title: Directivas de Access Control de cliente en Servicios de federación de Active Directory (AD FS) 2,0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4f5d2cfa8383bcf3c0813b272f8c4828473b8df9
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948607"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Directivas de Access Control de cliente en AD FS 2,0
Las directivas de acceso de cliente en Servicios de federación de Active Directory (AD FS) 2,0 permiten restringir o conceder a los usuarios acceso a los recursos.  En este documento se describe cómo habilitar las directivas de acceso de cliente en AD FS 2,0 y cómo configurar los escenarios más comunes.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Habilitación de la Directiva de acceso de cliente en AD FS 2,0

Para habilitar la Directiva de acceso de cliente, siga los pasos que se indican a continuación.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Paso 1: instalar el paquete acumulativo de actualizaciones 2 para AD FS paquete 2,0 en los servidores de AD FS

Descargue el paquete [acumulativo de actualizaciones 2 para servicios de Federación de Active Directory (AD FS) (AD FS) 2,0](https://support.microsoft.com/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) e instálelo en todos los servidores proxy de Federación y de servidor de Federación.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Paso 2: agregar cinco reglas de notificación a la Active Directory confianza del proveedor de notificaciones

Una vez que se haya instalado el paquete acumulativo de actualizaciones 2 en todos los servidores y servidores proxy de AD FS, utilice el siguiente procedimiento para agregar un conjunto de reglas de notificaciones que permita que los nuevos tipos de notificaciones estén disponibles para el motor de directivas.

Para ello, va a agregar cinco reglas de transformación de aceptación para cada uno de los nuevos tipos de notificaciones de contexto de solicitud mediante el procedimiento siguiente.

En la Active Directory confianza del proveedor de notificaciones, cree una nueva regla de transformación de aceptación para pasar por cada uno de los nuevos tipos de notificaciones del contexto de solicitud.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Para agregar una regla de notificación a la Active Directory confianza del proveedor de notificaciones para cada uno de los cinco tipos de notificación de contexto:


1. Haga clic en Inicio, seleccione programas, herramientas administrativas y, a continuación, haga clic en AD FS administración de 2,0.
2. En el árbol de consola, en AD FS 2.0 \ relaciones de confianza, haga clic en confianzas de proveedor de notificaciones, haga clic con el botón secundario en Active Directory y, a continuación, haga clic en editar reglas de notificación.
3. En el cuadro de diálogo editar reglas de notificaciones, seleccione la pestaña reglas de transformación de aceptación y, a continuación, haga clic en Agregar regla para iniciar el Asistente para reglas.
4. En la página Seleccionar plantilla de regla, en plantilla de regla de notificaciones, seleccione pasar o filtrar una notificaciones entrantes de la lista y, a continuación, haga clic en siguiente.
5. En la página configurar regla, en nombre de la regla de notificaciones, escriba el nombre para mostrar de esta regla. en tipo de notificaciones entrantes, escriba la siguiente dirección URL de tipo de notificaciones y, después, seleccione pasar por todos los valores de notificaciones.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Para comprobar la regla, selecciónela en la lista, haga clic en Editar regla y, a continuación, haga clic en ver lenguaje de reglas. El lenguaje de reglas de notificaciones debe aparecer de la siguiente manera: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Haga clic en Finalizar.
8. En el cuadro de diálogo editar reglas de notificaciones, haga clic en Aceptar para guardar las reglas.
9. Repita los pasos del 2 al 6 para crear una regla de notificaciones adicional para cada uno de los cuatro tipos de notificaciones restantes que se muestran a continuación hasta que se hayan creado las cinco reglas.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Paso 3: actualización de la relación de confianza para usuario autenticado de la plataforma de identidad Microsoft Office 365

Elige uno de los siguientes escenarios de ejemplo para configurar las reglas de notificaciones en la Microsoft Office 365 confianza del usuario de confianza de la plataforma de identidad que mejor se adapte a las necesidades de tu organización.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Escenarios de directivas de acceso de cliente para AD FS 2,0
En las secciones siguientes se describen los escenarios que existen para AD FS 2,0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Escenario 1: bloquear todo el acceso externo a Office 365

Este escenario de directiva de acceso de cliente permite el acceso desde todos los clientes internos y bloquea todos los clientes externos en función de la dirección IP del cliente externo. El conjunto de reglas se basa en la regla de autorización de emisión predeterminada para permitir el acceso a todos los usuarios. Puede usar el siguiente procedimiento para agregar una regla de autorización de emisión a la relación de confianza para usuario autenticado de Office 365.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para crear una regla para bloquear todo el acceso externo a Office 365



1. Haga clic en Inicio, seleccione programas, herramientas administrativas y, a continuación, haga clic en AD FS administración de 2,0.
2. En el árbol de consola, en AD FS 2.0 \ relaciones de confianza, haga clic en confianzas para usuario autenticado, haga clic con el botón secundario en la Microsoft Office 365 Identity Platform Trust y, a continuación, haga clic en editar reglas de notificaciones. 
3. En el cuadro de diálogo editar reglas de notificaciones, seleccione la pestaña reglas de autorización de emisión y, a continuación, haga clic en Agregar regla para iniciar el Asistente para reglas de notificaciones.
4. En la página Seleccionar plantilla de regla, en plantilla de regla de notificación, seleccione enviar notificaciones mediante una regla personalizada y, a continuación, haga clic en siguiente.
5. En la página configurar regla, en nombre de la regla de notificaciones, escriba el nombre para mostrar de esta regla. En regla personalizada, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Haga clic en Finalizar. Compruebe que la nueva regla aparece inmediatamente debajo de la regla permitir el acceso a todos los usuarios de la lista reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo editar reglas de notificaciones, haga clic en Aceptar.

>[!NOTE]
>Tendrá que reemplazar el valor anterior para "Regex de dirección IP pública" por una expresión IP válida; vea crear la expresión de intervalo de direcciones IP para obtener más información.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Escenario 2: bloquear todo el acceso externo a Office 365 excepto Exchange ActiveSync

En el ejemplo siguiente se permite el acceso a todas las aplicaciones de Office 365, incluido Exchange Online, desde clientes internos, incluido Outlook. Bloquea el acceso desde los clientes que se encuentran fuera de la red corporativa, como indica la dirección IP del cliente, excepto en el caso de los clientes de Exchange ActiveSync, como smartphones. El conjunto de reglas se basa en la regla de autorización de emisión predeterminada titulada permitir el acceso a todos los usuarios. Siga estos pasos para agregar una regla de autorización de emisión a la relación de confianza para usuario autenticado de Office 365 mediante el Asistente para reglas de notificaciones:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para crear una regla para bloquear todo el acceso externo a Office 365



1. Haga clic en Inicio, seleccione programas, herramientas administrativas y, a continuación, haga clic en AD FS administración de 2,0.
2. En el árbol de consola, en AD FS 2.0 \ relaciones de confianza, haga clic en confianzas para usuario autenticado, haga clic con el botón secundario en la Microsoft Office 365 Identity Platform Trust y, a continuación, haga clic en editar reglas de notificaciones. 
3. En el cuadro de diálogo editar reglas de notificaciones, seleccione la pestaña reglas de autorización de emisión y, a continuación, haga clic en Agregar regla para iniciar el Asistente para reglas de notificaciones.
4. En la página Seleccionar plantilla de regla, en plantilla de regla de notificación, seleccione enviar notificaciones mediante una regla personalizada y, a continuación, haga clic en siguiente.
5. En la página configurar regla, en nombre de la regla de notificaciones, escriba el nombre para mostrar de esta regla. En regla personalizada, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haga clic en Finalizar. Compruebe que la nueva regla aparece inmediatamente debajo de la regla permitir el acceso a todos los usuarios de la lista reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo editar reglas de notificaciones, haga clic en Aceptar.

>[!NOTE]
>Tendrá que reemplazar el valor anterior para "Regex de dirección IP pública" por una expresión IP válida; vea crear la expresión de intervalo de direcciones IP para obtener más información.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Escenario 3: bloquear todo el acceso externo a Office 365 excepto las aplicaciones basadas en explorador

El conjunto de reglas se basa en la regla de autorización de emisión predeterminada titulada permitir el acceso a todos los usuarios. Siga estos pasos para agregar una regla de autorización de emisión a la relación de confianza para usuario autenticado de la plataforma de identidad Microsoft Office 365 mediante el Asistente para reglas de notificaciones:

>[!NOTE]
>Este escenario no es compatible con un proxy de terceros debido a las limitaciones de los encabezados de directiva de acceso de cliente con solicitudes pasivas (basadas en Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para crear una regla que bloquee todo el acceso externo a Office 365 excepto las aplicaciones basadas en explorador



1. Haga clic en Inicio, seleccione programas, herramientas administrativas y, a continuación, haga clic en AD FS administración de 2,0.
2. En el árbol de consola, en AD FS 2.0 \ relaciones de confianza, haga clic en confianzas para usuario autenticado, haga clic con el botón secundario en la Microsoft Office 365 Identity Platform Trust y, a continuación, haga clic en editar reglas de notificaciones. 
3. En el cuadro de diálogo editar reglas de notificaciones, seleccione la pestaña reglas de autorización de emisión y, a continuación, haga clic en Agregar regla para iniciar el Asistente para reglas de notificaciones.
4. En la página Seleccionar plantilla de regla, en plantilla de regla de notificación, seleccione enviar notificaciones mediante una regla personalizada y, a continuación, haga clic en siguiente.
5. En la página configurar regla, en nombre de la regla de notificaciones, escriba el nombre para mostrar de esta regla. En regla personalizada, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haga clic en Finalizar. Compruebe que la nueva regla aparece inmediatamente debajo de la regla permitir el acceso a todos los usuarios de la lista reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo editar reglas de notificaciones, haga clic en Aceptar.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Escenario 4: bloquear todo el acceso externo a Office 365 para los grupos de Active Directory designados

En el ejemplo siguiente se habilita el acceso desde clientes internos basados en la dirección IP. Bloquea el acceso desde los clientes que se encuentran fuera de la red corporativa que tienen una dirección IP de cliente externa, excepto para las personas de un grupo de Active Directory especificado. el conjunto de reglas se basa en la regla de autorización de emisión predeterminada titulada acceso a Todos los usuarios. Siga estos pasos para agregar una regla de autorización de emisión a la relación de confianza para usuario autenticado de la plataforma de identidad Microsoft Office 365 mediante el Asistente para reglas de notificaciones:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Para crear una regla para bloquear todo el acceso externo a Office 365 para los grupos de Active Directory designados



1. Haga clic en Inicio, seleccione programas, herramientas administrativas y, a continuación, haga clic en AD FS administración de 2,0.
2. En el árbol de consola, en AD FS 2.0 \ relaciones de confianza, haga clic en confianzas para usuario autenticado, haga clic con el botón secundario en la Microsoft Office 365 Identity Platform Trust y, a continuación, haga clic en editar reglas de notificaciones. 
3. En el cuadro de diálogo editar reglas de notificaciones, seleccione la pestaña reglas de autorización de emisión y, a continuación, haga clic en Agregar regla para iniciar el Asistente para reglas de notificaciones.
4. En la página Seleccionar plantilla de regla, en plantilla de regla de notificación, seleccione enviar notificaciones mediante una regla personalizada y, a continuación, haga clic en siguiente.
5. En la página configurar regla, en nombre de la regla de notificaciones, escriba el nombre para mostrar de esta regla. En regla personalizada, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Haga clic en Finalizar. Compruebe que la nueva regla aparece inmediatamente debajo de la regla permitir el acceso a todos los usuarios de la lista reglas de autorización de emisión.
7. Para guardar la regla, en el cuadro de diálogo editar reglas de notificaciones, haga clic en Aceptar.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descripciones de la sintaxis del lenguaje de reglas de notificaciones que se usa en los escenarios anteriores

|                                                                                                   Descripción                                                                                                   |                                                                     Sintaxis del lenguaje de reglas de notificaciones                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              La regla AD FS predeterminada para permitir el acceso a todos los usuarios. Esta regla debe existir ya en la lista de reglas de autorización de emisión de confianza del usuario de confianza de la plataforma de identidad Microsoft Office 365.              |                                  = > problema (Type = "<https://schemas.microsoft.com/authorization/claims/permit>", Value = "true");                                   |
|                               Al agregar esta cláusula a una nueva regla personalizada, se especifica que la solicitud proviene del servidor proxy de Federación (es decir, que tiene el encabezado x-MS-proxy)                                |                                                                                                                                                                    |
|                                                                                 Se recomienda que todas las reglas lo incluyan.                                                                                  |                                    exists ([type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy>"])                                    |
|                                                         Se usa para establecer que la solicitud proviene de un cliente con una dirección IP en el intervalo aceptable definido.                                                         | NOT exists ([type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip>", Value = ~ "Regex de dirección IP pública proporcionada por el cliente"]) |
|                                    Esta cláusula se usa para especificar que si la aplicación a la que se tiene acceso no es Microsoft. Exchange. ActiveSync, se debe denegar la solicitud.                                     |       NOT exists ([type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application>", Value = = "Microsoft. Exchange. ActiveSync"])        |
|                                                      Esta regla permite determinar si la llamada se realizó a través de un explorador Web y no se denegará.                                                      |              NOT exists ([type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path>", Value = = "/ADFS/LS/"])               |
| Esta regla indica que se deben denegar los únicos usuarios de un grupo de Active Directory determinado (según el valor de SID). Agregar no a esta instrucción significa que se permite un grupo de usuarios, independientemente de la ubicación. |             exists ([type = = "<https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid>", Value = ~ "{Grupo SID valor del grupo de AD permitido}"])              |
|                                                                Esta es una cláusula necesaria para emitir una denegación cuando se cumplan todas las condiciones anteriores.                                                                 |                                   = > problema (Type = "<https://schemas.microsoft.com/authorization/claims/deny>", Value = "true");                                    |

### <a name="building-the-ip-address-range-expression"></a>Compilar la expresión de intervalo de direcciones IP

La solicitud x-MS-forwarded-Client-IP se rellena a partir de un encabezado HTTP que está establecido actualmente en Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación a AD FS. El valor de la demanda puede ser uno de los siguientes:

>[!Note] 
>Actualmente, Exchange Online solo admite direcciones IPV4 y no IPV6.

Una única dirección IP: la dirección IP del cliente que está conectada directamente a Exchange Online

>[!Note] 
>La dirección IP de un cliente de la red corporativa aparecerá como la dirección IP de la interfaz externa del proxy o la puerta de enlace de salida de la organización.

Los clientes que están conectados a la red corporativa mediante una VPN o Microsoft DirectAccess (DA) pueden aparecer como clientes corporativos internos o como clientes externos, en función de la configuración de VPN o DA.

Una o varias direcciones IP: cuando Exchange online no puede determinar la dirección IP del cliente que se conecta, establecerá el valor según el valor del encabezado x-forwarded-for, un encabezado no estándar que se puede incluir en las solicitudes basadas en HTTP y es compatible con muchos clientes, equilibradores de carga y servidores proxy en el mercado.

>[!Note]
>Varias direcciones IP, que indican la dirección IP del cliente y la dirección de cada proxy que pasó la solicitud, se separan con una coma.

Las direcciones IP relacionadas con la infraestructura de Exchange online no aparecerán en la lista.


#### <a name="regular-expressions"></a>Expresiones regulares

Cuando tiene que coincidir con un intervalo de direcciones IP, es necesario construir una expresión regular para realizar la comparación. En la siguiente serie de pasos, proporcionaremos ejemplos sobre cómo construir una expresión de este tipo para que coincida con los intervalos de direcciones siguientes (tenga en cuenta que tendrá que cambiar estos ejemplos para que coincidan con el intervalo de IP pública):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1:10.0.0.14

En primer lugar, el patrón básico que coincidirá con una sola dirección IP es el siguiente: \b # # #\.###\.###\.# # # \b

Extendiendo esto, podemos hacer coincidir dos direcciones IP diferentes con una expresión o como se indica a continuación: \b # # #\.###\.###\.# # # \b | \b # # #\.###\.###\.# # # \b

Por lo tanto, un ejemplo que coincida con solo dos direcciones (como 192.168.1.1 o 10.0.0.1) sería: \b192\.168\.1\.1 \ b | \b10\.0\.0\.1 \ b

Esto le ofrece la técnica por la que puede escribir cualquier número de direcciones. En los casos en los que es necesario permitir un intervalo de direcciones (por ejemplo, 192.168.1.1 – 192.168.1.25), la coincidencia se debe hacer carácter por carácter: \b192\.168\.1\.([1-9] | 1 [0-9] | 2 [0-5]) \b

>[!Note] 
>La dirección IP se trata como una cadena y no como un número.


La regla se desglosa de la siguiente manera: \b192\.168\.1\.

Coincide con cualquier valor que comience por 192.168.1.

El siguiente valor coincide con los intervalos necesarios para la parte de la dirección después del último separador decimal:


- ([1-9] coincide con las direcciones que terminan en 1-9
- | 1 [0-9] coincide con las direcciones que terminan en 10-19
- | 2 [0-5]) coincide con las direcciones que terminan en 20-25

>[!Note]
>Los paréntesis deben estar colocados correctamente, de modo que no empiece a buscar coincidencias con otras partes de direcciones IP.

Con el bloque 192 coincidente, podemos escribir una expresión similar para el bloque 10: \b10\.0\.0\.([1-9] | 1 [0-4]) \b

Además, la expresión siguiente debe coincidir con todas las direcciones de "192.168.1.1 ~ 25" y "10.0.0.1 ~ 14": \b192\.168\.1\.([1-9] | 1 [0-9] | 2 [0-5]) \b | \b10\.0\.0\.([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>Probar la expresión

Las expresiones regex pueden resultar bastante complicadas, por lo que se recomienda encarecidamente usar una herramienta de comprobación de Regex. Si realiza una búsqueda en Internet de "generador de expresiones regex en línea", encontrará varias utilidades en línea adecuadas que le permitirán probar sus expresiones con datos de ejemplo.

Al probar la expresión, es importante que comprenda qué se espera que coincida. El sistema de Exchange Online puede enviar muchas direcciones IP, separadas por comas. Las expresiones proporcionadas anteriormente funcionarán para este. Sin embargo, es importante pensar en esto al probar las expresiones Regex. Por ejemplo, puede usar la siguiente entrada de ejemplo para comprobar los ejemplos anteriores: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10, 0.0.1 











## <a name="validating-the-deployment"></a>Validación de la implementación

### <a name="security-audit-logs"></a>Registros de auditoría de seguridad

Para comprobar que las nuevas notificaciones de contexto de solicitud se envían y están disponibles para el AD FS canalización de procesamiento de notificaciones, habilite el registro de auditoría en el servidor de AD FS. A continuación, envíe algunas solicitudes de autenticación y compruebe los valores de notificaciones en las entradas del registro de auditoría de seguridad estándar. 

Para habilitar el registro de eventos de auditoría en el registro de seguridad en un servidor de AD FS, siga los pasos descritos en configuración de la auditoría de AD FS 2,0.

### <a name="event-logging"></a>Registro de eventos

De forma predeterminada, las solicitudes con error se registran en el registro de eventos de aplicación que se encuentra en registros de aplicaciones y servicios \ AD FS 2,0 \ administrador. para obtener más información sobre el registro de eventos para AD FS, vea [configurar el registro de eventos de AD FS 2,0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configuración de registros de seguimiento de AD FS detallado

AD FS eventos de seguimiento se registran en el registro de depuración de AD FS 2,0. Para habilitar el seguimiento, vea [configurar el seguimiento de depuración para AD FS 2,0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

Después de habilitar el seguimiento, use la siguiente sintaxis de línea de comandos para habilitar el nivel de registro detallado: wevtutil. exe SL "AD FS 2,0 Tracing/debug"/l: 5  

## <a name="related"></a>Relacionados
Para obtener más información sobre los nuevos tipos de notificación, consulte [AD FS tipos de notificaciones](AD-FS-Claims-Types.md).

