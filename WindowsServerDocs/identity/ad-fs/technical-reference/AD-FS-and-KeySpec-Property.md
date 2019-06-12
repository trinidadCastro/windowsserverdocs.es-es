---
title: Active Directory Federation Services y especificación de clave de propiedad información de certificado
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b0d08f678e9e612bb0ce9cc38d254564bd9b2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444091"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS y la propiedad KeySpec información de certificado
Especificación de clave ("KeySpec") es una propiedad asociada con un certificado y clave. Especifica si se puede usar una clave privada asociada con un certificado de firma, cifrado o ambos.   

Un valor KeySpec incorrecto puede causar errores de AD FS y Proxy de aplicación Web como:


- Error al intentar establecer una conexión SSL/TLS para AD FS o el Proxy de aplicación Web, sin eventos de AD FS registrado (aunque se pueden registrar eventos de SChannel 36888 y 36874)
- Error de inicio de sesión en el AD FS o WAP constituye la página de autenticación basada en, con ningún mensaje de error que se muestra en la página.

Verá lo siguiente en el registro de eventos:

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

## <a name="what-causes-the-problem"></a>¿Qué hace que el problema
La propiedad KeySpec identifica cómo se puede usar una clave que se generan o se recupera por Microsoft CryptoAPI (CAPI), de un Microsoft heredado almacenamiento proveedor criptográficos (CSP).

Un valor KeySpec de **1**, o **AT_KEYEXCHANGE**, se puede usar para la firma y cifrado.  Un valor de **2**, o **AT_SIGNATURE**, solo se usa para firmar.

El error de configuración más comunes de KeySpec está utilizando un valor de 2 para un certificado que no sea el certificado de firma de tokens.  

Para los certificados cuyas claves se generaron mediante proveedores de Cryptography Next Generation (CNG), no hay ningún concepto de especificación de clave y el valor de KeySpec siempre será cero.

Vea cómo comprobar un valor válido de KeySpec siguiente. 

### <a name="example"></a>Ejemplo
Un ejemplo de un CSP heredado es Microsoft Enhanced Cryptographic Provider. 

Formato de blob de clave RSA CSP Microsoft incluye un identificador de algoritmos, ya sea **CALG_RSA_KEYX** o **CALG_RSA_SIGN**, respectivamente, para atender las solicitudes para cualquier <strong>AT_KEYEXCHANGE ** o ** AT_ FIRMA</strong> claves.

Los identificadores de algoritmo de clave RSA se asignan a valores de KeySpec como se indica a continuación.

| Algoritmo del proveedor compatible| Valor de la especificación de clave para las llamadas CAPI |
| --- | --- |
|CALG_RSA_KEYX : Clave RSA que se puede usar para la firma y descifrado| AT_KEYEXCHANGE (o KeySpec = 1)|
CALG_RSA_SIGN: Solo la clave de firma RSA |AT_SIGNATURE (o KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Los valores de KeySpec y significados asociados
Estos son los significados de los distintos valores KeySpec:

|Valor KeySpec|Medio|Uso recomendado de AD FS|
| --- | --- | --- |
|0|El certificado es un certificado CNG|Solo el certificado SSL|
|1|Para un certificado CAPI (que no sean de CNG) heredado, la clave se puede utilizar para la firma y descifrado|    SSL, el token de firma de token de descifrado, certificados de comunicación de servicio|
|2|Para un certificado CAPI (que no sean de CNG) heredado, la clave se puede utilizar para la firma|no se recomienda|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Cómo comprobar el valor KeySpec de los certificados y claves
Para ver un valor de los certificados que puede usar el **certutil** herramienta de línea de comandos.  

El siguiente es un ejemplo: **certutil – v – almacenar mi**.  Esto volcará la información del certificado a la pantalla.

![KeySpec cert](media/AD-FS-and-KeySpec-Property/keyspec1.png)

En Buscar CERT_KEY_PROV_INFO_PROP_ID por dos cosas:


1. **ProviderType:** esto denota si el certificado se usa un proveedor de almacenamiento criptográfico (CSP) heredado o un proveedor de almacenamiento de claves basado en versiones más recientes certificado Next Generation (CNG) API.  Cualquier valor distinto de cero indica un proveedor heredado.
2. **KeySpec:** Los valores de KeySpec válidos para un certificado de AD FS son los siguientes:

   Proveedor CSP heredado (ProviderType no es igual a 0):

   |AD FS certificado propósito|Valores válidos KeySpec|
   | --- | --- |
   |Comunicación del servicio|1|
   |Descifrado de tokens|1|
   |Firma de tokens|1 y 2|
   |SSL|1|

   Proveedor de CNG (ProviderType = 0):

   |AD FS certificado propósito|Valores válidos KeySpec|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Cómo cambiar la especificación de clave para el certificado a un valor admitido
Cambiar el valor KeySpec no requiere el certificado que se vuelven a generar o volver a emitir la entidad emisora de certificados.  La especificación de clave puede cambiarse por volver a importar el certificado completado y la clave privada desde un archivo PFX en el almacén de certificados mediante los pasos siguientes:


1. En primer lugar, compruebe y registre los permisos de clave privados del certificado existente para que se pueden volver a configurar si es necesario después de volver a importar.
2. Exporte el certificado, incluida la clave privada a un archivo PFX.
3. Realice los pasos siguientes para cada servidor de AD FS y WAP
    1. Eliminar el certificado (de AD FS / servidor WAP)
    2. Abra un símbolo de PowerShell con privilegios elevados e importe el archivo PFX en cada servidor de AD FS y WAP utilizando la sintaxis de cmdlet siguiente, especificando el valor AT_KEYEXCHANGE (que funciona para todos los propósitos de certificado de AD FS):
        1. C:\>archivo de certutil – importpfx certfile.pfx AT_KEYEXCHANGE
        2. Escriba la contraseña PFX
    3. Una vez completado el anterior, haga lo siguiente
        1. Compruebe los permisos de clave privados
        2. Reinicie el servicio de adfs o wap





