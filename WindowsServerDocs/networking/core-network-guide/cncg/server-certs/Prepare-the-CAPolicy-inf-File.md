---
title: Preparar el archivo CAPolicy.inf
description: El archivo CAPolicy.inf contiene varias opciones que se usan al instalar el servicio de certificación de Active Directory (AD CS) o cuando se renueva el certificado de CA.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 80c7155224502379e2e9618ceb38709c5051a6b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857846"
---
# <a name="capolicyinf-syntax"></a>Sintaxis de CAPolicy.inf
>   Se aplica a: Windows Server (canal semianual), Windows Server 2016

El archivo CAPolicy.inf es un archivo de configuración que define las extensiones, restricciones y otras opciones de configuración que se aplican a un certificado de CA raíz y todos los certificados emitidos por la CA raíz. El archivo CAPolicy.inf debe instalarse en un servidor de host antes de la rutina de configuración para la raíz de que CA comienza. Cuando las restricciones de seguridad en una CA raíz que se van a modificarse y se debe renovar el certificado raíz y un archivo CAPolicy.inf actualizado debe instalarse en el servidor antes de que comience el proceso de renovación.

El archivo CAPolicy.inf es:

-   Crea y define manualmente por un administrador

-   Utilizado durante la creación de raíz y certificados de CA subordinada

-   Definido en la entidad de certificación firma donde inicie sesión y emitir el certificado (no la entidad emisora de certificados donde se concede la solicitud)

Una vez haya creado el archivo CAPolicy.inf, debe copiarla en el **% systemroot %** carpeta del servidor antes de instalar ADCS o renovar el certificado de CA.

El archivo CAPolicy.inf hace posible especificar y configurar una amplia variedad de opciones y los atributos de entidad de certificación. La siguiente sección describe todas las opciones para crear un archivo .inf adaptado a sus necesidades específicas.

## <a name="capolicyinf-file-structure"></a>Estructura del archivo CAPolicy.inf

Los siguientes términos se utilizan para describir la estructura del archivo .inf:

-   _Sección_ : es un área del archivo que se trata de un grupo lógico de claves. Los nombres de sección en los archivos .inf se identifican por la que aparecen entre corchetes. Aunque no todas, muchas secciones sirven para configurar las extensiones de certificado.

-   _Clave_ : es el nombre de una entrada y que aparece a la izquierda del signo igual.

-   _Valor_ : es el parámetro y que aparece a la derecha del signo igual.

En el ejemplo siguiente, **[versión]** está la sección **firma** es la clave, y **"\$Windows NT\$"** es el valor.

Por ejemplo:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Versión

Identifica el archivo como un archivo .inf. Versión es la única sección obligatoria y debe estar al principio del archivo CAPolicy.inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Enumera las directivas que se han definido por la organización, y si se han opcional u obligatorio. Varias directivas están separadas por comas. Los nombres tienen un significado en el contexto de una implementación específica, o en relación con las aplicaciones personalizadas que comprueban la presencia de estas directivas.

Para cada directiva definida, debe haber una sección que define la configuración de esa directiva en particular. Para cada directiva, debe proporcionar un identificador de objeto definido por el usuario (OID) y el texto desea que aparezca como la instrucción de directiva o un puntero de dirección URL a la instrucción de directiva. La dirección URL puede ser en forma de un HTTP, FTP o dirección URL de LDAP.

Si va a tener un texto descriptivo en la instrucción de directiva, a continuación, las tres líneas siguientes del archivo CAPolicy.inf sería:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Si va a usar una dirección URL para hospedar la instrucción de directiva de entidad emisora de certificados, a continuación, tres líneas siguientes en su lugar, sería:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Además:

-   Se admiten varias teclas de dirección URL y tenga en cuenta.

-   Se admiten claves de aviso y la dirección URL en la misma sección directivas.

-   Las direcciones URL con espacios o texto con espacios debe ir entre comillas. Esto es cierto para el **URL** clave, independientemente de la sección en la que aparece.

Un ejemplo de varios avisos y direcciones URL en una sección Directiva sería:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Puede especificar puntos de distribución de CRL (CDP) de un certificado de CA raíz en el archivo CAPolicy.inf.  Después de instalar la CA, puede configurar las direcciones URL de CDP que incluye la entidad de certificación en cada certificado emitido. El certificado de CA raíz muestra las direcciones URL especificadas en esta sección del archivo CAPolicy.inf. 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Información adicional acerca de esta sección:
-   compatible con:
    - HTTP 
    - Direcciones URL de archivo
    - Direcciones URL LDAP 
    - Varias direcciones URL
   
    >[!IMPORTANT]
    >No admite direcciones URL HTTPS.

-   Comillas deben delimitar las direcciones URL con espacios.

-   Si no se especifica ninguna dirección URL: es decir, si la **[punto de distribución CRL]** sección existe en el archivo, pero está vacía; se omite la extensión de acceso a información de entidad emisora del certificado de CA raíz. Normalmente esto es preferible al configurar una CA raíz. Windows no realizan la comprobación de revocación de un certificado de CA raíz, por lo que la extensión CDP es superflua en un certificado de CA raíz.

-    Entidad emisora de certificados puede publicar a una ruta UNC del archivo, por ejemplo, en un recurso compartido que representa la carpeta de un sitio Web donde un cliente recupera a través de HTTP.

-   Solo use esta sección si va a configurar una CA raíz o renovar el certificado de CA raíz. La CA determina las extensiones de CDP CA subordinadas.
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Puede especificar los puntos de acceso de la información de entidad en el archivo CAPolicy.inf para el certificado de CA raíz.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Algunas notas adicionales en la sección de acceso de la información de entidad:

-   Se admiten varias direcciones URL.

-   Se admiten HTTP, FTP, LDAP y direcciones URL de archivo. No se admiten direcciones URL HTTPS.

-   En esta sección solo se utiliza si está configurando una CA raíz o renovar el certificado de CA raíz. Las extensiones AIA CA subordinadas se determinan por la CA que emitió el certificado de CA subordinada.

-   Las direcciones URL con espacios deben ir entre comillas.

-   Si no se especifica ninguna dirección URL: es decir, si la **[AuthorityInformationAccess]** sección existe en el archivo, pero está vacía; se omite la extensión de punto de distribución CRL del certificado de CA raíz. Nuevamente, esto sería el valor preferido en el caso de un certificado de CA raíz porque no hay ninguna entidad superior de una CA raíz que necesitaría para hacer referencia a un vínculo a su certificado.

### <a name="certsrvserver"></a>certsrv_Server

Otra sección opcional del archivo CAPolicy.inf es [certsrv_server], que se usa para especificar la longitud de clave de renovación, el período de validez de renovación y el período de validez (CRL) de la lista de revocación de certificados para una CA que se va a renovar o instalado. Ninguna de las claves en esta sección son necesaria. Muchas de estas configuraciones tienen valores predeterminados son suficientes para la mayoría de las necesidades y simplemente se pueden omitir en el archivo CAPolicy.inf. Como alternativa, muchas de estas opciones se pueden cambiar después de haber instalado la entidad de certificación.

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

**RenewalKeyLength** establece el tamaño de clave para la renovación solo. Sólo se utiliza cuando se genera un nuevo par de claves durante la renovación de certificados de CA. El tamaño de clave para el certificado de CA inicial se establece cuando la CA está instalada.

Cuando se renueva un certificado de CA con un nuevo par de claves, la longitud de clave puede incrementar o disminuir. Por ejemplo, si ha establecido una raíz de CA tamaño de la clave de 4096 bytes o superior y, a continuación, descubre que tiene aplicaciones de Java o dispositivos de red que solo pueden admitir tamaños de clave de 2048 bytes. Si aumentar o disminuir el tamaño, debe volver a emitir todos los certificados emitidos por esa CA.

**RenewalValidityPeriod** y **RenewalValidityPeriodUnits** establecer la duración del nuevo certificado de CA raíz cuando se renueva el certificado de CA raíz anterior. Solo se aplica a una CA raíz. La duración del certificado de CA subordinada viene determinada por su superior. RenewalValidityPeriod puede tener los siguientes valores: Horas, días, semanas, meses y años.

**CRLPeriod** y **CRLPeriodUnits** establecer el período de validez de la CRL base. **CRLPeriod** puede tener los siguientes valores: Horas, días, semanas, meses y años.

**CRLDeltaPeriod** y **CRLDeltaPeriodUnits** establecer el período de validez de las diferencias CRL. **CRLDeltaPeriod** puede tener los siguientes valores: Horas, días, semanas, meses y años.

Cada una de estas configuraciones se puede configurar cuando se haya instalado la entidad de certificación:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

No olvide reiniciar Active Directory Certificate Services para que los cambios surtan efecto.

**LoadDefaultTemplates** solo se aplica durante la instalación de una CA empresarial. Esta configuración, ya sea True o False (o 1 o 0), determina si la entidad de certificación está configurada con cualquiera de las plantillas predeterminadas.

En una instalación predeterminada de la CA, se agrega un subconjunto de las plantillas de certificado predeterminadas a la carpeta de plantillas de certificado en el complemento entidad de certificación. Esto significa que, en cuanto se inicia el servicio de AD CS una vez instalado el rol de un usuario o equipo con los permisos adecuados puede inmediatamente inscribirse para un certificado.

Es posible que no desea emitir certificados inmediatamente después de que se ha instalado una entidad de certificación, por lo que puede usar la configuración de LoadDefaultTemplates para impedir que las plantillas predeterminadas que se agreguen a la CA de empresa. Si no hay ninguna plantilla configurada en la CA no puede emitir ningún certificado.

**AlternateSignatureAlgorithm** configura la CA para admitir el PKCS\#1 formato de firma V2.1 para el certificado de CA y las solicitudes de certificado. Cuando se establece en 1 en una CA raíz, el certificado de CA incluirá el PKCS\#1 formato de firma V2.1. Cuando se establece en una CA subordinada, la CA subordinada creará una solicitud de certificado que incluye el PKCS\#1 formato de firma V2.1.

**ForceUTF8** cambia el valor predeterminado de codificación de nombres distintivos relativos (RDN) en el asunto y el emisor distingue nombres UTF-8. Solo esos RDN que admiten UTF-8, como las que se definen como tipos de cadena de directorio mediante una solicitud de cambio, se ven afectados. Por ejemplo, el RDN de componente de dominio (DC) admite la codificación como IA5 o UTF-8, mientras que el Country RDN (C) solo admite la codificación como una cadena imprimible. La directiva ForceUTF8, por tanto, afectará el RDN de un controlador de dominio, pero no tendrá ningún efecto en un RDN C.

**EnableKeyCounting** configura la CA para incrementar un contador cada vez que se usa la clave de firma de la entidad de certificación. No habilite a esta opción a menos que tenga un módulo de seguridad de Hardware (HSM) y el proveedor de asociados de servicios criptográficos (CSP) que admite el recuento de claves. Ni Microsoft CSP seguro ni el recuento clave de proveedor de almacenamiento de claves (KSP) de Microsoft Software soporte técnico.


## <a name="create-the-capolicyinf-file"></a>Crear el archivo CAPolicy.inf

Antes de instalar AD CS, configurar el archivo CAPolicy.inf con una configuración específica para la implementación.

**Requisito previo:** Debe ser miembro del grupo Administradores.

1.  En el equipo donde va a instalar AD CS, abra Windows PowerShell, escriba **c:\CAPolicy.inf el Bloc de notas** y presione ENTRAR.

2.  Cuando se le pida que cree un archivo nuevo, haga clic en **Sí**.

3.  Como contenido del archivo, escriba lo siguiente:
   ```
   [Version]  
    Signature="$Windows NT$"  
    [PolicyStatementExtension]  
    Policies=InternalPolicy  
    [InternalPolicy]  
    OID=1.2.3.4.1455.67.89.5  
    Notice="Legal Policy Statement"  
    URL=https://pki.corp.contoso.com/pki/cps.txt  
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
1.  Haga clic en **archivo**y, a continuación, haga clic en **Guardar como**.

2.  Vaya a la carpeta % systemroot %.

3.  Compruebe que:

    -   El **Nombre de archivo** está establecido en **CAPolicy.inf**

    -   **Tipo** esté establecido en **Todos los archivos**

    -   **Codificación** sea **ANSI**

4.  Haga clic en **Guardar**.

5.  Cuando se le pregunte si desea sobrescribir el archivo, haga clic en **Sí**.

    ![Guardar como ubicación para el archivo CAPolicy.inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   Asegúrese de guardar el archivo CAPolicy.inf con la extensión inf. Si no escribes específicamente **.inf** al final de un nombre de archivo y seleccionas las opciones de la manera descrita, el archivo se guardará como un archivo de texto y no se usará durante la instalación de la entidad de certificación.

6.  Cierre el Bloc de notas.

>   [!IMPORTANT]  
>   En el archivo CAPolicy.inf, consulte hay una línea que especifica la dirección URL https://pki.corp.contoso.com/pki/cps.txt. La sección Internal Policy del archivo CAPolicy.inf se muestra como ejemplo de cómo se especificaría la ubicación de una orden de prácticas de certificación (CPS). En esta guía, no se indica para crear la instrucción de práctica de certificado (CPS).
