---
title: certutil
description: Artículo de referencia para el comando certutil, que es un programa de línea de comandos que vuelca y muestra la información de configuración de la entidad de certificación (CA), configura servicios de Certificate Server, copia de seguridad y restauración de componentes de CA y comprueba certificados, pares de claves y cadenas de certificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a2cea23d96c4cb438a2acac6d14c1bd37c67b56
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922649"
---
# <a name="certutil"></a>certutil

Certutil.exe es un programa de línea de comandos, que se instala como parte de los servicios de Certificate Server. Puede usar certutil.exe para volcar y mostrar la información de configuración de la entidad de certificación (CA), configurar servicios de Certificate Server, realizar copias de seguridad y restaurar componentes de CA y comprobar certificados, pares de claves y cadenas de certificados.

Si se ejecuta certutil en una entidad de certificación sin parámetros adicionales, se muestra la configuración de la entidad de certificación actual. Si se ejecuta certutil en una entidad de certificación que no sea de, el comando tiene como valor predeterminado ejecutar el `certutil [-dump]` comando.

> [!IMPORTANT]
> Es posible que las versiones anteriores de certutil no proporcionen todas las opciones que se describen en este documento. Puede ver todas las opciones que proporciona una versión específica de certutil mediante la ejecución `certutil -?` de o `certutil <parameter> -?` .

## <a name="parameters"></a>Parámetros

### <a name="-dump"></a>-dump

Vuelca la información de configuración o los archivos.

```
certutil [options] [-dump]
certutil [options] [-dump] file
```

```
[-f] [-silent] [-split] [-p password] [-t timeout]
```

### <a name="-asn"></a>-ASN

Analice el archivo ASN. 1.

```
certutil [options] -asn file [type]
```

`[type]`: Numeric CRYPT_STRING_ * tipo de descodificación

### <a name="-decodehex"></a>-decodehex

Descodifique un archivo codificado en hexadecimal.

```
certutil [options] -decodehex infile outfile [type]
```

`[type]`: tipo de codificación Numeric CRYPT_STRING_ *

```
[-f]
```

### <a name="-decode"></a>-descodificar

Descodifique un archivo codificado en Base64.

```
certutil [options] -decode infile outfile
```

```
[-f]
```

### <a name="-encode"></a>-codificar

Codifique un archivo en Base64.

```
certutil [options] -encode infile outfile
```

```
[-f] [-unicodetext]
```

### <a name="-deny"></a>-denegar

Denegar una solicitud pendiente.

```
certutil [options] -deny requestID
```

```
[-config Machine\CAName]
```

### <a name="-resubmit"></a>-reenviar

Vuelva a enviar una solicitud pendiente.

```
certutil [options] -resubmit requestId
```

```
[-config Machine\CAName]
```

### <a name="-setattributes"></a>-SetAttributes

Establecer atributos para una solicitud de certificado pendiente.

```
certutil [options] -setattributes RequestID attributestring
```

Donde:

- **requestID** es el identificador de solicitud numérico de la solicitud pendiente.

- **AttributeString** es el nombre del atributo de solicitud y los pares de valor.

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>Comentarios

- Los nombres y los valores deben estar separados por dos puntos, mientras que los pares de nombre y valor deben estar separados por una línea nueva. Por ejemplo: `CertificateTemplate:User\nEMail:User@Domain.com` donde la `\n` secuencia se convierte en un separador de nueva línea.

### <a name="-setextension"></a>-setextension

Establecer una extensión para una solicitud de certificado pendiente.

```
certutil [options] -setextension requestID extensionname flags {long | date | string | \@infile}
```

Donde:

- **requestID** es el identificador de solicitud numérico de la solicitud pendiente.

- **extensionname** es la cadena objectId para la extensión.

- **Flags** establece la prioridad de la extensión. `0`se recomienda, mientras `1` que la extensión se establece en Critical, se `2` deshabilita la extensión y `3` realiza ambas tareas.

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>Comentarios

- Si el último parámetro es numérico, se toma como un **valor Long**.

- Si el último parámetro se puede analizar como una fecha, se toma como una **fecha**.

- Si el último parámetro comienza con `\@` , el resto del token se toma como el nombre de archivo con datos binarios o un volcado hexadecimal de texto ASCII.

- Si el último parámetro es cualquier otro elemento, se toma como una cadena.

### <a name="-revoke"></a>-REVOKE

Revocar un certificado.

```
certutil [options] -revoke serialnumber [reason]
```

Donde:

- **SerialNumber** es una lista separada por comas de números de serie de certificados que se van a revocar.

- **motivo** es la representación numérica o simbólica del motivo de la revocación, incluido:

  - **0. CRL_REASON_UNSPECIFIED** : no especificado (valor predeterminado)

  - **1. CRL_REASON_KEY_COMPROMISE** de la clave en peligro

  - **2. CRL_REASON_CA_COMPROMISE** : compromiso de la entidad de certificación

  - **3. CRL_REASON_AFFILIATION_CHANGED** -afiliación cambiada

  - **4. CRL_REASON_SUPERSEDED** -reemplazado

  - **5. CRL_REASON_CESSATION_OF_OPERATION** el cese de la operación

  - **6. CRL_REASON_CERTIFICATE_HOLD** : retención de certificados

  - **8. CRL_REASON_REMOVE_FROM_CRL** quitar de CRL

  - **1. anular la revocación** : anular la revocación

```
[-config Machine\CAName]
```

### <a name="-isvalid"></a>-IsValid

Mostrar la disposición del certificado actual.

```
certutil [options] -isvalid serialnumber | certhash
```

```
[-config Machine\CAName]
```

### <a name="-getconfig"></a>-GetConfig

Obtiene la cadena de configuración predeterminada.

```
certutil [options] -getconfig
```

```
[-config Machine\CAName]
```

### <a name="-ping"></a>-ping

Intente ponerse en contacto con la interfaz de solicitud de servicios de certificados de Active Directory.

```
certutil [options] -ping [maxsecondstowait | camachinelist]
```

Donde:

- **camachinelist** es una lista separada por comas de nombres de equipo de CA. Para una sola máquina, use una coma de terminación. Esta opción también muestra el costo del sitio para cada máquina de CA.

```
[-config Machine\CAName]
```

### <a name="-cainfo"></a>-cainfo

Muestra información acerca de la entidad de certificación.

```
certutil [options] -cainfo [infoname [index | errorcode]]
```

Donde:

- **infoname** indica la propiedad de la entidad de certificación que se va a mostrar, en función de la siguiente sintaxis del argumento infoname:

  - versión del archivo de **archivo**

  - **producto** -versión del producto

  - **exitcount** : número de módulos de salida

  - **salir `[index]` ** de -Descripción del módulo de salida

  - **Directiva** : Descripción del módulo de directivas

  - **nombre** : nombre de la entidad de certificación

  - **sanitizedname** : nombre de la CA saneado

  - **DSName** : nombre corto de la CA saneado (nombre de DS)

  - **carpetaCompartida** : carpeta compartida

  - **Error1 ErrorCode** -texto del mensaje de error

  - **ErrorCode de error2** : texto de mensaje de error y código de error

  - **tipo: tipo** de entidad de certificación

  - **información** : información de CA

  - CA **primaria** -primaria

  - **certcount** : número de certificados de CA

  - **xchgcount** : número de certificados de intercambio de CA

  - **kracount** : recuento de certificados de Kra

  - **kraused** : recuento de uso de certificado Kra

  - **propidmax** -longitud máxima de CA PropId

  - **certstate `[index]` ** -Certificado de CA

  - **certversion `[index]` ** -Versión de certificado de CA

  - **certstatuscode `[index]` ** -Estado de comprobación de certificado de CA

  - **crlstate `[index]` ** -CRL

  - **krastate `[index]` ** -Certificado KRA

  - **crossstate + `[index]` ** -Certificado cruzado directo

  - **crossstate- `[index]` ** -Certificado cruzado hacia atrás

  - **certificado `[index]` ** de -Certificado de CA

  - **certchain `[index]` ** -Cadena de certificados de CA

  - **certcrlchain `[index]` ** -Cadena de certificados de CA con CRL

  - **xchg `[index]` ** -Certificado de intercambio de CA

  - **xchgchain `[index]` ** -Cadena de certificados de intercambio de CA

  - **xchgcrlchain `[index]` ** -Cadena de certificados de intercambio de CA con CRL

  - **Kra `[index]` ** -Certificado KRA

  - **entre + `[index]` ** -Certificado cruzado directo

  - **entre `[index]` ** -Certificado cruzado hacia atrás

  - **CRL `[index]` ** de -CRL base

  - **deltacrl `[index]` ** -Diferencia CRL

  - **crlstatus `[index]` ** -Estado de publicación de CRL

  - **deltacrlstatus `[index]` ** -Estado de publicación de diferencias CRL

  - **DNS** : nombre DNS

  - Separación **de roles y roles**

  - **ADS** -servidor avanzado

  - **plantillas** : plantillas

  - **CSP `[index]` ** de -Direcciones URL de OCSP

  - **AIA `[index]` ** -Direcciones URL de AIA

  - **CDP `[index]` ** de -Direcciones URL de CDP

  - **localename** : nombre de configuración regional de la entidad de certificación

  - **subjecttemplateoids** : OID de la plantilla de asunto

  - **&#42;** : muestra todas las propiedades

- **index** es el índice de propiedad opcional basado en cero.

- **ErrorCode** es el código de error numérico.

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cacert"></a>-CA. cert

Recupere el certificado de la entidad de certificación.

```
certutil [options] -ca.cert outcacertfile [index]
```

Donde:

- **outcacertfile** es el archivo de salida.

- **index** es el índice de renovación de certificados de entidad de certificación (el valor predeterminado es el más reciente).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cachain"></a>-CA. Chain

Recupere la cadena de certificados de la entidad de certificación.

```
certutil [options] -ca.chain outcacertchainfile [index]
```

Donde:

- **OutCACertChainFile** es el archivo de salida.

- **index** es el índice de renovación de certificados de entidad de certificación (el valor predeterminado es el más reciente).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-getcrl"></a>-getcrl

Obtiene una lista de revocación de certificados (CRL).

certutil [opciones]-getcrl OUTFILE [index] [Delta]

Donde:

- **index** es el índice de CRL o de clave (el valor predeterminado es CRL para la clave más reciente).

- **Delta** es la diferencia CRL (el valor predeterminado es CRL base).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-crl"></a>-CRL

Publicar nuevas listas de revocación de certificados (CRL) o diferencias CRL.

```
certutil [options] -crl [dd:hh | republish] [delta]
```

Donde:

- **DD: HH** es el nuevo período de validez de CRL en días y horas.

- **volver a publicar** vuelve a publicar las CRL más recientes.

- **Delta** publica solo las diferencias CRL (el valor predeterminado es base y diferencias CRL).

```
[-split] [-config Machine\CAName]
```

### <a name="-shutdown"></a>-Shutdown

Cierra el Active Directory servicios de Certificate Server.

```
certutil [options] -shutdown
```

```
[-config Machine\CAName]
```

### <a name="-installcert"></a>-installcert

Instala un certificado de entidad de certificación.

```
certutil [options] -installcert [cacertfile]
```

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-renewcert"></a>-renewcert

Renueva un certificado de entidad de certificación.

```
certutil [options] -renewcert [reusekeys] [Machine\ParentCAName]
```

- Use `-f` para omitir una solicitud de renovación pendiente y para generar una nueva solicitud.

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-schema"></a>-esquema

Vuelca el esquema para el certificado.

```
certutil [options] -schema [ext | attrib | cRL]
```
Donde:

- El comando tiene como valor predeterminado la solicitud y la tabla de certificados.

- **ext** es la tabla de extensión.

- el **atributo** es la tabla de atributos.

- **CRL** es la tabla de CRL.

```
[-split] [-config Machine\CAName]
```

### <a name="-view"></a>-Ver

Vuelca la vista de certificado.

```
certutil [options] -view [queue | log | logfail | revoked | ext | attrib | crl] [csv]
```

Donde:

- **Queue** vuelca una cola de solicitudes específica.

- el **registro** vuelca los certificados emitidos o revocados, además de las solicitudes con error.

- **logfail** vuelca las solicitudes con error.

- los volcados de los certificados revocados se **revocan** .

- **ext** vuelca la tabla de la extensión.

- el **atributo** vuelca la tabla de atributos.

- la **CRL** vuelca la tabla de CRL.

- **CSV** proporciona la salida mediante valores separados por comas.

```
[-silent] [-split] [-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

#### <a name="remarks"></a>Comentarios

- Para mostrar la columna **StatusCode** para todas las entradas, escriba`-out StatusCode`

- Para mostrar todas las columnas de la última entrada, escriba:`-restrict RequestId==$`

- Para mostrar el **RequestId** y la **disposición** de tres solicitudes, escriba:`-restrict requestID>37,requestID<40 -out requestID,disposition`

- Para mostrar los identificadores de**fila** y **los números de CRL** de los identificadores de fila de todas las CRL base, escriba:`-restrict crlminbase=0 -out crlrowID,crlnumber crl`

- Para mostrar, escriba:`-v -restrict crlminbase=0,crlnumber=3 -out crlrawcrl crl`

- Para mostrar toda la tabla de CRL, escriba:`CRL`

- Se usa `Date[+|-dd:hh]` para las restricciones de fecha.

- `now+dd:hh`Se utiliza para una fecha relativa a la hora actual.

### <a name="-db"></a>-db

Vuelca la base de datos sin formato.

```
certutil [options] -db
```

```
[-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

### <a name="-deleterow"></a>-deleteRow

Elimina una fila de la base de datos del servidor.

```
certutil [options] -deleterow rowID | date [request | cert | ext | attrib | crl]
```

Donde:

- la **solicitud** elimina las solicitudes con error y pendientes, en función de la fecha de envío.

- **CERT** elimina los certificados expirados y revocados, en función de la fecha de expiración.

- **ext** elimina la tabla de extensión.

- el **atributo** elimina la tabla de atributos.

- **CRL** elimina la tabla de CRL.

```
[-f] [-config Machine\CAName]
```

#### <a name="examples"></a>Ejemplos

- Para eliminar las solicitudes con error y pendientes enviadas el 22 de enero de 2001, escriba:`1/22/2001 request`

- Para eliminar todos los certificados que han expirado el 22 de enero de 2001, escriba:`1/22/2001 cert`

- Para eliminar la fila, los atributos y las extensiones del certificado para RequestID 37, escriba:`37`

- Para eliminar las CRL que expiraron el 22 de enero de 2001, escriba:`1/22/2001 crl`

### <a name="-backup"></a>-backup

Realiza una copia de seguridad de los servicios de Certificate Server Active Directory.

```
certutil [options] -backup backupdirectory [incremental] [keeplog]
```

Donde:

- **BackupDirectory** es el directorio donde se almacenan los datos de los que se ha realizado una copia de seguridad.

- **incremental** solo realiza una copia de seguridad incremental (el valor predeterminado es copia de seguridad completa).

- **keeplog** conserva los archivos de registro de la base de datos (el valor predeterminado es truncar los archivos de registro).

```
[-f] [-config Machine\CAName] [-p Password]
```

### <a name="-backupdb"></a>-backupdb

Realiza una copia de seguridad del Active Directory base de datos de servicios de certificados.

```
certutil [options] -backupdb backupdirectory [incremental] [keeplog]
```

Donde:

- **BackupDirectory** es el directorio en el que se almacenan los archivos de base de datos de copia de seguridad.

- **incremental** solo realiza una copia de seguridad incremental (el valor predeterminado es copia de seguridad completa).

- **keeplog** conserva los archivos de registro de la base de datos (el valor predeterminado es truncar los archivos de registro).

```
[-f] [-config Machine\CAName]
```

### <a name="-backupkey"></a>-backupkey

Realiza una copia de seguridad del certificado de servicios de certificados de Active Directory y de la clave privada.

```
certutil [options] -backupkey backupdirectory
```

Donde:

- **BackupDirectory** es el directorio en el que se va a almacenar el archivo PFX de copia de seguridad.

```
[-f] [-config Machine\CAName] [-p password] [-t timeout]
```

### <a name="-restore"></a>-restore

Restaura el Active Directory servicios de Certificate Server.

```
certutil [options] -restore backupdirectory
```

Donde:

- **BackupDirectory** es el directorio que contiene los datos que se van a restaurar.

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-restoredb"></a>-restoredb

Restaura el Active Directory base de datos de servicios de certificados.

```
certutil [options] -restoredb backupdirectory
```

Donde:

- **BackupDirectory** es el directorio que contiene los archivos de base de datos que se van a restaurar.

```
[-f] [-config Machine\CAName]
```

### <a name="-restorekey"></a>-restorekey

Restaura el certificado de servicios de certificados de Active Directory y la clave privada.

```
certutil [options] -restorekey backupdirectory | pfxfile
```

Donde:

- **BackupDirectory** es el directorio que contiene el archivo PFX que se va a restaurar.

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-importpfx"></a>-importpfx

Importe el certificado y la clave privada. Para obtener más información, consulte el `-store` parámetro de este artículo.

```
certutil [options] -importpfx [certificatestorename] pfxfile [modifiers]
```

Donde:

- **CertificateStoreName** es el nombre del almacén de certificados.

- los **Modificadores** son la lista separada por comas, que puede incluir uno o varios de los siguientes elementos:

  1. **AT_SIGNATURE** : cambia la especificación de especificación a Signature.

  2. **AT_KEYEXCHANGE** : cambia la especificación de clave al intercambio de claves.

  3. **Noexport** : hace que la clave privada no sea exportable

  4. **Nocert** : no importa el certificado

  5. **Nochain** : no importa la cadena de certificados

  6. **Noroot** : no importa el certificado raíz

  7. **Proteger** : protege las claves mediante una contraseña.

  8. **Noprotect** : no protege las claves mediante contraseña.

```
[-f] [-user] [-p password] [-csp provider]
```

#### <a name="remarks"></a>Comentarios

- El valor predeterminado es el almacén del equipo personal.

### <a name="-dynamicfilelist"></a>-dynamicfilelist

Muestra una lista de archivos dinámicos.

```
certutil [options] -dynamicfilelist
```

```
[-config Machine\CAName]
```

### <a name="-databaselocations"></a>-databaselocations

Muestra las ubicaciones de la base de datos.

```
certutil [options] -databaselocations
```

```
[-config Machine\CAName]
```

### <a name="-hashfile"></a>-hashfile

Genera y muestra un hash criptográfico sobre un archivo.

```
certutil [options] -hashfile infile [hashalgorithm]
```

### <a name="-store"></a>-Store

Vuelca el almacén de certificados.

```
certutil [options] -store [certificatestorename [certID [outputfile]]]
```

Donde:

- **CertificateStoreName** es el nombre del almacén de certificados. Por ejemplo:

  - `My, CA (default), Root,`

  - `ldap:///CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?one?objectClass=certificationAuthority (View Root Certificates)`

  - `ldap:///CN=CAName,CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Modify Root Certificates)`

  - `ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?certificateRevocationList?base?objectClass=cRLDistributionPoint (View CRLs)`

  - `ldap:///CN=NTAuthCertificates,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Enterprise CA Certificates)`

  - `ldap: (AD computer object certificates)`

  - `-user ldap: (AD user object certificates)`

- **certID** es el certificado o el token de coincidencia de CRL. Puede ser un número de serie, un certificado SHA-1, una CRL, una CTL o un hash de clave pública, un índice de certificado numérico (0, 1, etc.), un índice de CRL numérico (. 0,,1, etc.), un índice de CTL numérico (.. 0... 1, etc.), una clave pública, una firma o un ObjectId de extensión, un nombre común de sujeto de certificado, una dirección de correo electrónico, un nombre de UPN o DNS, un nombre de contenedor de claves o un nombre de CSP, un nombre de plantilla o ObjectId, un nombre de proveedor de directivas de aplicación o EKU, o un nombre común de emisor de CRL. Muchos de ellos pueden dar lugar a varias coincidencias.

- **OutputFile** es el archivo que se usa para guardar los certificados coincidentes.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-silent] [-split] [-dc DCName]
```

#### <a name="options"></a>Opciones

- La `-user` opción obtiene acceso a un almacén de usuario en lugar de a un almacén del equipo.

- La `-enterprise` opción obtiene acceso a un almacén empresarial de la máquina.

- La `-service` opción obtiene acceso a un almacén del servicio de equipo.

- La `-grouppolicy` opción obtiene acceso a un almacén de directivas de grupo del equipo.

Por ejemplo:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-addstore"></a>-AddStore

Agrega un certificado al almacén. Para obtener más información, consulte el `-store` parámetro de este artículo.

```
certutil [options] -addstore certificatestorename infile
```

Donde:

- **CertificateStoreName** es el nombre del almacén de certificados.

- **INFILE** es el archivo de certificado o CRL que se desea agregar al almacén.

```
[-f] [-user] [-enterprise] [-grouppolicy] [-dc DCName]
```

### <a name="-delstore"></a>-delstore

Elimina un certificado del almacén. Para obtener más información, consulte el `-store` parámetro de este artículo.

```
certutil [options] -delstore certificatestorename certID
```

Donde:

- **CertificateStoreName** es el nombre del almacén de certificados.

- **certID** es el certificado o el token de coincidencia de CRL.

```
[-enterprise] [-user] [-grouppolicy] [-dc DCName]
```

### <a name="-verifystore"></a>-verifystore

Comprueba un certificado en el almacén. Para obtener más información, consulte el `-store` parámetro de este artículo.

```
certutil [options] -verifystore certificatestorename [certID]
```

Donde:

- **CertificateStoreName** es el nombre del almacén de certificados.

- **certID** es el certificado o el token de coincidencia de CRL.

```
[-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-dc DCName] [-t timeout]
```

### <a name="-repairstore"></a>-repairstore

Repara una asociación de claves o actualiza las propiedades del certificado o el descriptor de seguridad de la clave. Para obtener más información, consulte el `-store` parámetro de este artículo.

```
certutil [options] -repairstore certificatestorename certIDlist [propertyinffile | SDDLsecuritydescriptor]
```

Donde:

- **CertificateStoreName** es el nombre del almacén de certificados.

- **certIDlist** es la lista separada por comas de tokens de coincidencia de certificados o CRL. Para obtener más información, consulte la `-store certID` Descripción de este artículo.

- **propertyinffile** es el archivo INF que contiene propiedades externas, entre las que se incluyen:

  ```
  [Properties]
      19 = Empty ; Add archived property, OR:
      19 =       ; Remove archived property

      11 = {text}Friendly Name ; Add friendly name property

      127 = {hex} ; Add custom hexadecimal property
          _continue_ = 00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f
          _continue_ = 10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f

      2 = {text} ; Add Key Provider Information property
        _continue_ = Container=Container Name&
        _continue_ = Provider=Microsoft Strong Cryptographic Provider&
        _continue_ = ProviderType=1&
        _continue_ = Flags=0&
        _continue_ = KeySpec=2

      9 = {text} ; Add Enhanced Key Usage property
        _continue_ = 1.3.6.1.5.5.7.3.2,
        _continue_ = 1.3.6.1.5.5.7.3.1,
  ```

```
[-f] [-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-csp provider]
```

### <a name="-viewstore"></a>-viewstore

Vuelca el almacén de certificados. Para obtener más información, consulte el `-store` parámetro de este artículo.

```
certutil [options] -viewstore [certificatestorename [certID [outputfile]]]
```

Donde:

- **CertificateStoreName** es el nombre del almacén de certificados.

- **certID** es el certificado o el token de coincidencia de CRL.

- **OutputFile** es el archivo que se usa para guardar los certificados coincidentes.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>Opciones

- La `-user` opción obtiene acceso a un almacén de usuario en lugar de a un almacén del equipo.

- La `-enterprise` opción obtiene acceso a un almacén empresarial de la máquina.

- La `-service` opción obtiene acceso a un almacén del servicio de equipo.

- La `-grouppolicy` opción obtiene acceso a un almacén de directivas de grupo del equipo.

Por ejemplo:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-viewdelstore"></a>-viewdelstore

Elimina un certificado del almacén.

```
certutil [options] -viewdelstore [certificatestorename [certID [outputfile]]]
```

Donde:

- **CertificateStoreName** es el nombre del almacén de certificados.

- **certID** es el certificado o el token de coincidencia de CRL.

- **OutputFile** es el archivo que se usa para guardar los certificados coincidentes.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>Opciones

- La `-user` opción obtiene acceso a un almacén de usuario en lugar de a un almacén del equipo.

- La `-enterprise` opción obtiene acceso a un almacén empresarial de la máquina.

- La `-service` opción obtiene acceso a un almacén del servicio de equipo.

- La `-grouppolicy` opción obtiene acceso a un almacén de directivas de grupo del equipo.

Por ejemplo:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-dspublish"></a>-dspublish

Publica un certificado o una lista de revocación de certificados (CRL) en Active Directory.

```
certutil [options] -dspublish certfile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | User | Machine]
```

```
certutil [options] -dspublish CRLfile [DSCDPContainer [DSCDPCN]]
```

Donde:

- **CERTFILE** es el nombre del archivo de certificado que se va a publicar.

- **NTAuthCA** publica el certificado en el almacén de DS Enterprise.

- **RootCA** publica el certificado en el almacén raíz de confianza de DS.

- **Certificación subordinada** publica el certificado de CA en el objeto de la CA de DS.

- **CrossCA** publica el certificado cruzado en el objeto de la CA de DS.

- **Kra** publica el certificado en el objeto agente de recuperación de claves de DS.

- El **usuario** publica el certificado en el objeto de usuario DS.

- El **equipo** publica el certificado en el objeto DS de la máquina.

- **CRLfile** es el nombre del archivo CRL que se va a publicar.

- **DSCDPContainer** es el CN del contenedor DS CDP, normalmente el nombre de la máquina de la entidad de certificación.

- **DSCDPCN** es el objeto CN de DS, que normalmente se basa en el índice de clave y el nombre corto de la CA saneado.

- Utilice `-f` para crear un nuevo objeto de DS.

```
[-f] [-user] [-dc DCName]
```

### <a name="-adtemplate"></a>-adtemplate

Muestra Active Directory plantillas.

```
certutil [options] -adtemplate [template]
```

```
[-f] [-user] [-ut] [-mt] [-dc DCName]
```

### <a name="-template"></a>-plantilla

Muestra las plantillas de certificado.

```
certutil [options] -template [template]
```

```
[-f] [-user] [-silent] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-templatecas"></a>-templatecas

Muestra las entidades de certificación (CA) de una plantilla de certificado.

```
certutil [options] -templatecas template
```

```
[-f] [-user] [-dc DCName]
```

### <a name="-catemplates"></a>-catemplates

Muestra las plantillas de la entidad de certificación.

```
certutil [options] -catemplates [template]
```

```
[-f] [-user] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]
```

### <a name="-setcasites"></a>-setcasites

Administra nombres de sitio, incluida la configuración, la comprobación y la eliminación de nombres de sitio de la entidad de certificación.

```
certutil [options] -setcasites [set] [sitename]
certutil [options] -setcasites verify [sitename]
certutil [options] -setcasites delete
```

Donde:

- **siteName** solo se permite cuando el destino es una sola entidad de certificación.

```
[-f] [-config Machine\CAName] [-dc DCName]
```

#### <a name="remarks"></a>Comentarios

- La `-config` opción tiene como destino una sola entidad de certificación (el valor predeterminado es todas las CA).

- La `-f` opción se puede usar para invalidar los errores de validación del **siteName** especificado o para eliminar todos los sitenames de CA.

> [!NOTE]
> Para obtener más información acerca de la configuración de las CA para el reconocimiento de sitios de Active Directory Domain Services (AD DS), consulte [reconocimiento de sitios de AD DS para clientes de AD CS y PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

### <a name="-enrollmentserverurl"></a>-enrollmentserverURL

Muestra, agrega o elimina las direcciones URL del servidor de inscripciones asociadas a una CA.

```
certutil [options] -enrollmentServerURL [URL authenticationtype [priority] [modifiers]]
certutil [options] -enrollmentserverURL URL delete
```

Donde:

- **AuthenticationType** especifica uno de los siguientes métodos de autenticación de cliente, al agregar una dirección URL:

  1. **Kerberos** : usar credenciales SSL de Kerberos.

  2. **nombre de usuario** : Use una cuenta con nombre para las credenciales SSL.

  3. **ClientCertificate**:-usar credenciales SSL de certificado X. 509.

  4. **anónimo** : usar credenciales SSL anónimas.

- **Delete** elimina la dirección URL especificada asociada a la CA.

- la **prioridad** predeterminada es `1` si no se especifica cuando se agrega una dirección URL.

- los **Modificadores** son una lista separada por comas, que incluye uno o varios de los siguientes elementos:

1. las solicitudes de renovación de **allowrenewalsonly** solo pueden enviarse a esta CA a través de esta dirección URL.

2. **allowkeybasedrenewal** : permite el uso de un certificado que no tiene ninguna cuenta asociada en AD. Esto solo se aplica al modo ClientCertificate y allowrenewalsonly

```
[-config Machine\CAName] [-dc DCName]
```

### <a name="-adca"></a>-adca

Muestra Active Directory entidades de certificación.

```
certutil [options] -adca [CAName]
```

```
[-f] [-split] [-dc DCName]
```

### <a name="-ca"></a>-CA

Muestra las entidades de certificación de la Directiva de inscripción.

```
certutil [options] -CA [CAName | templatename]
```

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policy"></a>-Directiva

Muestra la Directiva de inscripción.

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policycache"></a>-policycache

Muestra o elimina las entradas de caché de la Directiva de inscripción.

```
certutil [options] -policycache [delete]
```

Donde:

- **Delete** elimina las entradas de caché del servidor de directivas.

- **-f** elimina todas las entradas de la memoria caché

```
[-f] [-user] [-policyserver URLorID]
```

### <a name="-credstore"></a>-credstore

Muestra, agrega o elimina entradas del almacén de credenciales.

```
certutil [options] -credstore [URL]
certutil [options] -credstore URL add
certutil [options] -credstore URL delete
```

Donde:

- **URL** es la dirección URL de destino. También puede usar `*` para buscar coincidencias con todas las entradas o `https://machine*` para buscar coincidencias con un prefijo de dirección URL.

- **Agregar** agrega una entrada de almacén de credenciales. El uso de esta opción también requiere el uso de credenciales SSL.

- **Delete** elimina las entradas del almacén de credenciales.

- **-f** sobrescribe una sola entrada o elimina varias entradas.

```
[-f] [-user] [-silent] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-installdefaulttemplates"></a>-installdefaulttemplates

Instala las plantillas de certificado predeterminadas.

```
certutil [options] -installdefaulttemplates
```

```
[-dc DCName]
```

### <a name="-urlcache"></a>-URLcache

Muestra o elimina entradas de caché de direcciones URL.

```
certutil [options] -URLcache [URL | CRL | * [delete]]
```

Donde:

- **URL** es la dirección URL almacenada en caché.

- **CRL** solo se ejecuta en todas las direcciones URL de CRL almacenadas en caché.

- **&#42;** funciona en todas las direcciones URL almacenadas en caché.

- **Delete** elimina las direcciones URL relevantes de la caché local del usuario actual.

- **-f** fuerza la captura de una dirección URL específica y la actualización de la memoria caché.

```
[-f] [-split]
```

### <a name="-pulse"></a>-Pulse

Eventos de inscripción automática de impulsos.

```
certutil [options] -pulse
```

```
[-user]
```

### <a name="-machineinfo"></a>-machineinfo

Muestra información sobre el objeto de máquina Active Directory.

```
certutil [options] -machineinfo domainname\machinename$
```

### <a name="-dcinfo"></a>-DCInfo

Muestra información acerca del controlador de dominio. El valor predeterminado muestra los certificados DC sin comprobación.

```
certutil [options] -DCInfo [domain] [verify | deletebad | deleteall]
```

```
[-f] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

> [!TIP]
> La capacidad de especificar un dominio de Active Directory Domain Services (AD DS) **[dominio]** y para especificar un controlador de dominio (**-DC**) se ha agregado en Windows Server 2012. Para ejecutar correctamente el comando, debe usar una cuenta que sea miembro del grupo **Admins** . del dominio o **administradores de organización**. Las modificaciones de comportamiento de este comando son las siguientes:<ol><li>1. Si no se especifica un dominio y no se especifica un controlador de dominio específico, esta opción devuelve una lista de controladores de dominio que se van a procesar desde el controlador de dominio predeterminado.</li><li>2. Si no se especifica un dominio, pero se especifica un controlador de dominio, se genera un informe de los certificados en el controlador de dominio especificado.</li><li>3. Si se especifica un dominio, pero no se especifica un controlador de dominio, se genera una lista de controladores de dominio junto con los informes de los certificados de cada controlador de dominio de la lista.</li><li>4. Si se especifican el dominio y el controlador de dominio, se genera una lista de controladores de dominio desde el controlador de dominio de destino. También se genera un informe de los certificados para cada controlador de dominio de la lista.</li></ol>
>
>Por ejemplo, supongamos que hay un dominio denominado CPANDL con un controlador de dominio denominado CPANDL-DC1. Puede ejecutar el siguiente comando para recuperar una lista de controladores de dominio y sus certificados de CPANDL-DC1:`certutil -dc cpandl-dc1 -DCInfo cpandl`

### <a name="-entinfo"></a>-entinfo

Muestra información acerca de una entidad de certificación de empresa.

```
certutil [options] -entinfo domainname\machinename$
```

```
[-f] [-user]
```

### <a name="-tcainfo"></a>-tcainfo

Muestra información acerca de la entidad de certificación.

```
certutil [options] -tcainfo [domainDN | -]
```

```
[-f] [-enterprise] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

### <a name="-scinfo"></a>-scinfo

Muestra información acerca de la tarjeta inteligente.

```
certutil [options] -scinfo [readername [CRYPT_DELETEKEYSET]]
```

Donde:

- **CRYPT_DELETEKEYSET** elimina todas las claves de la tarjeta inteligente.

```
[-silent] [-split] [-urlfetch] [-t timeout]
```

### <a name="-scroots"></a>-scroots

Administra los certificados raíz de la tarjeta inteligente.

```
certutil [options] -scroots update [+][inputrootfile] [readername]
certutil [options] -scroots save \@in\\outputrootfile [readername]
certutil [options] -scroots view [inputrootfile | readername]
certutil [options] -scroots delete [readername]
```

```
[-f] [-split] [-p Password]
```

### <a name="-verifykeys"></a>-verifykeys

Comprueba un conjunto de claves público o privado.

```
certutil [options] -verifykeys [keycontainername cacertfile]
```

Donde:

- **keycontainername** es el nombre del contenedor de claves para la clave que se va a comprobar. Esta opción tiene como valor predeterminado las claves de máquina. Para cambiar a las claves de usuario, use `-user` .

- **cacertfile** firma o cifra los archivos de certificado.

```
[-f] [-user] [-silent] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentarios

- Si no se especifica ningún argumento, se comprueba la clave privada de cada certificado de entidad de certificación de firma.

- Esta operación solo se puede realizar en una CA local o en una clave local.

### <a name="-verify"></a>-comprobar

Comprueba un certificado, una lista de revocación de certificados (CRL) o una cadena de certificados.

```
certutil [options] -verify certfile [applicationpolicylist | - [issuancepolicylist]]
certutil [options] -verify certfile [cacertfile [crossedcacertfile]]
certutil [options] -verify CRLfile cacertfile [issuedcertfile]
certutil [options] -verify CRLfile cacertfile [deltaCRLfile]
```

Donde:

- **CERTFILE** es el nombre del certificado que se va a comprobar.

- **applicationpolicylist** es la lista opcional separada por comas de la Directiva de aplicación requerida objectId.

- **issuancepolicylist** es la lista opcional separada por comas de la Directiva de emisión requerida objectId.

- **cacertfile** es el certificado de CA de emisión opcional con el que realizar la comprobación.

- **crossedcacertfile** es el certificado opcional con certificación cruzada por **CERTFILE**.

- **CRLfile** es el archivo CRL que se usa para comprobar el **cacertfile**.

- **issuedcertfile** es el certificado opcional emitido que está incluido en CRLfile.

- **deltaCRLfile** es el archivo Delta CRL opcional.

```
[-f] [-enterprise] [-user] [-silent] [-split] [-urlfetch] [-t timeout]
```

#### <a name="remarks"></a>Comentarios

- El uso de **applicationpolicylist** restringe la creación de cadenas solo a las cadenas válidas para las directivas de aplicación especificadas.

- El uso de **issuancepolicylist** restringe la creación de cadenas solo a las cadenas válidas para las directivas de emisión especificadas.

- El uso de **cacertfile** comprueba los campos en el archivo con **CERTFILE** o **CRLfile**.

- El uso de **issuedcertfile** comprueba los campos del archivo con respecto a **CRLfile**.

- El uso de deltaCRLfile comprueba los campos en el archivo con **CERTFILE**.

- Si no se especifica **cacertfile** , la cadena completa se compila y se comprueba con **CERTFILE**.

- Si se especifican **cacertfile** y **crossedcacertfile** , los campos de ambos archivos se comprueban con **CERTFILE**.

### <a name="-verifyctl"></a>-verifyCTL

Comprueba el AuthRoot o la CTL de certificados no permitidos.

```
certutil [options] -verifyCTL CTLobject [certdir] [certfile]
```

Donde:

- **CTLobject** identifica la CTL que se va a comprobar, incluido:

  - **AuthRootWU** : Lee el archivo CAB de AuthRoot y los certificados coincidentes de la memoria caché de la dirección URL. Use `-f` para descargar desde Windows Update en su lugar.

  - **DisallowedWU** : Lee el archivo CAB de certificados no permitidos y el archivo de almacén de certificados no permitido de la memoria caché de la dirección URL. Use `-f` para descargar desde Windows Update en su lugar.

  - **AuthRoot** : Lee la CTL de AuthRoot en caché del registro. Use con `-f` y una **CERTFILE** que no sea de confianza para forzar la actualización del registro en caché AuthRoot y las CTL de certificados no permitidas.

  - No **permitido** : Lee la CTL de certificados no permitidos almacenados en caché del registro. Use con `-f` y una **CERTFILE** que no sea de confianza para forzar la actualización del registro en caché AuthRoot y las CTL de certificados no permitidas.

- **CTLfilename** especifica el archivo o la ruta de acceso http al archivo CTL o CAB.

- **certdir** especifica la carpeta que contiene los certificados que coinciden con las entradas CTL. Tiene como valor predeterminado la misma carpeta o sitio web que **CTLobject**. El uso de una ruta de acceso de carpeta http requiere un separador de ruta de acceso al final. Si no especifica **AuthRoot** o no se **permite**, se buscarán en varias ubicaciones los certificados coincidentes, incluidos los almacenes de certificados locales, los recursos de crypt32.dll y la memoria caché de la dirección URL local. Use `-f` para realizar la descarga desde Windows Update, según sea necesario.

- **CERTFILE** especifica los certificados que se van a comprobar. Los certificados coinciden con las entradas CTL y muestran los resultados. Esta opción suprime la mayoría de los resultados predeterminados.

```
[-f] [-user] [-split]
```

### <a name="-sign"></a>-Sign

Vuelve a firmar una lista de revocación de certificados (CRL) o un certificado.

```
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [startdate+dd:hh] [+serialnumberlist | -serialnumberlist | -objectIDlist | \@extensionfile]
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [#hashalgorithm] [+alternatesignaturealgorithm | -alternatesignaturealgorithm]
```

Donde:

- **infilelist** es la lista separada por comas de los archivos de certificado o CRL que se deben modificar y volver a firmar.

- **SerialNumber** es el número de serie del certificado que se va a crear. El período de validez y otras opciones no pueden estar presentes.

- **CRL** crea una CRL vacía. El período de validez y otras opciones no pueden estar presentes.

- **outfilelist** es la lista separada por comas de los archivos de salida de certificados o CRL modificados. El número de archivos debe coincidir con infilelist.

- **startDate + DD: HH** es el nuevo período de validez para los archivos de certificado o CRL, incluidos:

  - fecha adicional opcional

  - período de validez de días y horas opcionales

  Si se especifican ambos, se debe usar un separador de signo más (+). Use `now[+dd:hh]` para comenzar en el momento actual. `never`Se usa para no tener fecha de expiración (solo para CRL).

- **serialnumberlist** es la lista de números de serie separados por comas de los archivos que se van a agregar o quitar.

- **objectIDlist** es la lista de objectId de la extensión separada por comas de los archivos que se van a quitar.

- ** \@ extensionfile** es el archivo INF que contiene las extensiones que se van a actualizar o quitar. Por ejemplo:

  ```
  [Extensions]
      2.5.29.31 = ; Remove CRL Distribution Points extension
      2.5.29.15 = {hex} ; Update Key Usage extension
      _continue_=03 02 01 86
  ```

- **HashAlgorithm** es el nombre del algoritmo hash. Este solo debe ser el texto precedido por el `#` signo.

- **alternatesignaturealgorithm** es el especificador de algoritmo de firma alternativo.

```
[-nullsign] [-f] [-silent] [-cert certID]
```

#### <a name="remarks"></a>Comentarios

- Al usar el signo menos (-), se quitan los números de serie y las extensiones.

- El uso del signo más (+) agrega números de serie a una CRL.

- Puede usar una lista para quitar los números de serie y objectId de una CRL al mismo tiempo.

- El uso del signo menos antes de **alternatesignaturealgorithm** le permite usar el formato de firma heredado. El uso del signo más le permite usar el formato de firma alternativo. Si no especifica **alternatesignaturealgorithm**, se usa el formato de firma en el certificado o la CRL.

### <a name="-vroot"></a>-vroot

Crea o elimina raíces virtuales web y recursos compartidos de archivos.

```
certutil [options] -vroot [delete]
```

### <a name="-vocsproot"></a>-vocsproot

Crea o elimina raíces virtuales web para un proxy web OCSP.

```
certutil [options] -vocsproot [delete]
```

### <a name="-addenrollmentserver"></a>-addenrollmentserver

Agregue una aplicación de servidor de inscripciones y un grupo de aplicaciones si es necesario para la entidad de certificación especificada. Este comando no instala archivos binarios ni paquetes.

```
certutil [options] -addenrollmentserver kerberos | username | clientcertificate [allowrenewalsonly] [allowkeybasedrenewal]
```

Donde:

- **addenrollmentserver** requiere el uso de un método de autenticación para la conexión de cliente con el servidor de inscripción de certificados, que incluye:

  - **Kerberos** usa CREDENCIALes SSL de Kerberos.

  - **nombre de usuario** usa una cuenta con nombre para las credenciales SSL.

  - **ClientCertificate** usa CREDENCIALes SSL de certificado X. 509.

- **allowrenewalsonly** solo permite el envío de solicitudes de renovación a la entidad de certificación a través de la dirección URL.

- **allowkeybasedrenewal** permite el uso de un certificado sin cuenta asociada en Active Directory. Esto se aplica cuando se usa con el modo **ClientCertificate** y **allowrenewalsonly** .

```
[-config Machine\CAName]
```

### <a name="-deleteenrollmentserver"></a>-deleteenrollmentserver

Elimina una aplicación de servidor de inscripciones y un grupo de aplicaciones si es necesario para la entidad de certificación especificada. Este comando no instala archivos binarios ni paquetes.

```
certutil [options] -deleteenrollmentserver kerberos | username | clientcertificate
```

Donde:

- **deleteenrollmentserver** requiere el uso de un método de autenticación para la conexión de cliente con el servidor de inscripción de certificados, que incluye:

  - **Kerberos** usa CREDENCIALes SSL de Kerberos.

  - **nombre de usuario** usa una cuenta con nombre para las credenciales SSL.

  - **ClientCertificate** usa CREDENCIALes SSL de certificado X. 509.

```
[-config Machine\CAName]
```

### <a name="-addpolicyserver"></a>-addpolicyserver

Agregue una aplicación de servidor de directivas y un grupo de aplicaciones, si es necesario. Este comando no instala archivos binarios ni paquetes.

```
certutil [options] -addpolicyserver kerberos | username | clientcertificate [keybasedrenewal]
```

Donde:

- **addpolicyserver** requiere el uso de un método de autenticación para la conexión de cliente con el servidor de directivas de certificados, incluidos:

  - **Kerberos** usa CREDENCIALes SSL de Kerberos.

  - **nombre de usuario** usa una cuenta con nombre para las credenciales SSL.

  - **ClientCertificate** usa CREDENCIALes SSL de certificado X. 509.

- **keybasedrenewal** permite el uso de directivas devueltas al cliente que contiene plantillas de keybasedrenewal. Esta opción solo se aplica a la autenticación de **nombre de usuario** y **ClientCertificate** .

### <a name="-deletepolicyserver"></a>-deletepolicyserver

Elimina una aplicación de servidor de directivas y un grupo de aplicaciones, si es necesario. Este comando no quita archivos binarios ni paquetes.

certutil [opciones]-deletePolicyServer Kerberos | nombre de usuario | ClientCertificate [keybasedrenewal]

Donde:

- **deletepolicyserver** requiere el uso de un método de autenticación para la conexión de cliente con el servidor de directivas de certificados, incluidos:

  - **Kerberos** usa CREDENCIALes SSL de Kerberos.

  - **nombre de usuario** usa una cuenta con nombre para las credenciales SSL.

  - **ClientCertificate** usa CREDENCIALes SSL de certificado X. 509.

- **keybasedrenewal** permite el uso de un servidor de directivas de keybasedrenewal.

### <a name="-oid"></a>-OID

Muestra el identificador de objeto o establece un nombre para mostrar.

```
certutil [options] -oid objectID [displayname | delete [languageID [type]]]
certutil [options] -oid groupID
certutil [options] -oid agID | algorithmname [groupID]
```

Donde:

- **objectId** muestra o para agregar el nombre para mostrar.

- **GROUPID** es el número de GROUPID (decimal) que enumera objectId.

- **algID** es el Identificador hexadecimal que objectId busca.

- **algorithmname** es el nombre del algoritmo que objectId busca.

- **displayName** muestra el nombre que se va a almacenar en DS.

- **Delete** elimina el nombre para mostrar.

- **LanguageId** es el valor de identificador de idioma (el valor predeterminado es actual: 1033).

- **Type** es el tipo de objeto DS que se va a crear, incluido:

  - `1`-Plantilla (valor predeterminado)

  - `2`-Directiva de emisión

  - `3`-Directiva de aplicación

- `-f`crea un objeto DS.

### <a name="-error"></a>-error

Muestra el texto del mensaje asociado a un código de error.

```
certutil [options] -error errorcode
```

### <a name="-getreg"></a>-getreg

Muestra un valor del registro.

```
certutil [options] -getreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

Donde:

- **CA** usa la clave del registro de una entidad de certificación.

- **restore** usa la clave del registro restore de la entidad de certificación.

- la **Directiva** usa la clave del registro del módulo de directivas.

- **Exit** usa la clave del registro del primer módulo de salida.

- la **plantilla** usa la clave del registro de la plantilla (se usa `-user` para las plantillas de usuario).

- **ENROLL** usa la clave del registro de inscripción (se usa `-user` para el contexto de usuario).

- la **cadena** utiliza la clave del registro de configuración de la cadena.

- **policyservers** usa la clave del registro de los servidores de directivas.

- **ProgID** usa la Directiva o el ProgID del módulo de salida (nombre de la subclave del registro).

- **registryvaluename** usa el nombre del valor del registro ( `Name*` se usa para prefijar la coincidencia).

- el **valor** utiliza el nuevo valor de registro Numeric, String o Date o el nombre de archivo. Si un valor numérico comienza con `+` o `-` , los bits especificados en el nuevo valor se establecen o se borran en el valor del registro existente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentarios

- Si un valor de cadena comienza con `+` o `-` , y el valor existente es un `REG_MULTI_SZ` valor, la cadena se agrega o se quita del valor del registro existente. Para forzar la creación de un `REG_MULTI_SZ` valor, agregue `\n` al final del valor de la cadena.

- Si el valor comienza por `\@` , el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, se analiza como `[Date][+|-][dd:hh]` una fecha opcional más o menos días y horas opcionales. Si se especifican ambos, use un signo más (+) o un separador de signo menos (-). `now+dd:hh`Se utiliza para una fecha relativa a la hora actual.

- `chain\chaincacheresyncfiletime \@now`Se utiliza para vaciar de forma eficaz las CRL almacenadas en caché.

### <a name="-setreg"></a>-setreg

Establece un valor del registro.

```
certutil [options] -setreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]]registryvaluename value
```

Donde:

- **CA** usa la clave del registro de una entidad de certificación.

- **restore** usa la clave del registro restore de la entidad de certificación.

- la **Directiva** usa la clave del registro del módulo de directivas.

- **Exit** usa la clave del registro del primer módulo de salida.

- la **plantilla** usa la clave del registro de la plantilla (se usa `-user` para las plantillas de usuario).

- **ENROLL** usa la clave del registro de inscripción (se usa `-user` para el contexto de usuario).

- la **cadena** utiliza la clave del registro de configuración de la cadena.

- **policyservers** usa la clave del registro de los servidores de directivas.

- **ProgID** usa la Directiva o el ProgID del módulo de salida (nombre de la subclave del registro).

- **registryvaluename** usa el nombre del valor del registro ( `Name*` se usa para prefijar la coincidencia).

- el **valor** utiliza el nuevo valor de registro Numeric, String o Date o el nombre de archivo. Si un valor numérico comienza con `+` o `-` , los bits especificados en el nuevo valor se establecen o se borran en el valor del registro existente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentarios

- Si un valor de cadena comienza con `+` o `-` , y el valor existente es un `REG_MULTI_SZ` valor, la cadena se agrega o se quita del valor del registro existente. Para forzar la creación de un `REG_MULTI_SZ` valor, agregue `\n` al final del valor de la cadena.

- Si el valor comienza por `\@` , el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, se analiza como `[Date][+|-][dd:hh]` una fecha opcional más o menos días y horas opcionales. Si se especifican ambos, use un signo más (+) o un separador de signo menos (-). `now+dd:hh`Se utiliza para una fecha relativa a la hora actual.

- `chain\chaincacheresyncfiletime \@now`Se utiliza para vaciar de forma eficaz las CRL almacenadas en caché.

### <a name="-delreg"></a>-delreg

Elimina un valor del registro.

```
certutil [options] -delreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

Donde:

- **CA** usa la clave del registro de una entidad de certificación.

- **restore** usa la clave del registro restore de la entidad de certificación.

- la **Directiva** usa la clave del registro del módulo de directivas.

- **Exit** usa la clave del registro del primer módulo de salida.

- la **plantilla** usa la clave del registro de la plantilla (se usa `-user` para las plantillas de usuario).

- **ENROLL** usa la clave del registro de inscripción (se usa `-user` para el contexto de usuario).

- la **cadena** utiliza la clave del registro de configuración de la cadena.

- **policyservers** usa la clave del registro de los servidores de directivas.

- **ProgID** usa la Directiva o el ProgID del módulo de salida (nombre de la subclave del registro).

- **registryvaluename** usa el nombre del valor del registro ( `Name*` se usa para prefijar la coincidencia).

- el **valor** utiliza el nuevo valor de registro Numeric, String o Date o el nombre de archivo. Si un valor numérico comienza con `+` o `-` , los bits especificados en el nuevo valor se establecen o se borran en el valor del registro existente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentarios

- Si un valor de cadena comienza con `+` o `-` , y el valor existente es un `REG_MULTI_SZ` valor, la cadena se agrega o se quita del valor del registro existente. Para forzar la creación de un `REG_MULTI_SZ` valor, agregue `\n` al final del valor de la cadena.

- Si el valor comienza por `\@` , el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, se analiza como `[Date][+|-][dd:hh]` una fecha opcional más o menos días y horas opcionales. Si se especifican ambos, use un signo más (+) o un separador de signo menos (-). `now+dd:hh`Se utiliza para una fecha relativa a la hora actual.

- `chain\chaincacheresyncfiletime \@now`Se utiliza para vaciar de forma eficaz las CRL almacenadas en caché.

### <a name="-importkms"></a>-importKMS

Importa claves de usuario y certificados en la base de datos del servidor para el archivo de claves.

```
certutil [options] -importKMS userkeyandcertfile [certID]
```

Donde:

- **userkeyandcertfile** es un archivo de datos con claves privadas de usuario y certificados que se van a archivar. Este archivo puede ser:

  - Un archivo de exportación del servidor de administración de claves (KMS) de Exchange.

  - Un archivo PFX.

- certID es un token de coincidencia de certificado de descifrado de archivos de exportación de KMS. Para obtener más información, consulte el `-store` parámetro de este artículo.

- `-f`importa certificados no emitidos por la entidad de certificación.

```
[-f] [-silent] [-split] [-config Machine\CAName] [-p password] [-symkeyalg symmetrickeyalgorithm[,keylength]]
```

### <a name="-importcert"></a>-importcert

Importa un archivo de certificado en la base de datos.

```
certutil [options] -importcert certfile [existingrow]
```

Donde:

- **existingrow** importa el certificado en lugar de una solicitud pendiente para la misma clave.

- `-f`importa certificados no emitidos por la entidad de certificación.

```
[-f] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentarios

Es posible que la entidad de certificación también deba configurarse para admitir certificados externos. Para ello, escriba `import - certutil -setreg ca\KRAFlags +KRAF_ENABLEFOREIGN` .

### <a name="-getkey"></a>-getKey

Recupera un BLOB de recuperación de clave privada archivada, genera un script de recuperación o recupera las claves archivadas.

```
certutil [options] -getkey searchtoken [recoverybloboutfile]
certutil [options] -getkey searchtoken script outputscriptfile
certutil [options] -getkey searchtoken retrieve | recover outputfilebasename
```

Donde:

- el **script** genera un script para recuperar y recuperar claves (comportamiento predeterminado si se encuentran varios candidatos de recuperación coincidentes, o si no se especifica el archivo de salida).

- **recuperar** recupera uno o varios blobs de recuperación de claves (comportamiento predeterminado si se encuentra exactamente un candidato de recuperación coincidente y si se especifica el archivo de salida). Con esta opción se trunca cualquier extensión y se anexa la cadena específica del certificado y la extensión. REC para cada BLOB de recuperación de claves.  Cada archivo contiene una cadena de certificados y una clave privada asociada, todavía cifradas a uno o varios certificados de agente de recuperación de claves.

- **recuperar** recupera y recupera claves privadas en un solo paso (requiere claves privadas y certificados de agente de recuperación de claves). Con esta opción se trunca cualquier extensión y se anexa la extensión. p12.  Cada archivo contiene las cadenas de certificado recuperadas y las claves privadas asociadas, que se almacenan como un archivo PFX.

- **SearchToken** selecciona las claves y los certificados que se van a recuperar, incluido:

  - 1. Nombre común del certificado

  - 2. Número de serie del certificado

  - 3. Hash SHA-1 de certificado (huella digital)

  - 4. Hash SHA-1 de certificado (identificador de clave de sujeto)

  - 5. Nombre del solicitante (dominio\usuario)

  - 6. UPN (dominio de usuario \@ )

- **recoverybloboutfile** genera un archivo con una cadena de certificados y una clave privada asociada, que todavía está cifrada en uno o varios certificados de agente de recuperación de claves.

- **outputscriptfile** genera un archivo con un script por lotes para recuperar y recuperar las claves privadas.

- **outputfilebasename** genera un nombre base de archivo.

```
[-f] [-unicodetext] [-silent] [-config Machine\CAName] [-p password] [-protectto SAMnameandSIDlist] [-csp provider]
```

### <a name="-recoverkey"></a>-recoverkey

Recuperar una clave privada archivada.

```
certutil [options] -recoverkey recoveryblobinfile [PFXoutfile [recipientindex]]
```

```
[-f] [-user] [-silent] [-split] [-p password] [-protectto SAMnameandSIDlist] [-csp provider] [-t timeout]
```

### <a name="-mergepfx"></a>-mergePFX

Combina archivos PFX.

```
certutil [options] -mergePFX PFXinfilelist PFXoutfile [extendedproperties]
```

Donde:

- **PFXinfilelist** es una lista separada por comas de archivos de entrada pfx.

- **PFXoutfile** es el nombre del archivo de salida pfx.

- **ExtendedProperties** incluye cualquier propiedad extendida.

```
[-f] [-user] [-split] [-p password] [-protectto SAMnameAndSIDlist] [-csp provider]
```

#### <a name="remarks"></a>Comentarios

- La contraseña especificada en la línea de comandos debe ser una lista de contraseñas separadas por comas.

- Si se especifica más de una contraseña, se usa la última contraseña para el archivo de salida. Si solo se proporciona una contraseña o si la última contraseña es `*` , se solicitará al usuario la contraseña del archivo de salida.

### <a name="-convertepf"></a>-convertEPF

Convierte un archivo PFX en un archivo EPF.

```
certutil [options] -convertEPF PFXinfilelist PFXoutfile [cast | cast-] [V3CAcertID][,salt]
```

Donde:


- **PFXinfilelist** es una lista separada por comas de archivos de entrada pfx.

- **PFXoutfile** es el nombre del archivo de salida pfx.

- **EPF** es el nombre del archivo de salida de EPF.

- la **conversión** usa el cifrado Cast 64.

- **conversión: usa el** cifrado Cast 64 (exportación)

- **V3CAcertID** es el token de coincidencia de certificado de CA V3. Para obtener más información, consulte el `-store` parámetro de este artículo.

- **sal** es la cadena Salt del archivo de salida de EPF.

```
[-f] [-silent] [-split] [-dc DCName] [-p password] [-csp provider]
```

#### <a name="remarks"></a>Comentarios

- La contraseña especificada en la línea de comandos debe ser una lista de contraseñas separadas por comas.

- Si se especifica más de una contraseña, se usa la última contraseña para el archivo de salida. Si solo se proporciona una contraseña o si la última contraseña es `*` , se solicitará al usuario la contraseña del archivo de salida.

### <a name="-"></a>-?

Muestra la lista de parámetros.

```
certutil -?
certutil <name_of_parameter> -?
certutil -? -v
```

Donde:

- **-?** muestra la lista completa de parámetros

- **-`<name_of_parameter>` -?** muestra el contenido de la ayuda para el parámetro especificado.

- **-?-v** muestra una lista completa de parámetros y opciones.

## <a name="options"></a>Opciones

En esta sección se definen todas las opciones que se pueden especificar, basándose en el comando. Cada parámetro incluye información sobre qué opciones son válidas para su uso.

| Opciones | Descripción |
| ------- | ----------- |
| -nullsign | Use el hash de los datos como una firma. |
| -f | Forzar la sobrescritura. |
| -enterprise | Use el almacén de certificados del registro de empresa del equipo local. |
| -usuario | Use las claves de HKEY_CURRENT_USER o el almacén de certificados. |
| -GroupPolicy | Usar el almacén de certificados de la Directiva de grupo. |
| -ut | Mostrar plantillas de usuario. |
| -MT | Mostrar plantillas de máquina. |
| -Unicode | Escriba la salida redirigida en Unicode. |
| -UnicodeText | Escriba el archivo de salida en Unicode. |
| -GMT | Mostrar horas usando GMT. |
| -segundos | Mostrar horas usando segundos y milisegundos. |
| -silent | Use la `silent` marca para adquirir el contexto de cifrado. |
| -División | Divida los elementos de ASN. 1 incrustados y guárdelos en archivos. |
| -v | Proporcione información más detallada (detallada). |
| -PrivateKey | Mostrar datos de contraseña y de clave privada. |
| anclar PIN | PIN de tarjeta inteligente. |
| -urlfetch | Recupere y compruebe los certificados AIA y las CRL de CDP. |
| -config Machine\CAName | Cadena de nombre de equipo y entidad de certificación. |
| -policyserver URLorID | IDENTIFICADOR o dirección URL del servidor de directivas. Para la selección U/I, use `-policyserver` . Para todos los servidores de directivas, use`-policyserver *`|
| -anónimo | Usar credenciales SSL anónimas. |
| -Kerberos | Usar credenciales SSL de Kerberos. |
| -ClientCertificate clientcertID | Use las credenciales SSL del certificado X. 509. Para la selección U/I, use `-clientcertificate` . |
| -username nombredeusuario | Use una cuenta con nombre para las credenciales SSL. Para la selección U/I, use `-username` . |
| -CERT certID | Certificado de firma. |
| -controlador de dominio Nombrededc | Establecer como destino un controlador de dominio específico. |
| -Restrict restrictionlist | Lista de restricciones separadas por comas. Cada restricción consta de un nombre de columna, un operador relacional y un entero constante, una cadena o una fecha. Un nombre de columna puede ir precedido de un signo más o menos para indicar el criterio de ordenación. Por ejemplo: `requestID = 47`, `+requestername >= a, requestername` o `-requestername > DOMAIN, Disposition = 21` |
| -out columnlist | Lista de columnas separadas por comas. |
| -p contraseña | Contraseña |
| -protectto SAMnameandSIDlist | Lista de SID/nombre SAM separados por comas. |
| -proveedor de CSP | Proveedor |
| -t tiempo de espera | Tiempo de espera de captura de URL en milisegundos. |
| -symkeyalg symmetrickeyalgorithm [, keylength] | Nombre del algoritmo de clave simétrica con una longitud de clave opcional. Por ejemplo, `AES,128` o `3DES`. |

### <a name="additional-references"></a>Referencias adicionales

Para obtener más ejemplos sobre cómo usar este comando, vea.

- [Ejemplos de certutil para administrar Active Directory servicios de Certificate Server (AD CS) desde la línea de comandos](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)

- [Tareas certutil para administrar certificados](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc772898(v=ws.10))

- [Exportación de la solicitud binaria mediante el tutorial de la herramienta de línea de comandos certutil.exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)

- [Renovación del certificado de CA raíz](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)

- [comando certutil](certutil.md)
