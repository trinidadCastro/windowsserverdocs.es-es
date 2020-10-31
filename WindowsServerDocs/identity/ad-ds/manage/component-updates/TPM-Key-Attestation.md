---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: Atestación de clave de TPM
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 18bf859e6e02d3c01fead9291d31c8dac3f002f1
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070637"
---
# <a name="tpm-key-attestation"></a>Atestación de clave de TPM

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor** : Diego Turner, Ingeniero de soporte técnico de nivel superior con el grupo de Windows

> [!NOTE]
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.

## <a name="overview"></a>Información general
Aunque la compatibilidad con las claves protegidas con TPM ha existido desde Windows 8, no había ningún mecanismo para que las CA atestiguaran criptográficamente que la clave privada del solicitante de certificados está protegida realmente por un Módulo de plataforma segura (TPM). Esta actualización permite a una CA realizar esa atestación y reflejar la atestación en el certificado emitido.

> [!NOTE]
> En este artículo se da por supuesto que el lector está familiarizado con el concepto de plantilla de certificado (como referencia, consulte [plantillas de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))). También se da por supuesto que el lector está familiarizado con la configuración de las CA empresariales para emitir certificados basados en plantillas de certificado (como referencia, consulte [Checklist: configure CAS to Issue and Manage Certificates](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771533(v=ws.11))).

### <a name="terminology"></a>Terminología

|Término|Definición|
|--------|--------------|
|EK|Clave de aprobación. Se trata de una clave asimétrica incluida en el TPM (inyectada en el momento de la fabricación). El EK es único para cada TPM y puede identificarlo. No se puede cambiar ni quitar el EK.|
|EKpub|Hace referencia a la clave pública del EK.|
|EKPriv|Hace referencia a la clave privada de la EK.|
|EKCert|Certificado EK. Un certificado emitido por el fabricante del TPM para EKPub. No todos los TPM tienen EKCert.|
|TPM|Módulo de plataforma segura. Un TPM está diseñado para proporcionar funciones relacionadas con la seguridad basadas en hardware. El chip del TPM es un procesador de criptografía seguro diseñado para realizar operaciones criptográficas. El chip incluye varios mecanismos de seguridad física que hacen que sea resistente a las alteraciones y que las funciones de seguridad no permitan que el software malintencionado realice alteraciones.|

### <a name="background"></a>Información previa
A partir de Windows 8, se puede usar un Módulo de plataforma segura (TPM) para proteger la clave privada de un certificado. El proveedor de almacenamiento de claves (KSP) del proveedor de criptografía de plataforma de Microsoft habilita esta característica. Había dos preocupaciones con la implementación:

-   No se garantiza que una clave esté realmente protegida por un TPM (alguien puede suplantar fácilmente un KSP de software como un KSP de TPM con credenciales de administrador local).

-   No era posible limitar la lista de TPM que pueden proteger los certificados emitidos por la empresa (en caso de que el administrador de PKI desee controlar los tipos de dispositivos que se pueden usar para obtener certificados en el entorno).

### <a name="tpm-key-attestation"></a>Atestación de clave de TPM
La atestación de clave de TPM es la capacidad de la entidad que solicita un certificado a una entidad de certificación que puede demostrar criptográficamente que la clave RSA de la solicitud de certificado está protegida por "a" o "el" TPM en el que confía la entidad de certificación. El modelo de confianza de TPM se describe más adelante en la sección [información general de implementación](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) , más adelante en este tema.

### <a name="why-is-tpm-key-attestation-important"></a>¿Por qué es importante la atestación de clave de TPM?
Un certificado de usuario con una clave atestiguada por TPM proporciona un mayor control de seguridad, al que se hace una copia de seguridad mediante la no exportabilidad, el antimartillo y el aislamiento de claves que proporciona el TPM.

Con la atestación de clave de TPM, ahora es posible un nuevo paradigma de administración: un administrador puede definir el conjunto de dispositivos que los usuarios pueden usar para tener acceso a los recursos corporativos (por ejemplo, VPN o punto de acceso inalámbrico) y tener garantías **seguras** de que no se pueden usar otros dispositivos para acceder a ellos. Este nuevo paradigma de control de acceso es **seguro** porque está asociado a una identidad *de usuario enlazada al hardware* , que es más fuerte que una credencial basada en software.

### <a name="how-does-tpm-key-attestation-work"></a>¿Cómo funciona la atestación de clave de TPM?
En general, la atestación de clave de TPM se basa en los pilares siguientes:

1.  Cada TPM se suministra con una clave asimétrica única, denominada *clave de aprobación* (EK), grabada por el fabricante. Nos referimos a la parte pública de esta clave como *EKPub* y la clave privada asociada como *EKPriv* . Algunos chips de TPM también tienen un certificado EK que emite el fabricante para el EKPub. Hacemos referencia a este certificado como *EKCert* .

2.  Una entidad de certificación establece confianza en el TPM a través de EKPub o EKCert.

3.  Un usuario demuestra a la entidad de certificación que la clave RSA para la que se solicita el certificado está relacionada criptográficamente con el EKPub y que el usuario es el propietario del EKpriv.

4.  La CA emite un certificado con un OID de directiva de emisión especial para indicar que ahora se ha atestado a la clave para que la proteja un TPM.

## <a name="deployment-overview"></a><a name="BKMK_DeploymentOverview"></a>Introducción a la implementación
En esta implementación, se supone que se ha configurado una CA de Windows Server 2012 R2 Enterprise. Además, los clientes (Windows 8.1) se configuran para inscribirse en esa CA de empresa mediante plantillas de certificado.

Hay tres pasos para implementar la atestación de clave de TPM:

1.  **Planear el modelo de confianza de TPM:** El primer paso es decidir qué modelo de confianza de TPM se va a usar. Existen tres formas de hacerlo:

    -   **Confianza basada en credenciales de usuario:** La entidad de certificación empresarial confía en el EKPub proporcionado por el usuario como parte de la solicitud de certificado y no se realiza ninguna validación distinta de las credenciales de dominio del usuario.

    -   **Confianza basada en EKCert:** La entidad de certificación empresarial valida la cadena de EKCert que se proporciona como parte de la solicitud de certificado en una lista administrada por el administrador de *cadenas de certificados EK aceptables* . Las cadenas aceptables se definen por fabricante y se expresan a través de dos almacenes de certificados personalizados en la CA emisora (un almacén para el medio y otro para los certificados de CA raíz). Este modo de confianza significa que **todos los** TPM de un fabricante determinado son de confianza. Tenga en cuenta que, en este modo, los TPM que se usan en el entorno deben contener EKCerts.

    -   **Confianza basada en EKPub:** La entidad de certificación empresarial valida que el EKPub proporcionado como parte de la solicitud de certificado aparece en una lista administrada por el administrador de EKPubs permitidos. Esta lista se expresa como un directorio de archivos donde el nombre de cada archivo en este directorio es el hash SHA-2 del EKPub permitido. Esta opción ofrece el nivel de seguridad más alto, pero requiere más trabajo administrativo, ya que cada dispositivo se identifica individualmente. En este modelo de confianza, solo se permite que los dispositivos que han agregado EKPub de TPM a la lista de EKPubs permitidos se inscriban en un certificado atestado por TPM.

    Según el método que se use, la CA aplicará un OID de directiva de emisión diferente al certificado emitido. Para obtener más información sobre los OID de directivas de emisión, consulte la tabla OID de directivas de emisión en la sección [configurar una plantilla de certificado](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) de este tema.

    Tenga en cuenta que es posible elegir una combinación de modelos de confianza de TPM. En este caso, la CA aceptará cualquiera de los métodos de atestación y los OID de la Directiva de emisión reflejarán todos los métodos de atestación que se hayan realizado correctamente.

2.  **Configurar la plantilla de certificado:** La configuración de la plantilla de certificado se describe en la sección [detalles de implementación](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) de este tema. En este artículo no se explica cómo se asigna esta plantilla de certificado a la CA de empresa o cómo se concede el acceso de inscripción a un grupo de usuarios. Para obtener más información, consulte [Checklist: configure CAS to Issue and Manage Certificates](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771533(v=ws.11)).

3.  **Configurar la CA para el modelo de confianza de TPM**

    1.  **Confianza basada en credenciales de usuario:** No se requiere ninguna configuración específica.

    2.  **Confianza basada en EKCert:** El administrador debe obtener los certificados de la cadena de EKCert de los fabricantes de TPM e importarlos a dos nuevos almacenes de certificados, creados por el administrador, en la entidad de certificación que realiza la atestación de clave de TPM. Para obtener más información, consulte la sección configuración de la [entidad de certificación](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) de este tema.

    3.  **Confianza basada en EKPub:** El administrador debe obtener el EKPub de cada dispositivo que necesitará certificados atestados por TPM y agregarlos a la lista de EKPubs permitidos. Para obtener más información, consulte la sección configuración de la [entidad de certificación](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) de este tema.

    > [!NOTE]
    > -   Esta característica requiere Windows 8.1/Windows Server 2012 R2.
    > -   No se admite la atestación de clave de TPM para KSP de tarjeta inteligente de terceros. Debe usarse el KSP de proveedor de cifrado de plataforma de Microsoft.
    > -   La atestación de clave de TPM solo funciona con claves RSA.
    > -   No se admite la atestación de clave de TPM para una CA independiente.
    > -   La atestación de clave de TPM no admite [el procesamiento de certificados no persistente](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff934598(v=ws.10)).

## <a name="deployment-details"></a><a name="BKMK_DeploymentDetails"></a>Detalles de implementación

### <a name="configure-a-certificate-template"></a><a name="BKMK_ConfigCertTemplate"></a>Configurar una plantilla de certificado
Para configurar la plantilla de certificado para la atestación de clave de TPM, realice los pasos de configuración siguientes:

1.  Pestaña **Compatibilidad**

    En la sección **configuración de compatibilidad** :

    -   Asegúrese de que se ha seleccionado **Windows Server 2012 R2** para la **entidad de certificación** .

    -   Asegúrese de que se ha seleccionado **Windows 8.1/Windows Server 2012 R2** para el **destinatario del certificado** .

    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)

2.  Pestaña **Criptografía**

    Asegúrese de que se ha seleccionado **proveedor de almacenamiento de claves** para la categoría de **proveedor** y **RSA** está seleccionado para el **nombre del algoritmo** . Asegúrese **de que las solicitudes deben usar uno de los siguientes proveedores** está seleccionada y la opción **proveedor de criptografía de plataforma de Microsoft** está seleccionada en **proveedores** .

    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)

3.  Pestaña **atestación de clave**

    Se trata de una nueva pestaña para Windows Server 2012 R2:

    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)

    Elija un modo de atestación de las tres opciones posibles.

    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)

    -   **Ninguno:** Implica que no se debe usar la atestación de clave

    -   **Obligatorio, si el cliente es capaz:** Permite que los usuarios de un dispositivo que no admiten la atestación de clave de TPM sigan inscribiendo para ese certificado. Los usuarios que pueden realizar la atestación se distinguirán con un OID de directiva de emisión especial. Es posible que algunos dispositivos no puedan realizar la atestación debido a un TPM antiguo que no admite la atestación de clave o a que el dispositivo no tiene un TPM.

    -   **Requerido:** El cliente *debe* realizar la atestación de clave de TPM, de lo contrario se producirá un error en la solicitud de certificado.

    A continuación, elija el modelo de confianza de TPM. Hay tres opciones de nuevo:

    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)

    -   **Credenciales de usuario:** Permita que un usuario que realiza la autenticación conceda un TPM válido mediante la especificación de sus credenciales de dominio.

    -   **Certificado de aprobación:** El EKCert del dispositivo debe validarse mediante certificados de entidad de certificación intermedias del TPM administrados por el administrador para un certificado de CA raíz administrado por el administrador. Si elige esta opción, debe configurar los almacenes de certificados EKCA y EKRoot en la CA emisora como se describe en la sección configuración de la  [entidad](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) de certificación de este tema.

    -   **Clave de aprobación:** El EKPub del dispositivo debe aparecer en la lista administrada por el administrador de PKI. Esta opción ofrece el nivel de seguridad más alto, pero requiere más trabajo administrativo. Si elige esta opción, debe configurar una lista de EKPub en la CA emisora como se describe en la sección [configuración](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) de la entidad de certificación de este tema.

    Por último, decida qué directiva de emisión desea mostrar en el certificado emitido. De forma predeterminada, cada tipo de aplicación tiene un identificador de objeto (OID) asociado que se insertará en el certificado si pasa ese tipo de aplicación, tal y como se describe en la tabla siguiente. Tenga en cuenta que es posible elegir una combinación de métodos de cumplimiento. En este caso, la CA aceptará cualquiera de los métodos de atestación y el OID de la Directiva de emisión reflejará todos los métodos de atestación que se hayan realizado correctamente.

    **OID de directivas de emisión**

    |OID|Tipo de atestación de clave|Description|Nivel de seguridad|
    |-------|------------------------|---------------|-------------------|
    |1.3.6.1.4.1.311.21.30|EK|"EK verificado": para la lista administrada por el administrador de EK|Alto|
    |1.3.6.1.4.1.311.21.31|Certificado de aprobación|"Certificado EK comprobado": cuando se valida la cadena de certificados EK|Media|
    |1.3.6.1.4.1.311.21.32|Credenciales de usuario|"EK de confianza en uso": para la EK con atestación del usuario|Bajo|

    Los OID se insertarán en el certificado emitido si se selecciona **incluir directivas de emisión** (la configuración predeterminada).

    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)

    > [!TIP]
    > Un uso potencial de tener el OID presente en el certificado es limitar el acceso a redes VPN o inalámbricas a determinados dispositivos. Por ejemplo, la Directiva de acceso podría permitir la conexión (o el acceso a una VLAN diferente) si OID 1.3.6.1.4.1.311.21.30 está presente en el certificado. Esto le permite limitar el acceso a los dispositivos cuyo TPM EK esté presente en la lista EKPUB.

### <a name="ca-configuration"></a><a name="BKMK_CAConfig"></a>Configuración de CA

1.  **Configuración de almacenes de certificados EKCA y EKROOT en una CA emisora**

    Si seleccionó **certificado de aprobación** para la configuración de la plantilla, siga estos pasos de configuración:

    1.  Use Windows PowerShell para crear dos nuevos almacenes de certificados en el servidor de la entidad de certificación (CA) que realizará la atestación de clave de TPM.

    2.  Obtenga los certificados de CA intermedios y raíz de los fabricantes que desea permitir en el entorno de su empresa. Esos certificados se deben importar en los almacenes de certificados creados anteriormente (EKCA y EKROOT), según corresponda.

    El siguiente script de Windows PowerShell realiza ambos pasos. En el ejemplo siguiente, el fabricante de TPM ha proporcionado un certificado raíz *FabrikamRoot. cer* y un certificado de CA emisora *Fabrikamca. cer* .

    ```powershell
    PS C:>\cd cert:
    PS Cert:\>cd .\\LocalMachine
    PS Cert:\LocalMachine> new-item EKROOT
    PS Cert:\ LocalMachine> new-item EKCA
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT
    ```

2.  **Configuración de la lista de EKPUB si se usa el tipo de atestación EK**

    Si seleccionó **clave de aprobación** en la configuración de la plantilla, los siguientes pasos de configuración son crear y configurar una carpeta en la CA emisora que contiene archivos de 0 bytes, cada uno con el nombre del hash Sha-2 de una EK permitida. Esta carpeta actúa como una "lista de permitidos" de dispositivos a los que se permite obtener certificados atestados por clave de TPM. Dado que debe agregar manualmente el EKPUB para cada uno de los dispositivos que requieran un certificado atestado, proporciona a la empresa una garantía de los dispositivos que están autorizados a obtener certificados atestados de clave de TPM. La configuración de una CA para este modo requiere dos pasos:

    1.  **Cree la entrada del registro EndorsementKeyListDirectories:** Use la herramienta de línea de comandos certutil para configurar las ubicaciones de carpeta en las que se definen los EKpubs de confianza, tal y como se describe en la tabla siguiente.

        |Operación|Sintaxis de comandos|
        |-------------|------------------|
        |Agregar ubicaciones de carpeta|certutil.exe-setreg CA\EndorsementKeyListDirectories + " <folder> "|
        |Quitar ubicaciones de carpeta|certutil.exe-setreg CA\EndorsementKeyListDirectories-" <folder> "|

        El comando EndorsementKeyListDirectories en certutil es una configuración del registro como se describe en la tabla siguiente.

        |Nombre del valor|Tipo|Datos|
        |--------------|--------|--------|
        |EndorsementKeyListDirectories|REG_MULTI_SZ|<ruta de acceso LOCAL o UNC a EKPUB permitir listas ><p>Ejemplo:<p>*\\\blueCA.contoso.com\ekpub*<p>*\\\bluecluster1.contoso.com\ekpub*<p>D:\ekpub|

        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>

        *EndorsementKeyListDirectories* contendrá una lista de rutas de acceso del sistema de archivos local o UNC, cada una de las cuales apunta a una carpeta a la que la CA tiene acceso de lectura. Cada carpeta puede contener cero o más entradas de la lista de permitidos, donde cada entrada es un archivo con un nombre que es el hash SHA-2 de un EKpub de confianza, sin extensión de archivo.
        La creación o edición de esta configuración de clave del registro requiere un reinicio de la CA, al igual que las opciones de configuración del registro de CA existentes. Sin embargo, las modificaciones en el valor de configuración surtirán efecto inmediatamente y no requerirán que se reinicie la CA.

        > [!IMPORTANT]
        > Proteja las carpetas de la lista de manipulaciones y acceso no autorizado mediante la configuración de permisos para que solo los administradores autorizados tengan acceso de lectura y escritura. La cuenta de equipo de la CA requiere solo acceso de lectura.

    2.  **Rellene la lista EKPUB:** Use el siguiente cmdlet de Windows PowerShell para obtener el hash de clave pública del TPM EK usando Windows PowerShell en cada dispositivo y, a continuación, envíe este hash de clave pública a la CA y almacénelo en la carpeta EKPubList.

        ```powershell
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file
        ```

## <a name="troubleshooting"></a>Solución de problemas

### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Los campos de atestación de clave no están disponibles en una plantilla de certificado
Los campos de atestación de clave no están disponibles si la configuración de la plantilla no cumple los requisitos de atestación. Los motivos comunes son los siguientes:

1.  La configuración de compatibilidad no está configurada correctamente. Asegúrese de que están configurados de la siguiente manera:

    1.  **Entidad de certificación** : **Windows Server 2012 R2**

    2.  **Destinatario del certificado** : **Windows 8.1/Windows Server 2012 R2**

2.  La configuración de criptografía no está configurada correctamente. Asegúrese de que están configurados de la siguiente manera:

    1.  **Categoría de proveedor** : **proveedor de almacenamiento de claves**

    2.  **Nombre del algoritmo** : **RSA**

    3.  **Proveedores** : **proveedor de criptografía de la plataforma Microsoft**

3.  La configuración de control de solicitudes no está configurada correctamente. Asegúrese de que están configurados de la siguiente manera:

    1.  No se debe seleccionar la opción **permitir que la clave privada se pueda exportar** .

    2.  La opción **archivar clave privada de cifrado de sujeto** no debe estar seleccionada.

### <a name="verification-of-tpm-device-for-attestation"></a>Comprobación del dispositivo TPM para la atestación
Use el cmdlet de Windows PowerShell, **CONFIRM-CAEndorsementKeyInfo** , para comprobar que un dispositivo TPM específico es de confianza para la atestación de las CA. Hay dos opciones: una para comprobar el EKCert y la otra para comprobar un EKPub. El cmdlet se ejecuta localmente en una CA o en CA remotas mediante la comunicación remota de Windows PowerShell.

1.  Para comprobar la confianza en un EKPub, siga estos dos pasos:

    1.  **Extraiga el EKPub desde el equipo cliente:** El EKPub se puede extraer de un equipo cliente mediante **Get-TpmEndorsementKeyInfo** . En un símbolo del sistema con privilegios elevados, ejecute lo siguiente:

        ```
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256
        ```

    2.  **Comprobar la confianza en un EKCert de un equipo CA:** Copie la cadena extraída (el hash SHA-2 del EKPub) en el servidor (por ejemplo, por correo electrónico) y páselo al cmdlet Confirm-CAEndorsementKeyInfo. Tenga en cuenta que este parámetro debe tener 64 caracteres.

        ```
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>
        ```

2.  Para comprobar la confianza en un EKCert, siga estos dos pasos:

    1.  **Extraiga el EKCert desde el equipo cliente:** El EKCert se puede extraer de un equipo cliente mediante **Get-TpmEndorsementKeyInfo** . En un símbolo del sistema con privilegios elevados, ejecute lo siguiente:

        ```
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```

    2.  **Comprobar la confianza en un EKCert de un equipo CA:** Copie el EKCert extraído (EkCert. cer) en la CA (por ejemplo, por correo electrónico o xcopy). Por ejemplo, si copia el archivo de certificado en la carpeta "c:\diagnose" en el servidor de la CA, ejecute lo siguiente para finalizar la comprobación:

        ```
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo
        ```

## <a name="see-also"></a>Consulte también
[Información general sobre](/previous-versions/windows/it-pro/windows-8.1-and-8/jj131725(v=ws.11)) 
 tecnología de módulo de plataforma segura [Recurso externo: módulo de plataforma segura](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)
