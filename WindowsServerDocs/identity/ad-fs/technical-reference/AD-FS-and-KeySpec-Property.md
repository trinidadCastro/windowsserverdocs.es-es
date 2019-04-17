---
title: "Activa los servicios de federación de directorio y certificado de información de propiedad de especificación de clave"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: db58fcce054f34c4b0a3f6725456badae9fd0468
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS y propiedad KeySpec información de certificado
Especificación de clave ("KeySpec") es una propiedad asociada con un certificado y la clave. Especifica si una clave privada asociada con un certificado puede usarse para iniciar sesión, cifrado o ambos.   

Un valor KeySpec incorrecto puede provocar errores de AD FS y Proxy Web de la aplicación como:


- Error al intentar establecer una conexión de SSL/TLS para el Proxy de aplicación Web, o de AD FS con ningún evento de AD FS iniciado (aunque pueden haber iniciado eventos SChannel 36888 y 36874)
- Error al iniciar sesión en los AD FS o WAP formularios de página de autenticación basada en, sin ningún mensaje de error que se muestra en la página.

Es posible que veas las siguientes acciones en el registro de eventos:

    Log Name:      AD FS Tracing/Debug
    Source:        AD FS Tracing
    Date:          2/12/2015 9:03:08 AM
    Event ID:      67
    Task Category: None
    Level:         Error
    Keywords:      ADFSProtocol
    User:          S-1-5-21-3723329422-3858836549-556620232-1580884
    Computer:      ADFS1.contoso.com
    Description:
    Ignore corrupted SSO cookie.

## <a name="what-causes-the-problem"></a>¿Qué causa el problema
La propiedad KeySpec identifica cómo se puede usar una clave generados o recuperado por Microsoft CryptoAPI (CAPI), de un Microsoft heredado almacenamiento proveedor cifrado (CSP).

Un valor KeySpec de **1**, o **AT_KEYEXCHANGE**, puede usarse para la firma y cifrado.  Un valor de **2**, o **AT_SIGNATURE**, solo se usa para iniciar sesión.

La configuración incorrecta de KeySpec más común es con un valor de 2 para un certificado que no sean el token de certificado de firma.  

Para los certificados que se generaron cuyas claves con proveedores de criptografía de próxima generación (CNG), no hay ningún concepto de la especificación de clave y el valor de KeySpec siempre será cero.

Descubre cómo comprobar si hay un valor válido de KeySpec a continuación. 

### <a name="example"></a>Ejemplo
Un ejemplo de un CSP heredado es el proveedor criptográfico mejorado de Microsoft. 

Formato del blob de clave RSA CSP Microsoft incluye un identificador de algoritmos, ya sea **CALG_RSA_KEYX** o **CALG_RSA_SIGN**, respectivamente, a las solicitudes de servicio para cualquiera ** AT_KEYEXCHANGE ** o **AT_SIGNATURE** claves.
  
Los identificadores de algoritmos de claves RSA se asignan a los valores de KeySpec manera

| Algoritmo de proveedor compatible| Valor de la especificación de clave para las llamadas CAPI |
| --- | --- |
|CALG_RSA_KEYX: Clave RSA que se puede usar para la firma y el descifrado| AT_KEYEXCHANGE (o KeySpec = 1)|
CALG_RSA_SIGN: Solo clave de firma de RSA |AT_SIGNATURE (o KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Valores de KeySpec y significado
Estos son el significado de los distintos valores de KeySpec:

|Valor KeySpec|Medio|Uso de AD FS recomendado|
| --- | --- | --- |
|0|El certificado es un certificado CNG|Solo certificados SSL|
|1|Para un certificado CAPI (no de CNG) heredado, la clave puede usarse para la firma y el descifrado|    SSL, firma token descifrado, certificados de comunicación de servicio de tokens|
|2|Para un certificado CAPI (no de CNG) heredado, la clave puede usarse solo para iniciar sesión|No se recomienda|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Cómo comprobar el valor de KeySpec para los certificados y claves
Para ver un valor de certificados puede utilizar la **certutil** herramienta de línea de comandos.  

El siguiente es un ejemplo: **certutil – v – almacenar mi**.  Esto volcar la información de certificado a la pantalla.

![Certificado KeySpec](media/AD-FS-and-KeySpec-Property/keyspec1.png)

En CERT_KEY_PROV_INFO_PROP_ID, busque dos cosas:


1. **Tipo de proveedor:** esto indica si el certificado utiliza un proveedor de almacenamiento criptográficos (CSP) heredado o un proveedor de almacenamiento de claves basada en más reciente certificado Next Generation (CNG) API.  Cualquier valor distinto de cero indica un proveedor heredado.
2.  **KeySpec:** los siguientes son los valores válidos de KeySpec para un certificado de AD FS:

    Proveedor CSP heredado (tipo de proveedor no es igual a 0):
    
    |AD FS certificado propósito|Valores válidos KeySpec|
    | --- | --- |
    |Comunicación del servicio|1|
    |Descifrado de token|1|
    |Firma de tokens|1 y 2|
    |SSL|1|

    Proveedor de GNC (tipo de proveedor = 0):
    |AD FS certificado propósito|Valores válidos KeySpec|
    | --- | --- |   
    |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Cómo cambiar la keyspec para el certificado a un valor admitido
Cambiar el valor de KeySpec no requiere el certificado necesario volver a generar o volver a emitido por la entidad de certificación.  La KeySpec puede cambiarse al volver a importar el certificado completo y la clave privada de un archivo PFX en el almacén de certificados con los siguientes pasos:


1. En primer lugar, comprueba y registra los permisos de la claves privados en el certificado existente para que se pueden volver a configurar si es necesario después de volver a importar.
2. Exportar el certificado incluida la clave privada a un archivo PFX.
3. Realiza los siguientes pasos para cada servidor de AD FS y WAP
    1. Eliminar el certificado (desde la AD FS / servidor WAP)
    2. Abre un símbolo de PowerShell con privilegios elevados e importar el archivo PFX en cada servidor de AD FS y WAP mediante la sintaxis de cmdlet siguiente, especifica el valor AT_KEYEXCHANGE (que funciona para todos los usos de certificado de AD FS):
        1. C:\ > certutil – importpfx certfile.pfx AT_KEYEXCHANGE
        2. Escriba la contraseña PFX
    3. Una vez completado el ejemplo anterior, haz lo siguiente
        1. Revisar los permisos de la claves privados
        2. Reiniciar el servicio adfs o wap





