---
title: certutil
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a602472d30f19cb2d4a802423635e5788e78a43
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434555"
---
# <a name="certutil"></a>certutil

Certutil.exe es un programa de línea de comandos que se instala como parte de los servicios de Certificate Server. Puede utilizar Certutil.exe para volcar y mostrar información de configuración de certificación emisora (CA), configurar servicios de certificados de copia de seguridad y restaurar los componentes de la entidad de certificación y comprobar las cadenas de certificados, los pares de claves y certificados.

Cuando se ejecuta certutil en una entidad de certificación sin parámetros adicionales, muestra la configuración actual de la entidad de certificación. Cuando cerutil se ejecuta en una entidad, valor predeterminado es el comando para ejecutar el certutil [-volcado](#-dump) verbo.

> [!WARNING]
> Las versiones anteriores de certutil no pueden proporcionar todas las opciones que se describen en este documento. Puede ver todas las opciones que proporciona una versión específica de certutil ejecutando los comandos que se muestra en el [notaciones sintaxis](#syntax-notations) sección.

## <a name="menu"></a>Menú

Las secciones principales en este documento son:

- [Verbs](#verbs)
- [Notaciones de sintaxis](#syntax-notations)
- [Opciones](#options)
- [Ejemplos adicionales de certutil](#additional-certutil-examples)

## <a name="verbs"></a>Verbos

En la tabla siguiente se describe los verbos que se pueden usar con el comando certutil.

|Verbos|Descripción|
|-----|-----------|
|[-dump](#-dump)|Información de configuración o archivos de volcado de memoria|
|[-asn](#-asn)|Analizar el archivo ASN.1|
|[-decodehex](#-decodehex)|Descodificación de archivo con codificación hexadecimal|
|[-decode](#-decode)|Descodificar un archivo codificado en Base64|
|[-encode](#-encode)|Codificar un archivo en Base64|
|[-deny](#-deny)|Denegar una solicitud de certificado pendiente|
|[-resubmit](#-resubmit)|Volver a enviar una solicitud de certificado pendiente|
|[-setattributes](#-setattributes)|Establecimiento de atributos para una solicitud de certificado pendiente|
|[-setextension](#-setextension)|Establezca una extensión para una solicitud de certificado pendiente|
|[-revoke](#-revoke)|Revocar un certificado|
|[-isvalid](#-isvalid)|Mostrar la disposición del certificado actual|
|[-getconfig](#-getconfig)|Obtener la cadena de configuración predeterminada|
|[-ping](#-ping)|Intenta ponerse en contacto con la interfaz de solicitud de servicios de certificado Active Directory|
|-pingadmin|Intenta ponerse en contacto con la interfaz del Administrador de servicios de certificados de Active Directory|
|[-CAInfo](#-cainfo)|Mostrar información acerca de la entidad de certificación|
|[-ca.cert](#-cacert)|Recuperar el certificado para la entidad de certificación|
|[-ca.chain](#-cachain)|Recuperar la cadena de certificados para la entidad de certificación|
|[-GetCRL](#-getcrl)|Obtener una lista de revocación de certificados (CRL)|
|[-CRL](#-crl)|Publicar nuevas listas de revocación de certificados (CRL) [o solo diferencias CRL]|
|[-shutdown](#-shutdown)|Servicios de certificados de Active Directory de apagado|
|[-installCert](#-installcert)|Instalar un certificado de entidad de certificación|
|[-renewCert](#-renewcert)|Renovar un certificado de entidad de certificación|
|[-schema](#-schema)|El esquema para el certificado de volcado de memoria|
|[-view](#-view)|Volcado de memoria de la vista de certificado|
|[-db](#-db)|Volcado de memoria de la base de datos sin procesar|
|[-deleterow](#-deleterow)|Eliminar una fila de la base de datos|
|[-backup](#-backup)|Servicios de certificados de copia de seguridad de Active Directory|
|[-backupDB](#-backupdb)|Copia de seguridad de la base de datos de servicios de certificados de Active Directory|
|[-backupKey](#-backupkey)|Copia de seguridad de la clave privada y certificado de servicios de certificados de Active Directory|
|[-restore](#-restore)|Restauración de servicios de certificados de Active Directory|
|[-restoreDB](#-restoredb)|Restaurar la base de datos de servicios de certificados de Active Directory|
|[-restoreKey](#-restorekey)|Restaurar la clave privada y certificado de servicios de certificados de Active Directory|
|[-importPFX](#-importpfx)|Importar certificado y clave privada|
|[-dynamicfilelist](#-dynamicfilelist)|Mostrar una lista de archivos dinámicos|
|[-databaselocations](#-databaselocations)|Mostrar las ubicaciones de la base de datos|
|[-hashfile](#-hashfile)|Generar y mostrar un hash criptográfico en un archivo|
|[-store](#-store)|El almacén de certificados de volcado de memoria|
|[-addstore](#-addstore)|Agregar un certificado al almacén|
|[-delstore](#-delstore)|Eliminar un certificado del almacén|
|[-verifystore](#-verifystore)|Comprobar un certificado en el almacén|
|[-repairstore](#-repairstore)|Una asociación de clave de reparar o actualizar las propiedades del certificado o el descriptor de seguridad de claves|
|[-viewstore](#-viewstore)|El almacén de certificados de volcado de memoria|
|[-viewdelstore](#-viewdelstore)|Eliminar un certificado del almacén|
|[-dsPublish](#-dspublish)|Publicar un certificado o una lista de revocación de certificados (CRL) en Active Directory|
|[-ADTemplate](#-adtemplate)|Mostrar las plantillas de AD|
|[-Template](#-template)|Mostrar las plantillas de certificado|
|[-TemplateCAs](#-templatecas)|Mostrar las entidades de certificación (CA) para una plantilla de certificado|
|[-CATemplates](#-catemplates)|Mostrar las plantillas de CA|
|[-SetCASites](#-setcasites)|Administrar nombres de sitio para las entidades de certificación|
|[-enrollmentServerURL](#-enrollmentserverurl)|Mostrar, agregar o eliminar direcciones URL del servidor de inscripción asociadas con una entidad de certificación|
|[-ADCA](#-adca)|Mostrar las entidades de certificación de AD|
|[-CA](#-ca)|Mostrar las entidades de certificación de directivas de inscripción|
|[-Policy](#-policy)|Mostrar la directiva de inscripción|
|[-PolicyCache](#-policycache)|Mostrar o eliminar las entradas de caché de directiva de inscripción|
|[-CredStore](#-credstore)|Mostrar, agregar o eliminar las entradas de la credencial Store|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|Instalar plantillas de certificado predeterminadas|
|[-URLCache](#-urlcache)|Mostrar o eliminar las entradas de caché de dirección URL|
|[-pulse](#-pulse)|Eventos de inscripción automática de impulsos|
|[-MachineInfo](#-machineinfo)|Mostrar información sobre el objeto de equipo de Active Directory|
|[-DCInfo](#-dcinfo)|Mostrar información sobre el controlador de dominio|
|[-EntInfo](#-entinfo)|Mostrar información acerca de una CA de empresa|
|[-TCAInfo](#-tcainfo)|Mostrar información acerca de la entidad de certificación|
|[-SCInfo](#-scinfo)|Mostrar información acerca de la tarjeta inteligente|
|[-SCRoots](#-scroots)|Administrar certificados raíz de tarjeta inteligente|
|[-verifykeys](#-verifykeys)|Comprobar un conjunto de claves público o privado|
|[-verify](#-verify)|Comprobar un certificado, una lista de revocación de certificados (CRL) o una cadena de certificados|
|[-verifyCTL](#-verifyctl)|Comprobar AuthRoot o CTL de certificados no permitidos|
|[-sign](#-sign)|Volver a firmar un certificado o una lista de revocación de certificados (CRL)|
|[-vroot](#-vroot)|Crear o eliminar raíces virtuales web y recursos compartidos de archivos|
|[-vocsproot](#-vocsproot)|Crear o eliminar raíces virtuales de web para un servidor proxy web OCSP|
|[-addEnrollmentServer](#-addenrollmentserver)|Agregar una aplicación de servidor de inscripción|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|Eliminar una aplicación de servidor de inscripción|
|[-addPolicyServer](#-addpolicyserver)|Agregar una aplicación de servidor de directivas|
|[-deletePolicyServer](#-deletepolicyserver)|Eliminar una aplicación de servidor de directivas|
|[-oid](#-oid)|Mostrar el identificador de objeto o establecer un nombre para mostrar|
|[-error](#-error)|Mostrar el texto del mensaje asociado con un código de error|
|[-getreg](#-getreg)|Mostrar un valor del registro|
|[-setreg](#-setreg)|Establecer un valor del registro|
|[-delreg](#-delreg)|Eliminar un valor del registro|
|[-ImportKMS](#-importkms)|Importar certificados y claves de usuario en la base de datos para el archivo de claves|
|[-ImportCert](#-importcert)|Importar un archivo de certificado en la base de datos|
|[-GetKey](#-getkey)|Recuperar un blob de recuperación de claves privadas archivadas|
|[-RecoverKey](#-recoverkey)|Recuperar una clave privada archivada|
|[-MergePFX](#-mergepfx)|Combinar archivos PFX|
|[-ConvertEPF](#-convertepf)|Convertir un archivo PFX en un archivo EPF|
|-?|Muestra la lista de verbos|
|- *\<verb>* -?|Muestra ayuda para el verbo especificado.|
|-? -v|Muestra una lista completa de verbos y|

Vuelva a [menú](#menu)

## <a name="syntax-notations"></a>Notaciones de sintaxis

- Para conocer la sintaxis básica de línea de comandos, ejecute `certutil -?`
- ¿Para obtener la sintaxis sobre el uso de certutil con un verbo específico, ejecute **certutil**  *\<verbo >* **-?**
- Para enviar la sintaxis de certutil en un archivo de texto, ejecute los siguientes comandos:  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

En la tabla siguiente se describe la notación que se utiliza para indicar la sintaxis de línea de comandos.


|            Notación             |                  Descripción                  |
|---------------------------------|-----------------------------------------------|
| Texto sin corchetes ni llaves |         Elementos que se debe escribir como se muestra          |
|  \<Texto dentro de corchetes angulares >  | Marcador de posición para el que debe proporcionar un valor |
|  [Texto entre corchetes]  |                Elementos opcionales                 |
|      {Texto entre llaves}       |       Conjunto de elementos necesarios; Elija una       |
|         Vertical de la barra ()          |                       ) simple                       |
|          Puntos suspensivos (...)           |          Elementos que se pueden repetir           |

Vuelva a [menú](#menu)

## <a name="-dump"></a>-volcado de memoria

CertUtil [Options] [-dump]

CertUtil [opciones] [-volcado] archivo

Información de configuración o archivos de volcado de memoria

[-f]. [-silenciosa] [-dividir] [-p contraseña] [-t tiempo de espera]

Vuelva a [menú](#menu)

## <a name="-asn"></a>-asn

CertUtil [opciones] - asn archivo [tipo]

Analizar el archivo ASN.1

tipo: numérico CRYPT\_cadena\_ \* descodificación de tipo

Vuelva a [menú](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [opciones] - decodehex InFile OutFile [tipo]

tipo: numérico CRYPT\_cadena\_ \* tipo de codificación

[-f]

Vuelva a [menú](#menu)

## <a name="-decode"></a>-descodificar

CertUtil [opciones] - descodificar InFile OutFile

Descodificación de archivo codificado en Base64

[-f]

Vuelva a [menú](#menu)

## <a name="-encode"></a>-codificar

CertUtil [opciones] - codificar InFile OutFile

Codificar el archivo en Base64

[-f] [-UnicodeText]

Vuelva a [menú](#menu)

## <a name="-deny"></a>-denegar

CertUtil [opciones] - Denegar RequestId

Denegar solicitud pendiente

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-resubmit"></a>-volver a enviar

CertUtil [opciones] - volver a enviar Id. de solicitud

Volver a enviar la solicitud pendiente

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-setattributes"></a>-setattributes

CertUtil [opciones] - setattributes RequestId cadenaAtributo

Definir atributos para la solicitud pendiente

Id. de solicitud--numérico Id. de solicitud de solicitud pendiente

AttributeString--Solicitar los pares de nombre y valor de atributo

- Los nombres y valores están separados con puntos.
- Nombre de varios pares de valor son separado de nueva línea.
- Ejemplo: "CertificateTemplate:User\nEMail:User@Domain.com"
- Cada secuencia "\n" se convierte en un separador de línea nueva.

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-setextension"></a>-setextension

CertUtil [opciones] - setextension marcas RequestId ExtensionName {Long | Fecha | Cadena | @InFile}

Extensión del conjunto de solicitud pendiente

RequestId: identificador de solicitud numérico de una solicitud pendiente

ExtensionName--Cadena ObjectId de la extensión

Marcadores: se recomienda 0.  1 hace que la extensión crítica, lo deshabilita 2, 3 lo hará.

Si el último parámetro es numérico, se considera como un valor largo.

Si se puede analizar como una fecha, se considera como una fecha.

Si empieza con ' @', el resto del token es el nombre de archivo que contiene datos binarios o un volcado hexadecimal de texto ascii.

Nada se toma como una cadena.

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-revoke"></a>-revocar

CertUtil [Options] -revoke SerialNumber [Reason]

Revocar certificado

SerialNumber: Lista de números de serie del certificado para revocar separados por comas

Motivo: motivo de revocación numérico o simbólico

- 0: CRL_REASON_UNSPECIFIED: No se especifica (valor predeterminado)
- 1: CRL_REASON_KEY_COMPROMISE: Compromiso de clave
- 2: CRL_REASON_CA_COMPROMISE: CA comprometida
- 3: CRL_REASON_AFFILIATION_CHANGED: Afiliación cambiada
- 4: CRL_REASON_SUPERSEDED: Reemplazada
- 5: CRL_REASON_CESSATION_OF_OPERATION: Cese de operación
- 6: CRL_REASON_CERTIFICATE_HOLD: Certificado retenido
- 8: CRL_REASON_REMOVE_FROM_CRL: Quitar de CRL
- -1: Anular la revocación: Anular la revocación

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-isvalid"></a>-isvalid

CertUtil [opciones] - isvalid SerialNumber | CertHash

Disposición del certificado actual para mostrar

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-getconfig"></a>-getconfig

CertUtil [Options] -getconfig

Obtener la cadena de configuración predeterminada

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-ping"></a>-ping

CertUtil [opciones] - ping [MaxSecondsToWait | CAMachineList]

Active Directory Certificate Services interfaz ping de solicitud

CAMachineList--Lista de nombres de máquina CA separados por comas

1. Para una sola máquina, use una coma final
2. Muestra el coste de sitio para cada máquina de la entidad de certificación

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-cainfo"></a>-CAInfo

CertUtil [Options] -CAInfo [InfoName [Index | ErrorCode]]

Mostrar información de CA

InfoName--Indica la propiedad de entidad de certificación para mostrar (ver abajo). Use "\*" para todas las propiedades.

Índice: índice de propiedad opcional de base cero

Código de error: código de error numérico

[-f] [-split] [-config Machine\CAName]

Sintaxis del argumento NombreInfo:

- Archivo: Versión de archivo
- producto: Versión del producto
- exitcount: Recuento de módulos de salida
- salida [Index]: Descripción del módulo de salida
- Directiva: Descripción del módulo de directivas
- Nombre: Nombre de entidad de certificación
- sanitizedname: Nombre de entidad de certificación limpio
- dsname: Nombre corto saneado de CA (nombre DS)
- sharedfolder: Carpeta compartida
- ErrorCode error1: Texto del mensaje de error
- ErrorCode Error2: Texto del mensaje de error y el código de error
- type: Tipo de entidad de certificación
- info: Información de entidad emisora de certificados
- Primario: Parent CA
- certcount: Recuento de certificados de CA
- xchgcount: Recuento de certificados de intercambio de CA
- kracount: Recuento de certificados de KRA
- kraused: Número de certificados KRA usados
- propidmax: Maximum CA PropId
- certstate [Index]: Certificado de CA
- certversion [Index]: Versión de certificado de CA
- certstatuscode [Index]: Estado de comprobación de certificado de CA
- crlstate [Index]: CRL
- krastate [Index]: Certificado KRA
- crossstate + [Index]: Certificado cruzado directo
- crossstate- [Index]: Certificado cruzado con versiones anteriores
- CERT [Index]: Certificado de CA
- certchain [Index]: Cadena de certificados de CA
- certcrlchain [Index]: Cadena de certificados de CA con CRL
- xchg [Index]: Certificado de intercambio de CA
- xchgchain [Index]: Cadena de certificados de intercambio de CA
- xchgcrlchain [Index]: Cadena de certificados de intercambio de CA con CRL
- kra [Index]: Certificado KRA
- entre + [Index]: Certificado cruzado directo
- Cross-[Index]: Certificado cruzado con versiones anteriores
- CRL [Index]: CRL base
- deltacrl [Index]: Delta CRL
- crlstatus [Index]: Estado de la publicación de CRL
- deltacrlstatus [Index]: Estado de la publicación de diferencias CRL
- DNS: Nombre DNS
- Rol: Separación de roles
- anuncios: Advanced Server
- Plantillas: Plantillas
- csp [Index]: Direcciones URL OCSP
- aia [Index]: Direcciones URL AIA
- CDP [Index]: Direcciones URL CDP
- localename: Nombre de configuración regional de la entidad de certificación
- subjecttemplateoids: OID de la plantilla de asunto

Vuelva a [menú](#menu)

## <a name="-cacert"></a>-ca.cert

CertUtil [Options] -ca.cert OutCACertFile [Index]

Recuperar el certificado de CA

OutCACertFile: archivo de salida

Índice: Índice de renovación de certificado de CA (valor predeterminado es más reciente)

[-f] [-split] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-cachain"></a>-ca.chain

CertUtil [opciones] - ca.chain OutCACertChainFile [índice]

Recuperar la cadena de certificados de la entidad de certificación

OutCACertChainFile: archivo de salida

Índice: Índice de renovación de certificado de CA (valor predeterminado es más reciente)

[-f] [-split] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [Options] -GetCRL OutFile [Index] [delta]

Get CRL

Índice: CRL índice o clave de índice (valor predeterminado es CRL para la clave más reciente)

Delta: diferencias CRL (valor predeterminado es CRL base)

[-f] [-split] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-crl"></a>-CRL

CertUtil [Options] -CRL [dd:hh | republish] [delta]

Publicar nueva CRL [o diferencias CRL sólo]

hh--nuevo período de validez CRL en días y horas

volver a publicar: volver a publicar la CRL más reciente

Delta--diferencias CRL sólo (valor predeterminado es CRL base y delta)

[-split] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-shutdown"></a>-Cierre

CertUtil [Options] -shutdown

Servicios de certificados de Active Directory de apagado

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [Options] -installCert [CACertFile]

Instalar el certificado de entidad de certificación

[-f] [-silent] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [Options] -renewCert [ReuseKeys] [Machine\ParentCAName]

Renovar un certificado de entidad de certificación

Use -f para omitir una solicitud de renovación en espera y generar una nueva solicitud.

[-f] [-silent] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-schema"></a>-schema

CertUtil [Options] -schema [Ext | Attrib | CRL]

Esquema de volcado de certificado

Valores predeterminados de tabla de la solicitud y el certificado

Ext: Tabla de extensión

Attrib: Tabla de atributos

CRL: Tabla CRL

[-split] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-view"></a>-vista

CertUtil [opciones] - vista [cola | Registro | LogFail | Revocar | Ext | Attrib | CRL] [csv]

Volcar vista de certificado

Cola: Cola de solicitudes

Registro: Los certificados emitidos o revocados, además de las solicitudes con error

LogFail: Solicitudes con error

Para revocar: Certificados revocados

Ext: Tabla de extensión

Attrib: Tabla de atributos

CRL: Tabla CRL

csv: Salida de valores separados por comas

Para mostrar la columna StatusCode para todas las entradas:-out StatusCode

Para mostrar todas las columnas de la última entrada:-restringir "RequestId == $"

Para mostrar Id. de solicitud y la disposición de tres solicitudes:-restringir "RequestId > = 37, RequestId\<40"-out "Id. de solicitud, la disposición"

Para mostrar números de CRL y los identificadores de fila para todas las CRL de Base:-restringir "CRLMinBase = 0"-out "CRLRowId, CRLNumber" CRL

Para mostrar el número 3 de Base CRL: - v-restringir "CRLMinBase = 0, CRLNumber = 3"-out "CRLRawCRL" CRL

Para mostrar toda la tabla de la CRL: CRL

Use "fecha [+ |-hh]" para las restricciones de fecha

Use "ahora + hh" para una fecha relativa a la hora actual

[-silenciosa] [-dividir] [-config Machine\CAName] [-restringir RestrictionList] [-out ColumnList]

Vuelva a [menú](#menu)

## <a name="-db"></a>-db

CertUtil [Options] -db

Base de datos sin procesar de volcado de memoria

[-config Machine\CAName] [-restringir RestrictionList] [-out ColumnList]

Vuelva a [menú](#menu)

## <a name="-deleterow"></a>-deleterow

CertUtil [opciones] - deleterow RowId | Fecha [solicitar | CERT | Ext | Attrib | CRL]

Eliminar fila de la base de datos de servidor

La solicitud: No se pudo y (fecha de envío) de solicitudes pendientes

Cert: Certificados caducados y revocados (fecha de expiración)

Ext: Tabla de extensión

Attrib: Tabla de atributos

CRL: Tabla CRL (fecha de expiración)

Para eliminar con errores y solicitudes pendientes enviados por el 22 de enero de 2001: 22/1/2001 solicitud

Para eliminar todos los certificados que ha expirado el 22 de enero de 2001: 22/1/2001 cert

Para eliminar la fila del certificado, atributos y extensiones para RequestId 37: 37

Para eliminar las CRL que expiró el 22 de enero de 2001: 1/22/2001 CRL

[-f] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-backup"></a>-copia de seguridad

CertUtil [Options] -backup BackupDirectory [Incremental] [KeepLog]

Servicios de certificados de copia de seguridad de Active Directory

DirectorioCopiaSeguridad: directorio para almacenar datos copia de seguridad

Incremental: realizar copias de seguridad incrementales solo (copia de seguridad completa de forma predeterminada)

GuardarRegistro: conservar los archivos de registro de base de datos (valor predeterminado es truncar los archivos de registro)

[-f] [-config Machine\CAName] [-p Password]

Vuelva a [menú](#menu)

## <a name="-backupdb"></a>-backupDB

CertUtil [Options] -backupDB BackupDirectory [Incremental] [KeepLog]

Copia de seguridad base de datos de servicios de certificados de Active Directory

DirectorioCopiaSeguridad: directorio para almacenar una copia de seguridad archivos de base de datos

Incremental: realizar copias de seguridad incrementales solo (copia de seguridad completa de forma predeterminada)

GuardarRegistro: conservar los archivos de registro de base de datos (valor predeterminado es truncar los archivos de registro)

[-f] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [opciones] - backupKey BackupDirectory

Certificado de Active Directory Certificate Services copia de seguridad y la clave privada

DirectorioCopiaSeguridad: directorio para almacenar una copia de seguridad archivo PFX

[-f]. [-config Machine\CAName] [-p contraseña] [-t tiempo de espera]

Vuelva a [menú](#menu)

## <a name="-restore"></a>-restore

CertUtil [opciones] - restore BackupDirectory

Restauración de servicios de certificados de Active Directory

BackupDirectory: directorio que contiene los datos que se restaurarán

[-f] [-config Machine\CAName] [-p Password]

Vuelva a [menú](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [opciones] - restoreDB BackupDirectory

Restaurar base de datos de servicios de certificados de Active Directory

BackupDirectory: directorio que contiene los archivos de base de datos que se va a restaurar

[-f] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [opciones] - restoreKey BackupDirectory | ArchivoPFX

Restauración de servicios de certificados de Active Directory certificado y clave privada

BackupDirectory: directorio que contiene el archivo PFX que se puede restaurar

ArchivoPFX: Archivo PFX que se puede restaurar

[-f] [-config Machine\CAName] [-p Password]

Vuelva a [menú](#menu)

## <a name="-importpfx"></a>-importPFX

CertUtil [opciones] - importPFX [CertificateStoreName] ArchivoPFX [modificadores]

Importar certificado y clave privada

CertificateStoreName: Nombre del almacén de certificados.  Consulte [-almacenar](#-store).

ArchivoPFX: Archivo PFX que desea importar

Modificadores: Lista separada por comas de uno o varios de los siguientes:

1. AT_SIGNATURE: Cambio de la especificación de clave de firma
2. AT_KEYEXCHANGE: Cambiar la especificación de clave para el intercambio de claves
3. NoExport: Que no se puede exportar la clave privada
4. NoCert: No se importa el certificado
5. NoChain: No importe la cadena de certificados
6. NoRoot: Importe el certificado raíz
7. Proteger: Proteger las claves con contraseña
8. NoProtect: Contraseña no proteger las claves

Valores predeterminados al almacén personal del equipo.

[-f]. [-usuario] [-p contraseña] [-csp proveedor]

Vuelva a [menú](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [Options] -dynamicfilelist

Mostrar lista de archivos dinámicos

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [Options] -databaselocations

Mostrar las ubicaciones de la base de datos

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [opciones] - hashfile InFile [HashAlgorithm]

Generar y mostrar hash de cifrado a través de un archivo

Vuelva a [menú](#menu)

## <a name="-store"></a>-store

CertUtil [Options] -store [CertificateStoreName [CertId [OutputFile]]]

Almacén de certificados de volcado de memoria

CertificateStoreName: Nombre del almacén de certificados. Ejemplos:

- "My," "CA" (valor predeterminado), "Raíz",
- "ldap: / / / CN = entidades emisoras de certificados, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? uno? objectClass = certificationAuthority" (ver los certificados raíz)
- "ldap: / / / CN = nombreCA, CN = entidades emisoras de certificados, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? base? objectClass = certificationAuthority" (modificar los certificados raíz)
- "ldap: / / / CN = nombreCA, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (vista CRL)
- "ldap: / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? base? objectClass = certificationAuthority" (certificados de CA de empresa)
- ldap: (Certificados de objeto de equipo de AD)
- -usuario ldap: (Certificados de objeto de usuario de AD)

CertId: El certificado o el token de coincidencia CRL.  Esto puede ser un número de serie, un SHA-1 certificado, CRL, CTL o hash de clave pública, un índice numérico cert (0, 1 y así sucesivamente), un índice numérico de CRL (. 0,.1 y así sucesivamente), un índice CTL numérico (.. 0... 1 y así sucesivamente), una clave pública, firma o extensión ObjectId, un asunto de certificado nombre común, una dirección de correo electrónico, UPN o el nombre DNS, un nombre de contenedor de claves o nombre CSP, un nombre de plantilla o ObjectId, un EKU o ObjectId de las directivas de aplicación o un nombre común del emisor de CRL. Muchas de ellas pueden dar lugar a varias coincidencias.

OutputFile: archivo para guardar el certificado coincidente

Use - usuario tenga acceso a un almacén de usuario en lugar de un almacén de equipo.

Use - enterprise para tener acceso a un almacén de enterprise del equipo.

Use - service para tener acceso a un almacén del servicio de equipo.

Use - grouppolicy para tener acceso a un almacén de directivas de grupo de equipo.

Ejemplos:

- -enterprise NTAuth
- -enterprise Root 37
- -usuario Mi 26e0aaaf000000000004
- CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-addstore"></a>-addstore

CertUtil [opciones] - addstore CertificateStoreName InFile

Agregar el certificado al almacén

CertificateStoreName: Nombre del almacén de certificados.  Consulte [-almacenar](#-store).

InFile: Archivo de certificado o CRL a agregar al almacén.

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [Options] -delstore CertificateStoreName CertId

Eliminar certificado del almacén

CertificateStoreName: Nombre del almacén de certificados.  Consulte [-almacenar](#-store).

CertId: El certificado o el token de coincidencia CRL.  Consulte [-almacenar](#-store).

[-enterprise] [-usuario] [-GroupPolicy] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [Options] -verifystore CertificateStoreName [CertId]

Comprobar el certificado en almacén

CertificateStoreName: Nombre del almacén de certificados.  Consulte [-almacenar](#-store).

CertId: El certificado o el token de coincidencia CRL.  Consulte [-almacenar](#-store).

[-enterprise] [-usuario] [-GroupPolicy] [-silenciosa] [-dividir] [-dc DCName] [-t tiempo de espera]

Vuelva a [menú](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [Options] -repairstore CertificateStoreName CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

Asociación de clave de reparar o actualizar el descriptor de seguridad de las propiedades o la clave de certificado

CertificateStoreName: Nombre del almacén de certificados.  Consulte [-almacenar](#-store).

CertIdList: lista separada por comas de los tokens de coincidencia de certificado o CRL. Consulte [-almacenar](#-store) CertId descripción.

PropertyInfFile--Archivo INF que contiene propiedades externos:

```
[Properties]
     19 = Empty ; Add archived property, OR:
     19 =       ; Remove archived property

     11 = "{text}Friendly Name" ; Add friendly name property

     127 = "{hex}" ; Add custom hexadecimal property
         _continue_ = "00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f"
         _continue_ = "10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f"

     2 = "{text}" ; Add Key Provider Information property
       _continue_ = "Container=Container Name&"
       _continue_ = "Provider=Microsoft Strong Cryptographic Provider&"
       _continue_ = "ProviderType=1&"
       _continue_ = "Flags=0&"
       _continue_ = "KeySpec=2"

     9 = "{text}" ; Add Enhanced Key Usage property
       _continue_ = "1.3.6.1.5.5.7.3.2,"
       _continue_ = "1.3.6.1.5.5.7.3.1,"
```

[-f]. [-enterprise] [-usuario] [-GroupPolicy] [-silenciosa] [-dividir] [-csp proveedor]

Vuelva a [menú](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [Options] -viewstore [CertificateStoreName [CertId [OutputFile]]]

Almacén de certificados de volcado de memoria

CertificateStoreName: Nombre del almacén de certificados. Ejemplos:

- "My," "CA" (valor predeterminado), "Raíz",
- "ldap: / / / CN = entidades emisoras de certificados, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? uno? objectClass = certificationAuthority" (ver los certificados raíz)
- "ldap: / / / CN = nombreCA, CN = entidades emisoras de certificados, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? base? objectClass = certificationAuthority" (modificar los certificados raíz)
- "ldap: / / / CN = nombreCA, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (vista CRL)
- "ldap: / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? base? objectClass = certificationAuthority" (certificados de CA de empresa)
- ldap: (Certificados de objeto de equipo de AD)
- -usuario ldap: (Certificados de objeto de usuario de AD)

CertId: El certificado o el token de coincidencia CRL. Esto puede ser un número de serie, un SHA-1 certificado, CRL, CTL o hash de clave pública, un índice numérico cert (0, 1 y así sucesivamente), un índice numérico de CRL (. 0,.1 y así sucesivamente), un índice CTL numérico (.. 0... 1 y así sucesivamente), una clave pública, firma o extensión ObjectId, un asunto de certificado nombre común, una dirección de correo electrónico, UPN o el nombre DNS, un nombre de contenedor de claves o nombre CSP, un nombre de plantilla o ObjectId, un EKU o ObjectId de las directivas de aplicación o un nombre común del emisor de CRL. Muchas de ellas pueden dar lugar a varias coincidencias.

OutputFile: archivo para guardar el certificado coincidente

Use - usuario tenga acceso a un almacén de usuario en lugar de un almacén de equipo.

Use - enterprise para tener acceso a un almacén de enterprise del equipo.

Use - service para tener acceso a un almacén del servicio de equipo.

Use - grouppolicy para tener acceso a un almacén de directivas de grupo de equipo.

Ejemplos:

1. -enterprise NTAuth
2. -enterprise Root 37
3. -usuario Mi 26e0aaaf000000000004
4. CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [Options] -viewdelstore [CertificateStoreName [CertId [OutputFile]]]

Eliminar certificado del almacén

CertificateStoreName: Nombre del almacén de certificados. Ejemplos:

- "My," "CA" (valor predeterminado), "Raíz",
- "ldap: / / / CN = entidades emisoras de certificados, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? uno? objectClass = certificationAuthority" (ver los certificados raíz)
- "ldap: / / / CN = nombreCA, CN = entidades emisoras de certificados, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? base? objectClass = certificationAuthority" (modificar los certificados raíz)
- "ldap: / / / CN = nombreCA, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (vista CRL)
- "ldap: / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? el certificado de CA? base? objectClass = certificationAuthority" (certificados de CA de empresa)
- ldap: (Certificados de objeto de equipo de AD)
- -usuario ldap: (Certificados de objeto de usuario de AD)

CertId: El certificado o el token de coincidencia CRL. Esto puede ser un número de serie, un SHA-1 certificado, CRL, CTL o hash de clave pública, un índice numérico cert (0, 1 y así sucesivamente), un índice numérico de CRL (. 0,.1 y así sucesivamente), un índice CTL numérico (.. 0... 1 y así sucesivamente), una clave pública, firma o extensión ObjectId, un asunto de certificado nombre común, una dirección de correo electrónico, UPN o el nombre DNS, un nombre de contenedor de claves o nombre CSP, un nombre de plantilla o ObjectId, un EKU o ObjectId de las directivas de aplicación o un nombre común del emisor de CRL. Muchas de ellas pueden dar lugar a varias coincidencias.

OutputFile: archivo para guardar el certificado coincidente

Use - usuario tenga acceso a un almacén de usuario en lugar de un almacén de equipo.

Use - enterprise para tener acceso a un almacén de enterprise del equipo.

Use - service para tener acceso a un almacén del servicio de equipo.

Use - grouppolicy para tener acceso a un almacén de directivas de grupo de equipo.

Ejemplos:

1. -enterprise NTAuth
2. -enterprise Root 37
3. -usuario Mi 26e0aaaf000000000004
4. CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-dspublish"></a>-dsPublish

CertUtil [opciones] - dsPublish CertFile [NTAuthCA | RootCA | Entidad de certificación subordinada | CrossCA | KRA | Usuario | Máquina]

CertUtil [opciones] - dsPublish ArchivoCRL [contenedorDSCDP [DSCDPCN]]

Publicar certificado o CRL en Active Directory

CertFile: archivo de certificado para publicar

NTAuthCA: Publicar certificado en el almacén de DS Enterprise

RootCA: Publicar certificado en el almacén raíz de confianza de DS

SubCA: Publicar el certificado de CA en el objeto de entidad de certificación de DS

CrossCA: Publicar cross cert al objeto de entidad de certificación de DS

KRA: Publicar certificado en el objeto de DS Key Recovery Agent

Usuario: Publicar certificado en el objeto de DS del usuario

Equipo: Publicar certificado en el objeto de máquina DS

CRLFile: Archivo CRL para publicar

ContenedorDSCDP: Contenedor CDP DS CN, normalmente el nombre del equipo de CA

DSCDPCN: DS CDP objeto CN, generalmente según el nombre corto saneado de CA y el índice de clave

Utilice -f para crear el objeto de DS.

[-f] [-user] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [Options] -ADTemplate [Template]

Mostrar las plantillas de AD

[-f] [-user] [-ut] [-mt] [-dc DCName]

## <a name="-template"></a>-Template

CertUtil [Options] -Template [Template]

Mostrar las plantillas de directiva de inscripción

[-f] [-user] [-silent] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Vuelva a [menú](#menu)

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [opciones] - TemplateCAs plantilla

Entidades de certificación para mostrar de plantilla

[-f] [-user] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-catemplates"></a>-CATemplates

CertUtil [Options] -CATemplates [Template]

Mostrar las plantillas de CA

[-f] [-user] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [Options] -SetCASites [set] [SiteName]

CertUtil [Options] -SetCASites verify [SiteName]

CertUtil [Options] -SetCASites delete

Nombres de sitio de conjunto, compruebe o eliminar la entidad de certificación

- Use la opción - config como destino una sola entidad de certificación (valor predeterminado es todas las CA)
- *SiteName* se permite solo cuando el destino es una única entidad de certificación
- Use -f para invalidar los errores de validación para el elemento especificado *SiteName*
- Utilice -f para eliminar todos los nombres de sitio de CA

[-f] [-config Machine\CAName] [-dc DCName]

> [!NOTE]
> Para obtener más información sobre cómo configurar las entidades de certificación para el reconocimiento de sitios de Active Directory Domain Services (AD DS), consulte [reconocimiento de sitios de AD DS para AD CS y clientes PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

Vuelva a [menú](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [Options] -enrollmentServerURL [URL AuthenticationType [Priority] [Modifiers]]

Eliminación de dirección URL de CertUtil [opciones] - enrollmentServerURL

Mostrar, agregar o eliminar direcciones URL del servidor de inscripción asociadas con una entidad de certificación

AuthenticationType: Especifique uno de los siguientes métodos de autenticación de cliente al agregar una dirección URL

1. Kerberos: Usar credenciales Kerberos SSL
2. Nombre de usuario: Usar cuenta con las credenciales SSL
3. ClientCertificate: Use las credenciales de un certificado SSL X.509
4. Anonymous: Utilice credenciales anónimas de SSL

Delete: elimina la dirección URL especificada asociada con la entidad de certificación

Prioridad: el valor predeterminado es '1' Si no se especifica al agregar una dirección URL

Los modificadores--Lista separada por comas de uno o varios de los siguientes:

1. AllowRenewalsOnly: Solo las solicitudes de renovación se pueden enviar a esta CA a través de esta dirección URL
2. AllowKeyBasedRenewal: Permite el uso de un certificado que se dispone de ninguna cuenta asociada en Active Directory. Esto se aplica sólo con ClientCertificate y el modo de AllowRenewalsOnly

[-config Machine\CAName] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [Options] -ADCA [CAName]

Mostrar las entidades de certificación de AD

[-f]. [-dividir] [-dc DCName]

Vuelva a [menú](#menu)

## <a name="-ca"></a>-CA

CertUtil [Options] -CA [CAName | TemplateName]

Mostrar las entidades de certificación de directivas de inscripción

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Vuelva a [menú](#menu)

## <a name="-policy"></a>-Policy

Mostrar la directiva de inscripción

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Vuelva a [menú](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [Options] -PolicyCache [delete]

Mostrar o eliminar las entradas de caché de directiva de inscripción

eliminar: eliminar las entradas de caché del servidor de directivas

-f: use -f para eliminar todas las entradas de caché

[-f] [-user] [-PolicyServer URLOrId]

Vuelva a [menú](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [Options] -CredStore [URL]

Agregar dirección URL de CertUtil [opciones] - CredStore

CertUtil [opciones] - CredStore URL eliminar

Mostrar, agregar o eliminar las entradas de la credencial Store

Dirección URL: dirección URL de destino.  Use \* para que coincida con todas las entradas. Use https://machine\* para que coincida con un prefijo de dirección URL.

agregar: agregar una entrada de credencial Store. También se deben especificar las credenciales SSL.

eliminar: eliminar las entradas de la credencial Store

-f: utilice -f para sobrescribir una entrada o para eliminar varias entradas.

[-f] [-user] [-silent] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Vuelva a [menú](#menu)

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [opciones] - InstallDefaultTemplates

Instalar plantillas de certificado predeterminadas

[-dc DCName]

Vuelva a [menú](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [Options] -URLCache [URL | CRL | \* [delete]]

Mostrar o eliminar las entradas de caché de dirección URL

Dirección URL: dirección URL almacenada en caché

CRL: funcionan en todos los almacenados en caché direcciones URL de CRL solo

\*: operará en caché todas las direcciones URL de

eliminar: eliminar direcciones URL pertinentes de la caché local del usuario actual

Utilice -f para forzar la obtención de una dirección URL específica y actualizar la memoria caché.

[-f] [-split]

Vuelva a [menú](#menu)

## <a name="-pulse"></a>-pulse

CertUtil [Options] -pulse

Eventos de la inscripción automática de impulsos

[-usuario]

Vuelva a [menú](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [Options] -MachineInfo DomainName\MachineName$

Mostrar información de objetos de equipo de Active Directory

Vuelva a [menú](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [Options] -DCInfo [Domain] [Verify | DeleteBad | DeleteAll]

Mostrar información de controlador de dominio

Opción predeterminada es mostrar los certificados de controlador de dominio sin comprobar

[-f]. [-usuario] [-urlfetch] [-dc DCName] [-t tiempo de espera]

> [!TIP]
> La capacidad de especificar un dominio de Active Directory Domain Services (AD DS) **[dominio]** y especificar un controlador de dominio ( **-dc**) se agregó en Windows Server 2012. Para ejecutar correctamente el comando, debe usar una cuenta que sea miembro del **Admins. del dominio** o **administradores de empresas**. Las modificaciones de comportamiento de este comando son los siguientes:</br>> 1.  Si no se especifica un dominio y no se especifica un controlador de dominio específico, esta opción devuelve una lista de controladores de dominio para procesar desde el controlador de dominio predeterminado.</br>> 2.  Si no se especifica un dominio, pero se especifica un controlador de dominio, se genera un informe de los certificados en el controlador de dominio especificado.</br>> 3.  Si se especifica un dominio, pero no se especifica un controlador de dominio, se genera una lista de controladores de dominio junto con los informes de los certificados para cada controlador de dominio en la lista.</br>> 4.  Si se especifican el dominio y el controlador de dominio, se genera una lista de controladores de dominio desde el controlador de dominio de destino. También se genera un informe de los certificados de cada controlador de dominio en la lista.

Por ejemplo, suponga que hay un dominio denominado CPANDL con un controlador de dominio denominado DC1 de CPANDL. Puede ejecutar el siguiente comando para recuperar una lista de controladores de dominio y sus certificados que de CPANDL DC1: certutil -dc-dc1 cpandl - dcinfo cpandl

Vuelva a [menú](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [Options] -EntInfo DomainName\MachineName$

[-f] [-user]

Vuelva a [menú](#menu)

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [Options] -TCAInfo [DomainDN | -]

Mostrar información de CA

[-f]. [-enterprise] [-usuario] [-urlfetch] [-dc DCName] [-t tiempo de espera]

Vuelva a [menú](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [Options] -SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

Mostrar información de tarjeta inteligente

CRYPT_DELETEKEYSET: Eliminar todas las claves en la tarjeta inteligente

[-silenciosa] [-dividir] [-urlfetch] [-t tiempo de espera]

Vuelva a [menú](#menu)

## <a name="-scroots"></a>-SCRoots

CertUtil [Options] -SCRoots update [+][InputRootFile] [ReaderName]

CertUtil [Options] -SCRoots save @OutputRootFile [ReaderName]

CertUtil [Options] -SCRoots view [InputRootFile | ReaderName]

CertUtil [Options] -SCRoots delete [ReaderName]

Administrar certificados raíz de tarjeta inteligente

[-f]. [-dividir] [-p contraseña]

Vuelva a [menú](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [Options] -verifykeys [KeyContainerName CACertFile]

Compruebe el conjunto de claves pública y privada

KeyContainerName: el nombre de contenedor de claves de la clave para comprobar. Valores predeterminados para las claves del equipo.  Use - usuario para las claves de usuario.

ArchivoCertificadoCA: archivo de certificado de firma o cifrado

Si no se especifica ningún argumento, se comprueba cada certificado de entidad emisora de certificados de firma con su clave privada.

Esta operación solo puede realizarse en una entidad emisora de certificados local o de claves locales.

[-f] [-user] [-silent] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-verify"></a>-comprobar

CertUtil [Options] -verify CertFile [ApplicationPolicyList | - [IssuancePolicyList]]

CertUtil [Options] -verify CertFile [CACertFile [CrossedCACertFile]]

CertUtil [Options] -verify CRLFile CACertFile [IssuedCertFile]

CertUtil [Options] -verify CRLFile CACertFile [DeltaCRLFile]

Comprobar el certificado o CRL, la cadena

CertFile: Certificado para comprobar

ListaDirectivasAplicación: coma opcional separada por lista de ObjectID de directiva de aplicación necesaria

ListaDirectivasEmisión: coma opcional separada por lista de ObjectID de directiva de emisión necesarios

ArchivoCertificadoCA: opcional certificado de CA emisora va a comprobar

ArchivoCertCACruzado: los certificados opcional certificados cruzados por CertFile

CRLFile: CRL para comprobar

: Certificado emitido opcional cubierto por ArchivoCRL

CrossedCACertFile: opcional diferencias CRL

Si se especifica ListaDirectivasAplicación, compilar la cadena está restringido a las cadenas válidas para las directivas de aplicación especificado.

Si se especifica ListaDirectivasEmisión, compilar la cadena está restringido a cadenas válidas para las directivas de emisión especificadas.

Si se especifica CACertFile, los campos de CACertFile se comprueban con CertFile o ArchivoCRL.

Si no se especifica CACertFile, CertFile se utiliza para crear y comprobar una cadena completa.

Si se especifican archivoCertificadoCA y CACertFile, los campos de CACertFile se comprueban con CertFile.

Si no se especifica, se comprueban los campos de entera.

Si se especifica CrossedCACertFile, se comprueban los campos de CrossedCACertFile entera.

[-f]. [-enterprise] [-usuario] [-silenciosa] [-dividir] [-urlfetch] [-t tiempo de espera]

Vuelva a [menú](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [Options] -verifyCTL CTLObject [CertDir] [CertFile]

Comprobar AuthRoot o CTL de certificados no permitidos

CTLObject: Identifica la CTL para comprobar:

- AuthRootWU: lea AuthRoot CAB y certificados coincidentes de la memoria caché la dirección URL. Descargar desde Windows Update en su lugar, utilice -f.
- DisallowedWU: leer el archivo CAB de certificados no permitidos y no permite el archivo de almacén de certificados de la memoria caché la dirección URL.  Descargar desde Windows Update en su lugar, utilice -f.
- AuthRoot: lectura de registro en la memoria caché AuthRoot CTL.  Usar con -f y un CertFile que no sea de confianza ya para forzar la actualización del registro almacenado en caché AuthRoot y las CTL de certificados no permitidos.
- No permitido: lectura de registro almacenado en caché la CTL de certificados no permitidos. -f tiene el mismo comportamiento como AuthRoot.
- CTLFileName: archivo o http: ruta de acceso a la lista CTL o CAB

Certdir '.: carpeta que contiene los certificados que coinciden con las entradas de la CTL. Http: ruta de acceso de carpeta debe terminar con un separador de ruta de acceso. Si no se especifica una carpeta con AuthRoot o no permitido, se buscará en varias ubicaciones para la coincidencia de certificados: almacenes de certificados local, crypt32.dll recursos y la caché local de la dirección URL. Use -f para descargar desde Windows Update cuando sea necesario. En caso contrario, valor predeterminado es la misma carpeta o sitio web como el CTLObject.

CertFile: archivo que contiene los certificados que se van a comprobar. Los certificados se comparará con las entradas de la CTL y coinciden con los resultados que aparecen. Suprime la mayor parte de la salida predeterminada.

[-f] [-user] [-split]

Vuelva a [menú](#menu)

## <a name="-sign"></a>-inicio de sesión

CertUtil [Options] -sign InFileList|SerialNumber|CRL OutFileList [StartDate+dd:hh] [+SerialNumberList | -SerialNumberList | -ObjectIdList | @ExtensionFile]

CertUtil [opciones] - firmar InFileList | SerialNumber | CRL OutFileList [#HashAlgorithm] [+ AlternateSignatureAlgorithm | - AlternateSignatureAlgorithm]

Volver a firmar el certificado o CRL

InFileList: lista de archivos de certificado o CRL para modificar y volver a firmar separados por comas

SerialNumber: Número de serie del certificado para crear. Período de validez y otras opciones no deben estar presentes.

CRL: Creación de una CRL vacía. Período de validez y otras opciones no deben estar presentes.

OutFileList: lista separada por comas de los archivos de salida de certificado o CRL modificados. El número de archivos debe coincidir con InFileList.

StartDate + hh: nuevo período de validez: fecha opcional más; días opcionales y el período de validez de horas; Si se especifican ambos, utilice un separador de signo más (+). Use "ahora [+ hh]" al principio de la hora actual. Use "nunca" para no que ninguna fecha de caducidad (para las CRL sólo).

SerialNumber: número de serie de lista para agregar o quitar separados por comas

ObjectIdList: lista de ObjectId de extensión separada por comas para quitar

@ExtensionFile: Que contiene las extensiones para actualizar o eliminar el archivo de INF:

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm: Nombre del algoritmo hash precedido por un signo #

AlternateSignatureAlgorithm: especificador de algoritmo de firma alternativo

Hace que un signo menos números de serie y las extensiones que se va a quitar. Un signo hace que los números de serie que se agregarán a una CRL. Al quitar elementos de una CRL, la lista puede contener números de serie y outfile. Un signo menos antes AlternateSignatureAlgorithm hace que el formato de firma heredado que se usará. Un signo más antes AlternateSignatureAlgorithm hace que el formato de firma alternature para usarse. Si no se especifica AlternateSignatureAlgorithm, a continuación, se usa el formato de firma en el certificado o CRL.

[-nullsign] [-f] [-silent] [-Cert CertId]

Vuelva a [menú](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [Options] -vroot [delete]

Crear y eliminar raíces virtuales web y recursos compartidos de archivos

Vuelva a [menú](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [Options] -vocsproot [delete]

Crear y eliminar raíces virtuales de web para el proxy web OCSP

Vuelva a [menú](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [Options] -addEnrollmentServer Kerberos | UserName | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

Agregar una aplicación de servidor de inscripción

Agregar una aplicación de servidor de inscripción y el grupo de aplicaciones si es necesario, para la entidad de certificación especificado. Este comando no instala los archivos binarios o los paquetes. Uno de los siguientes métodos de autenticación con el que el cliente se conecta a un servidor de inscripción de certificados.

- Kerberos: Usar credenciales Kerberos SSL
- Nombre de usuario: Usar cuenta con las credenciales SSL
- ClientCertificate: Use las credenciales de un certificado SSL X.509
- AllowRenewalsOnly: Solo las solicitudes de renovación se pueden enviar a esta CA a través de esta dirección URL
- AllowKeyBasedRenewal: Permite el uso de un certificado que se dispone de ninguna cuenta asociada en Active Directory. Esto se aplica únicamente con el modo ClientCertificate y AllowRenewalsOnly.

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [opciones] - deleteEnrollmentServer Kerberos | Nombre de usuario | ClientCertificate

Eliminar una aplicación de servidor de inscripción

Eliminar una aplicación de servidor de inscripción y el grupo de aplicaciones si es necesario, para la entidad de certificación especificado. Este comando no elimina los archivos binarios o los paquetes. Uno de los siguientes métodos de autenticación con el que el cliente se conecta a un servidor de inscripción de certificados.

1. Kerberos: Usar credenciales Kerberos SSL
2. Nombre de usuario: Usar cuenta con las credenciales SSL
3. ClientCertificate: Use las credenciales de un certificado SSL X.509

[-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [Options] -addPolicyServer Kerberos | UserName | ClientCertificate [KeyBasedRenewal]

Agregar una aplicación de servidor de directivas

Si es necesario, agregue una aplicación de servidor de directivas y el grupo de aplicaciones. Este comando no instala los archivos binarios o los paquetes. Uno de los siguientes métodos de autenticación con el que el cliente se conecta a un servidor de directivas de certificado:

- Kerberos: Usar credenciales Kerberos SSL
- Nombre de usuario: Usar cuenta con las credenciales SSL
- ClientCertificate: Use las credenciales de un certificado SSL X.509
- KeyBasedRenewal: Solo las directivas que contienen plantillas KeyBasedRenewal se devuelven al cliente. Esta marca se aplica solo para la autenticación de nombre de usuario y ClientCertificate.

Vuelva a [menú](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [Options] -deletePolicyServer Kerberos | UserName | ClientCertificate [KeyBasedRenewal]

Eliminar una aplicación de servidor de directivas

Eliminar una aplicación de servidor de directivas y el grupo de aplicaciones si es necesario. Este comando no elimina los archivos binarios o los paquetes. Uno de los siguientes métodos de autenticación con el que el cliente se conecta a un servidor de directivas de certificado:

1. Kerberos: Usar credenciales Kerberos SSL
2. Nombre de usuario: Usar cuenta con las credenciales SSL
3. ClientCertificate: Use las credenciales de un certificado SSL X.509
4. KeyBasedRenewal: Servidor de directivas de KeyBasedRenewal

Vuelva a [menú](#menu)

## <a name="-oid"></a>oid:

CertUtil [Options] -oid ObjectId [DisplayName | delete [LanguageId [Type]]]

CertUtil [Options] -oid GroupId

CertUtil [Options] -oid AlgId | AlgorithmName [GroupId]

Mostrar el ObjectId o establecer el nombre para mostrar

- ObjectId: El valor de ObjectId para mostrar o agregar el nombre para mostrar
- GroupId: número decimal de GroupId de ObjectID enumerar
- AlgId--hexadecimal AlgId de ObjectId buscar
- AlgorithmName--Nombre del algoritmo de ObjectId buscar
- DisplayName: Nombre para mostrar para almacenar en DS
- eliminar: eliminar el nombre para mostrar
- LanguageId--Id. de idioma (el valor predeterminado es actual: 1033)
- Tipo--DS objeto tipo va a crear: 1 para la plantilla (valor predeterminado), 2 para la directiva de emisión, 3 para la directiva de aplicación
- Utilice -f para crear el objeto de DS.

[-f]

Vuelva a [menú](#menu)

## <a name="-error"></a>-error

CertUtil [opciones] - error ErrorCode

Mostrar texto de mensaje del código de error

Vuelva a [menú](#menu)

## <a name="-getreg"></a>-getreg

CertUtil [Options] -getreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]][RegistryValueName]

Mostrar el valor del registro

ca: Clave del registro de la entidad de certificación de uso

restore: Clave del registro de restauración de la entidad de certificación de uso

Directiva: Usar la clave de registro del módulo de directivas

salida: Use primero salga de clave de registro del módulo

Plantilla: Usar la clave del registro de plantilla (use - usuario para las plantillas de usuario)

enroll: Usar la clave del registro de inscripción (use - usuario para el contexto de usuario)

cadena: Usar la clave del registro de configuración de cadena

PolicyServers: Clave del registro de servidores de directivas de uso

ProgId: Usar la directiva o salir ProgId del módulo (nombre de subclave del registro)

NombreValorRegistro: nombre del valor de registro (utilice "nombre\*" para la coincidencia de prefijo)

Valor: nuevo numérico, fecha o cadena de valor del registro o nombre de archivo. Si un valor numérico que empieza por "+" o "-", los bits especificados en el nuevo valor se establece o borra el valor del registro existente.

Si un valor de cadena comienza con "+" o "-" y el valor existente es un valor REG_MULTI_SZ, la cadena se agrega o quita el valor del registro existente. Para forzar la creación de un valor REG_MULTI_SZ, agregue un "\n" al final del valor de cadena.

Si el valor empieza por "@", el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, en su lugar, se analiza como [Date] [+ |-] [hh]--una fecha opcional más o menos opcionales días y horas. Si se especifican ambos, use un signo más (+) o separador de signo menos (-). Use "ahora + hh" para una fecha relativa a la hora actual.

Use "chain\ChainCacheResyncFiletime @now" vaciar eficazmente las CRL almacenada en caché.

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-setreg"></a>-setreg

CertUtil [Options] -setreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]]RegistryValueName Value

Valor del registro de conjunto

ca: Clave del registro de la entidad de certificación de uso

restore: Clave del registro de restauración de la entidad de certificación de uso

Directiva: Usar la clave de registro del módulo de directivas

salida: Use primero salga de clave de registro del módulo

Plantilla: Usar la clave del registro de plantilla (use - usuario para las plantillas de usuario)

enroll: Usar la clave del registro de inscripción (use - usuario para el contexto de usuario)

cadena: Usar la clave del registro de configuración de cadena

PolicyServers: Clave del registro de servidores de directivas de uso

ProgId: Usar la directiva o salir ProgId del módulo (nombre de subclave del registro)

NombreValorRegistro: nombre del valor de registro (utilice "nombre\*" para la coincidencia de prefijo)

Valor: nuevo numérico, fecha o cadena de valor del registro o nombre de archivo. Si un valor numérico que empieza por "+" o "-", los bits especificados en el nuevo valor se establece o borra el valor del registro existente.

Si un valor de cadena comienza con "+" o "-" y el valor existente es un valor REG_MULTI_SZ, la cadena se agrega o quita el valor del registro existente. Para forzar la creación de un valor REG_MULTI_SZ, agregue un "\n" al final del valor de cadena.

Si el valor empieza por "@", el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, en su lugar, se analiza como [Date] [+ |-] [hh]--una fecha opcional más o menos opcionales días y horas. Si se especifican ambos, use un signo más (+) o separador de signo menos (-). Use "ahora + hh" para una fecha relativa a la hora actual.

Use "chain\ChainCacheResyncFiletime @now" vaciar eficazmente las CRL almacenada en caché.

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-delreg"></a>-delreg

CertUtil [Options] -delreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]][RegistryValueName]

Eliminar valor del registro

ca: Clave del registro de la entidad de certificación de uso

restore: Clave del registro de restauración de la entidad de certificación de uso

Directiva: Usar la clave de registro del módulo de directivas

salida: Use primero salga de clave de registro del módulo

Plantilla: Usar la clave del registro de plantilla (use - usuario para las plantillas de usuario)

enroll: Usar la clave del registro de inscripción (use - usuario para el contexto de usuario)

cadena: Usar la clave del registro de configuración de cadena

PolicyServers: Clave del registro de servidores de directivas de uso

ProgId: Usar la directiva o salir ProgId del módulo (nombre de subclave del registro)

NombreValorRegistro: nombre del valor de registro (utilice "nombre\*" para la coincidencia de prefijo)

Valor: nuevo numérico, fecha o cadena de valor del registro o nombre de archivo. Si un valor numérico que empieza por "+" o "-", los bits especificados en el nuevo valor se establece o borra el valor del registro existente.

Si un valor de cadena comienza con "+" o "-" y el valor existente es un valor REG_MULTI_SZ, la cadena se agrega o quita el valor del registro existente. Para forzar la creación de un valor REG_MULTI_SZ, agregue un "\n" al final del valor de cadena.

Si el valor empieza por "@", el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, en su lugar, se analiza como [Date] [+ |-] [hh]--una fecha opcional más o menos opcionales días y horas. Si se especifican ambos, use un signo más (+) o separador de signo menos (-). Use "ahora + hh" para una fecha relativa a la hora actual.

Use "chain\ChainCacheResyncFiletime @now" vaciar eficazmente las CRL almacenada en caché.

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [Options] -ImportKMS UserKeyAndCertFile [CertId]

Importar certificados y claves de usuario en la base de datos del servidor para el archivo de claves

UserKeyAndCertFile--El archivo de datos que contienen las claves privadas de usuario y certificados para archivarse.  Esto puede ser cualquiera de las siguientes acciones:

- Archivo de exportación del servidor de administración de claves (KMS) de Exchange
- Archivo PFX

CertId: Token de coincidencia de certificado de descifrado de archivo de exportación de KMS.  Consulte [-almacenar](#-store).

Utilice -f para importar certificados no emitidos por la entidad de certificación.

[-f]. [-silenciosa] [-dividir] [-config Machine\CAName] [-p contraseña] [-symkeyalg SymmetricKeyAlgorithm [, KeyLength]]

Vuelva a [menú](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [opciones] ImportCert - Certfile [ExistingRow]

Importar un archivo de certificado en la base de datos

Utilice ExistingRow para importar el certificado en lugar de una solicitud pendiente para la misma clave.

Utilice -f para importar certificados no emitidos por la entidad de certificación.

La entidad de certificación que también deba configurarse para admitir la importación de certificados externa: certutil - setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f] [-config Machine\CAName]

Vuelva a [menú](#menu)

## <a name="-getkey"></a>-GetKey

CertUtil [Options] -GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [opciones] - GetKey SearchToken script OutputScriptFile

CertUtil [opciones] - GetKey SearchToken recuperar | recuperar OutputFileBaseName

Recuperar el blob de recuperación de claves privadas archivadas, generar un script de recuperación o recuperar claves archivadas

script: generar un script para recuperar y recuperar las claves (comportamiento predeterminado si se encuentran varios candidatos de recuperación coincidente, o si el archivo de salida no está especificado).

recuperar: recuperar uno o varios Blobs de recuperación de clave (valor predeterminado comportamiento si no se encuentra exactamente un candidato de recuperación coincidentes, y si se especifica el archivo de salida)

recuperar: recuperar y recuperar claves privadas en un solo paso (requiere certificados de Key Recovery Agent y claves privadas)

SearchToken: Se utiliza para seleccionar las claves y certificados que se recupere.

puede ser cualquiera de las siguientes acciones:

1. Nombre común del certificado
2. Número de serie del certificado
3. Hash de SHA-1 del certificado (huella digital)
4. Hash de certificado KeyId SHA-1 (identificador de clave de asunto)
5. Nombre del solicitante (dominio\usuario)
6. UPN (user@domain)

ArchivoDeSalidaDeBlobDeRecuperación: archivo de salida que contiene una cadena de certificados y una clave privada asociada, siguen estando cifrados a uno o varios certificados de Key Recovery Agent.

OutputScriptFile: archivo de salida que contiene un script por lotes para recuperar y recuperar claves privadas.

OutputFileBaseName: nombre de base salida archivo. Para recuperar, cualquier extensión se trunca y se anexan a una cadena específica de certificado y la extensión .rec para cada blob de clave de recuperación.  Cada archivo contiene una cadena de certificados y una clave privada asociada, siguen estando cifrados a uno o varios certificados de Key Recovery Agent. Para recuperar, cualquier extensión se trunca y se anexa la extensión. p12.  Contiene las cadenas de certificado recuperado y claves privadas asociadas, almacenadas como un archivo PFX.

[-f] [-UnicodeText] [-silent] [-config Machine\CAName] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider]

Vuelva a [menú](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [Options] -RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

Recuperar la clave privada archivado

[-f]. [-usuario] [-silenciosa] [-dividir] [-p contraseña] [-ProtectTo SAMNameAndSIDList] [-csp proveedor] [-t tiempo de espera]

Vuelva a [menú](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [opciones] - MergePFX PFXInFileList archivoDeSalidaPFX [ExtendedProperties]

PFXInFileList: Lista de archivos de entrada PFX separados por comas

PFXOutFile: Archivo de salida PFX

Propiedades extendidas: Incluir propiedades extendidas

La contraseña especificada en la línea de comandos es una lista de la contraseña separados por comas.  Si se especifica más de una contraseña, se usa la última contraseña del archivo de salida.  Si sólo se proporciona una contraseña o si es la última contraseña "\*", se pedirá al usuario la contraseña del archivo de salida.

[-f]. [-usuario] [-dividir] [-p contraseña] [-ProtectTo SAMNameAndSIDList] [-csp proveedor]

Vuelva a [menú](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [opciones] - ConvertEPF ListaArchivoEntradaPFX ArchivoSalidaEPF [cast | cast-] [V3CACertId] [, Salt]

Convertir archivos PFX en archivo EPF

PFXInFileList: Lista de archivos de entrada PFX separados por comas

EPF: Archivo de salida EPF

conversión de tipos: Utilizar CAST 64 cifrado

cast-: Usar cifrado CAST 64 (exportar)

V3CACertId: Símbolo (token) de la coincidencia de certificado de CA V3.  Consulte [-almacenar](#-store) CertId descripción.

Valor "salt": Cadena salt del archivo de salida EPF

La contraseña especificada en la línea de comandos es una lista de la contraseña separados por comas. Si se especifica más de una contraseña, se usa la última contraseña del archivo de salida.  Si sólo se proporciona una contraseña o si es la última contraseña "\*", se pedirá al usuario la contraseña del archivo de salida.

[-f]. [-silenciosa] [-dividir] [-dc DCName] [-p contraseña] [-csp proveedor]

Vuelva a [menú](#menu)

## <a name="options"></a>Opciones

En esta sección define las opciones que se pueden especificar con el comando.

|Opciones|Descripción|
|-------|-----------|
|-nullsign|Usar hash de datos como firma|
|-f|Forzar la sobrescritura|
|-enterprise|Usar almacén de certificados de equipo local Enterprise del registro|
|-usuario|Use las teclas de HKEY_CURRENT_USER o almacén de certificados|
|-GroupPolicy|Almacén de certificados de directiva de grupo de uso|
|-ut|Mostrar las plantillas de usuario|
|-mt|Mostrar las plantillas de máquina|
|Unicode:|Escribir la salida redirigida en formato Unicode|
|-UnicodeText|Escribir archivo de salida en formato Unicode|
|-gmt|Mostrar horas como GMT|
|-segundos|Mostrar horas con segundos y milisegundos|
|-silent|Use la marca silencioso para adquirir contexto de cifrado|
|-dividir|Dividir elementos ASN.1 incrustados y guardar en archivos|
|-v|Operación detallada|
|-privatekey|Mostrar datos de la clave privadas y la contraseña|
|-Anclar PIN|NIP de tarjeta inteligente|
|-urlfetch|Recuperar y comprobar certificados AIA y CDP CRL|
|-config Machine\CAName|Cadena de nombre de entidad de certificación y equipo|
|-PolicyServer URLOrId|Dirección URL del servidor de directivas o identificador. Para la selección U / I, use - PolicyServer. Para todos los servidores de directivas, use - PolicyServer \*|
|-Anónimo|Utilice credenciales anónimas de SSL|
|-Kerberos|Usar credenciales Kerberos SSL|
|-ClientCertificate ClientCertId|Use las credenciales de un certificado SSL X.509. Para la selección U / I, use - clientCertificate.|
|Nombre de usuario - UserName|Usar cuenta con credenciales SSL. Para la selección U / I, use - UserName.|
|-Cert CertId|Certificado de firma|
|-dc DCName|Destino de un controlador de dominio específico|
|-restringir la lista de restricciones|Lista de restricciones separados por comas. Cada restricción consta de un nombre de columna, un operador relacional y constante entero, cadena o fecha. Un nombre de columna puede estar precedido por un signo más o menos para indicar el criterio de ordenación. Ejemplos:</br>"RequestId = 47"</br>"+ RequesterName > = a, RequesterName < b"</br>"-RequesterName > dominio, Disposition = 21"|
|-out ColumnList|Lista de columnas separados por comas|
|-p contraseña|Contraseña|
|-ProtectTo SAMNameAndSIDList|SAM SID/nombre de la lista separada por comas|
|-csp proveedor|Proveedor|
|-t tiempo de espera|Tiempo de espera de obtención de direcciones URL en milisegundos|
|-symkeyalg SymmetricKeyAlgorithm [, KeyLength]|Nombre del algoritmo simétrico de clave con una longitud de clave opcional, ejemplo: AES 128 o 3DES|

Vuelva a [menú](#menu)

## <a name="additional-certutil-examples"></a>Ejemplos adicionales de certutil

Para ver algunos ejemplos de cómo utilizar este comando, consulte

1. [Ejemplos de certutil para administrar servicios de certificados de Active Directory (AD CS) desde la línea de comandos](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [Tareas de certutil para administrar certificados](https://technet.microsoft.com/library/cc772898.aspx)
3. [Exportación de solicitud binario mediante el tutorial de la herramienta de línea de comandos CertUtil.exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [Renovación de certificados de CA raíz](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

Vuelva a [menú](#menu)
