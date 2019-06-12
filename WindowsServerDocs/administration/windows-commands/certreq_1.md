---
title: certreq
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b947a231228d66084bc61146f7347a76cf41406
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434510"
---
# <a name="certreq"></a>certreq



CertReq se puede usar para solicitar certificados de una entidad de certificación (CA), para recuperar una respuesta a una solicitud anterior de una entidad de certificación, para crear una nueva solicitud de un archivo .inf, para aceptar e instalar una respuesta a una solicitud, para construir una certificación cruzada o solicitud de subordinación completa desde un certificado de CA o solicitud existente y para firmar una solicitud de certificación cruzada o de subordinación completa.

> [!WARNING]
> - Las versiones anteriores de certreq no pueden proporcionar todas las opciones que se describen en este documento. Puede ver todas las opciones que proporciona una versión específica de certreq ejecutando los comandos que se muestra en la sección de sintaxis de notaciones.
> - CertReq no admite la creación de una nueva solicitud de certificado basada en una plantilla de atestación de clave cuando se encuentra en un entorno de procesamiento de eventos COMPLEJOS/CES

## <a name="BKMK_Contents"></a>Contenido

Las secciones principales en este artículo son los siguientes:
1.  [Verbs](#BKMK_Verbs)
2.  [Notaciones de sintaxis](#BKMK_notation)
3.  [Opciones](#BKMK_Options)
4.  [Formatos](#BKMK_Formats)
5.  [Ejemplos adicionales certreq](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>Verbs

No existe en la tabla siguiente se describe los verbos que se pueden usar con el comando certreq

|Modificador|Descripción|
|------|-----------|
|-Submit|Envía una solicitud a una entidad de certificación. Para obtener más información, consulte [Certreq-submit](#BKMK_Submit).|
|-recuperar *RequestID*|Recupera una respuesta a una solicitud anterior de una entidad de certificación. Para obtener más información, consulte [Certreq-recuperar](#BKMK_Retrieve).|
|-Nuevo|Crea una nueva solicitud de un archivo .inf. Para obtener más información, consulte [Certreq-nueva](#BKMK_New).|
|-Aceptar|Acepta e instala una respuesta a una solicitud de certificado. Para obtener más información, consulte [Certreq-acepte](#BKMK_accept).|
|-Policy|Establece la directiva para una solicitud. Para obtener más información, consulte [Certreq-directiva](#BKMK_policy).|
|: Inicio de sesión|Inicia una solicitud de certificación cruzada o de subordinación completa. Para obtener más información, consulte [Certreq-inicio de sesión](#BKMK_sign).|
|-Inscribir|Inscribe para o renueva un certificado. Para obtener más información, consulte [Certreq-inscribir](#BKMK_enroll).|
|-?|Muestra una lista de sintaxis certreq, opciones y descripciones.|
|*\<verb>* -?|Muestra ayuda para el verbo especificado.|
|-v -?|Muestra una lista detallada de la sintaxis certreq, opciones y descripciones.|

Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_notation"></a>Notaciones de sintaxis

-   Para conocer la sintaxis básica de línea de comandos, ejecute `certreq -?`
-   ¿Para obtener la sintaxis sobre el uso de certutil con un verbo específico, ejecute **certreq**  *\<verbo >* **-?**
-   Para enviar la sintaxis de certutil en un archivo de texto, ejecute los siguientes comandos:  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

En la tabla siguiente se describe la notación que se utiliza para indicar la sintaxis de línea de comandos.

|Notación|Descripción|
|--------|-----------|
|Texto sin corchetes ni llaves|Elementos que se debe escribir como se muestra|
|\<Texto dentro de corchetes angulares >|Marcador de posición para el que debe proporcionar un valor|
|[Texto entre corchetes]|Elementos opcionales|
|{Texto entre llaves}|Conjunto de elementos necesarios; Elija una|
|Barra vertical (&#124;)|Separador para elementos mutuamente excluyentes; Elija una|
|Puntos suspensivos (...)|Elementos que se pueden repetir|

Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq -submit

Este es el parámetro de certreq.exe de forma predeterminada, si se especifica ninguna opción explícitamente en el símbolo del sistema de línea de comandos, certreq.exe intenta enviar una solicitud de certificado a una entidad de certificación.
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
Debe especificar un archivo de solicitud de certificado cuando se usa el: opción de enviar. Si se omite este parámetro, se muestra una ventana Abrir archivo común donde puede seleccionar el archivo de solicitud de certificado adecuado.

Puede usar estos ejemplos como punto de partida para crear la solicitud de envío de certificado:

Para enviar una solicitud de certificado simple use el ejemplo siguiente:
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
Para solicitar un certificado mediante la especificación del atributo de SAN, consulte los pasos detallados en el artículo de Microsoft Knowledge Base 931351 [cómo agregar un nombre alternativo del sujeto a un certificado LDAP seguro](https://support.microsoft.com/kb/931351) en el "cómo usar la utilidad Certreq.exe sección Crear y enviar una solicitud de certificado que incluye una SAN".

Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq -retrieve

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   Si no se especifica el NombreDeEquipoCA o nombreCA en el cuadro de diálogo - config CAComputerName\CANamea aparece y muestra una lista de todas las entidades emisoras de certificados que están disponibles.
-   Si utiliza - config - en lugar de-config CAComputerName\CAName, la operación se procesa con la entidad de certificación predeterminada.
-   Puede usar certreq-recuperar *RequestID* para recuperar el certificado después de la entidad de certificación ha emitido. El *RequestID*PKC puede ser un número decimal o hexadecimal 0 x prefijo y puede ser un número de serie del certificado con ningún prefijo 0 x. También puede usar para recuperar cualquier certificado que haya sido emitido por la CA, incluidos los certificados revocados o caducados, sin tener en cuenta si la solicitud del certificado alguna vez estuvo en estado pendiente.
-   Si envía una solicitud a la entidad de certificación, el módulo de directivas de la entidad de certificación podría dejar la solicitud en un estado pendiente y devuelven el *RequestID* al llamador Certreq para su presentación. Finalmente, el Administrador de CA se emite el certificado o denegar la solicitud.

El comando siguiente recupera el identificador del certificado 20 y crea el archivo de certificado (.cer):
```
certreq -retrieve 20 MyCertificate.cer 
```
Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq -new

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
Puesto que permite un amplio conjunto de parámetros y las opciones que se especifique el archivo INF, es difícil de definir una plantilla predeterminada que los administradores deben usar para todos los propósitos. Por lo tanto, en esta sección se describen todas las opciones que le permite crear un archivo INF adaptado a sus necesidades específicas. Las palabras clave siguientes se utilizan para describir la estructura del archivo INF.
1.  Un *sección* es un área en el archivo INF que abarca un grupo lógico de claves. Una sección siempre aparece entre paréntesis en el archivo INF.
2.  Un *clave* es el parámetro que está a la izquierda del signo igual.
3.  Un *valor* es el parámetro que está a la derecha del signo igual.

Por ejemplo, un archivo INF mínimo tendría un aspecto similar al siguiente:
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
Estas son algunas de las secciones posibles que pueden agregarse al archivo INF:

**[NewRequest]**

En esta sección es obligatoria para un archivo INF que actúa como una plantilla para una nueva solicitud de certificado. En esta sección requiere al menos una clave con un valor.

|Key|Definición|Valor|Ejemplo|
|---|----------|-----|-------|
|Subject|Varias aplicaciones dependen de la información del firmante en un certificado. Por lo tanto, se recomienda que se especifique un valor para esta clave. Si el sujeto no se establece aquí, se recomienda que un nombre de sujeto se incluye como parte de la extensión de certificado nombre alternativo del firmante.|Valores de cadena de nombre distintivo relativos|Asunto = firmante "CN=computer1.contoso.com" = "CN = John Smith, CN = Users, DC = Contoso, DC = com"|
|Exportable|Si este atributo se establece en TRUE, se puede exportar la clave privada con el certificado. Para garantizar un alto nivel de seguridad, no deben ser exportables; las claves privadas Sin embargo, en algunos casos, podría requerirse para que la clave privada fuera exportable si varios equipos o usuarios deben compartir la misma clave privada.|es true, false|Exportable = TRUE. Las claves CNG pueden distinguir entre esto y plaintext exportable. No se pueden CAPI1 claves.|
|ExportableEncrypted|Especifica si se debe establecer la clave privada para ser exportable.|es true, false|ExportableEncrypted = true</br>Sugerencia: No todos los tamaños de clave públicos y los algoritmos funcionará con todos los algoritmos hash. Tamehe especificado que CSP también debe admitir el algoritmo hash especificado. Para ver la lista de los algoritmos hash admitidos, puede ejecutar el comando <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|Algoritmo hash que se usará para esta solicitud.|Sha256, sha384, sha512, sha1, md5, md4, md2|HashAlgorithm = sha1. Para ver la lista de los algoritmos hash admitidos, use: certutil - oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo|
|KeyAlgorithm|El algoritmo que se usará el proveedor de servicios para generar un par de claves público y privado.|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|No se recomienda establecer este parámetro para las nuevas solicitudes donde se genera el nuevo material de clave. El contenedor de claves se genera automáticamente y se mantiene el sistema. Para las solicitudes que se debe usar el material de clave existente, este valor se puede establecer en el nombre de contenedor de claves de la clave existente. Usar el certutil: comando para mostrar la lista de contenedores de claves disponibles para el contexto del equipo de clave. Usar el certutil – clave: comando de usuario para el contexto del usuario actual.|Valor de cadena aleatoria</br>Sugerencia: Debe usar comillas dobles alrededor de cualquier valor de clave INF que tiene espacios en blanco o caracteres especiales para evitar posibles problemas de análisis de INF.|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|Define la longitud de la clave pública y privada. La longitud de clave tiene un impacto en el nivel de seguridad del certificado. Longitud de clave mayor normalmente proporciona un mayor nivel de seguridad; Sin embargo, algunas aplicaciones pueden tener limitaciones relativas a la longitud de clave.|Cualquier longitud de clave válido que es compatible con el proveedor de servicios criptográficos.|KeyLength = 2048|
|KeySpec|Determina si la clave se puede utilizar para las firmas, para Exchange (cifrado) o para ambos.|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|KeyUsage|Define lo que debe usarse la clave del certificado.|CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)</br>Sugerencia: Los valores mostrados son valores hexadecimales de (decimales) para cada definición de bits. También se puede usar la sintaxis anterior: un solo valor hexadecimal con varios bits conjunto, en lugar de la representación simbólica. Por ejemplo, KeyUsage = 0xa0.</br>CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE -- 1</br>CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>Sugerencia: Varios valores usan una barra vertical (&#124;) separador de símbolos. Asegúrese de que usa comillas dobles cuando se usan varios valores para evitar INF existen problemas de análisis.|
|KeyUsageProperty|Recupera un valor que identifica el propósito específico para el que se puede usar una clave privada.|NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|Esta clave es importante cuando se necesita crear los certificados que pertenecen a la máquina y no un usuario. Se mantiene el material de clave que se genera en el contexto de seguridad de la entidad de seguridad (cuenta de usuario o equipo) que ha creado la solicitud. Cuando un administrador crea una solicitud de certificado en nombre de un equipo, el material de clave debe crearse en el contexto de seguridad del equipo y no el contexto de seguridad del administrador. En caso contrario, el equipo no pudo acceder su clave privada ya que sería en el contexto de seguridad del administrador.|es true, false|MachineKeySet = true</br>Sugerencia: El valor predeterminado es False.|
|NotBefore|Especifica una fecha o fecha y hora antes de que no se puede emitir la solicitud. NotBefore puede utilizarse con ValidityPeriod y ValidityPeriodUnits.|fecha o fecha y hora|NotBefore = "24/7/2012 10:31 AM"</br>Sugerencia: NotBefore y NotAfter son para RequestType = solo certificado. Análisis de fecha intenta tener en cuenta la configuración regional. Uso de mes en los nombres de ambigüedad y deberían funcionar en cada configuración regional.|
|NotAfter|Especifica una fecha o fecha y hora después de que no se puede emitir la solicitud. No se puede usar NotAfter con ValidityPeriod o ValidityPeriodUnits.|fecha o fecha y hora|NotAfter = "9/23/2014 10:31 AM"</br>Sugerencia: NotBefore y NotAfter son para RequestType = solo certificado. Análisis de fecha intenta tener en cuenta la configuración regional. Uso de mes en los nombres de ambigüedad y deberían funcionar en cada configuración regional.|
|PrivateKeyArchive|La configuración de PrivateKeyArchive sólo funciona si RequestType correspondiente se establece en "CMC" porque solo los mensajes de administración de certificado en formato de solicitud de CMS (CMC) permite la transferencia segura de clave privada del solicitante a la CA para el archivo de claves.|es true, false|PrivateKeyArchive = True|
|EncryptionAlgorithm|Para usar el algoritmo de cifrado.|Las opciones posibles varían en función de la versión del sistema operativo y el conjunto de proveedores de servicios criptográficos instalados. Para ver la lista de algoritmos disponibles, ejecute el comando <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> también debe admitir el CSP especificado que utiliza el algoritmo de cifrado simétrico especificado y la longitud.|EncryptionAlgorithm = 3des|
|EncryptionLength|Longitud del algoritmo de cifrado que se usará.|Cualquier longitud permitida por el algoritmo de cifrado especificado.|EncryptionLength = 128|
|ProviderName|El nombre del proveedor es el nombre para mostrar del CSP...|Si no conoce el nombre del proveedor del CSP usa, ejecute certutil – csplist desde una línea de comandos. El comando mostrará los nombres de todos los CSP que están disponibles en el sistema local|ProviderName = "Microsoft RSA SChannel Cryptographic Provider"|
|ProviderType|El tipo de proveedor se utiliza para seleccionar proveedores específicos según la funcionalidad de algoritmo específicos como "RSA Full".|Si no conoce el tipo de proveedor del CSP usa, ejecute certutil – csplist desde un símbolo del sistema de línea de comandos. El comando mostrará el tipo de proveedor de CSP de todos los que están disponibles en el sistema local.|ProviderType = 1|
|RenewalCert|Si necesita renovar un certificado que no existe en el sistema donde se genera la solicitud de certificado, debe especificar su hash de certificado como el valor de esta clave.|El hash del certificado de cualquier certificado que está disponible en el equipo donde se creó la solicitud de certificado. Si no conoce el hash del certificado, utilice el complemento MMC de certificados y fíjese en el certificado que se debe renovar. Abra las propiedades del certificado y vea el atributo "Huella digital" del certificado. Renovación de certificado requiere un PKCS #7 o en un formato de solicitud CMC.|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>Nota: Esto hace que la solicitud para inscribirse en nombre de otra solicitud de usuario. La solicitud también debe estar firmada con un certificado de Enrollment Agent o la entidad de certificación rechazará la solicitud. Use-opción de certificado para especificar el certificado de agente de inscripción.|El nombre del solicitante puede especificarse para las solicitudes de certificado si RequestType está establecida en PKCS #7 o CMC. Si se establece RequestType para PKCS #10, se omitirá esta clave. Solo puede establecerse el Requestername como parte de la solicitud. No se puede manipular el Requestername en una solicitud pendiente.|Dominio\nombre de usuario|Requestername = "Contoso\BSmith"|
|RequestType|Determina el estándar que se usa para generar y enviar la solicitud de certificado.|PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>CERT--4</br>SCEP -- fd00 (64768)</br>Sugerencia: Esta opción indica un certificado autofirmado o de emisión propia. No se genera una solicitud, pero en su lugar un nuevo certificado y, a continuación, instala el certificado. Autofirmados es el valor predeterminado. Especifique un certificado de firma con el: opción cert para crear un certificado emitido automáticamente que no está autofirmado.|RequestType = CMC|
|SecurityDescriptor</br>Sugerencia: Esto solo es relevante para las claves de tarjeta no inteligente de contexto de equipo.|Contiene la información de seguridad asociada con los objetos protegibles. Para los objetos protegibles más, puede especificar un descriptor de seguridad en la llamada de función que crea el objeto.|Las cadenas según [lenguaje de definición de descriptores de seguridad](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).|SecurityDescriptor = "D:P(A;; DISPONIBILIDAD GENERAL;; SY) (A; DISPONIBILIDAD GENERAL;; BA)"|
|AlternateSignatureAlgorithm|Especifica y recupera un valor booleano que indica si el identificador de objeto del algoritmo de firma (OID) de una firma de solicitud o de certificado PKCS #10 es discreto o combinados.|es true, false|AlternateSignatureAlgorithm = false</br>Sugerencia: Para una firma RSA, false indica un v1.5 Pkcs1. True indica que una firma v2.1.|
|Silencio|De forma predeterminada, esta opción permite el acceso CSP a la información de solicitud y de escritorio de usuario interactivo como un NIP de tarjeta inteligente del usuario. Si esta clave se establece en TRUE, el CSP no debe interactuar con el escritorio y no podrá mostrar ninguna interfaz de usuario para el usuario.|es true, false|Silenciosa = true|
|SMIME|Si este parámetro se establece en TRUE, se agrega una extensión con el valor de identificador de objeto 1.2.840.113549.1.9.15 a la solicitud. El número de identificadores de objeto depende de la en la versión del sistema operativo instalado y la capacidad CSP, que hacen referencia a los algoritmos de cifrado simétrico que se pueden usar las aplicaciones de Secure Multipurpose Internet Mail Extensions (S/MIME), como Outlook.|es true, false|SMIME = true|
|UseExistingKeySet|Este parámetro se utiliza para especificar que se debe usar un par de claves existente en la creación de una solicitud de certificado. Si esta clave se establece en TRUE, también debe especificar un valor para la clave RenewalCert o el nombre de contenedor de claves. No debe establecer la clave Exportable porque no se puede cambiar las propiedades de una clave existente. En este caso, no se genera ningún material de clave cuando se compila la solicitud de certificado.|es true, false|UseExistingKeySet = true|
|KeyProtection|Especifica un valor que indica cómo se protege una clave privada antes de su uso.|XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|Especifica un valor booleano que indica si las extensiones predeterminadas y los atributos se incluyen en la solicitud. Los valores predeterminados se representan mediante sus identificadores de objeto (OID).|es true, false|SuppressDefaults = true|
|FriendlyName|Un nombre descriptivo para el nuevo certificado.|Text|FriendlyName = "Server1"|
|ValidityPeriodUnits</br>Nota: Solo se utiliza cuando la solicitud de tipo = cert.|Especifica un número de unidades que se va a usar con ValidityPeriod.|Numérico|ValidityPeriodUnits = 3|
|ValidityPeriod</br>Nota: Solo se utiliza cuando la solicitud de tipo = cert.|VValidityPeriod debe ser un inglés de Estados Unidos plural el período de tiempo.|Años, meses, semanas, días, horas, minutos, segundos|ValidityPeriod = años|

Vuelva a [contenido](#BKMK_Contents)

**[Extensiones]**

En esta sección es opcional.


|  OID de la extensión   | Definición | Valor |                                                                                                                                                                                                                                                                                                                                                                                                                      Ejemplo                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN=User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continuar* = "correo electrónico =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continuar* = "DNS=host.domain.com &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continuar* = "DirectoryName = CN = nombre, DC = Domain, DC = com &"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *continue* = "URL=<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress=10.0.0.1&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continuar* = "RegisteredId = 1.2.3.4.5 &"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continuar* = "1.2.3.4.6.1={utf8}String &"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2={octet}AAECAwQFBgc=&"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continuar* = "1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07 &"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continuar* = "1.2.3.4.6.3={hex}04 08 00, 01 02, 03 04, 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37="{text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continuar* = "1.3.6.1.5.5.7.                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continuar* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              "{text}ca=0pathlength=3"                                                                                                                                                                                                                                                                                                                                                                                                              |
|     Crítico     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 Crítica = 2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     KeySpec      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE -- 0</br>AT_SIGNATURE -- 2</br>AT_KEYEXCHANGE--1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>CERT--4</br>SCEP -- fd00 (64768)                                                                                                                                                                                                                                                                                                                                                                                   |
|     KeyUsage     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE -- 1</br>CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)                                                                                                                                                                                                       |
| KeyUsageProperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)                                                                                                                                                                                                                                                                                                                                             |
|  KeyProtection   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG -- 0</br>NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  plantilla  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME -- 40000000 (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID--1000000 (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS--8000000 (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS--400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN--2000000 (33554432)                                                                            |
|  X500NameFlags   |            |       | CERT_NAME_STR_NONE -- 0</br>CERT_OID_NAME_STR--2</br>CERT_X500_NAME_STR--3</br>CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)</br>CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)</br>CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)</br>CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)</br>CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152) |

Vuelva a [contenido](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags permite que el archivo INF especificar qué campos de asunto y SubjectAltName extensión deben estar rellena certreq según el usuario actual o las propiedades de la máquina actual: Nombre DNS, UPN y así sucesivamente. Usar el literal "plantilla" significa que las marcas de nombre de plantilla se usan en su lugar. Esto permite un único archivo INF para usarse en varios contextos para generar solicitudes de información del firmante específicos del contexto.
>
> X500NameFlags especifica las marcas que se pasará directamente a API CertStrToName cuando el valor de asunto INF claves se convierte en un ASN.1 codifican nombre distintivo.

Para solicitar un certificado basado con certreq-nuevo use los pasos del ejemplo siguiente:

> [!WARNING]
> El contenido de este tema se basa en la configuración predeterminada de Windows Server 2008 AD CS; Por ejemplo, establecer la longitud de clave a 2048, seleccionando Microsoft Software Key Storage Provider como CSP y usando el algoritmo Hash seguro 1 (SHA1). Evaluar estas selecciones según los requisitos de directiva de seguridad de su compañía.

Para crear una copia del archivo de directiva (.inf) y guarde el ejemplo siguiente en el Bloc de notas y guarde como RequestConfig.inf:
```
[NewRequest] 
Subject = "CN=<FQDN of computer you are creating the certificate>" 
Exportable = TRUE 
KeyLength = 2048 
KeySpec = 1 
KeyUsage = 0xf0 
MachineKeySet = TRUE 
[RequestAttributes]
CertificateTemplate="WebServer"
[Extensions] 
OID = 1.3.6.1.5.5.7.3.1 
OID = 1.3.6.1.5.5.7.3.2  
```
En el equipo para el que está solicitando un certificado, escriba el comando siguiente:
```
CertReq –New RequestConfig.inf CertRequest.req 
```
El ejemplo siguiente se muestra cómo implementar la sintaxis de la sección [Strings] para OID y otros difíciles de interpretar los datos. El ejemplo de sintaxis de {texto de} nuevo para la extensión EKU, que usa una lista separada por comas de OID:
```
[Version]
Signature="$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = "2.5.29.37"
szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1"
szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

[NewRequest]
Subject = "CN=TestSelfSignedCert"
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%="{text}%szOID_PKIX_KP_SERVER_AUTH%,"
_continue_ = "%szOID_PKIX_KP_CLIENT_AUTH%"
```
Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_accept"></a>Certreq -accept

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
– Acepte parámetro vincula la clave privada generada previamente con el certificado emitido y quita la solicitud de certificado pendiente desde el sistema donde se solicita el certificado (si hay una solicitud coincidente).

Puede usar este ejemplo para aceptar un certificado de forma manual:
```
certreq -accept certnew.cer 
```

> [!WARNING]
> -Acepte verbo, -opciones de usuario y – machine indican si se debe instalar el certificado se instala en contexto del equipo o usuario. Si hay una solicitud pendiente en cualquier contexto que coincide con la clave pública que se va a instalar, estas opciones no son necesarios. Si no hay ninguna solicitud pendiente, a continuación, uno de ellos debe especificarse.

Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq -policy

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   El archivo de configuración que define las restricciones que se aplican a un certificado de CA cuando se define la subordinación cualificada se denomina Policy.inf...
-   Puede encontrar un ejemplo del archivo Policy.inf en el [Apéndice A de planeación e implementación de la certificación cruzada y subordinación](https://technet.microsoft.com/library/cc738878(WS.10).aspx) notas del producto.
-   Si escribe el certreq-directiva sin ningún parámetro adicional abrirá una ventana de cuadro de diálogo para que pueda seleccionar el solicitado tempbd (req, cmc, txt, der, cer o crt). Una vez que seleccione el archivo solicitado y haga clic en el botón Abrir, se abrirá otra ventana del cuadro de diálogo para seleccionar el archivo INF.

Puede usar este ejemplo para crear una solicitud de certificado cruzado:
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq -sign

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   Si escribe el certreq-inicio de sesión sin ningún parámetro adicional se abrirá una ventana de cuadro de diálogo para que pueda seleccionar el archivo solicitado (req, cmc, txt, der, cer o crt).
-   Firma la solicitud de subordinación completa puede requerir credenciales de administrador de empresa. Esto es una práctica recomendada para la emisión de certificados de firma para la subordinación cualificada.
-   El certificado usado para firmar la solicitud de subordinación completa se crea mediante la plantilla de subordinación completa. Administradores de organización tendrá que firmar la solicitud o conceder permisos de usuario para las personas que firmarán el certificado.
-   Al iniciar la solicitud CMC, deberá tener varios miembros del personal esta solicitud, según el nivel de garantía que está asociado con la subordinación cualificada de inicio de sesión.
-   Si la CA primaria de la CA subordinada completa que se va a instalar está sin conexión, debe obtener el certificado de CA para la CA subordinada completa desde el elemento primario sin conexión. Si la CA primaria está en línea, especifique el certificado de CA para la CA subordinada completa durante el Asistente para instalación de servicios de certificados.

La secuencia de comandos siguiente mostrará cómo crear una nueva solicitud de certificado, firmarlo y enviarlo:
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq -enroll

Para inscribir un certificado
```
certreq –enroll [Options] TemplateName
```
Para renovar un certificado existente
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
Sólo puede renovar los certificados que son de tiempo válido. Certificados expirados no se puede renovar y deben reemplazarse por un nuevo certificado.

Este es un ejemplo de renovar un certificado mediante su número de serie:
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
Un ejemplo de la inscripción para una plantilla de certificado llamado servidor Web mediante el uso de asterisco (*) para seleccionar el servidor de directivas a través de U/I:
```
certreq -enroll –machine –policyserver * "WebServer"
```
Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_Options"></a>Opciones

|Opciones|Descripción|
|-------|-----------|
|-cualquier|Forzar ICertRequest:: Submit para determinar el tipo de codificación.|
|-attrib \<AttributeString>|Especifica los pares de cadena de nombre y valor, separados por dos puntos.</br>Los pares de cadenas de nombre y valor independiente con \n (por ejemplo, Name1:Value1\nName2:Value2).|
|-binario|Formatos de archivos de salida como binario en lugar de codificada en base64.|
|-PolicyServer  *\<PolicyServer >*|"ldap:  *\<ruta de acceso >* "</br>Inserte el URI o un identificador único para un equipo que ejecuta el servicio Web de directiva de inscripción de certificados.</br>Para especificar que desea usar un archivo de solicitud examinando, solo tiene que usar un signo menos (-) inicio de sesión para  *\<policyserver >* .|
|-config \<ConfigString >|Procesa la operación mediante el uso de la entidad de certificación especificada en la cadena de configuración, que es CAHostName\CAName. Para una conexión https, especifique el URI del servidor de inscripción. Para la máquina local almacén de CA, use un signo menos (-) inicio de sesión.|
|-Anónimo|Use credenciales anónimas para servicios Web de inscripción de certificados.|
|-Kerberos|Usar credenciales Kerberos (dominio) para servicios Web de inscripción de certificados.|
|-ClientCertificate *\<ClientCertId>*|Puede reemplazar el  *\<ClientCertID >* con una huella digital del certificado, CN, EKU, plantilla, correo electrónico, UPN y el nuevo nombre = sintaxis de valor.|
|-UserName  *\<nombre de usuario >*|Se utiliza con servicios Web de inscripción de certificados. Puede sustituir  *\<UserName >* con el nombre de SAM o dominio\usuario. Esta opción es para su uso con la opción -p.|
|-p  *\<contraseña >*|Se utiliza con servicios Web de inscripción de certificados. Sustituya  *\<contraseña >* con la contraseña del usuario real. Esta opción es para su uso con la opción - UserName.|
|-usuario|Configura el - contexto de usuario para una nueva solicitud de certificado o especifica el contexto para una aceptación de un certificado. Este es el contexto predeterminado, si no se especifica en la plantilla o INF.|
|-machine|Configura una nueva solicitud de certificado o especifica el contexto para una aceptación de un certificado para el contexto del equipo. Debe ser coherente con la clave INF MachineKeyset y el contexto de la plantilla para nuevas solicitudes. Si no se especifica esta opción y la plantilla no establece un contexto, el valor predeterminado es el contexto de usuario.|
|-crl|Incluye los certificados listas de revocación (CRL) en la salida al archivo PKCS #7 codificado en base64 especificado por CertChainFileOut o en el archivo codificado en base64 especificado por RequestFileOut.|
|-rpc|Indica a los servicios de certificados de Active Directory (AD CS) para usar una conexión de servidor de procedimiento remoto (RPC) de la llamada en lugar de COM distribuido|
|-AdminForceMachine|Use la clave de servicio o la suplantación para enviar la solicitud desde el contexto de sistema Local. Requiere que el usuario que se invoca esta opción sea un miembro de los administradores locales.|
|-RenewOnBehalfOf|Enviar una renovación en el nombre de asunto identificado en el certificado de firma. Esto establece CR_IN_ROBO al llamar a [ICertRequest:: Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|Forzar que se sobrescriban los archivos existentes. Esto también omite plantillas y directivas de almacenamiento en caché.|
|-q|Usar el modo silencioso; Suprimir todos los mensajes interactivos.|
|Unicode:|Escribe salida Unicode cuando es redirigido o canalizar a otro comando, que resulta útil cuando se invoca desde scripts Windows PowerShell® salida estándar).|
|-UnicodeText|Envía los resultados de Unicode al escribir texto base64 codificado blobs, archivos de datos.|

Vuelva a [contenido](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Formatos

|Formatos|Descripción|
|-------|-----------|
|RequestFileIn|Nombre de archivo de entrada con codificación Base64 o binario: Solicitud de certificado PKCS #10, solicitud de certificado CMS, solicitud de renovación de certificado PKCS #7, certificado X.509 para contar con la certificación cruzada o solicitud de certificado de formato de etiqueta KeyGen.|
|RequestFileOut|Nombre de archivo de salida codificada en Base64|
|CertFileOut|Nombre del archivo x-509 con codificación Base64.|
|PKCS10FileOut|Para su uso con el [Certreq-directiva](#BKMK_policy) solo verbo. Con codificación Base64 PKCS10 nombre archivo de salida.|
|CertChainFileOut|Nombre del archivo PKCS #7 codificado en Base64.|
|FullResponseFileOut|Nombre de archivo de respuesta completa con codificación Base64.|
|PolicyFileIn|Para su uso con el [Certreq-directiva](#BKMK_policy) solo verbo. Que contiene una representación textual de las extensiones que se utilizan para calificar una solicitud de un archivo de INF.|

## <a name="BKMK_Examples"></a>Ejemplos adicionales certreq

Los artículos siguientes contienen ejemplos de uso certreq:
-   [Cómo solicitar un certificado con un nombre alternativo de sujeto personalizado](https://technet.microsoft.com/library/ff625722.aspx)
-   [Guía de laboratorio de pruebas: Implementar una jerarquía PKI de AD CS de dos niveles](https://technet.microsoft.com/library/hh831348.aspx)
-   [Apéndice 3: CertReq.exe sintaxis](https://technet.microsoft.com/library/cc736326.aspx)
-   [Cómo crear manualmente un certificado de SSL de servidor web](http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [Solicitar un certificado mediante una entidad de certificación de Windows Server 2008 de aprovisionamiento de AMT](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [Inscripción de certificados para el agente de System Center Operations Manager](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [Guía paso a paso de AD CS: Implementación de la jerarquía PKI de dos niveles](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [Cómo habilitar LDAP a través de SSL con una entidad de certificación de terceros](https://support.microsoft.com/kb/321051)

Vuelva a [contenido](#BKMK_Contents)
