---
title: Información de la propiedad de especificación de clave de certificado y Servicios de federación de Active Directory (AD FS)
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 51c9828cfe494c68422f4985e5b17113020c8414
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407410"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>Información de la propiedad de AD FS e especificación de certificados
La especificación de clave ("especificación") es una propiedad asociada a un certificado y una clave. Especifica si se puede usar una clave privada asociada a un certificado para la firma, el cifrado o ambos.   

Un valor de especificación de tipo incorrecto puede producir errores de AD FS y proxy de aplicación web como:


- Error al establecer una conexión SSL/TLS en AD FS o el proxy de aplicación Web, sin eventos de AD FS registrados (aunque se pueden registrar eventos SChannel 36888 y 36874)
- No se pudo iniciar sesión en la página de autenticación basada en formularios AD FS o WAP, sin ningún mensaje de error en la página.

Puede ver lo siguiente en el registro de eventos:

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

## <a name="what-causes-the-problem"></a>Lo que causa el problema
La propiedad especificación de clave identifica el modo en que se puede usar una clave generada o recuperada por Microsoft CryptoAPI (CAPI) de un proveedor de almacenamiento criptográfico (CSP) heredado de Microsoft.

Para la firma y el cifrado, se puede usar un valor de especificación de especificación de **1**o **AT_KEYEXCHANGE**.  Un valor de **2**, o **AT_SIGNATURE**, solo se usa para firmar.

La configuración de la especificación de tipos más común es el uso de un valor de 2 para un certificado que no sea el certificado de firma de tokens.  

En el caso de los certificados cuyas claves se generaron mediante proveedores CNG (Cryptography Next Generation), no hay ningún concepto de especificación de claves y el valor de la especificación de clave siempre será cero.

Vea cómo comprobar un valor válido de especificación de especificación a continuación. 

### <a name="example"></a>Ejemplo
Un ejemplo de un CSP heredado es el proveedor de servicios criptográficos mejorados de Microsoft. 

El formato BLOB de clave de CSP de Microsoft RSA incluye un identificador de algoritmo, ya sea **CALG_RSA_KEYX** o **CALG_RSA_SIGN**, respectivamente, a las solicitudes de servicio de las claves <strong>AT_KEYEXCHANGE * * o * * AT_SIGNATURE</strong> .

Los identificadores de algoritmo de clave RSA se asignan a los valores de la especificación

| Algoritmo admitido por el proveedor| Valor de especificación de clave para llamadas CAPI |
| --- | --- |
|CALG_RSA_KEYX : Clave RSA que se puede usar para la firma y el descifrado| AT_KEYEXCHANGE (o especificación de especificación = 1)|
CALG_RSA_SIGN : Clave de solo firma RSA |AT_SIGNATURE (o especificación de especificación = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Valores de especificación de especificación y significados asociados
A continuación se indican los significados de los distintos valores de especificación de especificación:

|Valor de especificación de especificación|Modo|Uso AD FS recomendado|
| --- | --- | --- |
|0|El certificado es un certificado CNG|Solo certificado SSL|
|1|En el caso de un certificado CAPI (no CNG) heredado, la clave se puede usar para la firma y el descifrado.|    SSL, firma de tokens, descifrado de tokens, certificados de comunicación de servicio|
|2|En el caso de un certificado CAPI (no CNG) heredado, la clave solo se puede usar para firmar.|No recomendado|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Comprobación del valor de la especificación de clave para los certificados o claves
Para ver un valor de certificados, puede usar la herramienta de línea de comandos **certutil** .  

El siguiente es un ejemplo: **certutil – v – Store My**.  Se volcará la información del certificado en la pantalla.

![Certificado de especificación](media/AD-FS-and-KeySpec-Property/keyspec1.png)

En CERT_KEY_PROV_INFO_PROP_ID, busque dos cosas:


1. **ProviderType:** indica si el certificado usa un proveedor de almacenamiento CRIPTOGRÁFICO (CSP) heredado o un proveedor de almacenamiento de claves basado en las API de nuevo certificado de nueva generación (CNG).  Cualquier valor distinto de cero indica un proveedor heredado.
2. **Especificación** A continuación se muestran los valores válidos de especificación de un certificado AD FS:

   Proveedor CSP heredado (ProviderType no es igual a 0):

   |AD FS propósito del certificado|Valores válidos de especificación de especificación|
   | --- | --- |
   |Comunicación del servicio|1|
   |Descifrado de tokens|1|
   |Firma de tokens|1 y 2|
   |SSL|1|

   Proveedor de CNG (ProviderType = 0):

   |AD FS propósito del certificado|Valores válidos de especificación de especificación|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Cómo cambiar la especificación de especificación del certificado por un valor admitido
El cambio del valor de la especificación de la configuración no requiere que la entidad de certificación vuelva a generar o a emitir el certificado.  La especificación de clave se puede cambiar volviendo a importar el certificado completo y la clave privada de un archivo PFX en el almacén de certificados mediante los pasos siguientes:


1. En primer lugar, compruebe y registre los permisos de clave privada en el certificado existente para que se puedan volver a configurar si es necesario después de volver a importar.
2. Exporte el certificado, incluida la clave privada, a un archivo PFX.
3. Realice los pasos siguientes para cada servidor AD FS y WAP
    1. Eliminar el certificado (del servidor AD FS/WAP)
    2. Abra un símbolo del sistema de PowerShell con privilegios elevados e importe el archivo PFX en cada AD FS y en el servidor WAP mediante la sintaxis de cmdlet siguiente, especificando el valor de AT_KEYEXCHANGE (que funciona para todos los AD FS propósitos del certificado):
        1. C: \>certutil – importpfx CERTFILE. pfx AT_KEYEXCHANGE
        2. Escriba la contraseña PFX
    3. Una vez completado el procedimiento anterior, haga lo siguiente:
        1. comprobar los permisos de clave privada
        2. reiniciar el servicio ADFS o WAP





