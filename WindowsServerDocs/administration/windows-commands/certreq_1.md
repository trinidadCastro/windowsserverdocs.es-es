---
title: certreq
description: Artículo de referencia para el comando CertReq, que solicita certificados de una entidad de certificación (CA), recupera una respuesta a una solicitud anterior de una CA, crea una nueva solicitud a partir de un archivo. inf, acepta e instala una respuesta a una solicitud, construye una solicitud de certificación cruzada o de subordinación completa desde un certificado o una solicitud de CA existente y firma una solicitud de certificación cruzada o de subordinación completa.
ms.topic: reference
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c5b82b490abb5564392be3a26db5c47a5a372f6e
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864285"
---
# <a name="certreq"></a>certreq

El comando CertReq se puede usar para solicitar certificados de una entidad de certificación (CA). para recuperar una respuesta a una solicitud anterior de una entidad de certificación (CA), para crear una nueva solicitud desde un archivo. inf, para aceptar e instalar una respuesta a una solicitud, para construir una solicitud de certificación cruzada o de subordinación cualificada a partir de un certificado o una solicitud de CA existente, y para firmar una solicitud de certificación cruzada o de subordinación completa.

> [!IMPORTANT]
> Es posible que las versiones anteriores del comando CertReq no proporcionen todas las opciones que se describen aquí. Para ver las opciones admitidas en función de las versiones específicas de CertReq, ejecute la opción de ayuda de la línea de comandos, `certreq -v -?` .
>
> El comando CertReq no admite la creación de una nueva solicitud de certificado basada en una plantilla de atestación de clave cuando se está en un entorno de procesamiento de eventos complejos/CES.

> [!WARNING]
> El contenido de este tema se basa en la configuración predeterminada de Windows Server; por ejemplo, el establecimiento de la longitud de clave en 2048, la selección del proveedor de almacenamiento de claves de software de Microsoft como CSP y el uso de Algoritmo hash seguro 1 (SHA1). Evalúe estas selecciones en función de los requisitos de la Directiva de seguridad de su empresa.

## <a name="syntax"></a>Sintaxis

```
certreq [-submit] [options] [requestfilein [certfileout [certchainfileout [fullresponsefileOut]]]]
certreq -retrieve [options] requestid [certfileout [certchainfileout [fullresponsefileOut]]]
certreq -new [options] [policyfilein [requestfileout]]
certreq -accept [options] [certchainfilein | fullresponsefilein | certfilein]
certreq -sign [options] [requestfilein [requestfileout]]
certreq –enroll [options] templatename
certreq –enroll –cert certId [options] renew [reusekeys]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------- | ----------- |
| -enviar | Envía una solicitud a una entidad de certificación. |
| -recuperar `<requestid>` | Recupera una respuesta a una solicitud anterior de una entidad de certificación. |
| -nuevo | Crea una nueva solicitud a partir de un archivo. inf. |
| -Accept | Acepta e instala una respuesta a una solicitud de certificado. |
| -Directiva | Establece la Directiva para una solicitud. |
| -Sign | Firma una solicitud de certificación cruzada o de subordinación completa. |
| -inscribirse | Inscribe o renueva un certificado. |
| -? | Muestra una lista de sintaxis, opciones y descripciones de CertReq. |
| `<parameter>` -? | Muestra la ayuda del parámetro especificado. |
| -v-? | Muestra una lista detallada de la sintaxis, las opciones y las descripciones de CertReq. |

## <a name="examples"></a>Ejemplos

### <a name="certreq--submit"></a>CertReq-Submit

Para enviar una solicitud de certificado simple:

```
certreq –submit certrequest.req certnew.cer certnew.pfx
```

#### <a name="remarks"></a>Comentarios

- Este es el parámetro predeterminado certreq.exe. Si no se especifica ninguna opción en el símbolo de la línea de comandos, certreq.exe intenta enviar una solicitud de certificado a una entidad de certificación. Debe especificar un archivo de solicitud de certificado al utilizar la opción **– submit** . Si se omite este parámetro, aparece una ventana **Abrir archivo** común, que permite seleccionar el archivo de solicitud de certificado adecuado.

- Para solicitar un certificado especificando el atributo SAN, consulte la sección *uso de la utilidad certreq.exe para crear y enviar una solicitud de certificado* del artículo 931351 de Microsoft Knowledge base [Cómo agregar un nombre alternativo del firmante a un certificado LDAP seguro](https://support.microsoft.com/kb/931351).

### <a name="certreq--retrieve"></a>CertReq: recuperar

Para recuperar el identificador de certificado 20 y crear un archivo de certificado (. cer), denominado el *certificado*:

```
certreq -retrieve 20 MyCertificate.cer
```

#### <a name="remarks"></a>Comentarios

- Use CertReq: Retrieve *RequestId* para recuperar el certificado después de que la entidad de certificación lo haya emitido. *RequestId* PKC puede ser un prefijo 0x o hexadecimal con 0x y puede ser un número de serie de certificado sin prefijo 0x. También puede utilizarlo para recuperar cualquier certificado emitido por la entidad de certificación, incluidos los certificados revocados o expirados, sin tener en cuenta si la solicitud del certificado estaba en el estado pendiente.

- Si envía una solicitud a la entidad de certificación, el módulo de directivas de la entidad de certificación podría dejar la solicitud en un estado pendiente y devolver el *RequestId* al autor de la llamada CertReq para su presentación. Finalmente, el administrador de la entidad de certificación emitirá el certificado o denegará la solicitud.

### <a name="certreq--new"></a>CertReq-New

Para crear una nueva solicitud:

```
[newrequest]
; At least one value must be set in this section
subject = CN=W2K8-BO-DC.contoso2.com
```

A continuación se muestran algunas de las secciones posibles que se pueden agregar al archivo INF:

#### <a name="newrequest"></a>[newrequest]

Esta área del archivo INF es obligatoria para todas las plantillas de solicitud de certificado nuevas y debe incluir al menos un parámetro con un valor.

| Clave<sup>1</sup> | Descripción | Valor<sup>2</sup> | Ejemplo |
| --- | ---------- | ----- | ------- |
| Asunto | Varias aplicaciones se basan en la información del firmante de un certificado. Se recomienda especificar un valor para esta clave. Si el asunto no se establece aquí, se recomienda incluir un nombre de sujeto como parte de la extensión de certificado de nombre alternativo del firmante. | Valores de cadena de nombre distintivo relativo | Subject = CN = Equipo1. contoso. com subject = CN = John Smith, CN = users, DC = Contoso, DC = com |
| Exportable | Si se establece en TRUE, la clave privada se puede exportar con el certificado. Para garantizar un alto nivel de seguridad, las claves privadas no deben ser exportables; sin embargo, en algunos casos, podría ser necesario si varios equipos o usuarios deben compartir la misma clave privada. | `true | false` | `Exportable = TRUE`. Las claves CNG pueden distinguir entre este y el texto sin formato exportable. No se pueden CAPI1 claves. |
| ExportableEncrypted | Especifica si la clave privada debe configurarse para ser exportable. | `true | false` | `ExportableEncrypted = true`<p>**Sugerencia:** No todos los tamaños y algoritmos de clave pública funcionarán con todos los algoritmos hash. El CSP especificado también debe admitir el algoritmo hash especificado. Para ver la lista de algoritmos hash admitidos, puede ejecutar el comando: `certutil -oid 1 | findstr pwszCNGAlgid | findstr /v CryptOIDInfo` |
| HashAlgorithm | Algoritmo hash que se va a usar para esta solicitud. | `Sha256, sha384, sha512, sha1, md5, md4, md2` | `HashAlgorithm = sha1`. Para ver la lista de algoritmos hash admitidos, use: certutil-OID 1 | Findstr pwszCNGAlgid | Findstr/v CryptOIDInfo|
| KeyAlgorithm| Algoritmo que usará el proveedor de servicios para generar un par de claves pública y privada.| `RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521` | `KeyAlgorithm = RSA` |
| KeyContainer | No se recomienda establecer este parámetro para las nuevas solicitudes donde se genera el nuevo material de clave. El sistema genera y mantiene automáticamente el contenedor de claves.<p>En el caso de las solicitudes en las que se debe usar el material de clave existente, este valor se puede establecer en el nombre del contenedor de claves de la clave existente. Use el `certutil –key` comando para mostrar la lista de contenedores de claves disponibles para el contexto del equipo. Use el `certutil –key –user` comando para el contexto del usuario actual.| Valor de cadena aleatoria<p>**Sugerencia:** Use comillas dobles alrededor de cualquier valor de clave INF que tenga espacios en blanco o caracteres especiales para evitar posibles problemas de análisis de INF. | `KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}` |
| KeyLength | Define la longitud de la clave pública y privada. La longitud de la clave tiene un impacto en el nivel de seguridad del certificado. Una longitud de clave mayor suele proporcionar un nivel de seguridad superior. sin embargo, algunas aplicaciones pueden tener limitaciones respecto a la longitud de la clave. | Cualquier longitud de clave válida admitida por el proveedor de servicios de cifrado. | `KeyLength = 2048` |
| Especificación | Determina si la clave se puede utilizar para firmas, para Exchange (cifrado) o para ambos. | `AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE` | `KeySpec = AT_KEYEXCHANGE` |
| KeyUsage | Define para qué se debe usar la clave de certificado. | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> | `KeyUsage = CERT_DIGITAL_SIGNATURE_KEY_USAGE | CERT_KEY_ENCIPHERMENT_KEY_USAGE`<p>**Sugerencia:** Varios valores usan una canalización (|). Asegúrese de usar comillas dobles al usar varios valores para evitar problemas de análisis de INF. Los valores que se muestran son valores hexadecimales (decimal) para cada definición de bits. También se puede usar la sintaxis anterior: un único valor hexadecimal con varios bits establecidos, en lugar de la representación simbólica. Por ejemplo, `KeyUsage = 0xa0`. |
| KeyUsageProperty | Recupera un valor que identifica el propósito específico para el que se puede usar una clave privada. | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> | `KeyUsageProperty = NCRYPT_ALLOW_DECRYPT_FLAG | NCRYPT_ALLOW_SIGNING_FLAG` |
| MachineKeySet | Esta clave es importante cuando es necesario crear certificados que son propiedad de la máquina y no de un usuario. El material de clave que se genera se mantiene en el contexto de seguridad de la entidad de seguridad (cuenta de usuario o de equipo) que ha creado la solicitud. Cuando un administrador crea una solicitud de certificado en nombre de un equipo, el material de clave se debe crear en el contexto de seguridad de la máquina y no en el contexto de seguridad del administrador. De lo contrario, el equipo no pudo obtener acceso a su clave privada, ya que se encontraba en el contexto de seguridad del administrador. | `true | false`. El valor predeterminado es false. | `MachineKeySet = true` |
| NotBefore | Especifica una fecha o una fecha y hora antes de la cual no se puede emitir la solicitud. `NotBefore` se puede usar con `ValidityPeriod` y `ValidityPeriodUnits` . | Fecha o fecha y hora | `NotBefore = 7/24/2012 10:31 AM`<p>**Sugerencia:** `NotBefore` y `NotAfter` son solo para R `equestType=cert` . El análisis de fechas intenta ser sensible a la configuración regional. El uso de nombres de meses eliminará la ambigüedad y debería funcionar en todas las configuraciones regionales. |
| NotAfter | Especifica una fecha o fecha y hora después de la cual no se puede emitir la solicitud. `NotAfter` no se puede utilizar con `ValidityPeriod` o `ValidityPeriodUnits` . | Fecha o fecha y hora | `NotAfter = 9/23/2014 10:31 AM`<p>**Sugerencia:** `NotBefore` y `NotAfter` son `RequestType=cert` solo para. El análisis de fechas intenta ser sensible a la configuración regional. El uso de nombres de meses eliminará la ambigüedad y debería funcionar en todas las configuraciones regionales. |
| PrivateKeyArchive | La configuración PrivateKeyArchive solo funciona si el RequestType correspondiente está establecido en CMC porque solo el formato de solicitud de mensajes de administración de certificados sobre CMS (CMC) permite transferir de forma segura la clave privada del solicitante a la CA para el archivo de claves. | `true | false` | `PrivateKeyArchive = true` |
| EncryptionAlgorithm | El algoritmo de cifrado que se va a usar. | Las opciones posibles varían en función de la versión del sistema operativo y del conjunto de proveedores de servicios criptográficos instalados. Para ver la lista de algoritmos disponibles, ejecute el comando: `certutil -oid 2 | findstr pwszCNGAlgid` . El CSP especificado debe admitir también el algoritmo de cifrado simétrico y la longitud especificados. | `EncryptionAlgorithm = 3des` |
| EncryptionLength | Longitud del algoritmo de cifrado que se va a usar. | Cualquier longitud permitida por el EncryptionAlgorithm especificado. | `EncryptionLength = 128` |
| ProviderName | El nombre del proveedor es el nombre para mostrar del CSP. | Si no conoce el nombre del proveedor de CSP que está utilizando, ejecute `certutil –csplist` desde una línea de comandos. El comando mostrará los nombres de todos los CSP que están disponibles en el sistema local. | `ProviderName = Microsoft RSA SChannel Cryptographic Provider` |
| ProviderType | El tipo de proveedor se usa para seleccionar proveedores específicos en función de la capacidad de un algoritmo concreto, como RSA Full. | Si no conoce el tipo de proveedor del CSP que está utilizando, ejecute `certutil –csplist` desde un símbolo de la línea de comandos. El comando mostrará el tipo de proveedor de todos los CSP que están disponibles en el sistema local. | `ProviderType = 1` |
| RenewalCert | Si necesita renovar un certificado que existe en el sistema donde se genera la solicitud de certificado, debe especificar su hash de certificado como valor de esta clave. | El hash del certificado de cualquier certificado que esté disponible en el equipo donde se crea la solicitud de certificado. Si no conoce el hash del certificado, use el Snap-In de MMC certificados y examine el certificado que se debe renovar. Abra las propiedades del certificado y vea el `Thumbprint` atributo del certificado. La renovación de certificados requiere un `PKCS#7` `CMC` formato de solicitud o. | `RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D` |
| RequesterName | Realiza la solicitud de inscripción en nombre de otra solicitud de usuario. La solicitud también debe estar firmada con un certificado de agente de inscripción o la CA rechazará la solicitud. Use la `-cert` opción para especificar el certificado de agente de inscripción. Se puede especificar el nombre del solicitante para las solicitudes de certificado si `RequestType` se establece en `PKCS#7` o `CMC` . Si `RequestType` se establece en `PKCS#10` , se omitirá esta clave. `Requestername`Solo se puede establecer como parte de la solicitud. No se puede manipular `Requestername` en una solicitud pendiente. | `Domain\User` | `Requestername = Contoso\BSmith` |
| RequestType | Determina el estándar que se usa para generar y enviar la solicitud de certificado. | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul>**Sugerencia:** Esta opción indica que se trata de un certificado autofirmado o emitido por sí mismo. No genera una solicitud, sino un nuevo certificado y, a continuación, instala el certificado. Autofirmado es el valor predeterminado. Especifique un certificado de firma mediante la opción – CERT para crear un certificado emitido automáticamente que no sea autofirmado. | `RequestType = CMC` |
| SecurityDescriptor | Contiene la información de seguridad asociada a los objetos protegibles. Para la mayoría de los objetos protegibles, puede especificar el descriptor de seguridad de un objeto en la llamada de función que crea el objeto. Cadenas basadas en el [lenguaje de definición de descriptores de seguridad](/windows/win32/secauthz/security-descriptor-definition-language).<p>**Sugerencia:** Esto solo es relevante para las claves de la tarjeta no inteligente de contexto del equipo. | `SecurityDescriptor = D:P(A;;GA;;;SY)(A;;GA;;;BA)` |
| AlternateSignatureAlgorithm | Especifica y recupera un valor booleano que indica si el identificador de objeto (OID) del algoritmo de firma para una solicitud PKCS # 10 o una firma de certificado es discreto o combinado. | `true | false` | `AlternateSignatureAlgorithm = false`<p>En el caso de una firma RSA, `false` indica un `Pkcs1 v1.5` , mientras que `true` indica una `v2.1` firma. |
| Silencioso | De forma predeterminada, esta opción permite al CSP tener acceso al escritorio del usuario interactivo y solicitar información como un PIN de tarjeta inteligente del usuario. Si esta clave se establece en TRUE, el CSP no debe interactuar con el escritorio y se le bloqueará para que no muestre ninguna interfaz de usuario al usuario. | `true | false` | `Silent = true` |
| SMIME | Si este parámetro se establece en TRUE, se agrega a la solicitud una extensión con el valor de identificador de objeto 1.2.840.113549.1.9.15. El número de identificadores de objeto depende de la versión del sistema operativo instalada y de la capacidad de CSP, que hace referencia a los algoritmos de cifrado simétricos que pueden usar las aplicaciones de extensiones multipropósito de correo Internet (S/MIME) seguras, como Outlook. | `true | false` | `SMIME = true` |
| UseExistingKeySet | Este parámetro se usa para especificar que se debe utilizar un par de claves existente en la creación de una solicitud de certificado. Si esta clave se establece en TRUE, también debe especificar un valor para la clave RenewalCert o el nombre de KeyContainer. No debe establecer la clave exportable porque no puede cambiar las propiedades de una clave existente. En este caso, no se genera material de clave cuando se compila la solicitud de certificado. | `true | false` | `UseExistingKeySet = true` |
| KeyProtection | Especifica un valor que indica cómo se protege una clave privada antes de su uso. | <ul><li>`XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0`</li><li>`XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> | `KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG` |
| SuppressDefaults | Especifica un valor booleano que indica si las extensiones y atributos predeterminados se incluyen en la solicitud. Los valores predeterminados se representan mediante sus identificadores de objeto (OID). | `true | false` | `SuppressDefaults = true` |
| FriendlyName | Un nombre descriptivo para el nuevo certificado. | Texto | `FriendlyName = Server1` |
| ValidityPeriodUnits | Especifica el número de unidades que se va a utilizar con ValidityPeriod. Nota: solo se usa cuando el `request type=cert` . | Numérico | `ValidityPeriodUnits = 3` |
| ValidityPeriod | ValidityPeriod debe ser un período de tiempo plural del Inglés de EE. UU. Nota: solo se usa cuando el tipo de solicitud = cert. | `Years |  Months | Weeks | Days | Hours | Minutes | Seconds` | `ValidityPeriod = Years` |

<sup>1</sup> A la izquierda del signo igual (=)

<sup>2</sup> A la derecha del signo igual (=)

#### <a name="extensions"></a>Extension

Esta sección es opcional.

| OID de extensión | Definición | Ejemplo |
| ------------- | ---------- | ----- | ------- |
| 2.5.29.17 | | 2.5.29.17 = {Text} |
| *continue* | | `continue = UPN=User@Domain.com&` |
| *continue* | | `continue = EMail=User@Domain.com&` |
| *continue* | | `continue = DNS=host.domain.com&` |
| *continue* | | `continue = DirectoryName=CN=Name,DC=Domain,DC=com&` |
| *continue* | | `continue = URL=<http://host.domain.com/default.html&>` |
| *continue* | | `continue = IPAddress=10.0.0.1&` |
| *continue* | | `continue = RegisteredId=1.2.3.4.5&` |
| *continue* | | `continue = 1.2.3.4.6.1={utf8}String&` |
| *continue* | | `continue = 1.2.3.4.6.2={octet}AAECAwQFBgc=&` |
| *continue* | | `continue = 1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&` |
| *continue* | | `continue = 1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&` |
| *continue* | | `continue = 1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07` |
| 2.5.29.37 | | `2.5.29.37={text}` |
| *continue* | | `continue = 1.3.6.1.5.5.7` |
| *continue* | | `continue = 1.3.6.1.5.5.7.3.1` |
| 2.5.29.19 | | `{text}ca=0pathlength=3` |
| Crítico | | `Critical=2.5.29.19` |
| Especificación | | <ul><li>`AT_NONE -- 0`</li><li>`AT_SIGNATURE -- 2`</li><li>`AT_KEYEXCHANGE -- 1`</ul></li> |
| RequestType | | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul> |
| KeyUsage | | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> |
| KeyUsageProperty | | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> |
| KeyProtection | | <ul><li>`NCRYPT_UI_NO_PROTECTION_FLAG -- 0`</li><li>`NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> |
| SubjectNameFlags | template | <ul><li>`CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME -- 40000000 (1073741824)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)`</li><li>`CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)`</li></ul> |
| X500NameFlags | | <ul><li>`CERT_NAME_STR_NONE -- 0`</li><li>`CERT_OID_NAME_STR -- 2`</li><li>`CERT_X500_NAME_STR -- 3`</li><li>`CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)`</li><li>`CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)`</li><li>`CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)`</li><li>`CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)`</li><li>`CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)`</li><li>`CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)`</li><li>`CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)`</li><li>`CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)`</li><li>`CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)`</li><li>`CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)`</li><li>`CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)`</li><li>`CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)`</li><li>`CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152)`</li></ul> |

> [!NOTE]
> `SubjectNameFlags` permite que el archivo INF especifique qué campos de extensión de **asunto** y **SubjectAltName** se deben rellenar automáticamente mediante CertReq en función del usuario actual o de las propiedades de la máquina actual: nombre DNS, UPN, etc. El uso de la plantilla literal significa que en su lugar se utilizan marcas de nombre de plantilla. Esto permite usar un único archivo INF en varios contextos para generar solicitudes con información de asunto específica del contexto.
>
> `X500NameFlags` especifica las marcas que se van a pasar directamente a `CertStrToName` la API cuando el `Subject INF keys` valor se convierte en un **nombre** distintivo con codificación ASN. 1.

#### <a name="example"></a>Ejemplo

Para crear un archivo de directivas (. inf) en el Bloc de notas y guardarlo como *requestconfig. inf*:

```
[NewRequest]
Subject = CN=<FQDN of computer you are creating the certificate>
Exportable = TRUE
KeyLength = 2048
KeySpec = 1
KeyUsage = 0xf0
MachineKeySet = TRUE
[RequestAttributes]
CertificateTemplate=WebServer
[Extensions]
OID = 1.3.6.1.5.5.7.3.1
OID = 1.3.6.1.5.5.7.3.2
```

En el equipo para el que está solicitando un certificado:

```
certreq –new requestconfig.inf certrequest.req
```

Para usar la sintaxis de la sección [Strings] para OID y otros datos difíciles de interpretar. El nuevo ejemplo de la sintaxis {Text} para la extensión EKU, que usa una lista separada por comas de OID:

```
[Version]
Signature=$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = 2.5.29.37
szOID_PKIX_KP_SERVER_AUTH = 1.3.6.1.5.5.7.3.1
szOID_PKIX_KP_CLIENT_AUTH = 1.3.6.1.5.5.7.3.2

[NewRequest]
Subject = CN=TestSelfSignedCert
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%={text}%szOID_PKIX_KP_SERVER_AUTH%,
_continue_ = %szOID_PKIX_KP_CLIENT_AUTH%
```

### <a name="certreq--accept"></a>CertReq: Accept

El `–accept` parámetro vincula la clave privada generada previamente con el certificado emitido y quita la solicitud de certificado pendiente del sistema en el que se solicita el certificado (si hay una solicitud coincidente).

Para aceptar un certificado manualmente:

```
certreq -accept certnew.cer
```

> [!WARNING]
> El uso del `-accept` parámetro con `-user` las `–machine` Opciones y indica si el certificado de instalación debe instalarse en el contexto de **usuario** o de **equipo** . Si hay una solicitud pendiente en cualquier contexto que coincida con la clave pública que se va a instalar, estas opciones no son necesarias. Si no hay ninguna solicitud pendiente, se debe especificar uno de ellos.

### <a name="certreq--policy"></a>CertReq: Directiva

El archivo Policy. inf es un archivo de configuración que define las restricciones que se aplican a una certificación de CA cuando se define una subordinación cualificada.

Para generar una solicitud de certificado cruzado:

```
certreq -policy certsrv.req policy.inf newcertsrv.req
```

`certreq -policy`Si usa sin ningún parámetro adicional, se abre una ventana de cuadro de diálogo, que le permite seleccionar los archivos solicitados (. req,. CMC,. txt,. der,. cer o. CRT). Después de seleccionar el archivo solicitado y hacer clic en **abrir**, se abre otra ventana de diálogo que le permite seleccionar el archivo Policy. inf.

#### <a name="examples"></a>Ejemplos

Busque un ejemplo del archivo Policy. inf en la [Sintaxis de CAPolicy. inf](../../networking/core-network-guide/cncg/server-certs/prepare-the-capolicy-inf-file.md).

### <a name="certreq--sign"></a>CertReq: Sign

Para crear una nueva solicitud de certificado, firmarla y enviarla:

```
certreq -new policyfile.inf myrequest.req
certreq -sign myrequest.req myrequest.req
certreq -submit myrequest_sign.req myrequest_cert.cer
```

#### <a name="remarks"></a>Comentarios

- Si usa `certreq -sign` sin ningún parámetro adicional, se abrirá una ventana de diálogo para que pueda seleccionar el archivo solicitado (req, CMC, txt, der, cer o CRT).

- La firma de la solicitud de subordinación completa puede requerir credenciales de **Administrador de empresa** . Se trata de un procedimiento recomendado para emitir certificados de firma para la subordinación cualificada.

- El certificado usado para firmar la solicitud de subordinación calificada usa la plantilla de subordinación completa. Los administradores de empresa tendrán que firmar la solicitud o conceder permisos de usuario a los usuarios que firmen el certificado.

- Es posible que sea necesario que el personal adicional firme la solicitud de CMC después de usted. Esto dependerá del nivel de seguridad asociado con la subordinación cualificada.

- Si la CA primaria de la CA subordinada completa que está instalando está sin conexión, debe obtener el certificado de CA para la CA subordinada completa del elemento primario sin conexión. Si la CA primaria está en línea, especifique el certificado de CA para la CA subordinada completa durante el Asistente para la **instalación de servicios de certificados** .

### <a name="certreq--enroll"></a>CertReq: inscribirse

Puede usar este comentario para inscribir o renovar los certificados.

#### <a name="examples"></a>Ejemplos

Para inscribir un certificado, use la plantilla *WebServer* y seleccionando el servidor de directivas con U/I:

```
certreq -enroll –machine –policyserver * WebServer
```

Para renovar un certificado mediante un número de serie:

```
certreq –enroll -machine –cert 61 2d 3c fe 00 00 00 00 00 05 renew
```

Solo puede renovar certificados válidos. Los certificados expirados no se pueden renovar y deben reemplazarse por un nuevo certificado.

## <a name="options"></a>Opciones

| Opciones | Descripción |
| ------- | ----------- |
| -Any | `Force ICertRequest::Submit` para determinar el tipo de codificación.|
| -attrib `<attributestring>` | Especifica los pares de cadena de **nombre** y **valor** , separados por dos puntos.<p>Separe los pares de **nombre** y **valor** de cadena mediante `\n` (por ejemplo, nombre1: value1\nName2: valor2). |
| binario | Da formato binario a los archivos de salida en lugar de a la codificación Base64. |
| -policyserver `<policyserver>` | LDAP `<path>`<br>Inserte el URI o el identificador único de un equipo que ejecute el servicio Web de directiva de inscripción de certificados.<p>Para especificar que desea utilizar un archivo de solicitud examinando, solo tiene que usar un signo menos (-) para `<policyserver>` . |
| -config `<ConfigString>` | Procesa la operación utilizando la entidad de certificación especificada en la cadena de configuración, que es **CAHostName\CAName**. Para una conexión https: \\ \, especifique el URI del servidor de inscripciones. Para la CA del almacén del equipo local, use un signo menos (-). |
| -anónimo | Use credenciales anónimas para los servicios Web de inscripción de certificados. |
| -Kerberos | Use credenciales de Kerberos (dominio) para los servicios Web de inscripción de certificados. |
| -ClientCertificate `<ClientCertId>` | Puede reemplazar `<ClientCertId>` por una huella digital de certificado, CN, EKU, plantilla, correo electrónico, UPN o la nueva `name=value` Sintaxis. |
| -username `<username>` | Se usa con los servicios Web de inscripción de certificados. Puede sustituir `<username>` por el nombre Sam o el valor **dominio\usuario** . Esta opción es para su uso con la `-p` opción. |
| -p `<password>` | Se usa con los servicios Web de inscripción de certificados. Sustituya `<password>` por la contraseña del usuario real. Esta opción es para su uso con la `-username` opción. |
| -usuario | Configura el `-user` contexto para una nueva solicitud de certificado o especifica el contexto de una aceptación de certificado. Este es el contexto predeterminado, si no se especifica ninguno en el archivo INF o la plantilla. |
| -equipo | Configura una nueva solicitud de certificado o especifica el contexto de una aceptación de certificado para el contexto de la máquina. En el caso de las nuevas solicitudes, debe ser coherente con la clave de MachineKeyset INF y el contexto de la plantilla. Si no se especifica esta opción y la plantilla no establece un contexto, el valor predeterminado es el contexto del usuario. |
| -CRL | Incluye listas de revocación de certificados (CRL) en la salida del archivo PKCS #7 codificado en Base64 especificado por `certchainfileout` o en el archivo codificado en Base64 especificado por `requestfileout` . |
| -RPC | Indica a Active Directory servicios de Certificate Server (AD CS) que utilice una conexión de servidor de llamada a procedimiento remoto (RPC) en lugar de COM distribuido. |
| -adminforcemachine | Use el servicio de clave o la suplantación para enviar la solicitud desde el contexto del sistema local. Requiere que el usuario que invoca esta opción sea miembro de los administradores locales. |
| -renewonbehalfof | Envíe una renovación en nombre del firmante identificado en el certificado de firma. Esto establece CR_IN_ROBO al llamar al [método ICertRequest:: submit](/windows/win32/api/certcli/nf-certcli-icertrequest-submit) |
| -f | Forzar la sobrescritura de los archivos existentes. Esto también omite las plantillas y directivas de almacenamiento en caché. |
| -q | Usar el modo silencioso; suprime todos los mensajes interactivos. |
| -Unicode | Escribe la salida Unicode cuando la salida estándar se redirige o se canaliza a otro comando, lo que ayuda cuando se invoca desde scripts de Windows PowerShell. |
| -unicodetext | Envía la salida Unicode al escribir blobs de datos codificados de texto Base64 en los archivos. |

## <a name="formats"></a>Formatos

| Formatos | Descripción |
| ------- | ----------- |
| requestfilein | Nombre del archivo de entrada binario o codificado en Base64: PKCS #10 solicitud de certificado, solicitud de certificado CMS, solicitud de renovación de certificado PKCS #7, certificado X. 509 que va a ser una solicitud de certificado con formato de etiqueta con certificación cruzada o formato de etiqueta KeyGen. |
| requestfileout | Nombre del archivo de salida con codificación Base64. |
| certfileout | Nombre de archivo X-509 con codificación Base64. |
| PKCS10fileout | Solo se usa con el `certreq -policy` parámetro. Nombre del archivo de salida de PKCS10 codificado en Base64. |
| certchainfileout | Nombre de archivo PKCS #7 codificado en Base64. |
| fullresponsefileout | Nombre del archivo de respuesta completo con codificación Base64. |
| policyfilein | Solo se usa con el `certreq -policy` parámetro. Archivo INF que contiene una representación textual de las extensiones utilizadas para calificar una solicitud. |

## <a name="additional-resources"></a>Recursos adicionales

Los artículos siguientes contienen ejemplos de uso de CertReq:

- [Cómo agregar un nombre alternativo del firmante a un certificado LDAP seguro](https://support.microsoft.com/help/931351/how-to-add-a-subject-alternative-name-to-a-secure-ldap-certificate)

- [Test Lab Guide: Deploying an AD CS Two-Tier PKI Hierarchy](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831348(v=ws.11))

- [Apéndice 3: sintaxis de Certreq.exe](/previous-versions/windows/it-pro/windows-server-2003/cc736326(v=ws.10))

- [Creación manual de un certificado SSL de servidor Web](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/how-to-create-a-web-server-ssl-certificate-manually/ba-p/1128529)

- [Inscripción de certificados para el agente de System Center Operations Manager](/system-center/scom/plan-planning-agent-deployment)

- [Introducción a los Servicios de certificados de Active Directory](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11))

- [Habilitación de LDAP a través de SSL con una entidad de certificación de terceros](https://support.microsoft.com/help/321051/how-to-enable-ldap-over-ssl-with-a-third-party-certification-authority)
