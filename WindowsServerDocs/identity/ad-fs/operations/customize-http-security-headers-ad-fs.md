---
title: Personalizar encabezados de respuesta de seguridad HTTP con AD FS
description: Este documento descirbes cómo personalizar los encabezados de seguridad para protegerse frente a vulnerabilidades de seguridad.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd3ad4e6547194a971d8a51ecb95ee56f5e4e8c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822726"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>Personalizar encabezados de respuesta de seguridad HTTP con AD FS de 2019 
Se aplica a: Windows Server 2019 
 
Para protegerse frente a vulnerabilidades de seguridad frecuentes y ofrecen a los administradores la capacidad de aprovechar las ventajas de los últimos avances en los mecanismos de protección basada en explorador, AD FS 2019 agrega la funcionalidad para personalizar los encabezados de respuesta de seguridad HTTP enviado por AD FS. Esto se logra a través de la introducción de dos nuevos cmdlets: `Get-AdfsResponseHeaders` y `Set-AdfsResponseHeaders`.  
 
En este documento, veremos normalmente usa encabezados de respuesta de seguridad para demostrar cómo personalizar los encabezados enviados por AD FS de 2019.   
 
>[!NOTE]
>El documento se da por supuesto que se ha instalado AD FS de 2019.  

 
Antes de tratar los encabezados, echemos un vistazo a algunos escenarios de creación de la necesidad de que los administradores personalizar los encabezados de seguridad 
 
## <a name="scenarios"></a>Escenarios 
1. Administrador ha habilitado [ **Strict-Transport-Security HTTP (HSTS)** ](#http-strict-transport-security-hsts) (obliga a todas las conexiones a través de cifrado de HTTPS) para proteger a los usuarios que pueden tener acceso a la aplicación web mediante HTTP desde un acceso público Wi-Fi punto que es posible que podrían prestarse a intrusiones. Le gustaría reforzar aún más la seguridad habilitando HSTS para los subdominios.  
2. Administrador ha configurado el [ **X-Frame-Options** ](#x-frame-options) encabezado de respuesta (evita el procesamiento de cualquier página web en un iFrame) para proteger las páginas web que se clickjacked. Sin embargo, debe personalizar el valor del encabezado debido a un nuevo requisito de negocio para mostrar los datos (en un iFrame) desde una aplicación con un origen diferente (dominio).
3. Administrador ha habilitado [ **X-XSS-Protection** ](#x-xss-protection) (evita los ataques de scripts entre) para sanear y bloquear la página si el explorador detecta los ataques de scripts entre. Sin embargo, debe personalizar el encabezado para permitir que la página se cargue una vez que se depura.  
4. Administrador debe habilitar [ **recursos entre orígenes de uso compartido (CORS)** ](#cross-origin-resource-sharing-cors-headers) y establezca el origen (dominio) en AD FS para permitir una aplicación de página única tener acceso a una API web con otro dominio.  
5. Administrador ha habilitado [ **directiva de seguridad de contenido (CSP)** ](#content-security-policy-csp) ataques de encabezado para evitar que entre la inserción de secuencias de comandos y los datos de sitio al no permitir las solicitudes entre dominios. Sin embargo, debido a un nuevo requisito de negocio que necesita personalizar el encabezado para permitir que la página web para cargar imágenes desde cualquier origen y restringir los medios a los proveedores de confianza.  

 
## <a name="http-security-response-headers"></a>Encabezados de respuesta de seguridad HTTP 
Los encabezados de respuesta se incluyen en la respuesta HTTP saliente enviada por AD FS en un explorador web. Los encabezados se pueden enumerar mediante la `Get-AdfsResponseHeaders` cmdlet tal y como se muestra a continuación.  

![Respuesta de encabezado](media\customize-http-security-headers-ad-fs\header1.png)

El `ResponseHeaders` atributo en la captura de pantalla anterior identifica los encabezados de seguridad que se incluirán por AD FS en cada respuesta HTTP. Los encabezados de respuesta se enviarán solo si `ResponseHeadersEnabled` está establecido en `True` (valor predeterminado). El valor puede establecerse en `False` para impedir que cualquiera de los encabezados de seguridad incluidas en la respuesta HTTP de AD FS. Pero esto no se recomienda.  Para ello, use lo siguiente:

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>Strict--seguridad de transporte HTTP (HSTS) 
HSTS es un mecanismo de directiva de seguridad de web que ayuda a mitigar los ataques de degradación de protocolo y al secuestro de cookie para los servicios que tienen puntos de conexión HTTP y HTTPS. Permite a los servidores web declarar que los exploradores web (u otros agentes de usuario de cumplimiento) solo deben interactuar con él mediante HTTPS y nunca a través del protocolo HTTP.  
 
Todos los extremos de AD FS para el tráfico de autenticación web se abren exclusivamente a través de HTTPS. Como resultado, AD FS mitiga eficazmente las amenazas que proporciona el mecanismo de directiva de seguridad de transporte estricto HTTP (de forma predeterminada no hay ninguna degradación a HTTP ya que no hay ningún agente de escucha en HTTP). El encabezado se puede personalizar mediante el establecimiento de los parámetros siguientes 
 
- **max-age =&lt;tiempo expiran&gt;**  : la hora de expiración (en segundos) especifica cuánto debe solo puede acceder al sitio mediante HTTPS. Valor predeterminado y recomendado es 31536000 segundos (1 año).  
- **includeSubDomains** : se trata de un parámetro opcional. Si se especifica, la regla HSTS se aplica a todos los subdominios también.  
 
#### <a name="hsts-customization"></a>Personalización de HSTS 
De forma predeterminada, el encabezado está habilitado y `max-age` establecido en 1 año; sin embargo, los administradores pueden modificar el `max-age` (no se recomienda la reducir el valor de max-age) o habilitar HSTS para subdominios a través de la `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

Por ejemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

De forma predeterminada, el encabezado se incluye en el `ResponseHeaders` atributo; sin embargo, los administradores pueden quitar el encabezado a través de la `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-Frame-Options 
AD FS de forma predeterminada no permite que aplicaciones externas pueden usar iFrames al realizar inicios de sesión interactivos. Esto se hace para evitar que un estilo determinado de ataques de suplantación de identidad. Tenga en cuenta que los inicios de sesión no interactivo pueden realizarse a través de iFrame debido a la seguridad de nivel de sesión anterior que se ha establecido.  
 
Sin embargo, en algunos casos poco frecuentes que confía en una aplicación específica que requiere la página de inicio de sesión de FS AD interactivo capaz de iFrame. El encabezado 'X-Frame-Options' se utiliza para este propósito.  
 
Este encabezado de respuesta de seguridad HTTP se utiliza para comunicarse con el explorador si puede representar una página en un &lt;marco&gt;/&lt;iframe&gt;. El encabezado puede establecerse en uno de los valores siguientes: 
 
- **denegar** : no se mostrará la página en un marco. Este es el valor predeterminado y la configuración recomendada.  
- **sameorigin** : la página solo se mostrará en el marco de si el origen es el mismo que el origen de la página web. La opción no es muy útil a menos que todos los antecesores también se encuentran en el mismo origen.  
- **Permitir de <specified origin>**  : la página solo se mostrará en el marco de si el origen (p. ej., https://www. ". com) coincide con el origen específico en el encabezado. 

#### <a name="x-frame-options-customization"></a>Personalización de X-Frame-Options  
De forma predeterminada, el encabezado se establecerá para denegar; Sin embargo, los administradores pueden modificar el valor a través de la `Set-AdfsResponseHeaders` cmdlet.  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

Por ejemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

De forma predeterminada, el encabezado se incluye en el `ResponseHeaders` atributo; sin embargo, los administradores pueden quitar el encabezado a través de la `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
Este encabezado de respuesta de seguridad HTTP se utiliza para evitar que las páginas web de carga cuando se detectan ataques de scripting entre sitios (XSS) por los exploradores. Esto se conoce como el filtrado de XSS. El encabezado puede establecerse en uno de los siguientes valores 
 
- **0** – XSS deshabilita el filtrado. No se recomienda.  
- **1** : filtrado permite XSS. Si se detecta el ataque XSS, el explorador saneará la página.   
- **1. modo = block** – XSS permite el filtrado. Si se detecta el ataque XSS, explorador impedirá la representación de la página. Este es el valor predeterminado y la configuración recomendada.  

#### <a name="x-xss-protection-customization"></a>Personalización de X-XSS-Protection 
De forma predeterminada, el encabezado se establecerá en 1; modo = bloque; Sin embargo, los administradores pueden modificar el valor a través de la `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

Por ejemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

De forma predeterminada, el encabezado se incluye en el `ResponseHeaders` atributo; sin embargo, los administradores pueden quitar el encabezado a través de la `Set-AdfsResponseHeaders` cmdlet. 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>Entre los encabezados de uso compartido de recursos de orígenes (CORS) 
Seguridad del explorador Web impide que una página web que realizan solicitudes entre orígenes iniciadas desde scripts. Sin embargo, en ocasiones, puede acceder a recursos de otros orígenes (dominios). CORS es un estándar de W3C que permite que un servidor a ser menos exigentes con la directiva de mismo origen. Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras.  
 
Para entender mejor la solicitud de CORS, vamos a tutorial un escenario donde una sola página (SPA) de la aplicación debe llamar a una API web con un dominio diferente. Además, vamos a considerar que SPA y API se configuran en ADFS 2019 y AD FS tiene CORS habilitado, es decir, AD FS puede identificar los encabezados de CORS en la solicitud HTTP, validar los valores de encabezado e incluir encabezados CORS adecuados en la respuesta (obtener más información sobre cómo habilitar y configuración de CORS en AD FS 2019 en la siguiente sección de personalización de la CORS). Ejemplo de flujo: 

1. Usuario tiene acceso a la SPA a través del explorador del cliente y se redirige al extremo de autenticación de AD FS para la autenticación. Puesto que la SPA está configurado para el flujo de concesión implícita, solicitud devuelve un acceso y el identificador de token en el explorador tras una autenticación correcta.  
2. Después de la autenticación de usuario, el front-end JavaScript incluido en SPA realiza una solicitud para obtener acceso a la API web. La solicitud se redirige a AD FS con los siguientes encabezados
    - Opciones: se describen las opciones de comunicación para el recurso de destino 
    - Origen: incluye el origen de la API web
    - Acceso Access-Control-Request-Method: identifica el método HTTP (por ejemplo, eliminar) que se usará cuando se realiza la solicitud real 
    - Access-Control-Request-Headers - identifica los encabezados HTTP que se usará cuando se realiza la solicitud real 
    
   >[!NOTE]
   >Solicitud de CORS se parece a una solicitud HTTP estándar, sin embargo, la presencia de un encabezado origin indica que la solicitud entrante es la CORS relacionados. 
3. AD FS comprueba que la incluida en el encabezado de origen de API web aparece en los orígenes de confianza configurados en AD FS (obtener más información sobre cómo modificar los orígenes de confianza en la siguiente sección de personalización de la CORS). AD FS, a continuación, responde con encabezados siguientes.  
    - Access-Control-Allow-Origin: mismo valor como se muestra en el encabezado de origen 
    - Access-Control-permitir-Method: mismo valor como se muestra en el encabezado de Access-Control-Request-Method 
    - Access-Control-Allow-Headers - mismo valor como se muestra en el encabezado de Access-Control-Request-Headers 
4. Explorador envía la solicitud real que incluya los siguientes encabezados 
    - Método HTTP (por ejemplo, eliminar) 
    - Origen: incluye el origen de la API web 
    - Todos los encabezados incluidos en el encabezado de respuesta de Access-Control-Allow-Headers 
5. Una vez comprobado, AD FS aprueba la solicitud mediante la inclusión del dominio de web API (origen) en el encabezado de respuesta de Access-Control-Allow-Origin.  
6. La inclusión del encabezado Access-Control-Allow-Origin permitirá que el explorador a seguir adelante con una llamada a la API solicitada.

#### <a name="cors-customization"></a>Personalización de la CORS 
De forma predeterminada, no se habilitará la funcionalidad CORS; Sin embargo, los administradores pueden habilitar la funcionalidad mediante el cmdlet Set-AdfsResponseHeaders.  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

Una de ellas habilitada, los administradores podrán enumerar una lista de orígenes de confianza con el mismo cmdlet. Por ejemplo, el siguiente comando permitiría a las solicitudes CORS desde los orígenes **https&#58;//example1.com** y **https&#58;//example1.com**. 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> Los administradores pueden permitir que las solicitudes CORS desde cualquier origen mediante la inclusión de "*" en la lista de orígenes de confianza, aunque no se recomienda este enfoque debido a un mensaje de advertencia y de las vulnerabilidades de seguridad se proporciona si deciden. 

### <a name="content-security-policy-csp"></a>Directiva de seguridad del contenido (CSP) 
Este encabezado de respuesta de seguridad HTTP se utiliza para impedir el scripting entre sitios, el secuestro de clic y otros ataques de inyección de datos evitando que los exploradores de ejecutar accidentalmente contenido malintencionado. Los exploradores que no son compatibles con CSP simplemente omite los encabezados de respuesta CSP.  
 
#### <a name="csp-customization"></a>Personalización de CSP 
Personalización del encabezado CSP implica la modificación de la directiva de seguridad que define el Explorador de recursos puede cargar para la página web. La directiva de seguridad predeterminada es  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
El **predeterminada src** directiva se usa para modificar [src - directivas](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) sin indicar explícitamente cada directiva. Por ejemplo, en el ejemplo siguiente la directiva 1 es igual que la directiva 2.  

Directiva 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
Directiva 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

Si una directiva se muestra explícitamente, el valor especificado invalida el valor dado para src de forma predeterminada. En el ejemplo siguiente, img-src tendrá el valor como ' *' (que permite a las imágenes que se cargue desde cualquier origen) mientras que otras directivas - src tardará el valor como 'Auto' (restringir el mismo origen como la página web).  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
Seguir orígenes se puede definir para la directiva predeterminada src 
 
- 'self' – especificar esto restringe el origen del contenido para cargar en el origen de la página web 
- 'unsafe en línea' – Especifique esto en la directiva permite el uso de inline JavaScript y CSS 
- 'unsafe-eval' – Especifique esto en la directiva permite el uso de texto a JavaScript como mecanismos de eval 
- 'none' – especificar esto restringe el contenido desde cualquier origen para cargar 
- datos:-especificar los datos: Los URI permite que los creadores de contenido incrustar archivos pequeños insertada en documentos. No se recomienda el uso.  
 
>[!NOTE]
>AD FS usa JavaScript en el proceso de autenticación y, por tanto, permite JavaScript mediante la inclusión de 'unsafe en línea' y 'unsafe eval' en default orígenes directiva.  

### <a name="custom-headers"></a>Encabezados personalizados 
Además de lo anterior se muestran los encabezados de respuesta de seguridad (HSTS, CSP, X-Frame-Options, X-XSS-Protection y CORS), AD FS 2019 ofrece la posibilidad de establecer los encabezados de nuevo.  
 
Por ejemplo: Para establecer un nuevo encabezado "TestHeader" con el valor como "TestHeaderValue" 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

Una vez establecido, el encabezado nuevo se envía en la respuesta de AD FS (fiddler siguiente fragmento de código).  
 
![Fiddler](media\customize-http-security-headers-ad-fs\header2.png)

## <a name="web-browswer-compatibility"></a>Compatibilidad de Web ya
Use la tabla y los vínculos siguientes para determinar qué exploradores web son compatibles con cada uno de los encabezados de respuesta de seguridad.

|Encabezados de respuesta de seguridad HTTP|Compatibilidad con exploradores|
|-----|-----|
|Strict--seguridad de transporte HTTP (HSTS)|[Compatibilidad del explorador HSTS](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-Frame-Options|[Compatibilidad del explorador de X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[Compatibilidad del explorador de X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|Cross Origin Resource Sharing, (CORS)|[Compatibilidad de exploradores CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|Directiva de seguridad del contenido (CSP)|[Compatibilidad de exploradores CSP](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Siguiente

- [Utilizar las guías de Ayuda de AD FS troublehshooting](https://aka.ms/adfshelp/troubleshooting )
- [Solución de problemas de AD FS](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
