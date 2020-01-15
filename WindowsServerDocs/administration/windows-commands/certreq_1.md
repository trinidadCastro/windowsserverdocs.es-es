---
title: certreq
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8e3c98fd965fc6923c6af7893261bd1e0eee7d8b
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947591"
---
# <a name="certreq"></a>certreq



CertReq se puede usar para solicitar certificados de una entidad de certificación (CA), para recuperar una respuesta a una solicitud anterior de una CA, para crear una nueva solicitud desde un archivo. inf, para aceptar e instalar una respuesta a una solicitud, para construir una certificación cruzada o solicitud de subordinación completa de una solicitud o un certificado de entidad de certificación existente, y para firmar una solicitud de certificación cruzada o de subordinación completa.

> [!WARNING]
> - Es posible que las versiones anteriores de CertReq no proporcionen todas las opciones que se describen en este documento. Puede ver todas las opciones que proporciona una versión específica de CertReq; para ello, ejecute los comandos que se muestran en la sección notación de sintaxis.
> - CertReq no admite la creación de una nueva solicitud de certificado basada en una plantilla de atestación de clave en un entorno de procesamiento de eventos complejos/CES

## <a name="BKMK_Contents"></a>Contenido

Las secciones principales de este artículo son las siguientes:
1.  [Verbos](#BKMK_Verbs)
2.  [Notación de sintaxis](#BKMK_notation)
3.  [Opciones](#BKMK_Options)
4.  [Aplique](#BKMK_Formats)
5.  [Ejemplos adicionales de CertReq](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>Verbos

En la tabla siguiente se describen los verbos que se pueden utilizar con el comando CertReq.

|Cambia|Descripción|
|------|-----------|
|-Enviar|Envía una solicitud a una entidad de certificación. Para obtener más información, vea [CertReq-Submit](#BKMK_Submit).|
|-recuperar *RequestId*|Recupera una respuesta a una solicitud anterior de una entidad de certificación. Para obtener más información, vea [CertReq-Retrieve](#BKMK_Retrieve).|
|-Nuevo|Crea una nueva solicitud a partir de un archivo. inf. Para obtener más información, vea [CertReq-New](#BKMK_New).|
|-Accept|Acepta e instala una respuesta a una solicitud de certificado. Para obtener más información, vea [CertReq-Accept](#BKMK_accept).|
|-Directiva|Establece la Directiva para una solicitud. Para obtener más información, vea [CertReq-Policy](#BKMK_policy).|
|-Sign|Firma una solicitud de certificación cruzada o de subordinación completa. Para obtener más información, vea [CertReq-Sign](#BKMK_sign).|
|-Inscribirse|Inscribe o renueva un certificado. Para obtener más información, vea [CertReq-ENROLL](#BKMK_enroll).|
|-?|Muestra una lista de sintaxis, opciones y descripciones de CertReq.|
|*\<> de verbo* -?|Muestra la ayuda para el verbo especificado.|
|-v-?|Muestra una lista detallada de la sintaxis, las opciones y las descripciones de CertReq.|

Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_notation"></a>Notación de sintaxis

-   Para la sintaxis básica de la línea de comandos, ejecute `certreq -?`
-   Para obtener la sintaxis sobre el uso de certutil con un verbo específico, ejecute **certreq** *\<Verb >* **-?**
-   Para enviar toda la sintaxis de certutil a un archivo de texto, ejecute los siguientes comandos:  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

La siguiente tabla describe la notación que se usa para indicar la sintaxis de línea de comandos.

|Notation|Descripción|
|--------|-----------|
|Texto sin corchetes ni llaves|Elementos que se deben escribir como se muestran|
|\<texto dentro de los corchetes angulares >|Marcador de posición para el que debe proporcionar un valor|
|[Texto entre corchetes]|Elementos opcionales|
|{Texto entre llaves}|Conjunto de elementos requeridos; elija uno.|
|Barra vertical (&#124;)|Separador para elementos mutuamente exclusivos; elija uno.|
|Puntos suspensivos (...)|Elementos que pueden estar repetidos|

Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_Submit"></a>CertReq-Submit

Este es el parámetro predeterminado CertReq. exe, si no se especifica ninguna opción explícitamente en el símbolo de la línea de comandos, CertReq. exe intenta enviar una solicitud de certificado a una CA.
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
Debe especificar un archivo de solicitud de certificado al utilizar la opción – submit. Si se omite este parámetro, se muestra una ventana Abrir archivo común en la que puede seleccionar el archivo de solicitud de certificado adecuado.

Puede usar estos ejemplos como punto de partida para compilar la solicitud de envío de certificado:

Para enviar una solicitud de certificado simple, use el ejemplo siguiente:
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
Para solicitar un certificado especificando el atributo SAN, consulte los pasos detallados en el artículo 931351 de Microsoft Knowledge base [Cómo agregar un nombre alternativo del firmante a un certificado LDAP seguro](https://support.microsoft.com/kb/931351) en la sección "Cómo usar la utilidad CertReq. exe para crear y enviar una solicitud de certificado que incluya una San".

Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>CertReq: recuperar

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   Si no se especifica el cuadro de diálogo Nombredeequipoca o nombreCA en-config CAComputerName\CANamea, aparecerá una lista de todas las entidades de certificación disponibles.
-   Si usa -config - en lugar de -config NombreDeEquipoCA\NombreCA, la operación se procesa empleando la entidad de certificación predeterminada.
-   Puede usar CertReq-Retrieve *RequestId* para recuperar el certificado después de que la CA lo haya emitido realmente. *RequestId*PKC puede ser un prefijo 0x o hexadecimal con 0x y puede ser un número de serie de certificado sin prefijo 0x. También puede usarlo para recuperar cualquier certificado que haya emitido alguna vez por la entidad de certificación, incluidos los certificados revocados o expirados, sin tener en cuenta si la solicitud del certificado estaba en el estado pendiente.
-   Si envía una solicitud a la CA, el módulo de directivas de la CA podría dejar la solicitud en un estado pendiente y devolver el *RequestId* al autor de la llamada CertReq para su presentación. Finalmente, el administrador de la CA emitirá el certificado o denegará la solicitud.

El siguiente comando recupera el identificador de certificado 20 y crea el archivo de certificado (. cer):
```
certreq -retrieve 20 MyCertificate.cer 
```
Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_New"></a>CertReq-New

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
Dado que el archivo INF permite especificar un amplio conjunto de parámetros y opciones, es difícil definir una plantilla predeterminada que los administradores deben usar para todos los propósitos. Por lo tanto, en esta sección se describen todas las opciones que permiten crear un archivo INF adaptado a las necesidades específicas. Las siguientes palabras clave se usan para describir la estructura del archivo INF.
1.  Una *sección* es un área del archivo INF que cubre un grupo lógico de claves. Una sección siempre aparece entre corchetes en el archivo INF.
2.  Una *clave* es el parámetro situado a la izquierda del signo igual.
3.  Un *valor* es el parámetro situado a la derecha del signo igual.

Por ejemplo, un archivo INF mínimo tendría un aspecto similar al siguiente:
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
A continuación se muestran algunas de las secciones posibles que se pueden agregar al archivo INF:

**[NewRequest]**

Esta sección es obligatoria para un archivo INF que actúa como plantilla para una nueva solicitud de certificado. En esta sección se requiere al menos una clave con un valor.

|Tecla|Definición|Valor|Ejemplo|
|---|----------|-----|-------|
|Sujeto|Varias aplicaciones se basan en la información del firmante de un certificado. Por lo tanto, se recomienda especificar un valor para esta clave. Si no se establece el asunto aquí, se recomienda incluir un nombre de sujeto como parte de la extensión de certificado de nombre alternativo del firmante.|Valores de cadena de nombre distintivo relativo|Subject = "CN = Equipo1. contoso. com" Subject = "CN = John Smith, CN = users, DC = Contoso, DC = com"|
|Exportable|Si este atributo se establece en TRUE, la clave privada se puede exportar con el certificado. Para garantizar un alto nivel de seguridad, las claves privadas no deben ser exportables; sin embargo, en algunos casos, es posible que sea necesario hacer que la clave privada se pueda exportar si varios equipos o usuarios deben compartir la misma clave privada.|true, false|Exportable = TRUE. Las claves CNG pueden distinguir entre este y el texto sin formato exportable. Las claves CAPI1 no pueden.|
|ExportableEncrypted|Especifica si la clave privada debe configurarse para ser exportable.|true, false|ExportableEncrypted = true</br>Sugerencia: no todos los tamaños y algoritmos de clave pública funcionarán con todos los algoritmos hash. Tamehe CSP especificado también debe admitir el algoritmo hash especificado. Para ver la lista de algoritmos hash admitidos, puede ejecutar el comando <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|Algoritmo hash que se va a usar para esta solicitud.|Sha256, SHA384, SHA512, SHA1, MD5, MD4, MD2|HashAlgorithm = SHA1. Para ver la lista de algoritmos hash admitidos, use: certutil- &#124; OID 1 &#124; Findstr pwszCNGAlgid Findstr/v CryptOIDInfo|
|KeyAlgorithm|Algoritmo que usará el proveedor de servicios para generar un par de claves pública y privada.|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|No se recomienda establecer este parámetro para las nuevas solicitudes donde se genera el nuevo material de clave. El sistema genera y mantiene automáticamente el contenedor de claves. En el caso de las solicitudes en las que se debe usar el material de clave existente, este valor se puede establecer en el nombre del contenedor de claves de la clave existente. Use el comando certutil – Key para mostrar la lista de contenedores de claves disponibles para el contexto del equipo. Use el comando certutil – Key – User para el contexto del usuario actual.|Valor de cadena aleatoria</br>Sugerencia: debe usar comillas dobles alrededor de cualquier valor de clave INF que tenga espacios en blanco o caracteres especiales para evitar posibles problemas de análisis de INF.|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|Define la longitud de la clave pública y privada. La longitud de la clave tiene un impacto en el nivel de seguridad del certificado. Una longitud de clave mayor suele proporcionar un nivel de seguridad superior. sin embargo, algunas aplicaciones pueden tener limitaciones respecto a la longitud de la clave.|Cualquier longitud de clave válida admitida por el proveedor de servicios de cifrado.|KeyLength = 2048|
|Especificación|Determina si la clave se puede utilizar para firmas, para Exchange (cifrado) o para ambos.|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|Especificación de especificación = AT_KEYEXCHANGE|
|KeyUsage|Define para qué se debe usar la clave de certificado.|CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>Sugerencia: los valores que se muestran son valores hexadecimales (decimal) para cada definición de bits. También se puede usar la sintaxis anterior: un único valor hexadecimal con varios bits establecidos, en lugar de la representación simbólica. Por ejemplo, KeyUsage = 0XA0.</br>CERT_NON_REPUDIATION_KEY_USAGE--40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>Sugerencia: varios valores usan un separador de símbolos de canalización (&#124;). Asegúrese de usar comillas dobles al usar varios valores para evitar problemas de análisis de INF.|
|KeyUsageProperty|Recupera un valor que identifica el propósito específico para el que se puede usar una clave privada.|NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--FFFFFF (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|Esta clave es importante cuando es necesario crear certificados que son propiedad de la máquina y no de un usuario. El material de clave que se genera se mantiene en el contexto de seguridad de la entidad de seguridad (cuenta de usuario o de equipo) que ha creado la solicitud. Cuando un administrador crea una solicitud de certificado en nombre de un equipo, el material de clave se debe crear en el contexto de seguridad de la máquina y no en el contexto de seguridad del administrador. De lo contrario, el equipo no pudo obtener acceso a su clave privada, ya que se encontraba en el contexto de seguridad del administrador.|true, false|MachineKeySet = true</br>Sugerencia: el valor predeterminado es false.|
|NotBefore|Especifica una fecha o una fecha y hora antes de la cual no se puede emitir la solicitud. NotBefore se puede usar con ValidityPeriod y ValidityPeriodUnits.|fecha o fecha y hora|NotBefore = "7/24/2012 10:31 AM"</br>Sugerencia: NotBefore y nopérezer son solo para RequestType = cert. El análisis de fechas intenta ser sensible a la configuración regional. El uso de nombres de meses eliminará la ambigüedad y debería funcionar en todas las configuraciones regionales.|
|NotAfter|Especifica una fecha o fecha y hora después de la cual no se puede emitir la solicitud. Noolaetar no se puede usar con ValidityPeriod o ValidityPeriodUnits.|fecha o fecha y hora|Nopérezer = "9/23/2014 10:31 AM"</br>Sugerencia: NotBefore y nopérezer son solo para RequestType = cert. El análisis de fechas intenta ser sensible a la configuración regional. El uso de nombres de meses eliminará la ambigüedad y debería funcionar en todas las configuraciones regionales.|
|PrivateKeyArchive|La configuración PrivateKeyArchive solo funciona si el RequestType correspondiente está establecido en "CMC" porque solo el formato de solicitud de mensajes de administración de certificados a través de CMS (CMC) permite transferir de forma segura la clave privada del solicitante a la CA para el archivo de claves.|true, false|PrivateKeyArchive = true|
|EncryptionAlgorithm|El algoritmo de cifrado que se va a usar.|Las opciones posibles varían en función de la versión del sistema operativo y del conjunto de proveedores de servicios criptográficos instalados. Para ver la lista de algoritmos disponibles, ejecute el comando <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> el CSP especificado debe admitir también el algoritmo de cifrado simétrico y la longitud especificados.|EncryptionAlgorithm = 3DES|
|EncryptionLength|Longitud del algoritmo de cifrado que se va a usar.|Cualquier longitud permitida por el EncryptionAlgorithm especificado.|EncryptionLength = 128|
|ProviderName|El nombre del proveedor es el nombre para mostrar del CSP.|Si no conoce el nombre del proveedor del CSP que está utilizando, ejecute certutil – csplist desde una línea de comandos. El comando mostrará los nombres de todos los CSP que están disponibles en el sistema local.|ProviderName = "proveedor de servicios criptográficos de Microsoft RSA SChannel"|
|ProviderType|El tipo de proveedor se usa para seleccionar proveedores específicos en función de la capacidad de un algoritmo concreto, como "RSA Full".|Si no conoce el tipo de proveedor del CSP que está utilizando, ejecute certutil – csplist desde un símbolo de la línea de comandos. El comando mostrará el tipo de proveedor de todos los CSP que están disponibles en el sistema local.|ProviderType = 1|
|RenewalCert|Si necesita renovar un certificado que existe en el sistema donde se genera la solicitud de certificado, debe especificar su hash de certificado como valor de esta clave.|El hash del certificado de cualquier certificado que esté disponible en el equipo donde se crea la solicitud de certificado. Si no conoce el hash del certificado, use el complemento MMC certificados y examine el certificado que se debe renovar. Abra las propiedades del certificado y vea el atributo "huella digital" del certificado. La renovación de certificados requiere un formato de solicitud PKCS # 7 o CMC.|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>Nota: Esto hace que la solicitud se inscriba en nombre de otra solicitud de usuario. La solicitud también debe estar firmada con un certificado de agente de inscripción o la CA rechazará la solicitud. Use la opción-CERT para especificar el certificado de agente de inscripción.|Se puede especificar el nombre del solicitante para las solicitudes de certificado si RequestType está establecido en PKCS # 7 o CMC. Si RequestType se establece en PKCS # 10, se omitirá esta clave. Requestername solo se puede establecer como parte de la solicitud. No se puede manipular Requestername en una solicitud pendiente.|Dominio\usuario|Requestername = "Contoso\BSmith"|
|RequestType|Determina el estándar que se usa para generar y enviar la solicitud de certificado.|PKCS10--1</br>PKCS7--2</br>CMC--3</br>Certificado: 4</br>SCEP--fd00 (64768)</br>Sugerencia: esta opción indica que se trata de un certificado autofirmado o emitido por sí mismo. No genera una solicitud, sino un nuevo certificado y, a continuación, instala el certificado. Autofirmado es el valor predeterminado. Especifique un certificado de firma mediante la opción – CERT para crear un certificado emitido automáticamente que no sea autofirmado.|RequestType = CMC|
|SecurityDescriptor</br>Sugerencia: esto solo es relevante para las claves de las tarjetas no inteligentes de contexto del equipo.|Contener la información de seguridad asociada a los objetos protegibles. Para la mayoría de los objetos protegibles, puede especificar el descriptor de seguridad de un objeto en la llamada de función que crea el objeto.|Cadenas basadas en el [lenguaje de definición de descriptores de seguridad](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).|SecurityDescriptor = "D:P (A;; GA;;; SY) (A;; GA;;; BA) "|
|AlternateSignatureAlgorithm|Especifica y recupera un valor booleano que indica si el identificador de objeto (OID) del algoritmo de firma para una solicitud PKCS # 10 o una firma de certificado es discreto o combinado.|true, false|AlternateSignatureAlgorithm = false</br>Sugerencia: para una firma de RSA, False indica un Pkcs1 v 1.5. True indica una firma v 2.1.|
|Silencio|De forma predeterminada, esta opción permite al CSP tener acceso al escritorio del usuario interactivo y solicitar información como un PIN de tarjeta inteligente del usuario. Si esta clave se establece en TRUE, el CSP no debe interactuar con el escritorio y se le bloqueará para que no muestre ninguna interfaz de usuario al usuario.|true, false|Silencioso = true|
|SMIME|Si este parámetro se establece en TRUE, se agrega a la solicitud una extensión con el valor de identificador de objeto 1.2.840.113549.1.9.15. El número de identificadores de objeto depende de la versión del sistema operativo instalada y de la capacidad de CSP, que hace referencia a los algoritmos de cifrado simétricos que pueden usar las aplicaciones de extensiones multipropósito de correo Internet (S/MIME) seguras, como Outlook.|true, false|SMIME = true|
|UseExistingKeySet|Este parámetro se usa para especificar que se debe utilizar un par de claves existente en la creación de una solicitud de certificado. Si esta clave se establece en TRUE, también debe especificar un valor para la clave RenewalCert o el nombre de KeyContainer. No debe establecer la clave exportable porque no puede cambiar las propiedades de una clave existente. En este caso, no se genera material de clave cuando se compila la solicitud de certificado.|true, false|UseExistingKeySet = true|
|KeyProtection|Especifica un valor que indica cómo se protege una clave privada antes de su uso.|XCN_NCRYPT_UI_NO_PROTCTION_FLAG--0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG--1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|Especifica un valor booleano que indica si las extensiones y atributos predeterminados se incluyen en la solicitud. Los valores predeterminados se representan mediante sus identificadores de objeto (OID).|true, false|SuppressDefaults = true|
|FriendlyName|Un nombre descriptivo para el nuevo certificado.|Text|FriendlyName = "servidor1"|
|ValidityPeriodUnits</br>Nota: solo se usa cuando el tipo de solicitud = cert.|Especifica el número de unidades que se va a utilizar con ValidityPeriod.|Numérico|ValidityPeriodUnits = 3|
|ValidityPeriod</br>Nota: solo se usa cuando el tipo de solicitud = cert.|VValidityPeriod debe ser un período de tiempo plural del Inglés de EE. UU.|Años, meses, semanas, días, horas, minutos, segundos|ValidityPeriod = años|

Volver a [contenido](#BKMK_Contents)

**Extension**

Esta sección es opcional.


|  OID de extensión   | Definición | Valor |                                                                                                                                                                                                                                                                                                                                                                                                                      Ejemplo                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{Text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "EMail =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "DNS = host. domain. com &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continue* = "DIRECTORYNAME = CN = Name, DC = Domain, DC = com &"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *continue* = "URL =<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress = 10.0.0.1 &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "RegisteredId = 1.2.3.4.5 &"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continue* = "1.2.3.4.6.1 = {Utf8} cadena &"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2 = {octet} AAECAwQFBgc = &"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.2.3.4.6.2 = {octet} {Hex} 00 01 02 03 04 05 06 07 &"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3 = {ASN} BAgAAQIDBAUGBw = = &"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continue* = "1.2.3.4.6.3 = {Hex} 04 08 00 01 02 03 04 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37 = "{Text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continue* = "1.3.6.1.5.5.7.                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              "{Text} CA = 0pathlength = 3"                                                                                                                                                                                                                                                                                                                                                                                                              |
|     Crítico     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 Crítico = 2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     Especificación      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE--0</br>AT_SIGNATURE--2</br>AT_KEYEXCHANGE--1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10--1</br>PKCS7--2</br>CMC--3</br>Certificado: 4</br>SCEP--fd00 (64768)                                                                                                                                                                                                                                                                                                                                                                                   |
|     KeyUsage     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE--40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)                                                                                                                                                                                                       |
| KeyUsageProperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--FFFFFF (16777215)                                                                                                                                                                                                                                                                                                                                             |
|  KeyProtection   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG--0</br>NCRYPT_UI_PROTECT_KEY_FLAG--1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  plantilla  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME--40 MILLONES (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH--80 MILLONES (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN--10 MILLONES (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL--20 MILLONES (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME--8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID--1 MILLÓN (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS--8 MILLONES (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS--400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL--4 MILLONES (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN--800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN--2 MILLONES (33554432)                                                                            |
|  X500NameFlags   |            |       | CERT_NAME_STR_NONE--0</br>CERT_OID_NAME_STR--2</br>CERT_X500_NAME_STR--3</br>CERT_NAME_STR_SEMICOLON_FLAG--40 MILLONES (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG--20 MILLONES (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG--10 MILLONES (268435456)</br>CERT_NAME_STR_CRLF_FLAG--8 MILLONES (134217728)</br>CERT_NAME_STR_COMMA_FLAG--4 MILLONES (67108864)</br>CERT_NAME_STR_REVERSE_FLAG--2 MILLONES (33554432)</br>CERT_NAME_STR_FORWARD_FLAG--1 MILLÓN (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG--10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG--20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG--40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG--80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG--100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG--200000 (2097152) |

Volver a [contenido](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags permite al archivo INF especificar qué campos de extensión de asunto y SubjectAltName deben rellenarse automáticamente por CertReq en función del usuario actual o de las propiedades de la máquina actual: nombre DNS, UPN, etc. El uso de la "plantilla" literal significa que se utilizan en su lugar las marcas de nombre de plantilla. Esto permite usar un único archivo INF en varios contextos para generar solicitudes con información de asunto específica del contexto.
>
> X500NameFlags especifica las marcas que se van a pasar directamente a la API de CertStrToName cuando el valor de las claves INF de asunto se convierte en un nombre distintivo con codificación ASN. 1.

Para solicitar un certificado basado en CertReq-New, use los pasos del ejemplo siguiente:

> [!WARNING]
> El contenido de este tema se basa en la configuración predeterminada de Windows Server 2008 AD CS; por ejemplo, el establecimiento de la longitud de clave en 2048, la selección del proveedor de almacenamiento de claves de software de Microsoft como CSP y el uso de Algoritmo hash seguro 1 (SHA1). Evalúe estas selecciones en función de los requisitos de la Directiva de seguridad de su empresa.

Para crear una copia del archivo de directivas (. inf) y guardar el ejemplo siguiente en el Bloc de notas y guardarlo como RequestConfig. inf:
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
En el equipo para el que está solicitando un certificado, escriba el siguiente comando:
```
CertReq –New RequestConfig.inf CertRequest.req 
```
En el ejemplo siguiente se muestra la implementación de la sintaxis de la sección [Strings] para OID y otros datos difíciles de interpretar. El nuevo ejemplo de la sintaxis {Text} para la extensión EKU, que usa una lista separada por comas de OID:
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
Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_accept"></a>CertReq: Accept

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
El parámetro – Accept vincula la clave privada generada previamente con el certificado emitido y quita la solicitud de certificado pendiente del sistema en el que se solicita el certificado (si hay una solicitud coincidente).

Puede usar este ejemplo para aceptar un certificado manualmente:
```
certreq -accept certnew.cer 
```

> [!WARNING]
> El verbo-Accept, las opciones-user y – Machine indican si el certificado que se va a instalar debe instalarse en el contexto de usuario o de equipo. Si hay una solicitud pendiente en cualquier contexto que coincida con la clave pública que se va a instalar, estas opciones no son necesarias. Si no hay ninguna solicitud pendiente, se debe especificar uno de ellos.

Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_policy"></a>CertReq: Directiva

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   El archivo de configuración que define las restricciones que se aplican a un certificado de CA cuando se define la subordinación calificada se denomina Policy. inf.
-   Puede encontrar un ejemplo del archivo Policy. inf en las notas del producto [del apéndice a de planeamiento e implementación de la certificación cruzada y la subordinación completa](https://technet.microsoft.com/library/cc738878(WS.10).aspx) .
-   Si escribe la Directiva CertReq sin ningún parámetro adicional, se abrirá una ventana de cuadro de diálogo para que pueda seleccionar el archivos solicitado (req, CMC, txt, der, cer o CRT). Una vez que seleccione el archivo solicitado y haga clic en el botón abrir, se abrirá otra ventana del cuadro de diálogo para seleccionar el archivo INF.

Puede usar este ejemplo para crear una solicitud de certificado cruzado:
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_sign"></a>CertReq: Sign

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   Si escribe el parámetro CertReq-Sign sin ningún parámetro adicional, se abrirá una ventana de diálogo para que pueda seleccionar el archivo solicitado (req, CMC, txt, der, cer o CRT).
-   La firma de la solicitud de subordinación completa puede requerir credenciales de administrador de empresa. Se trata de un procedimiento recomendado para emitir certificados de firma para la subordinación cualificada.
-   El certificado usado para firmar la solicitud de subordinación calificada se crea con la plantilla de subordinación completa. Los administradores de empresas tendrán que firmar la solicitud o conceder permisos de usuario a los usuarios que firmarán el certificado.
-   Al firmar la solicitud de CMC, es posible que deba hacer que varias personas firmen esta solicitud, según el nivel de seguridad asociado a la subordinación cualificada.
-   Si la CA primaria de la CA subordinada completa que está instalando está sin conexión, debe obtener el certificado de CA para la CA subordinada completa del elemento primario sin conexión. Si la CA primaria está en línea, especifique el certificado de CA para la CA subordinada completa durante el Asistente para la instalación de servicios de certificados.

La secuencia de comandos siguiente mostrará cómo crear una nueva solicitud de certificado, firmarla y enviarla:
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_enroll"></a>CertReq: inscribirse

Para inscribirse en un certificado
```
certreq –enroll [Options] TemplateName
```
Para renovar un certificado existente
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
Solo puede renovar los certificados que sean válidos en el tiempo. Los certificados expirados no se pueden renovar y deben reemplazarse por un nuevo certificado.

Aquí se muestra un ejemplo de renovación de un certificado con su número de serie:
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
Aquí se muestra un ejemplo de inscripción en una plantilla de certificado denominada WebServer mediante el asterisco (*) para seleccionar el servidor de directivas a través de U/I:
```
certreq -enroll –machine –policyserver * "WebServer"
```
Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_Options"></a>Opciones

|botón|Descripción|
|-------|-----------|
|-Any|Fuerce ICertRequest:: submit para determinar el tipo de codificación.|
|-attrib \<AttributeString >|Especifica los pares de cadena de nombre y valor, separados por dos puntos.</br>Separe los pares de nombre y valor de cadena con \n (por ejemplo, Nombre1: Value1\nName2: Valor2).|
|binario|Da formato binario a los archivos de salida en lugar de a la codificación Base64.|
|-PolicyServer *\<PolicyServer >*|"LDAP: *\<ruta de acceso >* "</br>Inserte el URI o el identificador único de un equipo que ejecute el Servicio web de directiva de inscripción de certificados.</br>Para especificar que desea utilizar un archivo de solicitud examinando, solo tiene que usar un signo menos (-) para *\<policyserver >* .|
|-config \<ConfigString >|Procesa la operación utilizando la entidad de certificación especificada en la cadena de configuración, que es CAHostName\CAName. Para una conexión HTTPS, especifique el URI del servidor de inscripciones. Para la CA del almacén del equipo local, use un signo menos (-).|
|-Anonymous|Use credenciales anónimas para los servicios Web de inscripción de certificados.|
|-Kerberos|Use credenciales de Kerberos (dominio) para los servicios Web de inscripción de certificados.|
|-ClientCertificate *\<ClientCertId >*|Puede reemplazar el *\<ClientCertID >* por una huella digital de certificado, CN, EKU, plantilla, correo electrónico, UPN y la nueva sintaxis de nombre = valor.|
|-UserName *\<nombredeusuario >*|Se usa con los servicios Web de inscripción de certificados. Puede sustituir *\<nombre de usuario >* por el nombre del SAM o dominio\usuario. Esta opción se usa con la opción-p.|
|-p *\<contraseña >*|Se usa con los servicios Web de inscripción de certificados. Sustituya *\<contraseña >* por la contraseña del usuario real. Esta opción se usa con la opción-UserName.|
|-usuario|Configura el contexto de usuario para una nueva solicitud de certificado o especifica el contexto de una aceptación de certificado. Este es el contexto predeterminado, si no se especifica ninguno en el archivo INF o la plantilla.|
|-equipo|Configura una nueva solicitud de certificado o especifica el contexto de una aceptación de certificado para el contexto de la máquina. En el caso de las nuevas solicitudes, debe ser coherente con la clave de MachineKeyset INF y el contexto de la plantilla. Si no se especifica esta opción y la plantilla no establece un contexto, el valor predeterminado es el contexto del usuario.|
|-CRL|Incluye listas de revocación de certificados (CRL) en la salida al archivo PKCS #7 codificado en Base64 especificado por CertChainFileOut o al archivo codificado en Base64 especificado por RequestFileOut.|
|-RPC|Indica a Active Directory servicios de Certificate Server (AD CS) que utilice una conexión de servidor de llamada a procedimiento remoto (RPC) en lugar de COM distribuido.|
|-AdminForceMachine|Use el servicio de clave o la suplantación para enviar la solicitud desde el contexto del sistema local. Requiere que el usuario que invoca esta opción sea miembro de los administradores locales.|
|-RenewOnBehalfOf|Envíe una renovación en nombre del firmante identificado en el certificado de firma. Esto establece CR_IN_ROBO al llamar a [ICertRequest:: submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|Forzar la sobrescritura de los archivos existentes. Esto también omite las plantillas y directivas de almacenamiento en caché.|
|-q|Usar el modo silencioso; suprime todos los mensajes interactivos.|
|-Unicode|Escribe la salida Unicode cuando la salida estándar se redirige o se canaliza a otro comando, lo que ayuda cuando se invoca desde scripts® de Windows PowerShell).|
|-UnicodeText|Envía la salida Unicode al escribir blobs de datos codificados de texto Base64 en los archivos.|

Volver a [contenido](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Aplique

|Formatos|Descripción|
|-------|-----------|
|RequestFileIn|Nombre del archivo de entrada binario o codificado en Base64: PKCS #10 solicitud de certificado, solicitud de certificado CMS, solicitud de renovación de certificado PKCS #7, certificado X. 509 que va a ser una solicitud de certificado con formato de etiqueta con certificación cruzada o formato de etiqueta KeyGen.|
|RequestFileOut|Nombre del archivo de salida con codificación Base64|
|CertFileOut|Nombre de archivo X-509 con codificación Base64.|
|PKCS10FileOut|Solo se usa con el verbo [CertReq-Policy](#BKMK_policy) . Nombre del archivo de salida de PKCS10 codificado en Base64.|
|CertChainFileOut|Nombre de archivo PKCS #7 codificado en Base64.|
|FullResponseFileOut|Nombre del archivo de respuesta completo con codificación Base64.|
|PolicyFileIn|Solo se usa con el verbo [CertReq-Policy](#BKMK_policy) . Archivo INF que contiene una representación textual de las extensiones utilizadas para calificar una solicitud.|

## <a name="BKMK_Examples"></a>Ejemplos adicionales de CertReq

Los artículos siguientes contienen ejemplos de uso de CertReq:
-   [Cómo solicitar un certificado con un nombre alternativo de firmante personalizado](https://technet.microsoft.com/library/ff625722.aspx)
-   [Guía del laboratorio de pruebas: implementar una jerarquía de PKI de dos niveles para AD CS](https://technet.microsoft.com/library/hh831348.aspx)
-   [Apéndice 3: sintaxis de CertReq. exe](https://technet.microsoft.com/library/cc736326.aspx)
-   [Creación manual de un certificado SSL de servidor Web](https://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [Solicitar un certificado de aprovisionamiento de AMT mediante una CA de Windows Server 2008](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [Inscripción de certificados para el agente de System Center Operations Manager](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [Guía paso a paso de AD CS: implementación de la jerarquía de PKI de dos niveles](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [Habilitación de LDAP a través de SSL con una entidad de certificación de terceros](https://support.microsoft.com/kb/321051)

Volver a [contenido](#BKMK_Contents)
