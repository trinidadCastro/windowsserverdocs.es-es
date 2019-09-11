---
title: Preparar el archivo CAPolicy.inf
description: El archivo CAPolicy. inf contiene varias opciones que se usan al instalar el servicio de certificación de Active Directory (AD CS) o al renovar el certificado de CA.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fb2e25dcd27ed3046eeeb444a9f167ccff6e1dd3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868962"
---
# <a name="capolicyinf-syntax"></a>Sintaxis de CAPolicy. inf
>   Se aplica a: Windows Server (canal semianual), Windows Server 2016

El archivo CAPolicy. inf es un archivo de configuración que define las extensiones, las restricciones y otras opciones de configuración que se aplican a un certificado de CA raíz y a todos los certificados emitidos por la CA raíz. El archivo CAPolicy. inf debe instalarse en un servidor host antes de que se inicie la rutina de instalación de la CA raíz. Cuando se van a modificar las restricciones de seguridad en una entidad de certificación raíz, se debe renovar el certificado raíz y debe instalarse un archivo CAPolicy. inf actualizado en el servidor antes de que comience el proceso de renovación.

El archivo CAPolicy. inf es:

-   Creado y definido manualmente por un administrador

-   Se emplea durante la creación de certificados de entidad de certificación raíz y subordinada

-   Definido en la entidad de certificación de firma donde se firma y emite el certificado (no la entidad de certificación en la que se concede la solicitud)

Una vez que haya creado el archivo CAPolicy. inf, debe copiarlo en la carpeta **% systemroot%** del servidor antes de instalar el ADCS o renovar el certificado de la entidad de certificación.

El archivo CAPolicy. inf permite especificar y configurar una amplia variedad de opciones y atributos de la entidad de certificación. En la siguiente sección se describen todas las opciones para crear un archivo. inf adaptado a las necesidades específicas.

## <a name="capolicyinf-file-structure"></a>Estructura del archivo CAPolicy. inf

Los siguientes términos se usan para describir la estructura del archivo. inf:

-   _Sección_ : es un área del archivo que cubre un grupo lógico de claves. Los nombres de sección de los archivos. inf se identifican entre corchetes. Muchas secciones, pero no todas, se usan para configurar las extensiones de certificado.

-   _Key_ : es el nombre de una entrada y aparece a la izquierda del signo igual.

-   _Valor_ : es el parámetro y aparece a la derecha del signo igual.

En el ejemplo siguiente, **[Version]** es la sección, **Signature** es la clave y **"\$Windows\$NT"** es el valor.

Ejemplo:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>`Version`

Identifica el archivo como un archivo. inf. La versión es la única sección necesaria y debe estar al principio del archivo CAPolicy. inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Muestra las directivas definidas por la organización y si son opcionales u obligatorias. Varias directivas se separan mediante comas. Los nombres tienen significado en el contexto de una implementación específica o con respecto a las aplicaciones personalizadas que comprueban la presencia de estas directivas.

Para cada directiva definida, debe haber una sección que defina la configuración de esa Directiva concreta. Para cada Directiva, debe proporcionar un identificador de objeto definido por el usuario (OID) y el texto que desea que se muestre como la instrucción de directiva o un puntero de dirección URL a la instrucción de directiva. La dirección URL puede tener el formato de una dirección URL HTTP, FTP o LDAP.

Si va a tener texto descriptivo en la instrucción de la Directiva, las tres líneas siguientes del archivo CAPolicy. inf tendrían el siguiente aspecto:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Si va a utilizar una dirección URL para hospedar la instrucción de directiva de CA, en su lugar, las tres líneas siguientes tendrían el siguiente aspecto:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Además:

-   Se admiten varias claves de URL y aviso.

-   Se admiten las claves de aviso y dirección URL en la misma directiva.

-   Las direcciones URL con espacios o texto con espacios deben ir entre comillas. Esto se aplica a la clave de **dirección URL** , independientemente de la sección en la que aparezca.

Un ejemplo de varios avisos y direcciones URL en una sección de directiva sería similar al siguiente:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Puede especificar puntos de distribución de CRL (CDP) para un certificado de CA raíz en el archivo CAPolicy. inf.  Después de instalar la CA, puede configurar las direcciones URL de CDP que la CA incluye en cada certificado emitido. El certificado de CA raíz muestra las direcciones URL especificadas en esta sección del archivo CAPolicy. inf. 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Información adicional sobre esta sección:
-   Admite
    - HTTP 
    - Direcciones URL de archivo
    - Direcciones URL LDAP 
    - Varias direcciones URL
   
    >[!IMPORTANT]
    >No admite direcciones URL HTTPS.

-   Las comillas deben encerrar las direcciones URL con espacios.

-   Si no se especifica ninguna dirección URL, es decir, si la sección **[CRLDistributionPoint]** existe en el archivo pero está vacía, se omite la extensión de acceso a la información de entidad de certificación del certificado de CA raíz. Esto suele ser preferible al configurar una CA raíz. Windows no realiza la comprobación de revocación en un certificado de CA raíz, por lo que la extensión CDP es superflua en un certificado de CA raíz.

-    La CA puede publicar en un archivo UNC, por ejemplo, en un recurso compartido que representa la carpeta de un sitio web en el que un cliente recupera a través de HTTP.

-   Use esta sección solo si está configurando una CA raíz o renovando el certificado de CA raíz. La CA determina las extensiones CDP de la entidad de certificación subordinada.
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Puede especificar los puntos de acceso a la información de entidad en el archivo CAPolicy. inf para el certificado de CA raíz.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Algunas notas adicionales en la sección acceso a la información de entidad:

-   Se admiten varias direcciones URL.

-   Se admiten las direcciones URL HTTP, FTP, LDAP y FILE. No se admiten direcciones URL HTTPS.

-   Esta sección solo se utiliza si está configurando una CA raíz o renovando el certificado de CA raíz. Las extensiones AIA de CA subordinadas vienen determinadas por la CA que emitió el certificado de la CA subordinada.

-   Las direcciones URL con espacios deben ir entre comillas.

-   Si no se especifica ninguna dirección URL, es decir, si la sección **[AuthorityInformationAccess]** existe en el archivo pero está vacía, se omite la extensión de punto de distribución de CRL del certificado de CA raíz. De nuevo, esta sería la configuración preferida en el caso de un certificado de entidad de certificación raíz, ya que no hay ninguna autoridad mayor que una CA raíz a la que deba hacer referencia un vínculo a su certificado.

### <a name="certsrv_server"></a>certsrv_Server

Otra sección opcional del archivo CAPolicy. inf es [certsrv_server], que se usa para especificar la longitud de la clave de renovación, el período de validez de la renovación y el período de validez de la lista de revocación de certificados (CRL) para una CA que se va a renovar o instalar. No se requiere ninguna de las claves de esta sección. Muchas de estas opciones tienen valores predeterminados que son suficientes para la mayoría de las necesidades y se pueden omitir simplemente del archivo CAPolicy. inf. Como alternativa, muchas de estas opciones se pueden cambiar una vez instalada la CA.

Un ejemplo tendría el siguiente aspecto:

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

**RenewalKeyLength** establece el tamaño de la clave solo para la renovación. Solo se utiliza cuando se genera un nuevo par de claves durante la renovación de certificados de entidad de certificación. El tamaño de la clave del certificado de entidad de certificación inicial se establece cuando se instala la CA.

Al renovar un certificado de entidad de certificación con un nuevo par de claves, la longitud de la clave puede aumentar o disminuir. Por ejemplo, si ha establecido un tamaño de clave de CA raíz de 4096 bytes o superior y, a continuación, descubre que tiene aplicaciones Java o dispositivos de red que solo admiten tamaños de clave de 2048 bytes. Si aumenta o disminuye el tamaño, debe volver a emitir todos los certificados emitidos por esa CA.

**RenewalValidityPeriod** y **RenewalValidityPeriodUnits** establecen la duración del nuevo certificado de CA raíz al renovar el certificado de CA raíz antiguo. Solo se aplica a una CA raíz. La duración del certificado de una CA subordinada viene determinada por su superior. RenewalValidityPeriod puede tener los siguientes valores: Horas, días, semanas, meses y años.

**CRLPeriod** y **CRLPeriodUnits** establecen el período de validez de la CRL base. **CRLPeriod** puede tener los siguientes valores: Horas, días, semanas, meses y años.

**CRLDeltaPeriod** y **configurar crldeltaperiodunits** establecen el período de validez de la diferencia CRL. **CRLDeltaPeriod** puede tener los siguientes valores: Horas, días, semanas, meses y años.

Cada una de estas opciones se puede configurar después de instalar la CA:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

Recuerde reiniciar Active Directory servicios de Certificate Server para que los cambios surtan efecto.

**LoadDefaultTemplates** solo se aplica durante la instalación de una CA empresarial. Esta configuración, ya sea true o false (o 1 o 0), dicta si la CA está configurada con cualquiera de las plantillas predeterminadas.

En una instalación predeterminada de la CA, se agrega un subconjunto de las plantillas de certificado predeterminadas a la carpeta de plantillas de certificado en el complemento entidad de certificación. Esto significa que, tan pronto como se inicie el servicio de AD CS después de instalar el rol, un usuario o equipo con permisos suficientes podrá inscribirse inmediatamente para obtener un certificado.

Es posible que no quiera emitir ningún certificado inmediatamente después de instalar una CA, por lo que puede usar la opción LoadDefaultTemplates para evitar que las plantillas predeterminadas se agreguen a la CA empresarial. Si no hay ninguna plantilla configurada en la CA, puede no emitir ningún certificado.

**AlternateSignatureAlgorithm** configura la CA para admitir el formato de firma\#PKCS 1 v 2.1 tanto para el certificado de CA como para las solicitudes de certificado. Cuando se establece en 1 en una entidad de certificación raíz, el certificado de\#CA incluirá el formato de firma PKCS 1 v 2.1. Cuando se establece en una CA subordinada, la CA subordinada creará una solicitud de certificado\#que incluye el formato de firma PKCS 1 v 2.1.

**ForceUTF8** cambia la codificación predeterminada de los nombres distintivos relativos (RDN) en el asunto y los nombres distintivos del emisor a UTF-8. Solo se ven afectados los RDN que admiten UTF-8, como los que se definen como tipos de cadena de directorio. Por ejemplo, el RDN del componente de dominio (DC) admite la codificación como IA5 o UTF-8, mientras que el RDN del país (C) solo admite la codificación como una cadena que se puede imprimir. Por lo tanto, la Directiva ForceUTF8 afectará a un RDN de DC, pero no afectará a un RDN de C.

**EnableKeyCounting** configura la CA para incrementar un contador cada vez que se usa la clave de firma de la CA. No habilite esta opción a menos que tenga un módulo de seguridad de hardware (HSM) y un proveedor de servicios criptográficos (CSP) asociado que admita el recuento de claves. Ni el CSP strong de Microsoft ni el proveedor de almacenamiento de claves (KSP) de software de Microsoft admiten el recuento de claves.


## <a name="create-the-capolicyinf-file"></a>Crear el archivo CAPolicy. inf

Antes de instalar AD CS, configure el archivo CAPolicy. inf con la configuración específica de la implementación.

**Requisitos previos** Debe ser miembro del grupo administradores.

1. En el equipo en el que planea instalar AD CS, abra Windows PowerShell, escriba **notepad c:\CAPolicy.inf** y presione Entrar.

2. Cuando se le pida que cree un archivo nuevo, haga clic en **Sí**.

3. Como contenido del archivo, escriba lo siguiente:
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
4. Haga clic en **archivo**y, a continuación, en **Guardar como**.

5. Navegue hasta la carpeta% SystemRoot%.

6. Compruebe que:

   -   El **Nombre de archivo** está establecido en **CAPolicy.inf**

   -   **Tipo** esté establecido en **Todos los archivos**

   -   **Codificación** sea **ANSI**

7. Haga clic en **Guardar**.

8. Cuando se le pregunte si desea sobrescribir el archivo, haga clic en **Sí**.

   ![Guardar como ubicación del archivo CAPolicy. inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

   > [!CAUTION]
   >   Asegúrese de guardar el archivo CAPolicy.inf con la extensión inf. Si no escribes específicamente **.inf** al final de un nombre de archivo y seleccionas las opciones de la manera descrita, el archivo se guardará como un archivo de texto y no se usará durante la instalación de la entidad de certificación.

9. Cierre el Bloc de notas.

> [!IMPORTANT]
>   En el archivo CAPolicy. inf, puede ver que hay una línea que especifica la https://pki.corp.contoso.com/pki/cps.txt dirección URL. La sección Internal Policy del archivo CAPolicy.inf se muestra como ejemplo de cómo se especificaría la ubicación de una orden de prácticas de certificación (CPS). En esta guía, no se le indicará que cree la declaración de prácticas de certificación (CPS).
