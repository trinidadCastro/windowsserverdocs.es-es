---
title: Preparar el archivo CAPolicy.inf
description: CAPolicy.inf contiene distintas opciones que se usan al instalar el servicio de certificación de Active Directory (AD CS) o cuando se renueva la entidad de certificación certificado.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9618d4abe512b487f4f22ffde85a052c1c52ef22
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2018
---
# <a name="capolicyinf-syntax"></a>Sintaxis de CAPolicy.inf
>   Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

CAPolicy.inf es un archivo de configuración que define las extensiones, restricciones y otras opciones de configuración que se aplican a un certificado de entidad emisora de certificados raíz y todos los certificados emitidos por la CA raíz. El archivo CAPolicy.inf debe instalarse en un servidor de host antes de la rutina de instalación para la raíz de la que entidad de certificación comienza. Cuando las restricciones de seguridad en una entidad de certificación raíz que quieres modificar, debe renovarse el certificado raíz y un archivo CAPolicy.inf actualizado debe instalarse en el servidor antes de que comience el proceso de renovación.

CAPolicy.inf es:

-   Crea y define manualmente un administrador

-   Usa durante la creación de raíz y certificados de CA subordinados

-   Definido en la CA de firma que iniciar sesión y emitir el certificado (no la CA donde se concede la solicitud)

Cuando hayas creado tu archivo CAPolicy.inf, tienes que copiar en el **% systemroot %** carpeta del servidor antes de instalar ADC o renovar el certificado de CA.

CAPolicy.inf hace posible especificar y configurar una amplia variedad de atributos de entidad emisora de certificados y las opciones. La siguiente sección describe todas las opciones para crear un archivo .inf adaptado a tus necesidades específicas.

## <a name="capolicyinf-file-structure"></a>Estructura de archivo CAPolicy.inf

Los siguientes términos se usan para describir la estructura de archivos .inf:

-   _Sección_ – es un área del archivo que se trata de un grupo lógico de claves. Los nombres de sección en archivos .inf se identifican mediante aparezca entre corchetes. Muchos, pero no para todas las secciones se usan para configurar las extensiones de certificado.

-   _Clave_ : es el nombre de una entrada y que aparece a la izquierda del signo igual.

-   _Valor_ : es el parámetro y aparece a la derecha del signo igual.

En el ejemplo siguiente, **[versión]** está la sección **firma** es la clave y **"\ $ Windows NT \ $"** es el valor.

Ejemplo:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Versión

Identifica el archivo como un archivo .inf. Versión es la única sección obligatoria y debe estar al principio del archivo CAPolicy.inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Enumera las directivas que se han definido por la organización, y si son obligatorios u opcionales. Varias directivas están separadas por comas. Los nombres tienen sentido en el contexto de una implementación específica, o en relación con aplicaciones personalizadas que comprobar la presencia de estas directivas.

Para cada directiva definida, debe haber una sección que define la configuración de esta directiva específica. Para cada directiva, debes proporcionar un identificador de objeto definido por el usuario (OID) y el texto que quieras que aparece como la instrucción de directiva o un puntero de la dirección URL a la instrucción de directiva. La dirección URL puede estar en el formulario de HTTP, FTP o dirección URL de LDAP.

Si vas a tener texto descriptivo en la instrucción de directiva, a continuación, las tres siguientes líneas de CAPolicy.inf sería:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Si vas a usar una dirección URL para hospedar la instrucción de directiva de entidad emisora de certificados, tres siguientes líneas en lugar de ello podría ser como:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Además:

-   Se admiten varias teclas de dirección URL y el aviso.

-   Las claves de aviso y la dirección URL en la misma sección Directiva son compatibles.

-   Direcciones URL con espacios o texto con espacios debe estar entre comillas. Esto es así para el **URL** clave, independientemente de la sección en el que aparece.

Sería un ejemplo de varios avisos y las direcciones URL en una sección de directiva:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Puedes especificar puntos de distribución CRL (CDP) para un certificado de CA de raíz en CAPolicy.inf. Después de haber instalado la entidad de certificación, puedes configurar las direcciones URL de CDP que la entidad de certificación que se incluye en cada certificado emitido. Las direcciones URL que se especifican en esta sección del archivo CAPolicy.inf se incluyen en el mismo certificado de CA de raíz.

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Más información acerca de esta sección:

-   Se admiten varias direcciones URL.

-   Se admiten las direcciones URL LDAP, HTTP y FTP. No se admiten las URL HTTPS.

-   Esta sección solo se usa si está configurando una CA de raíz o renovación del certificado de CA de raíz. Las extensiones de CDP de CA subordinadas viene determinadas por la CA que emite el certificado de CA subordinadas.

-   Las direcciones URL con espacios deben estar entre comillas.

-   Si no se especifica ninguna dirección URL, es decir, si la **[CRLDistributionPoint]** sección existe en el archivo, pero está vacía, se omite la extensión de punto de distribución CRL en el certificado de CA de raíz. Normalmente esto es preferible cuando la configuración de una CA de raíz. Windows no puede efectuar comprobar la revocación de un certificado de CA de raíz, por lo que la extensión CDP es lo superflua en un certificado de CA de raíz.

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Puedes especificar los puntos de acceso de la información de entidad en CAPolicy.inf para el certificado de CA de raíz.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Algunas notas adicionales en la sección de acceso a información de entidad:

-   Se admiten varias direcciones URL.

-   HTTP, FTP, LDAP y direcciones URL de archivos son compatibles. No se admiten las URL HTTPS.

-   Esta sección solo se usa si está configurando una CA de raíz o renovación del certificado de CA de raíz. Las extensiones AIA de CA subordinadas viene determinadas por la CA que emitió el certificado de CA subordinadas.

-   Las direcciones URL con espacios deben estar entre comillas.

-   Si no se especifica ninguna dirección URL, es decir, si la **[AuthorityInformationAccess]** sección existe en el archivo, pero está vacía, se omite la extensión de punto de distribución CRL en el certificado de CA de raíz. De nuevo, esto sería la opción preferida en el caso de un certificado de CA de raíz porque no hay ninguna autoridad superior a una entidad de certificación que tendría que hacer referencia a un vínculo a su certificado raíz.

### <a name="certsrvserver"></a>certsrv_Server

Otra sección opcional de CAPolicy.inf es [certsrv_server], que se usa para especificar la longitud de clave de renovación, el período de validez de renovación y el período de validez certificados (CRL) de la lista de revocación de una entidad emisora de certificados se renueva o instalado. Ninguna de las teclas en esta sección son necesaria. Muchas de estas opciones de configuración tienen valores predeterminados que son suficientes para la mayoría de las necesidades y el archivo CAPolicy.inf simplemente se pueden omitir. Como alternativa, muchas de estas opciones de configuración se pueden cambiar después de haber instalado la CA.

Un ejemplo sería:

```
[certsrv_server]
RenewalKeyLength=2048
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=5
CRLPeriod=Days
CRLPeriodUnits=2
CRLDeltaPeriod=Hours
CRLDeltaPeriodUnits=4
ClockSkewMinutes=20
LoadDefaultTemplates=True
AlternateSignatureAlgorithm=0
ForceUTF8=0
EnableKeyCounting=0
```

**RenewalKeyLength** establece el tamaño de renovación solo clave. Esto solo se usa cuando se genera un nuevo par de claves durante la renovación de certificado de CA. Cuando se instala la CA, se establece el tamaño de la clave para el certificado de CA inicial.

Cuando renovar un certificado de entidad emisora de certificados con un nuevo par de claves, la longitud de clave puede aumenta o disminuye. Por ejemplo, si has configurado una raíz de tamaño de clave de entidad emisora de certificados de 4096 bytes o superior y, a continuación, descubrir tienes Java aplicaciones o dispositivos de red que solo admite tamaños de clave de 2048 bytes. Si aumentan o disminuir el tamaño, debe volver a emitir todos los certificados emitidos por esa entidad emisora.

**RenewalValidityPeriod** y **RenewalValidityPeriodUnits** establecer la duración del nuevo certificado de CA de raíz cuando se renueva el antiguo certificado de CA de raíz. Solo se aplica a una CA de raíz. La duración del certificado de una CA subordinada viene determinada por su superior. RenewalValidityPeriod puede tener los siguientes valores: horas, días, semanas, meses y años.

**CRLPeriod** y **CRLPeriodUnits** establecer el período de validez de la CRL de base. **CRLPeriod** puede tener los siguientes valores: horas, días, semanas, meses y años.

**CRLDeltaPeriod** y **CRLDeltaPeriodUnits** establecer el período de validez de las diferencias CRL. **CRLDeltaPeriod** puede tener los siguientes valores: horas, días, semanas, meses y años.

Cada una de estas opciones de configuración puede configurarse después de haber instalado la entidad de certificación:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

Recuerda que tienes que reiniciar los servicios de certificados de Active Directory para que los cambios surtan efecto.

**LoadDefaultTemplates** solo se aplica durante la instalación de una CA de empresa. Esta configuración, ya sea True o False (o 1 o 0), determina si la entidad de certificación está configurado con cualquiera de las plantillas predeterminadas.

En una instalación predeterminada de la CA, se agrega un subconjunto de las plantillas de certificado predeterminado a la carpeta Plantillas de certificado en el complemento entidad de certificación. Esto significa que en cuanto se inicie el servicio de AD CS después de haber instalado el rol de un usuario o equipo con los permisos adecuados puede inmediatamente inscribir un certificado.

Es podrán que no quieras emitir los certificados inmediatamente después de que se ha instalado una entidad de certificación, así que puedes usar la configuración de LoadDefaultTemplates para impedir que las plantillas predeterminadas que se agregan a la CA de empresa. Si no hay ninguna plantilla configurada en la CA no puede emitir ningún certificado.

**AlternateSignatureAlgorithm** configura la CA para admitir el formato de firma PKCS\ #1 V2.1 para el certificado de CA y solicitudes de certificados. Cuando se establece en 1 en una raíz de la CA el certificado de CA incluirá el formato de firma PKCS\ #1 V2.1. Cuando se establece en subordinada CA, la CA subordinada creará una solicitud de certificado incluye el formato de firma PKCS\ #1 V2.1.

**ForceUTF8** cambia el valor predeterminado de codificación de nombres relativos distintivos (DN) en asunto y emisor nombres UTF-8 completos. Solo esos DN que admiten UTF-8, como las que están definidas como tipos de cadena del directorio con una solicitud de cambio, se ven afectados. Por ejemplo, el RDN para el componente de dominio (DC) admite la codificación IA5 o UTF-8, mientras que la Country RDN (C) solo admite la codificación como una cadena de impresión. La directiva ForceUTF8 afectará, por tanto, un RDN DC, pero no afectará a un RDN C.

**EnableKeyCounting** configura la CA para agregar un contador cada vez que se usa la clave de firma de la CA. A menos que tengas un módulo de seguridad de Hardware (HSM) y un proveedor de servicios criptográficos asociados (CSP) que admite el recuento de clave, no habilites a esta configuración. El CSP de Microsoft seguro ni el recuento clave de Microsoft proveedor de almacenamiento de claves (KSP) de Software soporte técnico.


## <a name="create-the-capolicyinf-file"></a>Crear el archivo CAPolicy.inf

Antes de instalar AD CS, configurar el archivo CAPolicy.inf con una configuración específica para la implementación.

**Requisito previo:** debe ser miembro del grupo de administradores.

1.  En el equipo donde vas a instalar AD CS, abre Windows PowerShell, escribe **el Bloc de notas c:.inf** y presiona ENTRAR.

2.  Cuando se pida que cree un nuevo archivo, haz clic en **Sí **.

3.  Como el contenido del archivo, escribe lo siguiente:
   ```
   [Version]  
    Signature="$Windows NT$"  
    [PolicyStatementExtension]  
    Policies=InternalPolicy  
    [InternalPolicy]  
    OID=1.2.3.4.1455.67.89.5  
    Notice="Legal Policy Statement"  
    URL=http://pki.corp.contoso.com/pki/cps.txt  
    [Certsrv_Server]  
    RenewalKeyLength=2048  
    RenewalValidityPeriod=Years  
    RenewalValidityPeriodUnits=5  
    CRLPeriod=weeks  
    CRLPeriodUnits=1  
    LoadDefaultTemplates=0  
    AlternateSignatureAlgorithm=1  
    [CRLDistributionPoint]  
    [AuthorityInformationAccess]
   ```
1.  Haz clic en **archivo**y, a continuación, haz clic en **Guardar como **.

2.  Navega a la carpeta % systemroot %.

3.  Asegúrate de lo siguiente:

    -   **Nombre de archivo** se establece en **CAPolicy.inf**

    -   **Guardar como tipo** se establece en **todos los archivos**

    -   **Codificación** es **ANSI**

4.  Haz clic en **guardar**.

5.  Cuando se te pida para sobrescribir el archivo, haz clic en **Sí **.

    ![Guardar como ubicación para el archivo CAPolicy.inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   Asegúrate de guardar CAPolicy.inf con la extensión de inf. Si no se escribe específicamente **.inf** al final del nombre de archivo y selecciona las opciones como se describe el archivo se guardará como un archivo de texto y no se usará durante la instalación de la CA.

6.  Cierra el Bloc de notas.

>   [!IMPORTANT]  
>   En CAPolicy.inf, puedes ver hay una línea especifica la dirección URL http://pki.corp.contoso.com/pki/cps.txt. La sección Directiva interno de CAPolicy.inf solo se muestra como un ejemplo de cómo debe especificar la ubicación de una instrucción de procedimiento de certificado (CPS). En esta guía, que no se indique para crear la declaración de práctica de certificado (CPS).
