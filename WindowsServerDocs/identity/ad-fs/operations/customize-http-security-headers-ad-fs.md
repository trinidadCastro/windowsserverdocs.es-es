---
title: Personalizar encabezados de respuesta de seguridad HTTP con AD FS
description: Este documento descirbes cómo personalizar los encabezados de seguridad para protegerse frente a vulnerabilidades de seguridad.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7c85339c10a8546705edd2d064e34cbf5c0838d4
ms.sourcegitcommit: 2cc251eb5bc3069bf09bc08e06c3478fcbe1f321
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84333926"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>Personalización de encabezados de respuesta de seguridad HTTP con AD FS 2019 
 
Para proteger contra las vulnerabilidades de seguridad comunes y proporcionar a los administradores la posibilidad de aprovechar los últimos avances en los mecanismos de protección basados en explorador, AD FS 2019 agregó la funcionalidad para personalizar los encabezados de respuesta de seguridad HTTP enviados por AD FS. Esto se logra mediante la introducción de dos nuevos cmdlets: `Get-AdfsResponseHeaders` y `Set-AdfsResponseHeaders` .  

>[!NOTE]
>La funcionalidad para personalizar los encabezados de respuesta de seguridad HTTP (excepto los encabezados CORS) mediante cmdlets: `Get-AdfsResponseHeaders` y `Set-AdfsResponseHeaders` se ha trasladado a AD FS 2016. Puede Agregar la funcionalidad a la AD FS 2016 mediante la instalación de [KB4493473](https://support.microsoft.com/help/4493473/windows-10-update-kb4493473) y [KB4507459](https://support.microsoft.com/help/4507459/windows-10-update-kb4507459). 

En este documento se tratarán los encabezados de respuesta de seguridad que se usan habitualmente para demostrar cómo personalizar los encabezados enviados por AD FS 2019.   
 
>[!NOTE]
>En el documento se da por supuesto que se ha instalado AD FS 2019.  

 
Antes de analizar los encabezados, echemos un vistazo a algunos escenarios para crear la necesidad de que los administradores personalicen los encabezados de seguridad. 
 
## <a name="scenarios"></a>Escenarios 
1. El administrador ha habilitado [**http STRICT-Transport-Security (HSTS)**](#http-strict-transport-security-hsts) (fuerza a todas las conexiones a través del cifrado https) para proteger a los usuarios que pueden tener acceso a la aplicación Web mediante http desde un punto de acceso Wi-Fi público que podría ser atacado. Les gustaría reforzar aún más la seguridad habilitando HSTS para los subdominios.  
2. El administrador ha configurado el encabezado de respuesta [**X-Frame-Options**](#x-frame-options) (impide la representación de una página web en un iframe) para proteger las páginas web de clickjacked. Sin embargo, necesitan personalizar el valor del encabezado debido a un nuevo requisito empresarial para mostrar los datos (en iFrame) de una aplicación con un origen diferente (dominio).
3. El administrador ha habilitado [**X-XSS-Protection**](#x-xss-protection) (evita ataques de scripting cruzados) para corregir y bloquear la página si el explorador detecta ataques de scripting cruzados. Sin embargo, necesitan personalizar el encabezado para permitir que la página se cargue una vez sanear.  
4. El administrador debe habilitar el [**uso compartido de recursos entre orígenes (CORS)**](#cross-origin-resource-sharing-cors-headers) y establecer el origen (dominio) en AD FS para permitir que una aplicación de una sola página tenga acceso a una API Web con otro dominio.  
5. El administrador ha habilitado el encabezado de la [**Directiva de seguridad de contenido (CSP)**](#content-security-policy-csp) para evitar ataques de inserción de datos y de scripts entre sitios al no permitir solicitudes entre dominios. Sin embargo, debido a un nuevo requisito empresarial, necesitan personalizar el encabezado para permitir que la página web Cargue imágenes de cualquier origen y restrinja los medios a los proveedores de confianza.  

 
## <a name="http-security-response-headers"></a>Encabezados de respuesta de seguridad HTTP 
Los encabezados de respuesta se incluyen en la respuesta HTTP de salida enviada por AD FS a un explorador Web. Los encabezados se pueden enumerar con el `Get-AdfsResponseHeaders` cmdlet, como se muestra a continuación.  

![Respuesta del encabezado](media/customize-http-security-headers-ad-fs/header1.png)

El `ResponseHeaders` atributo de la captura de pantalla anterior identifica los encabezados de seguridad que se incluirán AD FS en cada respuesta http. Los encabezados de respuesta se enviarán solo si `ResponseHeadersEnabled` se establece en `True` (valor predeterminado). El valor se puede establecer en `False` para evitar que AD FS incluido cualquiera de los encabezados de seguridad en la respuesta http. Sin embargo, esto no es recomendable.  Para ello, use lo siguiente:

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP STRICT-Transport-Security (HSTS) 
HSTS es un mecanismo de directiva de seguridad Web que ayuda a mitigar los ataques de degradación del protocolo y la apropiación de cookies para los servicios que tienen puntos de conexión HTTP y HTTPS. Permite que los servidores web declaren que los exploradores web (u otros agentes de usuario que cumplen los requisitos) solo deben interactuar con esta mediante HTTPS y nunca a través del protocolo HTTP.  
 
Todos los puntos de conexión de AD FS para el tráfico de autenticación web se abren exclusivamente a través de HTTPS. Como resultado, AD FS mitiga eficazmente las amenazas que proporciona el mecanismo de la Directiva de seguridad de transporte HTTP STRICT (de forma predeterminada, no hay ninguna degradación de HTTP, ya que no hay agentes de escucha en HTTP). El encabezado se puede personalizar estableciendo los parámetros siguientes:
 
- **Max-Age = &lt; expire-Time &gt; ** : el tiempo de expiración (en segundos) especifica cuánto tiempo se debe tener acceso al sitio solo mediante HTTPS. El valor predeterminado y el recomendado es de 31536000 segundos (1 año).  
- **includeSubDomains** : este es un parámetro opcional. Si se especifica, la regla HSTS se aplica también a todos los subdominios.  
 
#### <a name="hsts-customization"></a>Personalización de HSTS 
De forma predeterminada, el encabezado está habilitado y `max-age` establecido en 1 año; sin embargo, los administradores pueden modificar `max-age` (no se recomienda reducir el valor de Max-Age) o habilitar HSTS para subdominios mediante el `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

Ejemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

De forma predeterminada, el encabezado se incluye en el `ResponseHeaders` atributo; sin embargo, los administradores pueden quitar el encabezado a través del `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-Frame-Options 
De forma predeterminada, AD FS no permite que las aplicaciones externas usen iFrames al realizar inicios de sesión interactivos. Esto se hace para evitar cierto estilo de ataques de suplantación de identidad (phishing). Tenga en cuenta que los inicios de sesión no interactivos se pueden realizar a través de iFrame debido a la seguridad de nivel de sesión anterior que se ha establecido.  
 
Sin embargo, en algunos casos poco frecuentes, puede confiar en una aplicación específica que requiera compatibilidad con iFrame interactivo AD FS página de inicio de sesión. Para este fin se usa el encabezado "X-Frame-Options".  
 
Este encabezado de respuesta de seguridad http se usa para comunicar con el explorador si puede representar una página en un &lt; iframe de marco &gt; / &lt; &gt; . El encabezado se puede establecer en uno de los siguientes valores: 
 
- **Deny** : no se mostrará la página de un marco. Esta es la configuración predeterminada y recomendada.  
- **sameorigin** : la página solo se mostrará en el marco si el origen es el mismo que el origen de la Página Web. La opción no es muy útil a menos que todos los antecesores estén también en el mismo origen.  
- **permitir desde <specified origin> ** -La página solo se mostrará en el marco si el origen (por ejemplo, https://www . "). com) coincide con el origen específico del encabezado. Esto puede ser compatible con exploradores limitados.

#### <a name="x-frame-options-customization"></a>Personalización X-Frame-Options  
De forma predeterminada, header se establecerá en deny; sin embargo, los administradores pueden modificar el valor a través del `Set-AdfsResponseHeaders` cmdlet.  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

Ejemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

De forma predeterminada, el encabezado se incluye en el `ResponseHeaders` atributo; sin embargo, los administradores pueden quitar el encabezado a través del `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>Protección de X-XSS 
Este encabezado de respuesta de seguridad HTTP se usa para impedir que se carguen las páginas web cuando los exploradores detectan ataques de scripting entre sitios (XSS). Esto se conoce como filtrado XSS. El encabezado se puede establecer en uno de los siguientes valores:
 
- **0** : deshabilita el filtrado XSS. No se recomienda.  
- **1** : habilita el filtrado XSS. Si se detecta un ataque XSS, el explorador desaverá la página.   
- **1; modo = bloque** : habilita el filtrado XSS. Si se detecta un ataque XSS, el explorador evitará la representación de la página. Esta es la configuración predeterminada y recomendada.  

#### <a name="x-xss-protection-customization"></a>Personalización de la protección X-XSS 
De forma predeterminada, el encabezado se establecerá en 1; Mode = bloque; sin embargo, los administradores pueden modificar el valor a través del `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

Ejemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

De forma predeterminada, el encabezado se incluye en el `ResponseHeaders` atributo; sin embargo, los administradores pueden quitar el encabezado a través del `Set-AdfsResponseHeaders` cmdlet. 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>Encabezados de uso compartido de recursos entre orígenes (CORS) 
La seguridad del explorador Web impide que una página web realice solicitudes entre orígenes iniciadas desde los scripts. Sin embargo, a veces es posible que desee tener acceso a recursos de otros orígenes (dominios). CORS es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen. Con CORS, un servidor puede permitir explícitamente algunas solicitudes de origen cruzado y rechazar otras.  
 
Para comprender mejor la solicitud de CORS, veamos un escenario en el que una aplicación de una sola página (SPA) necesita llamar a una API Web con un dominio diferente. Además, consideremos que SPA y API están configurados en ADFS 2019 y AD FS tiene CORS habilitado, es decir, AD FS pueden identificar los encabezados CORS en la solicitud HTTP, validar los valores del encabezado e incluir los encabezados de CORS adecuados en la respuesta (más adelante se explica cómo habilitar y configurar CORS en AD FS 2019 en la sección de personalización de CORS). Flujo de ejemplo: 

1. El usuario accede a SPA a través del explorador del cliente y se redirige al extremo de autenticación de AD FS para la autenticación. Dado que SPA está configurado para el flujo de concesión implícita, la solicitud devuelve un token de identificador y de acceso al explorador después de la autenticación correcta.  
2. Después de la autenticación de usuario, el código JavaScript de front-end incluido en SPA realiza una solicitud para acceder a la API Web. La solicitud se redirige a AD FS con los encabezados siguientes:
    - Opciones: describe las opciones de comunicación para el recurso de destino. 
    - Origin: incluye el origen de la API Web
    - Access-Control-request-Method: identifica el método HTTP (por ejemplo, DELETE) que se usará cuando se realice la solicitud real. 
    - Access-Control-request-headers: identifica los encabezados HTTP que se usarán cuando se realice la solicitud real. 
    
   >[!NOTE]
   >La solicitud de CORS se parece a una solicitud HTTP estándar, sin embargo, la presencia de un encabezado de origen indica que la solicitud entrante está relacionada con CORS. 
3. AD FS comprueba que el origen de la API Web incluido en el encabezado aparece en la lista de orígenes de confianza configurados en AD FS (más adelante, en la sección detalles sobre cómo modificar orígenes de confianza en la personalización de CORS). A continuación, AD FS responde con los encabezados siguientes:  
    - Access-Control-Allow-Origin: valor igual que en el encabezado Origin 
    - Access-Control-Allow-Method – valor igual que en el encabezado Access-Control-request-Method 
    - Access-Control-Allow-headers-valor igual que en el encabezado Access-Control-request-headers 
4. El explorador envía la solicitud real, incluidos los encabezados siguientes:
    - Método HTTP (por ejemplo, DELETE) 
    - Origin: incluye el origen de la API Web 
    - Todos los encabezados incluidos en el encabezado de respuesta Access-Control-Allow-headers 
5. Una vez comprobado, AD FS aprueba la solicitud incluyendo el dominio de la API Web (Origin) en el encabezado de respuesta Access-Control-Allow-Origin.  
6. La inclusión del encabezado Access-Control-Allow-Origin permitirá que el explorador siga con la llamada a la API solicitada.

#### <a name="cors-customization"></a>Personalización de CORS 
De forma predeterminada, la funcionalidad CORS no se habilitará; sin embargo, los administradores pueden habilitar la funcionalidad a través del cmdlet Set-AdfsResponseHeaders.  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

Una vez habilitada, los administradores podrán enumerar una lista de orígenes de confianza mediante el mismo cmdlet. Por ejemplo, el siguiente comando permitiría solicitudes de CORS desde los orígenes **https&#58;//example1.com** y **https&#58;//example1.com**. 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> Los administradores pueden permitir solicitudes de CORS desde cualquier origen incluyendo "*" en la lista de orígenes de confianza, aunque no se recomienda este enfoque debido a las vulnerabilidades de seguridad y se proporciona un mensaje de advertencia si se eligen. 

### <a name="content-security-policy-csp"></a>Directiva de seguridad de contenido (CSP) 
Este encabezado de respuesta de seguridad HTTP se usa para evitar ataques de inserción de datos, el secuestro y otros ataques de inyección de datos, ya que impide que los exploradores ejecuten accidentalmente contenido malintencionado. Los exploradores que no admiten CSP simplemente omiten los encabezados de respuesta de CSP.  
 
#### <a name="csp-customization"></a>Personalización de CSP 
La personalización del encabezado de CSP implica la modificación de la Directiva de seguridad que define el explorador de recursos que se puede cargar para la Página Web. La Directiva de seguridad predeterminada es  
 
`Content-Security-Policy: default-src ‘self' ‘unsafe-inline' ‘'unsafe-eval'; img-src ‘self' data:;` 
 
La directiva **default-src** se usa para modificar las [directivas-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) sin enumerar cada directiva explícitamente. Por ejemplo, en el ejemplo siguiente, la directiva 1 es la misma que la Directiva 2.  

Directiva 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
Directiva 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self'; img-src ‘self'; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

Si una directiva se muestra explícitamente, el valor especificado invalida el valor especificado para default-src. En el ejemplo siguiente, img-src tomará el valor como ' * ' (permitiendo que las imágenes se carguen desde cualquier origen) mientras que otras directivas-src tomarán el valor como ' Self ' (restringiendo el mismo origen que la página web).  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self'; img-src *" 
```
Se pueden definir los siguientes orígenes para la directiva predeterminada-src:
 
- ' Self ': si se especifica, se restringe el origen del contenido que se va a cargar en el origen de la Página Web. 
- ' Unsafe-inline ': especificar esto en la Directiva permite el uso de JavaScript y CSS en línea 
- ' Unsafe-eval ': especificar esto en la Directiva permite el uso de texto en mecanismos de JavaScript como eval 
- ' none ': si se especifica, se restringe el contenido de cualquier origen que se va a cargar. 
- datos:-especificar datos: los URI permiten a los creadores de contenido incrustar archivos pequeños insertados en los documentos. Uso no recomendado.  
 
>[!NOTE]
>AD FS usa JavaScript en el proceso de autenticación y, por lo tanto, habilita JavaScript mediante la inclusión de orígenes ' Unsafe-inline ' y ' Unsafe-eval ' en la directiva predeterminada.  

### <a name="custom-headers"></a>Encabezados personalizados 
Además de los encabezados de respuesta de seguridad enumerados anteriormente (HSTS, CSP, X-Frame-Options, X-XSS-Protection y CORS), AD FS 2019 proporciona la capacidad de establecer nuevos encabezados.  
 
Ejemplo: para establecer un nuevo encabezado "TestHeader" con el valor "TestHeaderValue" 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

Una vez establecido, el nuevo encabezado se envía en la respuesta AD FS (siguiente fragmento de código Fiddler).  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browser-compatibility"></a>Compatibilidad con exploradores Web
Utilice la tabla y los vínculos siguientes para determinar qué exploradores Web son compatibles con cada uno de los encabezados de respuesta de seguridad.

|Encabezados de respuesta de seguridad HTTP|Compatibilidad con exploradores|
|-----|-----|
|HTTP STRICT-Transport-Security (HSTS)|[Compatibilidad del explorador de HSTS](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-Frame-Options|[Compatibilidad del explorador X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|Protección de X-XSS|[Compatibilidad del explorador X-XSS-Protection](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|Uso compartido de recursos entre orígenes (CORS)|[Compatibilidad del explorador CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|Directiva de seguridad de contenido (CSP)|[Compatibilidad del explorador de CSP](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Siguientes

- [Usar la ayuda de AD FS guías de solución de problemas](https://aka.ms/adfshelp/troubleshooting )
- [Solución de problemas de AD FS](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
