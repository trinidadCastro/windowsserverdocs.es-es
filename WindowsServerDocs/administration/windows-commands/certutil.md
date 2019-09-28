---
title: certutil
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45c9946cc53fe3a901c3f6ee53f082a5b3d086c0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379652"
---
# <a name="certutil"></a>certutil

Certutil. exe es un programa de línea de comandos que se instala como parte de los servicios de Certificate Server. Puede usar certutil. exe para volcar y mostrar la información de configuración de la entidad de certificación (CA), configurar servicios de Certificate Server, realizar copias de seguridad y restaurar componentes de CA y comprobar certificados, pares de claves y cadenas de certificados.

Cuando se ejecuta certutil en una entidad de certificación sin parámetros adicionales, se muestra la configuración de la entidad de certificación actual. Cuando cerutil se ejecuta en una entidad de certificación que no es de, el comando tiene como valor predeterminado ejecutar el verbo certutil [-dump](#-dump) .

> [!WARNING]
> Es posible que las versiones anteriores de certutil no proporcionen todas las opciones que se describen en este documento. Puede ver todas las opciones que proporciona una versión específica de certutil; para ello, ejecute los comandos que se muestran en la sección [notación de sintaxis](#syntax-notations) .

## <a name="menu"></a>Menú

Las secciones principales de este documento son:

- [Verbos](#verbs)
- [Notación de sintaxis](#syntax-notations)
- [Opciones](#options)
- [Ejemplos de certutil adicionales](#additional-certutil-examples)

## <a name="verbs"></a>Verbos

En la tabla siguiente se describen los verbos que se pueden utilizar con el comando certutil.

|Verbos|Descripción|
|-----|-----------|
|[-dump](#-dump)|Archivos o información de configuración de volcado|
|[-ASN](#-asn)|Analizar archivo ASN. 1|
|[-decodehex](#-decodehex)|Descodificar archivo codificado en hexadecimal|
|[-descodificar](#-decode)|Descodificar un archivo codificado en Base64|
|[-codificar](#-encode)|Codificar un archivo en Base64|
|[-denegar](#-deny)|Denegar una solicitud de certificado pendiente|
|[-reenviar](#-resubmit)|Volver a enviar una solicitud de certificado pendiente|
|[-SetAttributes](#-setattributes)|Establecer atributos para una solicitud de certificado pendiente|
|[-setextension](#-setextension)|Establecer una extensión para una solicitud de certificado pendiente|
|[-REVOKE](#-revoke)|Revocar un certificado|
|[-IsValid](#-isvalid)|Mostrar la disposición del certificado actual|
|[-GetConfig](#-getconfig)|Obtención de la cadena de configuración predeterminada|
|[-ping](#-ping)|Intenta ponerse en contacto con la interfaz de solicitud de servicios de certificados de Active Directory|
|-pingadmin|Intenta ponerse en contacto con el Active Directory la interfaz de administración de servicios de certificados|
|[-CAInfo](#-cainfo)|Mostrar información acerca de la entidad de certificación|
|[-CA. cert](#-cacert)|Recuperar el certificado de la entidad de certificación|
|[-CA. Chain](#-cachain)|Recuperar la cadena de certificados de la entidad de certificación|
|[-GetCRL](#-getcrl)|Obtener una lista de revocación de certificados (CRL)|
|[-CRL](#-crl)|Publicar nuevas listas de revocación de certificados (CRL) [o solo diferencias CRL]|
|[-Shutdown](#-shutdown)|Cerrar servicios de Certificate Server Active Directory|
|[-installCert](#-installcert)|Instalar un certificado de entidad de certificación|
|[-renewCert](#-renewcert)|Renovar un certificado de entidad de certificación|
|[-esquema](#-schema)|Volcar el esquema para el certificado|
|[-Ver](#-view)|Volcar la vista de certificados|
|[-dB](#-db)|Volcar la base de datos sin procesar|
|[-deleteRow](#-deleterow)|Eliminar una fila de la base de datos del servidor|
|[-backup](#-backup)|Servicios de certificados Active Directory de copia de seguridad|
|[-backupDB](#-backupdb)|Copia de seguridad de la Active Directory base de datos de servicios de certificados|
|[-backupKey](#-backupkey)|Copia de seguridad del certificado de servicios de certificados de Active Directory y la clave privada|
|[-restaurar](#-restore)|Restaurar Active Directory servicios de Certificate Server|
|[-restoreDB](#-restoredb)|Restaurar la Active Directory base de datos de servicios de certificados|
|[-restoreKey](#-restorekey)|Restaurar la clave privada y el certificado de servicios de certificados de Active Directory|
|[-importPFX](#-importpfx)|Importar certificado y clave privada|
|[-dynamicfilelist](#-dynamicfilelist)|Mostrar una lista de archivos dinámicos|
|[-databaselocations](#-databaselocations)|Mostrar ubicaciones de bases de datos|
|[-hashfile](#-hashfile)|Generar y mostrar un hash criptográfico sobre un archivo|
|[-Store](#-store)|Volcar el almacén de certificados|
|[-AddStore](#-addstore)|Agregar un certificado al almacén|
|[-delstore](#-delstore)|Eliminar un certificado del almacén|
|[-verifystore](#-verifystore)|Comprobación de un certificado en el almacén|
|[-repairstore](#-repairstore)|Reparar una asociación de claves o actualizar las propiedades del certificado o el descriptor de seguridad de la clave|
|[-viewstore](#-viewstore)|Volcar el almacén de certificados|
|[-viewdelstore](#-viewdelstore)|Eliminar un certificado del almacén|
|[-dsPublish](#-dspublish)|Publicar un certificado o una lista de revocación de certificados (CRL) en Active Directory|
|[-ADTemplate](#-adtemplate)|Mostrar plantillas de AD|
|[-Plantilla](#-template)|Mostrar plantillas de certificado|
|[-TemplateCAs](#-templatecas)|Mostrar las entidades de certificación (CA) de una plantilla de certificado|
|[-CATemplates](#-catemplates)|Mostrar plantillas para CA|
|[-SetCASites](#-setcasites)|Administrar nombres de sitio para CA|
|[-enrollmentServerURL](#-enrollmentserverurl)|Mostrar, agregar o eliminar las direcciones URL del servidor de inscripciones asociadas a una CA|
|[-ADCA](#-adca)|Mostrar CA de AD|
|[-CA](#-ca)|Mostrar CA de directiva de inscripción|
|[-Directiva](#-policy)|Mostrar Directiva de inscripción|
|[-PolicyCache](#-policycache)|Mostrar o eliminar entradas de caché de directiva de inscripción|
|[-CredStore](#-credstore)|Mostrar, agregar o eliminar entradas del almacén de credenciales|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|Instalar plantillas de certificado predeterminadas|
|[-URLCache](#-urlcache)|Mostrar o eliminar entradas de caché de URL|
|[-Pulse](#-pulse)|Eventos de inscripción automática de impulsos|
|[-MachineInfo](#-machineinfo)|Mostrar información sobre el objeto de máquina Active Directory|
|[-DCInfo](#-dcinfo)|Mostrar información sobre el controlador de dominio|
|[-EntInfo](#-entinfo)|Mostrar información acerca de una CA de empresa|
|[-TCAInfo](#-tcainfo)|Mostrar información acerca de la CA|
|[-SCInfo](#-scinfo)|Mostrar información acerca de la tarjeta inteligente|
|[-SCRoots](#-scroots)|Administrar certificados raíz de tarjeta inteligente|
|[-verifykeys](#-verifykeys)|Comprobar un conjunto de claves público o privado|
|[-comprobar](#-verify)|Comprobar un certificado, una lista de revocación de certificados (CRL) o una cadena de certificados|
|[-verifyCTL](#-verifyctl)|Comprobar AuthRoot o CTL de certificados no permitidos|
|[-Sign](#-sign)|Volver a firmar una lista de revocación de certificados (CRL) o un certificado|
|[-vroot](#-vroot)|Crear o eliminar raíces virtuales web y recursos compartidos de archivos|
|[-vocsproot](#-vocsproot)|Crear o eliminar raíces virtuales web para un proxy web OCSP|
|[-addEnrollmentServer](#-addenrollmentserver)|Agregar una aplicación de servidor de inscripciones|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|Eliminar una aplicación de servidor de inscripciones|
|[-addPolicyServer](#-addpolicyserver)|Agregar una aplicación de servidor de directivas|
|[-deletePolicyServer](#-deletepolicyserver)|Eliminar una aplicación de servidor de directivas|
|[-OID](#-oid)|Mostrar el identificador de objeto o establecer un nombre para mostrar|
|[-error](#-error)|Mostrar el texto del mensaje asociado a un código de error|
|[-getreg](#-getreg)|Mostrar un valor del registro|
|[-setreg](#-setreg)|Establecer un valor del registro|
|[-delreg](#-delreg)|Eliminar un valor del registro|
|[-ImportKMS](#-importkms)|Importar claves de usuario y certificados en la base de datos del servidor para el archivo de claves|
|[-ImportCert](#-importcert)|Importar un archivo de certificado en la base de datos|
|[-GetKey](#-getkey)|Recuperar un BLOB de recuperación de clave privada archivada|
|[-RecoverKey](#-recoverkey)|Recuperación de una clave privada archivada|
|[-MergePFX](#-mergepfx)|Combinar archivos PFX|
|[-ConvertEPF](#-convertepf)|Convertir un archivo PFX en un archivo EPF|
|-?|Muestra la lista de verbos|
|-> de verbo:?  *\<*|Muestra la ayuda para el verbo especificado.|
|-? -v|Muestra una lista completa de verbos y|

[Menú](#menu) volver a

## <a name="syntax-notations"></a>Notación de sintaxis

- Para la sintaxis básica de la línea de comandos, ejecute`certutil -?`
- Para obtener la sintaxis sobre el uso de certutil con un verbo específico, ejecute el verbo **certutil**  *\<>* **-?**
- Para enviar toda la sintaxis de certutil a un archivo de texto, ejecute los siguientes comandos:  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

En la tabla siguiente se describe la notación que se usa para indicar la sintaxis de línea de comandos.


|            Notación             |                  Descripción                  |
|---------------------------------|-----------------------------------------------|
| Texto sin corchetes o llaves |         Elementos que debe escribir como se muestra          |
|  \<Texto dentro de corchetes angulares >  | Marcador de posición para el que se debe proporcionar un valor. |
|  [Texto entre corchetes]  |                Elementos opcionales                 |
|      {Texto entre llaves}       |       Conjunto de elementos necesarios; Elija una       |
|         Barra vertical (          |                       ) simple                       |
|          Puntos suspensivos (...)           |          Elementos que se pueden repetir           |

[Menú](#menu) volver a

## <a name="-dump"></a>-dump

CertUtil [opciones] [-dump]

CertUtil [opciones] [-dump] File

Archivos o información de configuración de volcado

[-f] [-SILENT] [-Split] [-p contraseña] [-t tiempo de espera]

[Menú](#menu) volver a

## <a name="-asn"></a>-ASN

CertUtil [opciones]-archivo ASN [tipo]

Analizar archivo ASN. 1

tipo: tipo de\_descodificación de cadena\_ \* de cifrado numérico

[Menú](#menu) volver a

## <a name="-decodehex"></a>-decodehex

CertUtil [opciones]-decodehex INFILE OUTFILE [tipo]

tipo: tipo de\_codificación\_ de cadena\* cifrada numérica

[-f]

[Menú](#menu) volver a

## <a name="-decode"></a>-descodificar

CertUtil [opciones]-descodificar OUTFILE

Descodificación de archivos codificados en Base64

[-f]

[Menú](#menu) volver a

## <a name="-encode"></a>-codificar

CertUtil [opciones]-codificar OUTFILE

Codificar archivo en Base64

[-f] [-UnicodeText]

[Menú](#menu) volver a

## <a name="-deny"></a>-denegar

CertUtil [opciones]-denegar RequestId

Denegar solicitud pendiente

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-resubmit"></a>-reenviar

CertUtil [opciones]-reenviar RequestId

Reenviar solicitud pendiente

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-setattributes"></a>-SetAttributes

CertUtil [opciones]-SetAttributes RequestId AttributeString

Establecer atributos para la solicitud pendiente

RequestId: ID. de solicitud numérica de solicitud pendiente

AttributeString: pares de nombre y valor de atributo de solicitud

- Los nombres y valores se separan con dos puntos.
- Varios nombres, pares de valor se separan de nueva línea.
- Ejemplo: "CertificateTemplate:User\nEMail:User@Domain.com"
- Cada secuencia "\n" se convierte en un separador de nueva línea.

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-setextension"></a>-setextension

CertUtil [opciones]-setextension RequestId ExtensionName marcas {Long | Fecha | Cadena | \@InFile}

Establecer la extensión para la solicitud pendiente

RequestId: ID. de solicitud numérica de una solicitud pendiente

ExtensionName--ObjectId cadena de la extensión

Flags: se recomienda 0.  1 hace que la extensión sea crítica, 2 la deshabilita, 3 realiza ambos.

Si el último parámetro es numérico, se toma como un valor Long.

Si se puede analizar como una fecha, se toma como una fecha.

Si empieza por '\@', el resto del token es el nombre de archivo que contiene datos binarios o un volcado hexadecimal de texto ASCII.

Todo lo demás se toma como una cadena.

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-revoke"></a>-REVOKE

CertUtil [opciones]-revocación de SerialNumber [motivo]

Revocar certificado

SerialNumber Lista separada por comas de números de serie de certificado para revocar

Motivo: razón de revocación numérica o simbólica

- 0: CRL_REASON_UNSPECIFIED: No especificado (valor predeterminado)
- 1: CRL_REASON_KEY_COMPROMISE: Riesgo de clave
- 2: CRL_REASON_CA_COMPROMISE: Compromiso de CA
- 3: CRL_REASON_AFFILIATION_CHANGED: afiliación modificada
- 4: CRL_REASON_SUPERSEDED: Reemplazada
- 5: CRL_REASON_CESSATION_OF_OPERATION: Cese de operación
- 6: CRL_REASON_CERTIFICATE_HOLD: Retención de certificado
- 8: CRL_REASON_REMOVE_FROM_CRL: Quitar de CRL
- -1: Anular Anular

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-isvalid"></a>-IsValid

CertUtil [opciones]-IsValid SerialNumber | CertHash

Mostrar disposición actual del certificado

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-getconfig"></a>-GetConfig

CertUtil [opciones]-GetConfig

Obtiene la cadena de configuración predeterminada

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-ping"></a>-ping

CertUtil [opciones]-ping [MaxSecondsToWait | CAMachineList]

Ping Active Directory interfaz de solicitud de servicios de certificados

CAMachineList: lista de nombres de máquina de CA separados por comas

1. Para una sola máquina, use una coma de terminación
2. Muestra el costo del sitio para cada máquina de CA

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-cainfo"></a>-CAInfo

CertUtil [opciones]-CAInfo [InfoName [índice | ErrorCode]]

Mostrar información de CA

InfoName: indica la propiedad de la entidad de certificación que se va a mostrar (consulte a continuación). Use "\*" para todas las propiedades.

Index: índice de propiedad de base cero opcional

ErrorCode: código de error numérico

[-f] [-Split] [-config Machine\CAName]

Sintaxis del argumento InfoName:

- filesystem Versión de archivo
- Manuales Versión del producto
- exitcount: Recuento de módulos de salida
- Exit [índice]: Descripción del módulo de salida
- directivas Descripción del módulo de directivas
- Name Nombre de CA
- sanitizedname: Nombre de CA saneado
- DSName Nombre corto de CA saneado (nombre de DS)
- carpetaCompartida Carpeta compartida
- Código de Error1: Texto del mensaje de error
- Código de error2: Texto del mensaje de error y código de error
- automáticamente Tipo de CA
- superclase Información de CA
- aérea CA primaria
- certcount: Número de certificados de CA
- xchgcount: Recuento de certificados de intercambio de CA
- kracount: Recuento de certificados KRA
- kraused: Recuento de uso de certificado KRA
- propidmax: PropId de CA máximo
- certstate [índice]: Certificado de CA
- certversion [índice]: Versión de certificado de CA
- certstatuscode [índice]: Estado de comprobación de certificado de CA
- crlstate [índice]: LCR
- krastate [índice]: Certificado KRA
- crossstate + [índice]: Reenviar certificado cruzado
- crossstate-[índice]: Certificado cruzado hacia atrás
- CERT [índice]: Certificado de CA
- certchain [índice]: Cadena de certificados de CA
- certcrlchain [índice]: Cadena de certificados de CA con CRL
- xchg [índice]: Certificado de intercambio de CA
- xchgchain [índice]: Cadena de certificados de intercambio de CA
- xchgcrlchain [índice]: Cadena de certificados de intercambio de CA con CRL
- KRA [índice]: Certificado KRA
- Cross + [índice]: Reenviar certificado cruzado
- Cross-[index]: Certificado cruzado hacia atrás
- CRL [índice]: CRL base
- deltacrl [índice]: Diferencias CRL
- crlstatus [índice]: Estado de publicación de CRL
- deltacrlstatus [índice]: Estado de publicación de diferencias CRL
- DN Nombre DNS
- role Separación de roles
- anuncios Advanced Server
- templa Plantillas
- CSP [índice]: Direcciones URL de OCSP
- AIA [índice]: Direcciones URL de AIA
- CDP [índice]: Direcciones URL de CDP
- localename Nombre de configuración regional de CA
- subjecttemplateoids: OID de la plantilla de asunto

[Menú](#menu) volver a

## <a name="-cacert"></a>-CA. cert

CertUtil [opciones]-CA. cert OutCACertFile [índice]

Recuperar el certificado de la entidad de certificación

OutCACertFile: archivo de salida

Ajustar Índice de renovación de certificado de CA (el valor predeterminado es el más reciente)

[-f] [-Split] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-cachain"></a>-CA. Chain

CertUtil [opciones]-CA. Chain OutCACertChainFile [índice]

Recuperar la cadena de certificados de la entidad de certificación

OutCACertChainFile: archivo de salida

Ajustar Índice de renovación de certificado de CA (el valor predeterminado es el más reciente)

[-f] [-Split] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-getcrl"></a>-GetCRL

CertUtil [opciones]-GetCRL OUTFILE [index] [Delta]

Obtener CRL

Ajustar Índice de CRL o índice de clave (el valor predeterminado es CRL para la clave más reciente)

Delta: diferencia CRL (el valor predeterminado es CRL base)

[-f] [-Split] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-crl"></a>-CRL

CertUtil [opciones]-CRL [DD: HH | volver a publicar] [Delta]

Publicar nuevas CRL [o solo diferencias CRL]

DD: HH--nuevo período de validez de CRL en días y horas

volver a publicar: volver a publicar las CRL más recientes

Delta--diferencias CRL solamente (el valor predeterminado es CRL de base y Delta)

[-Split] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-shutdown"></a>-Shutdown

CertUtil [opciones]-Shutdown

Cerrar servicios de Certificate Server Active Directory

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-installcert"></a>-installCert

CertUtil [opciones]-installCert [CACertFile]

Instalación del certificado de entidad de certificación

[-f] [-SILENT] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-renewcert"></a>-renewCert

CertUtil [opciones]-renewCert [ReuseKeys] [Machine\ParentCAName]

Renovar certificado de entidad de certificación

Use-f para omitir una solicitud de renovación pendiente y generar una nueva solicitud.

[-f] [-SILENT] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-schema"></a>-esquema

CertUtil [opciones]-esquema [ext | Attrib | LCR

Volcado del esquema de certificados

Tiene como valor predeterminado la solicitud y la tabla de certificados

Total Tabla de extensión

Atributo Tabla de atributos

LCR Tabla CRL

[-Split] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-view"></a>-Ver

CertUtil [opciones]-ver [cola | Registro | LogFail | Se ha revocado | Ext | Attrib | CRL] [CSV]

Volcar vista de certificado

Cola: Cola de solicitudes

Registro: Certificados emitidos o revocados, más solicitudes con error

LogFail: Solicitudes con error

Revocado Certificados revocados

Total Tabla de extensión

Atributo Tabla de atributos

LCR Tabla CRL

CVS Salida como valores separados por comas

Para mostrar la columna StatusCode para todas las entradas:-out StatusCode

Para mostrar todas las columnas de la última entrada:-Restrict "RequestId = = $"

Para mostrar RequestId y disposition para tres solicitudes:-Restrict "RequestId > = 37,\<RequestId 40"-out "RequestId, disposition"

Para mostrar los identificadores de fila y los números CRL de todas las CRL base:-Restrict "CRLMinBase = 0"-out "CRLRowId, CRLNumber" CRL

Para mostrar el número de CRL base 3:-v-Restrict "CRLMinBase = 0, CRLNumber = 3"-out "CRLRawCRL" CRL

Para mostrar toda la tabla de CRL: LCR

Usar "Date [+ |-DD: HH]" para las restricciones de fecha

Usar "Now + DD: HH" para una fecha relativa a la hora actual

[-SILENT] [-Split] [-config Machine\CAName] [-Restrict RestrictionList] [-out ColumnList]

[Menú](#menu) volver a

## <a name="-db"></a>-dB

CertUtil [opciones]-BD

Volcar base de datos sin procesar

[-config Machine\CAName] [-Restrict RestrictionList] [-out ColumnList]

[Menú](#menu) volver a

## <a name="-deleterow"></a>-deleteRow

CertUtil [opciones]-deleteRow RowId | Fecha [solicitud | CERT | Ext | Attrib | LCR

Eliminar fila de base de datos del servidor

Solicite Solicitudes con errores y pendientes (fecha de envío)

Cert: Certificados expirados y revocados (fecha de expiración)

Total Tabla de extensión

Atributo Tabla de atributos

LCR Tabla de CRL (fecha de expiración)

Para eliminar las solicitudes con error y pendientes enviadas el 22 de enero de 2001: solicitud 1/22/2001

Para eliminar todos los certificados que expiraron el 22 de enero de 2001: certificado 1/22/2001

Para eliminar la fila de certificado, atributos y extensiones para RequestId 37: 37

Para eliminar las CRL que expiraron el 22 de enero de 2001: CRL 1/22/2001

[-f] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-backup"></a>-backup

CertUtil [opciones]-backup BackupDirectory [incremental] [KeepLog]

Servicios de certificados Active Directory de copia de seguridad

BackupDirectory: directorio en el que se almacenan los datos de copia de seguridad

Incremental: realizar solo copia de seguridad incremental (el valor predeterminado es copia de seguridad completa)

KeepLog: conservar los archivos de registro de base de datos (el valor predeterminado es truncar los archivos de registro)

[-f] [-config Machine\CAName] [-p contraseña]

[Menú](#menu) volver a

## <a name="-backupdb"></a>-backupDB

CertUtil [opciones]-backupDB BackupDirectory [incremental] [KeepLog]

Copia de seguridad Active Directory base de datos de servicios de certificados

BackupDirectory: directorio en el que se almacenan los archivos de base de datos de copia de seguridad

Incremental: realizar solo copia de seguridad incremental (el valor predeterminado es copia de seguridad completa)

KeepLog: conservar los archivos de registro de base de datos (el valor predeterminado es truncar los archivos de registro)

[-f] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-backupkey"></a>-backupKey

CertUtil [opciones]-backupKey BackupDirectory

Copia de seguridad Active Directory certificado y clave privada del servicio de certificados

BackupDirectory: directorio en el que se almacena la copia de seguridad del archivo PFX

[-f] [-config Machine\CAName] [-p contraseña] [-t tiempo de espera]

[Menú](#menu) volver a

## <a name="-restore"></a>-restaurar

CertUtil [opciones]-restore BackupDirectory

Restaurar Active Directory servicios de Certificate Server

BackupDirectory: directorio que contiene los datos que se van a restaurar

[-f] [-config Machine\CAName] [-p contraseña]

[Menú](#menu) volver a

## <a name="-restoredb"></a>-restoreDB

CertUtil [opciones]-restoreDB BackupDirectory

Restaurar Active Directory base de datos de servicios de certificados

BackupDirectory: directorio que contiene los archivos de base de datos que se van a restaurar

[-f] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-restorekey"></a>-restoreKey

CertUtil [opciones]-restoreKey BackupDirectory | PFXFile

Restaurar Active Directory certificado y la clave privada del servicio de certificados

BackupDirectory: directorio que contiene el archivo PFX que se va a restaurar

PFXFile: Archivo PFX que se va a restaurar

[-f] [-config Machine\CAName] [-p contraseña]

[Menú](#menu) volver a

## <a name="-importpfx"></a>-importPFX

CertUtil [opciones]-importPFX [CertificateStoreName] PFXFile [modificadores]

Importar certificado y clave privada

CertificateStoreName: Nombre del almacén de certificados.  Vea [-Store](#-store).

PFXFile: Archivo PFX que se va a importar

Modificadores Lista separada por comas de uno o varios de los siguientes elementos:

1. AT_SIGNATURE: Cambiar la especificación de especificación a Signatura
2. AT_KEYEXCHANGE: Cambiar la especificación de clave a intercambio de claves
3. Noexport: Convertir la clave privada en no exportable
4. Nocert: No importar el certificado
5. Nochain: No importar la cadena de certificados
6. Noroot: No importar el certificado raíz
7. Proteger Proteger claves con contraseña
8. Noprotect: No proteger claves con contraseña

El valor predeterminado es el almacén del equipo personal.

[-f] [-usuario] [-p contraseña] [-proveedor CSP]

[Menú](#menu) volver a

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [opciones]-dynamicfilelist

Mostrar lista de archivos dinámicos

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-databaselocations"></a>-databaselocations

CertUtil [opciones]-databaselocations

Mostrar ubicaciones de bases de datos

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-hashfile"></a>-hashfile

CertUtil [opciones]-hashfile INFILE [HashAlgorithm]

Generar y mostrar hash criptográfico sobre un archivo

[Menú](#menu) volver a

## <a name="-store"></a>-Store

CertUtil [Options]-Store [CertificateStoreName [CertId [OutputFile]]]

Volcado del almacén de certificados

CertificateStoreName: Nombre del almacén de certificados. Ejemplos:

- "Mi", "CA" (valor predeterminado), "raíz",
- "ldap:///CN=Certification authoritys, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? objectClass = certificationAuthority" (ver certificados raíz)
- "ldap:///CN=CAName,CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? base? objectClass = certificationAuthority" (modificar certificados raíz)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (ver CRL)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? base? objectClass = certificationAuthority" (certificados de CA de empresa)
- LDAP (Certificados de objeto de equipo de AD)
- -LDAP de usuario: (Certificados de objeto de usuario de AD)

Idcert Token de coincidencia de CRL o certificado.  Puede ser un número de serie, un certificado SHA-1, una CRL, una CTL o un hash de clave pública, un índice de certificado numérico (0, 1, etc.), un índice de CRL numérico (. 0,,1, etc.), un índice de CTL numérico (.. 0... 1, etc.), una clave pública, una firma o un ObjectId de extensión, un nombre común de sujeto de certificado, una dirección de correo electrónico, un nombre de UPN o DNS, un nombre de contenedor de claves o un nombre de CSP, un nombre de plantilla o ObjectId, un nombre de proveedor de directivas de aplicación o EKU, o un nombre común de emisor de CRL. Muchos de ellos pueden dar lugar a varias coincidencias.

OutputFile: archivo para guardar el certificado coincidente

Use-User para tener acceso a un almacén de usuario en lugar de a un almacén del equipo.

Use-Enterprise para tener acceso a un almacén empresarial de la máquina.

Use-Service para tener acceso a un almacén del servicio de equipo.

Use-grouppolicy para tener acceso a un almacén de directivas de grupo del equipo.

Ejemplos:

- -Enterprise NTAuth
- -Enterprise raíz 37
- -usuario mi 26e0aaaf000000000004
- CA. 11

[-f] [-Enterprise] [-usuario] [-GroupPolicy] [-SILENT] [-Split] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-addstore"></a>-AddStore

CertUtil [opciones]-AddStore CertificateStoreName INFILE

Agregar certificado al almacén

CertificateStoreName: Nombre del almacén de certificados.  Vea [-Store](#-store).

INFILE Archivo de certificado o CRL que se va a agregar al almacén.

[-f] [-Enterprise] [-usuario] [-GroupPolicy] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-delstore"></a>-delstore

CertUtil [opciones]-delstore CertificateStoreName CertId

Eliminar certificado del almacén

CertificateStoreName: Nombre del almacén de certificados.  Vea [-Store](#-store).

Idcert Token de coincidencia de CRL o certificado.  Vea [-Store](#-store).

[-Enterprise] [-usuario] [-GroupPolicy] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-verifystore"></a>-verifystore

CertUtil [opciones]-verifystore CertificateStoreName [CertId]

Comprobar el certificado en el almacén

CertificateStoreName: Nombre del almacén de certificados.  Vea [-Store](#-store).

Idcert Token de coincidencia de CRL o certificado.  Vea [-Store](#-store).

[-Enterprise] [-usuario] [-GroupPolicy] [-SILENT] [-Split] [-DC Nombrededc] [-t tiempo de espera]

[Menú](#menu) volver a

## <a name="-repairstore"></a>-repairstore

CertUtil [opciones]-repairstore CertificateStoreName CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

Reparar Asociación de claves o actualizar las propiedades del certificado o el descriptor de seguridad de clave

CertificateStoreName: Nombre del almacén de certificados.  Vea [-Store](#-store).

CertIdList: lista separada por comas de tokens de coincidencia de certificados o CRL. Vea [-Store](#-store) CertId Description.

PropertyInfFile: archivo INF que contiene propiedades externas:

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

[-f] [-Enterprise] [-usuario] [-GroupPolicy] [-SILENT] [-Split] [-proveedor CSP]

[Menú](#menu) volver a

## <a name="-viewstore"></a>-viewstore

CertUtil [opciones]-viewstore [CertificateStoreName [CertId [OutputFile]]]

Volcado del almacén de certificados

CertificateStoreName: Nombre del almacén de certificados. Ejemplos:

- "Mi", "CA" (valor predeterminado), "raíz",
- "ldap:///CN=Certification authoritys, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? objectClass = certificationAuthority" (ver certificados raíz)
- "ldap:///CN=CAName,CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? base? objectClass = certificationAuthority" (modificar certificados raíz)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (ver CRL)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? base? objectClass = certificationAuthority" (certificados de CA de empresa)
- LDAP (Certificados de objeto de equipo de AD)
- -LDAP de usuario: (Certificados de objeto de usuario de AD)

Idcert Token de coincidencia de CRL o certificado. Puede ser un número de serie, un certificado SHA-1, una CRL, una CTL o un hash de clave pública, un índice de certificado numérico (0, 1, etc.), un índice de CRL numérico (. 0,,1, etc.), un índice de CTL numérico (.. 0... 1, etc.), una clave pública, una firma o un ObjectId de extensión, un nombre común de sujeto de certificado, una dirección de correo electrónico, un nombre de UPN o DNS, un nombre de contenedor de claves o un nombre de CSP, un nombre de plantilla o ObjectId, un nombre de proveedor de directivas de aplicación o EKU, o un nombre común de emisor de CRL. Muchos de ellos pueden dar lugar a varias coincidencias.

OutputFile: archivo para guardar el certificado coincidente

Use-User para tener acceso a un almacén de usuario en lugar de a un almacén del equipo.

Use-Enterprise para tener acceso a un almacén empresarial de la máquina.

Use-Service para tener acceso a un almacén del servicio de equipo.

Use-grouppolicy para tener acceso a un almacén de directivas de grupo del equipo.

Ejemplos:

1. -Enterprise NTAuth
2. -Enterprise raíz 37
3. -usuario mi 26e0aaaf000000000004
4. CA. 11

[-f] [-Enterprise] [-usuario] [-GroupPolicy] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [opciones]-viewdelstore [CertificateStoreName [CertId [OutputFile]]]

Eliminar certificado del almacén

CertificateStoreName: Nombre del almacén de certificados. Ejemplos:

- "Mi", "CA" (valor predeterminado), "raíz",
- "ldap:///CN=Certification authoritys, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? objectClass = certificationAuthority" (ver certificados raíz)
- "ldap:///CN=CAName,CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? base? objectClass = certificationAuthority" (modificar certificados raíz)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (ver CRL)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? el certificado? base? objectClass = certificationAuthority" (certificados de CA de empresa)
- LDAP (Certificados de objeto de equipo de AD)
- -LDAP de usuario: (Certificados de objeto de usuario de AD)

Idcert Token de coincidencia de CRL o certificado. Puede ser un número de serie, un certificado SHA-1, una CRL, una CTL o un hash de clave pública, un índice de certificado numérico (0, 1, etc.), un índice de CRL numérico (. 0,,1, etc.), un índice de CTL numérico (.. 0... 1, etc.), una clave pública, una firma o un ObjectId de extensión, un nombre común de sujeto de certificado, una dirección de correo electrónico, un nombre de UPN o DNS, un nombre de contenedor de claves o un nombre de CSP, un nombre de plantilla o ObjectId, un nombre de proveedor de directivas de aplicación o EKU, o un nombre común de emisor de CRL. Muchos de ellos pueden dar lugar a varias coincidencias.

OutputFile: archivo para guardar el certificado coincidente

Use-User para tener acceso a un almacén de usuario en lugar de a un almacén del equipo.

Use-Enterprise para tener acceso a un almacén empresarial de la máquina.

Use-Service para tener acceso a un almacén del servicio de equipo.

Use-grouppolicy para tener acceso a un almacén de directivas de grupo del equipo.

Ejemplos:

1. -Enterprise NTAuth
2. -Enterprise raíz 37
3. -usuario mi 26e0aaaf000000000004
4. CA. 11

[-f] [-Enterprise] [-usuario] [-GroupPolicy] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-dspublish"></a>-dsPublish

CertUtil [opciones]-dsPublish CertFile [NTAuthCA | RootCA | Certificación subordinada | CrossCA | KRA | Usuario | Sistema

CertUtil [opciones]-dsPublish CRLFile [DSCDPContainer [DSCDPCN]]

Publicar certificado o CRL en Active Directory

CertFile: archivo de certificado que se va a publicar

NTAuthCA: Publicar certificado en el almacén de DS Enterprise

RootCA Publicar certificado en el almacén raíz de confianza de DS

Certificación subordinada Publicar certificado de CA en objeto de CA de DS

CrossCA: Publicar certificado cruzado en objeto CA de DS

KRA Publicar certificado en el objeto de agente de recuperación de claves de DS

Usuario: Publicar certificado en el objeto DS de usuario

Sistema Publicar certificado en el objeto DS de la máquina

CRLFile Archivo CRL que se va a publicar

DSCDPContainer: CN del contenedor de CDP DS, normalmente el nombre de la máquina de la CA

DSCDPCN: CN del objeto CDP de DS, normalmente basado en el índice de clave y el nombre corto de la CA saneado

Use-f para crear un objeto DS.

[-f] [-usuario] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [opciones]-ADTemplate [plantilla]

Mostrar plantillas de AD

[-f] [-usuario] [-UT] [-MT] [-DC Nombrededc]

## <a name="-template"></a>-Plantilla

CertUtil [opciones]-plantilla [plantilla]

Mostrar plantillas de directiva de inscripción

[-f] [-usuario] [-SILENT] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nombreDeUsuario] [-p contraseña]

[Menú](#menu) volver a

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [opciones]-plantilla TemplateCAs

Mostrar las CA de la plantilla

[-f] [-usuario] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-catemplates"></a>-CATemplates

CertUtil [opciones]-CATemplates [plantilla]

Mostrar plantillas para CA

[-f] [-usuario] [-UT] [-MT] [-config Machine\CAName] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-setcasites"></a>-SetCASites

CertUtil [opciones]-SetCASites [Set] [Nombresitio]

CertUtil [opciones]-SetCASites Verify [Nombresitio]

CertUtil [opciones]-SetCASites eliminar

Establecer, comprobar o eliminar nombres de sitio de CA

- Use la opción-config para tener como destino una sola entidad de certificación (el valor predeterminado es todas las CA)
- *SiteName* solo se permite cuando el destino es una sola CA
- Use-f para invalidar los errores de validación para el *siteName* especificado
- Usar-f para eliminar todos los nombres de sitio de CA

[-f] [-config Machine\CAName] [-DC Nombrededc]

> [!NOTE]
> Para obtener más información acerca de la configuración de las CA para el reconocimiento de sitios de Active Directory Domain Services (AD DS), consulte [reconocimiento de sitios de AD DS para clientes de AD CS y PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

[Menú](#menu) volver a

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [opciones]-enrollmentServerURL [dirección URL AuthenticationType [prioridad] [modificadores]]

CertUtil [opciones]-enrollmentServerURL URL Delete

Mostrar, agregar o eliminar las direcciones URL del servidor de inscripciones asociadas a una CA

AuthenticationType Especifique uno de los siguientes métodos de autenticación de cliente al agregar una dirección URL

1. Kerberos: Usar credenciales SSL de Kerberos
2. Nombre Usar cuenta con nombre para las credenciales SSL
3. ClientCertificate Usar credenciales SSL de certificado X. 509
4. Anonymous: Usar credenciales SSL anónimas

eliminar: elimina la dirección URL especificada asociada a la CA.

Priority: el valor predeterminado es "1" si no se especifica al agregar una dirección URL.

Modificadores: lista separada por comas de uno o varios de los siguientes elementos:

1. AllowRenewalsOnly: Solo se pueden enviar solicitudes de renovación a esta CA a través de esta dirección URL
2. AllowKeyBasedRenewal: Permite el uso de un certificado que no tiene ninguna cuenta asociada en AD. Esto solo se aplica al modo ClientCertificate y AllowRenewalsOnly

[-config Machine\CAName] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-adca"></a>-ADCA

CertUtil [opciones]-ADCA [nombreCA]

Mostrar CA de AD

[-f] [-Split] [-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-ca"></a>-CA

CertUtil [opciones]-CA [Nombredeca | TemplateName

Mostrar CA de directiva de inscripción

[-f] [-usuario] [-SILENT] [-Split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nombreDeUsuario] [-p contraseña]

[Menú](#menu) volver a

## <a name="-policy"></a>-Directiva

Mostrar Directiva de inscripción

[-f] [-usuario] [-SILENT] [-Split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nombreDeUsuario] [-p contraseña]

[Menú](#menu) volver a

## <a name="-policycache"></a>-PolicyCache

CertUtil [opciones]-PolicyCache [DELETE]

Mostrar o eliminar entradas de caché de directiva de inscripción

eliminar: eliminar las entradas de caché del servidor de directivas

-f: Use-f para eliminar todas las entradas de la memoria caché

[-f] [-usuario] [-PolicyServer URLOrId]

[Menú](#menu) volver a

## <a name="-credstore"></a>-CredStore

CertUtil [opciones]-CredStore [URL]

CertUtil [opciones]-CredStore URL Add

CertUtil [opciones]-CredStore URL Delete

Mostrar, agregar o eliminar entradas del almacén de credenciales

URL: dirección URL de destino.  Se \* usa para hacer coincidir todas las entradas. Use https://machine\ * para buscar coincidencias con un prefijo de dirección URL.

Agregar: agregue una entrada al almacén de credenciales. También se deben especificar las credenciales SSL.

eliminar: eliminar entradas del almacén de credenciales

-f: Use-f para sobrescribir una entrada o eliminar varias entradas.

[-f] [-usuario] [-SILENT] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nombreDeUsuario] [-p contraseña]

[Menú](#menu) volver a

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [opciones]-InstallDefaultTemplates

Instalar plantillas de certificado predeterminadas

[-DC Nombrededc]

[Menú](#menu) volver a

## <a name="-urlcache"></a>-URLCache

CertUtil [opciones]-URLCache [URL | CRL | \* [eliminar]]

Mostrar o eliminar entradas de caché de URL

URL: URL almacenada en caché

CRL: funcionar solo en todas las direcciones URL de CRL almacenadas en caché

\*: operar en todas las direcciones URL almacenadas en caché

eliminar: eliminar las direcciones URL relevantes de la caché local del usuario actual

Use-f para forzar la captura de una dirección URL específica y la actualización de la memoria caché.

[-f] [-Split]

[Menú](#menu) volver a

## <a name="-pulse"></a>-Pulse

CertUtil [opciones]-Pulse

Eventos de inscripción automática de impulsos

[-usuario]

[Menú](#menu) volver a

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [opciones]-MachineInfo DomainName\MachineName $

Mostrar información de objeto de equipo de Active Directory

[Menú](#menu) volver a

## <a name="-dcinfo"></a>-DCInfo

CertUtil [opciones]-DCInfo [dominio] [comprobar | DeleteBad | DeleteAll

Mostrar información del controlador de dominio

El valor predeterminado es mostrar los certificados de controlador de dominio sin comprobación.

[-f] [-usuario] [-urlfetch] [-DC Nombrededc] [-t tiempo de espera]

> [!TIP]
> La capacidad de especificar un dominio de Active Directory Domain Services (AD DS) **[dominio]** y para especificar un controlador de dominio ( **-DC**) se ha agregado en Windows Server 2012. Para ejecutar correctamente el comando, debe usar una cuenta que sea miembro del grupo Admins. del **dominio** o **administradores de organización**. Las modificaciones de comportamiento de este comando son las siguientes:</br>> 1.  Si no se especifica un dominio y no se especifica un controlador de dominio específico, esta opción devuelve una lista de controladores de dominio que se van a procesar desde el controlador de dominio predeterminado.</br>> 2.  Si no se especifica un dominio, pero se especifica un controlador de dominio, se genera un informe de los certificados en el controlador de dominio especificado.</br>> 3.  Si se especifica un dominio, pero no se especifica un controlador de dominio, se genera una lista de controladores de dominio junto con los informes de los certificados de cada controlador de dominio de la lista.</br>> 4.  Si se especifican el dominio y el controlador de dominio, se genera una lista de controladores de dominio desde el controlador de dominio de destino. También se genera un informe de los certificados para cada controlador de dominio de la lista.

Por ejemplo, supongamos que hay un dominio denominado CPANDL con un controlador de dominio denominado CPANDL-DC1. Puede ejecutar el siguiente comando para recuperar una lista de controladores de dominio y sus certificados de CPANDL-DC1: certutil-DC CPANDL-DC1-dcinfo CPANDL

[Menú](#menu) volver a

## <a name="-entinfo"></a>-EntInfo

CertUtil [opciones]-EntInfo DomainName\MachineName $

[-f] [-usuario]

[Menú](#menu) volver a

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [opciones]-TCAInfo [DomainDN |-]

Mostrar información de CA

[-f] [-Enterprise] [-usuario] [-urlfetch] [-DC Nombrededc] [-t tiempo de espera]

[Menú](#menu) volver a

## <a name="-scinfo"></a>-SCInfo

CertUtil [opciones]-SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

Mostrar información de tarjeta inteligente

CRYPT_DELETEKEYSET: Eliminar todas las claves de la tarjeta inteligente

[-SILENT] [-Split] [-urlfetch] [-t tiempo de espera]

[Menú](#menu) volver a

## <a name="-scroots"></a>-SCRoots

CertUtil [opciones]-SCRoots Update [+] [InputRootFile] [ReaderName]

CertUtil [opciones]-SCRoots guardar \@OutputRootFile [ReaderName]

CertUtil [opciones]-SCRoots vista [InputRootFile | ReaderName]

CertUtil [opciones]-SCRoots eliminar [ReaderName]

Administrar certificados raíz de tarjeta inteligente

[-f] [-Split] [-p contraseña]

[Menú](#menu) volver a

## <a name="-verifykeys"></a>-verifykeys

CertUtil [opciones]-verifykeys [KeyContainerName CACertFile]

Comprobar el conjunto de claves pública y privada

KeyContainerName: nombre del contenedor de claves de la clave que se va a comprobar. El valor predeterminado es las claves de máquina.  Use-User para las claves de usuario.

CACertFile: firma o archivo de certificado de cifrado

Si no se especifica ningún argumento, se comprueba la clave privada de cada certificado de entidad de certificación de firma.

Esta operación solo se puede realizar en una CA local o en una clave local.

[-f] [-usuario] [-SILENT] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-verify"></a>-comprobar

CertUtil [opciones]-Verify CertFile [ApplicationPolicyList |-[IssuancePolicyList]]

CertUtil [opciones]-Verify CertFile [CACertFile [CrossedCACertFile]]

CertUtil [opciones]-Verify CRLFile CACertFile [IssuedCertFile]

CertUtil [opciones]-Verify CRLFile CACertFile [DeltaCRLFile]

Comprobar certificado, CRL o cadena

Archivo CERTFILE Certificado que se va a comprobar

ApplicationPolicyList: lista opcional separada por comas de la Directiva de aplicación necesaria objectId

IssuancePolicyList: lista opcional separada por comas de la Directiva de emisión requerida objectId

CACertFile: certificado de CA de emisión opcional con el que realizar la comprobación

CrossedCACertFile: certificado opcional con certificación cruzada por CertFile

CRLFile CRL que se va a comprobar

IssuedCertFile: certificado emitido opcional que está incluido en CRLFile

DeltaCRLFile: diferencia CRL opcional

Si se especifica ApplicationPolicyList, la creación de cadenas está restringida a las cadenas válidas para las directivas de aplicación especificadas.

Si se especifica IssuancePolicyList, la creación de cadenas está restringida a las cadenas válidas para las directivas de emisión especificadas.

Si se especifica CACertFile, los campos de CACertFile se comprueban con CertFile o CRLFile.

Si no se especifica CACertFile, CertFile se utiliza para compilar y comprobar una cadena completa.

Si se especifican CACertFile y CrossedCACertFile, los campos de CACertFile y CrossedCACertFile se comprueban con CertFile.

Si se especifica IssuedCertFile, los campos de IssuedCertFile se comprueban con CRLFile.

Si se especifica DeltaCRLFile, los campos de DeltaCRLFile se comprueban con CRLFile.

[-f] [-Enterprise] [-usuario] [-SILENT] [-Split] [-urlfetch] [-t tiempo de espera]

[Menú](#menu) volver a

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [opciones]-verifyCTL CTLObject [CertDir] [CertFile]

Comprobar AuthRoot o CTL de certificados no permitidos

CTLObject: Identifica la CTL que se va a comprobar:

- AuthRootWU: Lea el archivo CAB de AuthRoot y los certificados coincidentes de la memoria caché de la dirección URL. Use-f para descargar desde Windows Update en su lugar.
- DisallowedWU: leer el archivo CAB de certificados no permitidos y el archivo de almacén de certificados no permitido de la memoria caché de la dirección URL.  Use-f para descargar desde Windows Update en su lugar.
- AuthRoot: lectura de la CTL del registro en caché AuthRoot.  Use con-f y un CertFile que no sea de confianza para forzar la actualización del AuthRoot en caché del registro y las CTL de certificados no permitidas.
- No permitido: lectura de certificados no permitidos en caché del registro de lectura. -f tiene el mismo comportamiento que con AuthRoot.
- CTLFileName: archivo o http: ruta de acceso a CTL o CAB

CertDir: carpeta que contiene los certificados que coinciden con las entradas CTL. Una ruta de acceso http: Folder debe terminar con un separador de ruta de acceso. Si una carpeta no se especifica con AuthRoot o no se permite, se buscarán en varias ubicaciones los certificados coincidentes: almacenes de certificados locales, recursos de crypt32. dll y la memoria caché de dirección URL local. Use-f para realizar la descarga desde Windows Update cuando sea necesario. De lo contrario, el valor predeterminado es la misma carpeta o sitio web que CTLObject.

CertFile: archivo que contiene los certificados que se van a comprobar. Los certificados se compararán con las entradas CTL y se mostrarán los resultados de coincidencia. Suprime la mayoría de los resultados predeterminados.

[-f] [-usuario] [-Split]

[Menú](#menu) volver a

## <a name="-sign"></a>-Sign

CertUtil [opciones]-Sign InFileList | SerialNumber | CRL OutFileList [StartDate + DD: HH] [+ SerialNumberList |-SerialNumberList |-ObjectIdList | \@ExtensionFile]

CertUtil [opciones]-Sign InFileList | SerialNumber | CRL OutFileList [#HashAlgorithm] [+ AlternateSignatureAlgorithm |-AlternateSignatureAlgorithm]

Volver a firmar CRL o certificado

InFileList: lista separada por comas de archivos de certificado o CRL para modificar y volver a firmar

SerialNumber Número de serie del certificado que se va a crear. El período de validez y otras opciones no deben estar presentes.

LCR Cree una CRL vacía. El período de validez y otras opciones no deben estar presentes.

OutFileList: lista separada por comas de los archivos de salida de certificados o CRL modificados. El número de archivos debe coincidir con InFileList.

StartDate + DD: HH: nuevo período de validez: opcional Date Plus; período de validez de días y horas opcional; Si se especifican ambos, use un separador de signo más (+). Use "Now [+ DD: HH]" para comenzar en el momento actual. Use "Never" para no tener fecha de expiración (solo para CRL).

SerialNumberList: lista de números de serie separados por comas que se van a agregar o quitar

ObjectIdList: lista de ObjectId de extensión separada por comas que se va a quitar

\@ExtensionFile: Archivo INF que contiene extensiones para actualizar o quitar:

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm Nombre del algoritmo hash precedido por un signo #

AlternateSignatureAlgorithm: especificador de algoritmo de firma alternativo

Un signo menos hace que se quiten los números de serie y las extensiones. Un signo más hace que los números de serie se agreguen a una CRL. Al quitar elementos de una CRL, la lista puede contener tanto números de serie como objectId. Un signo menos antes de AlternateSignatureAlgorithm hace que se use el formato de firma heredado. Un signo más antes de AlternateSignatureAlgorithm hace que se use el formato de firma alternature. Si no se especifica AlternateSignatureAlgorithm, se usa el formato de firma en el certificado o la CRL.

[-nullsign] [-f] [-SILENT] [-CERT CertId]

[Menú](#menu) volver a

## <a name="-vroot"></a>-vroot

CertUtil [opciones]-vroot [eliminar]

Crear o eliminar raíces virtuales web y recursos compartidos de archivos

[Menú](#menu) volver a

## <a name="-vocsproot"></a>-vocsproot

CertUtil [opciones]-vocsproot [DELETE]

Crear o eliminar raíces virtuales web para el proxy web OCSP

[Menú](#menu) volver a

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [opciones]-addEnrollmentServer Kerberos | Nombre de usuario | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

Agregar una aplicación de servidor de inscripciones

Agregue una aplicación de servidor de inscripciones y un grupo de aplicaciones si es necesario para la CA especificada. Este comando no instala archivos binarios ni paquetes. Uno de los siguientes métodos de autenticación con el que el cliente se conecta a un servidor de inscripción de certificados.

- Kerberos: Usar credenciales SSL de Kerberos
- Nombre Usar cuenta con nombre para las credenciales SSL
- ClientCertificate Usar credenciales SSL de certificado X. 509
- AllowRenewalsOnly: Solo se pueden enviar solicitudes de renovación a esta CA a través de esta dirección URL
- AllowKeyBasedRenewal: permite el uso de un certificado que no tiene ninguna cuenta asociada en AD. Esto solo se aplica al modo ClientCertificate y AllowRenewalsOnly.

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [opciones]-deleteEnrollmentServer Kerberos | Nombre de usuario | ClientCertificate

Eliminar una aplicación de servidor de inscripciones

Elimine una aplicación de servidor de inscripciones y un grupo de aplicaciones si es necesario para la CA especificada. Este comando no quita archivos binarios ni paquetes. Uno de los siguientes métodos de autenticación con el que el cliente se conecta a un servidor de inscripción de certificados.

1. Kerberos: Usar credenciales SSL de Kerberos
2. Nombre Usar cuenta con nombre para las credenciales SSL
3. ClientCertificate Usar credenciales SSL de certificado X. 509

[-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [opciones]-addPolicyServer Kerberos | Nombre de usuario | ClientCertificate [KeyBasedRenewal]

Agregar una aplicación de servidor de directivas

Agregue una aplicación de servidor de directivas y un grupo de aplicaciones si es necesario. Este comando no instala archivos binarios ni paquetes. Uno de los siguientes métodos de autenticación con el que el cliente se conecta a un servidor de directivas de certificados:

- Kerberos: Usar credenciales SSL de Kerberos
- Nombre Usar cuenta con nombre para las credenciales SSL
- ClientCertificate Usar credenciales SSL de certificado X. 509
- KeyBasedRenewal: Solo se devuelven al cliente las directivas que contienen plantillas KeyBasedRenewal. Esta marca solo se aplica para la autenticación de nombre de usuario y ClientCertificate.

[Menú](#menu) volver a

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [opciones]-deletePolicyServer Kerberos | Nombre de usuario | ClientCertificate [KeyBasedRenewal]

Eliminar una aplicación de servidor de directivas

Elimine una aplicación de servidor de directivas y un grupo de aplicaciones si es necesario. Este comando no quita archivos binarios ni paquetes. Uno de los siguientes métodos de autenticación con el que el cliente se conecta a un servidor de directivas de certificados:

1. Kerberos: Usar credenciales SSL de Kerberos
2. Nombre Usar cuenta con nombre para las credenciales SSL
3. ClientCertificate Usar credenciales SSL de certificado X. 509
4. KeyBasedRenewal: Servidor de directivas de KeyBasedRenewal

[Menú](#menu) volver a

## <a name="-oid"></a>-OID

CertUtil [opciones]-OID ObjectId [DisplayName | delete [LanguageId [tipo]]]

CertUtil [opciones]-OID GroupId

CertUtil [opciones]-OID AlgId | AlgorithmName [GroupId]

Mostrar ObjectId o establecer nombre para mostrar

- ObjectId--ObjectId para mostrar o agregar el nombre para mostrar
- GroupId: número de GroupId decimal para objectId que se va a enumerar
- AlgId: AlgId hexadecimal de ObjectId para buscar
- AlgorithmName: nombre del algoritmo para ObjectId que se buscará
- DisplayName: nombre para mostrar que se va a almacenar en DS
- eliminar: eliminar nombre para mostrar
- Iddeidioma: ID. de idioma (el valor predeterminado es actual: 1033)
- Tipo: tipo de objeto DS que se va a crear: 1 para plantilla (valor predeterminado), 2 para la Directiva de emisión, 3 para la Directiva de aplicación
- Use-f para crear un objeto DS.

[-f]

[Menú](#menu) volver a

## <a name="-error"></a>-error

CertUtil [opciones]-error ErrorCode

Mostrar texto de mensaje de código de error

[Menú](#menu) volver a

## <a name="-getreg"></a>-getreg

CertUtil [opciones]-getreg [{CA | restore | Directiva | salir | plantilla | inscribir | cadena | PolicyServers} \[ProgId @ no__t-1] [RegistryValueName]

Mostrar valor del registro

California Usar la clave del registro de la CA

restaurar Usar la clave del registro de restauración de la CA

directivas Usar la clave del registro del módulo de directivas

abandonar Usar la clave del registro del primer módulo de salida

plantillas Usar la clave del registro de la plantilla (use-User para plantillas de usuario)

scribiendo Usar la clave del registro de inscripción (use-User para el contexto de usuario)

IAM Usar la clave del registro de configuración de cadena

PolicyServers: Usar la clave del registro de servidores de directivas

Programa Use la Directiva o el ProgId del módulo de salida (nombre de la subclave del registro)

RegistryValueName: nombre del valor del registro (use\*"Name" para prefijar la coincidencia)

Valor: nuevo valor de registro numérico, de cadena o de fecha o nombre de archivo. Si un valor numérico comienza con "+" o "-", los bits especificados en el nuevo valor se establecen o borran en el valor del registro existente.

Si un valor de cadena comienza con "+" o "-", y el valor existente es un valor REG_MULTI_SZ, la cadena se agrega o se quita del valor del registro existente. Para forzar la creación de un valor REG_MULTI_SZ, agregue "\n" al final del valor de la cadena.

Si el valor comienza por "\@", el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, se analiza en su lugar como [Date] [+ |-] [DD: HH]--una fecha opcional más o menos días y horas opcionales. Si se especifican ambos, use un signo más (+) o un separador de signo menos (-). Use "Now + DD: HH" para una fecha relativa a la hora actual.

Utilice "chain\ChainCacheResyncFiletime \@Now" para vaciar de forma eficaz las CRL almacenadas en caché.

[-f] [-usuario] [-GroupPolicy] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-setreg"></a>-setreg

CertUtil [opciones]-setreg [{CA | restore | Directiva | salir | plantilla | inscribir | cadena | PolicyServers} \[ProgId @ no__t-1] valor RegistryValueName

Establecer valor del registro

California Usar la clave del registro de la CA

restaurar Usar la clave del registro de restauración de la CA

directivas Usar la clave del registro del módulo de directivas

abandonar Usar la clave del registro del primer módulo de salida

plantillas Usar la clave del registro de la plantilla (use-User para plantillas de usuario)

scribiendo Usar la clave del registro de inscripción (use-User para el contexto de usuario)

IAM Usar la clave del registro de configuración de cadena

PolicyServers: Usar la clave del registro de servidores de directivas

Programa Use la Directiva o el ProgId del módulo de salida (nombre de la subclave del registro)

RegistryValueName: nombre del valor del registro (use\*"Name" para prefijar la coincidencia)

Valor: nuevo valor de registro numérico, de cadena o de fecha o nombre de archivo. Si un valor numérico comienza con "+" o "-", los bits especificados en el nuevo valor se establecen o borran en el valor del registro existente.

Si un valor de cadena comienza con "+" o "-", y el valor existente es un valor REG_MULTI_SZ, la cadena se agrega o se quita del valor del registro existente. Para forzar la creación de un valor REG_MULTI_SZ, agregue "\n" al final del valor de la cadena.

Si el valor comienza por "\@", el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, se analiza en su lugar como [Date] [+ |-] [DD: HH]--una fecha opcional más o menos días y horas opcionales. Si se especifican ambos, use un signo más (+) o un separador de signo menos (-). Use "Now + DD: HH" para una fecha relativa a la hora actual.

Utilice "chain\ChainCacheResyncFiletime \@Now" para vaciar de forma eficaz las CRL almacenadas en caché.

[-f] [-usuario] [-GroupPolicy] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-delreg"></a>-delreg

CertUtil [opciones]-delreg [{CA | restore | Directiva | salir | plantilla | inscribir | cadena | PolicyServers} \[ProgId @ no__t-1] [RegistryValueName]

Eliminar valor del registro

California Usar la clave del registro de la CA

restaurar Usar la clave del registro de restauración de la CA

directivas Usar la clave del registro del módulo de directivas

abandonar Usar la clave del registro del primer módulo de salida

plantillas Usar la clave del registro de la plantilla (use-User para plantillas de usuario)

scribiendo Usar la clave del registro de inscripción (use-User para el contexto de usuario)

IAM Usar la clave del registro de configuración de cadena

PolicyServers: Usar la clave del registro de servidores de directivas

Programa Use la Directiva o el ProgId del módulo de salida (nombre de la subclave del registro)

RegistryValueName: nombre del valor del registro (use\*"Name" para prefijar la coincidencia)

Valor: nuevo valor de registro numérico, de cadena o de fecha o nombre de archivo. Si un valor numérico comienza con "+" o "-", los bits especificados en el nuevo valor se establecen o borran en el valor del registro existente.

Si un valor de cadena comienza con "+" o "-", y el valor existente es un valor REG_MULTI_SZ, la cadena se agrega o se quita del valor del registro existente. Para forzar la creación de un valor REG_MULTI_SZ, agregue "\n" al final del valor de la cadena.

Si el valor comienza por "\@", el resto del valor es el nombre del archivo que contiene la representación de texto hexadecimal de un valor binario. Si no hace referencia a un archivo válido, se analiza en su lugar como [Date] [+ |-] [DD: HH]--una fecha opcional más o menos días y horas opcionales. Si se especifican ambos, use un signo más (+) o un separador de signo menos (-). Use "Now + DD: HH" para una fecha relativa a la hora actual.

Utilice "chain\ChainCacheResyncFiletime \@Now" para vaciar de forma eficaz las CRL almacenadas en caché.

[-f] [-usuario] [-GroupPolicy] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-importkms"></a>-ImportKMS

CertUtil [opciones]-ImportKMS UserKeyAndCertFile [CertId]

Importar claves de usuario y certificados en la base de datos del servidor para el archivo de claves

UserKeyAndCertFile: el archivo de datos que contiene las claves privadas del usuario y los certificados que se van a archivar.  Puede ser cualquiera de los siguientes:

- Archivo de exportación del servidor de administración de claves (KMS) de Exchange
- Archivo PFX

Idcert Token de coincidencia de certificado de descifrado de exportación de KMS.  Vea [-Store](#-store).

Use-f para importar certificados no emitidos por la CA.

[-f] [-SILENT] [-Split] [-config Machine\CAName] [-p contraseña] [-symkeyalg SymmetricKeyAlgorithm [, KeyLength]]

[Menú](#menu) volver a

## <a name="-importcert"></a>-ImportCert

CertUtil [opciones]-ImportCert CERTFILE [ExistingRow]

Importar un archivo de certificado en la base de datos

Use ExistingRow para importar el certificado en lugar de una solicitud pendiente para la misma clave.

Use-f para importar certificados no emitidos por la CA.

Es posible que la CA también necesite configurarse para admitir la importación de certificados externos: certutil-setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f] [-config Machine\CAName]

[Menú](#menu) volver a

## <a name="-getkey"></a>-GetKey

CertUtil [opciones]-GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [opciones]-GetKey SearchToken script OutputScriptFile

CertUtil [opciones]-GetKey SearchToken recuperar | recuperación de OutputFileBaseName

Recuperar el BLOB de recuperación de clave privada archivada, generar un script de recuperación o recuperar claves archivadas

script: genera un script para recuperar y recuperar claves (comportamiento predeterminado si se encuentran varios candidatos de recuperación coincidentes, o si no se especifica el archivo de salida).

recuperar: recupera uno o varios blobs de recuperación de claves (comportamiento predeterminado si se encuentra exactamente un candidato de recuperación coincidente y si se especifica el archivo de salida)

recuperar: recuperar y recuperar claves privadas en un solo paso (requiere claves privadas y certificados de agente de recuperación de claves)

SearchToken: Se usa para seleccionar las claves y los certificados que se van a recuperar.

Puede ser cualquiera de los siguientes:

1. Nombre común del certificado
2. Número de serie del certificado
3. Hash SHA-1 de certificado (huella digital)
4. Hash SHA-1 de certificado (identificador de clave de sujeto)
5. Nombre del solicitante (dominio\usuario)
6. UPN (dominio\@de usuario)

RecoveryBlobOutFile: archivo de salida que contiene una cadena de certificados y una clave privada asociada, que todavía se ha cifrado en uno o varios certificados de agente de recuperación de claves.

OutputScriptFile: archivo de salida que contiene un script por lotes para recuperar y recuperar las claves privadas.

OutputFileBaseName: nombre base del archivo de salida. Para la recuperación, se trunca cualquier extensión y se anexa una cadena específica del certificado y la extensión. REC para cada BLOB de recuperación de claves.  Cada archivo contiene una cadena de certificados y una clave privada asociada, todavía cifradas a uno o varios certificados de agente de recuperación de claves. Para la recuperación, se trunca cualquier extensión y se anexa la extensión. p12.  Contiene las cadenas de certificado recuperadas y las claves privadas asociadas, almacenadas como un archivo PFX.

[-f] [-UnicodeText] [-SILENT] [-config Machine\CAName] [-p contraseña] [-Protectto SAMNameAndSIDList] [-proveedor CSP]

[Menú](#menu) volver a

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [opciones]-RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

Recuperar clave privada archivada

[-f] [-usuario] [-SILENT] [-Split] [-p contraseña] [-Protectto SAMNameAndSIDList] [-proveedor CSP] [-t tiempo de espera]

[Menú](#menu) volver a

## <a name="-mergepfx"></a>-MergePFX

CertUtil [opciones]-MergePFX PFXInFileList PFXOutFile [ExtendedProperties]

PFXInFileList: Lista de archivos de entrada PFX separados por comas

PFXOutFile: Archivo de salida PFX

ExtendedProperties Incluir propiedades extendidas

La contraseña especificada en la línea de comandos es una lista de contraseñas separadas por comas.  Si se especifica más de una contraseña, se usa la última contraseña para el archivo de salida.  Si solo se proporciona una contraseña o si la última contraseña es "\*", se solicitará al usuario la contraseña del archivo de salida.

[-f] [-usuario] [-Split] [-p contraseña] [-Protectto SAMNameAndSIDList] [-proveedor CSP]

[Menú](#menu) volver a

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [opciones]-ConvertEPF PFXInFileList EPFOutFile [Cast | Cast-] [V3CACertId] [, sal]

Convertir archivos PFX en archivo EPF

PFXInFileList: Lista de archivos de entrada PFX separados por comas

EPF Archivo de salida de EPF

vía Usar cifrado de conversión 64

conversión: Usar cifrado de conversión 64 (exportar)

V3CACertId: Token de coincidencia de certificado de CA V3.  Vea [-Store](#-store) CertId Description.

Salt Cadena Salt del archivo de salida de EPF

La contraseña especificada en la línea de comandos es una lista de contraseñas separadas por comas. Si se especifica más de una contraseña, se usa la última contraseña para el archivo de salida.  Si solo se proporciona una contraseña o si la última contraseña es "\*", se solicitará al usuario la contraseña del archivo de salida.

[-f] [-SILENT] [-Split] [-DC Nombrededc] [-p contraseña] [-proveedor CSP]

[Menú](#menu) volver a

## <a name="options"></a>Opciones

En esta sección se definen las opciones que se pueden especificar con el comando.

|Opciones|Descripción|
|-------|-----------|
|-nullsign|Usar hash de datos como Signatura|
|-f|Forzar sobrescritura|
|-Enterprise|Usar el almacén de certificados del registro de la máquina local|
|-usuario|Usar claves de HKEY_CURRENT_USER o el almacén de certificados|
|-GroupPolicy|Usar directiva de grupo almacén de certificados|
|-UT|Mostrar plantillas de usuario|
|-MT|Mostrar plantillas de equipo|
|-Unicode|Escribir la salida redirigida en Unicode|
|-UnicodeText|Escribir archivo de salida en Unicode|
|-GMT|Mostrar horas como GMT|
|-segundos|Mostrar horas con segundos y milisegundos|
|-silencioso|Usar marca silenciosa para adquirir el contexto de cifrado|
|-División|Dividir elementos ASN. 1 incrustados y guardar en archivos|
|-v|Operación detallada|
|-PrivateKey|Mostrar datos de contraseña y de clave privada|
|anclar PIN|PIN de tarjeta inteligente|
|-urlfetch|Recuperación y comprobación de certificados AIA y CRL de CDP|
|-config Machine\CAName|Cadena de nombre de equipo y CA|
|-PolicyServer URLOrId|Identificador o dirección URL del servidor de directivas. Para la selección U/I, use-PolicyServer. Para todos los servidores de directivas, use-PolicyServer\*|
|-Anónimo|Usar credenciales SSL anónimas|
|-Kerberos|Usar credenciales SSL de Kerberos|
|-ClientCertificate ClientCertId|Use las credenciales SSL del certificado X. 509. Para la selección U/I, use-clientCertificate.|
|-UserName nombredeusuario|Use una cuenta con nombre para las credenciales SSL. Para la selección U/I, use-UserName.|
|-CERT CertId|Certificado de firma|
|-controlador de dominio Nombrededc|Establecer como destino un controlador de dominio específico|
|-Restrict RestrictionList|Lista de restricciones separadas por comas. Cada restricción consta de un nombre de columna, un operador relacional y un entero constante, una cadena o una fecha. Un nombre de columna puede ir precedido de un signo más o menos para indicar el criterio de ordenación. Ejemplos:</br>"RequestId = 47"</br>"+ RequesterName > = a, RequesterName < b"</br>"-RequesterName > dominio, disposición = 21"|
|-out ColumnList|Lista de columnas separadas por comas|
|-p contraseña|Contraseña|
|-Protectto SAMNameAndSIDList|Lista de SID/nombre SAM separados por comas|
|-Proveedor de CSP|Proveedor|
|-t tiempo de espera|Tiempo de espera de captura de URL en milisegundos|
|-symkeyalg SymmetricKeyAlgorithm [, KeyLength]|Nombre del algoritmo de clave simétrica con una longitud de clave opcional, ejemplo: AES, 128 o 3DES|

[Menú](#menu) volver a

## <a name="additional-certutil-examples"></a>Ejemplos de certutil adicionales

Para ver algunos ejemplos de cómo usar este comando, consulte

1. [Ejemplos de certutil para administrar Active Directory servicios de Certificate Server (AD CS) desde la línea de comandos](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [Tareas certutil para administrar certificados](https://technet.microsoft.com/library/cc772898.aspx)
3. [Exportación de solicitud binaria mediante el tutorial de la herramienta de línea de comandos CertUtil. exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [Renovación del certificado de CA raíz](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

[Menú](#menu) volver a
