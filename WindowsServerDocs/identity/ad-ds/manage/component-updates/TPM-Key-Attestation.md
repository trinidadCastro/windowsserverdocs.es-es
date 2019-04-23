---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: Atestación de clave de TPM
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3cfc047fe1a66617abbda1de5f2c5842dcbdeb12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862886"
---
# <a name="tpm-key-attestation"></a>Atestación de clave de TPM

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, jefe ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
## <a name="overview"></a>Información general  
Aunque la compatibilidad para claves protegidas por el TPM ha existido desde Windows 8, se han producido ningún mecanismo para las entidades de certificación atestar criptográficamente que la clave privada del solicitante de certificado está realmente protegida por un módulo de plataforma segura (TPM). Esta actualización permite que una entidad de certificación para realizar esa atestación y reflejar ese certificado en el certificado emitido.  
  
> [!NOTE]  
> En este artículo se da por supuesto que el lector está familiarizado con el concepto de plantilla de certificado (como referencia, vea [plantillas de certificado](https://technet.microsoft.com/library/cc730705.aspx)). También se supone que el lector está familiarizado con la configuración de las CA de empresa para emitir certificados basados en plantillas de certificado (como referencia, vea [lista de comprobación: Configuración de CA para emitir y administrar certificados](https://technet.microsoft.com/library/cc771533.aspx)).  
  
### <a name="terminology"></a>Terminología  
  
|Término|Definición|  
|--------|--------------|  
|EK|Clave de aprobación. Se trata de una clave asimétrica se encuentra dentro del TPM (que se insertó en el momento de la fabricación). El EK es único para cada TPM y puede identificarlo. El EK no puede modificarse o quitarse.|  
|EKpub|Hace referencia a la clave pública de la EK.|  
|EKPriv|Hace referencia a la clave privada de aprobación.|  
|EKCert|Certificado EK. Un certificado emitido por el fabricante TPM para EKPub. No todos los TPM tienen EKCert.|  
|TPM|Módulo de plataforma segura. Un TPM está diseñado para proporcionar funciones de relacionadas con la seguridad basada en hardware. El chip del TPM es un procesador de criptografía seguro diseñado para realizar operaciones criptográficas. El chip incluye varios mecanismos de seguridad física que hacen que sea resistente a las alteraciones y que las funciones de seguridad no permitan que el software malintencionado realice alteraciones.|  
  
### <a name="background"></a>Background  
A partir de Windows 8, se puede usar un módulo de plataforma segura (TPM) para proteger la clave privada de un certificado. El proveedor de almacenamiento de claves (KSP) de Microsoft Platform Crypto proveedor habilita esta característica. Había dos problemas con la implementación:  

-   Se ha producido ninguna garantía de que una clave está realmente protegida por un TPM (alguien puede fácilmente suplantar la identidad de un KSP software como un KSP TPM con las credenciales de administrador local).

-   No era posible limitar la lista de los TPM que se pueden proteger los certificados emitido (en caso de que el Administrador de PKI desea controlar los tipos de dispositivos que pueden utilizarse para obtener los certificados en el entorno) de empresa.  

### <a name="tpm-key-attestation"></a>Atestación de clave de TPM  
Atestación de clave de TPM es la capacidad de la entidad que solicita un certificado para demostrar criptográficamente a una entidad de certificación que la clave RSA en la solicitud de certificado está protegida por TPM de "un" o "the" que confía en la entidad de certificación. El modelo de confianza TPM se explica más en el [información general sobre implementación](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) sección más adelante en este tema.  
  
### <a name="why-is-tpm-key-attestation-important"></a>¿Por qué es importante la atestación de clave de TPM?  
Un certificado de usuario con una clave de atestación de TPM ofrece la mayor garantía de seguridad, una copia de seguridad no exportabilidad, repetición anti y el aislamiento de claves proporcionado por el TPM.  
  
Con la atestación de clave de TPM, ahora es posible un nuevo paradigma de administración: Un administrador puede definir el conjunto de dispositivos que los usuarios pueden usar para tener acceso a recursos corporativos (por ejemplo, VPN o punto de acceso inalámbrico) y tener **seguro** garantiza que ningún otro dispositivo puede utilizarse para tener acceso a ellos. Este nuevo paradigma de control de acceso es **seguro** porque está vinculada a una *enlazadas al hardware* identidad del usuario, que es más fuerte que una credencial basada en software.
  
### <a name="how-does-tpm-key-attestation-work"></a>¿Cómo funciona la atestación de clave de TPM?  
En general, la atestación de clave de TPM se basa en los pilares de la siguientes:  
  
1.  Cada TPM se suministra con una clave asimétrica única, denominada la *clave de aprobación* grabarse (EK), por el fabricante. Nos referimos a la parte pública de esta clave como *EKPub* y la clave privada asociada como *EKPriv*. Algunos chips TPM también tienen un certificado EK emitido por el fabricante para el EKPub. Nos referimos a este certificado como *EKCert*.  
  
2.  Una entidad de certificación establece la confianza en el TPM a través de EKPub o EKCert.  
  
3.  Un usuario demuestra la entidad de certificación que la clave RSA para el que se solicita el certificado criptográficamente está relacionado con el EKPub y que el usuario posee el EKpriv.  
  
4.  La CA emite un certificado con una directiva de emisión especial OID para indicar que la clave es ahora sancionada estén protegidos por un TPM.  
  
## <a name="BKMK_DeploymentOverview"></a>Introducción a la implementación  
En esta implementación, se supone que una CA de empresa de Windows Server 2012 R2 se ha configurado. Además, los clientes (Windows 8.1) está configurados para inscribirse en esa entidad de certificación empresarial con plantillas de certificado. 

Hay tres pasos para implementar la atestación de clave de TPM:  
  
1.  **Planear el modelo de confianza TPM:** El primer paso es decidir qué modelo de confianza TPM a utilizar. Existen 3 formas admitidas para hacer esto:  
  
    -   **En función de la credencial de usuario de confianza:** La CA de empresa confía en el EKPub proporcionado por el usuario como parte de la solicitud de certificado y se realiza ninguna validación que no sean las credenciales de dominio del usuario.  
  
    -   **En función de EKCert de confianza:** La CA de empresa valida la cadena de EKCert que se proporciona como parte de la solicitud de certificado con una lista controlada por el Administrador de *aceptables cadenas de certificado EK*. Las cadenas aceptables son definidos por el fabricante y se expresan a través de dos almacenes de certificados personalizados en la CA emisora (un almacén para el nivel intermedio) y otro para certificados de CA raíz. Este modo de confianza significa que **todas** TPM de un determinado fabricante son de confianza. Tenga en cuenta que en este modo, los TPM en uso en el entorno deben contener EKCerts.
  
    -   **Confianza basada en EKPub:** La CA de empresa se valida que el EKPub proporcionada como parte de la solicitud de certificado aparece en una lista controlada por el Administrador de permiso EKPubs. Esta lista se expresa como un directorio de archivos donde el nombre de cada archivo en este directorio es el hash SHA-2 de la EKPub permitido. Esta opción ofrece el mayor nivel de aseguramiento, pero requiere más esfuerzo administrativo, ya que cada dispositivo se identifica de forma individual. En este modelo de confianza, se permiten solo los dispositivos que han tenido EKPub del TPM en sus agregado a la lista de permitidos de EKPubs para inscribir un certificado de atestación de TPM.  
  
    Dependiendo de qué método se utiliza, la entidad de certificación aplicará una OID de la directiva de emisión diferente al certificado emitido. Para obtener más información acerca de la directiva de emisión OID, vea la tabla de OID de directiva de emisión de la [configurar una plantilla de certificado](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) en este tema.  
  
    Tenga en cuenta que es posible elegir una combinación de modelos de confianza TPM. En este caso, la CA aceptará cualquiera de los métodos de atestación y la directiva de emisión que OID reflejarán todos los métodos de atestación que se realice correctamente.  
  
2.  **Configurar la plantilla de certificado:** Configuración de la plantilla de certificado se describe en el [detalles de implementación](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) en este tema. En este artículo no cubre cómo esta plantilla de certificado se asigna a la CA de empresa o cómo inscribir concedido el acceso a un grupo de usuarios. Para obtener más información, consulte [lista de comprobación: Configuración de CA para emitir y administrar certificados](https://technet.microsoft.com/library/cc771533.aspx).  
  
3.  **Configurar la CA para el modelo de confianza TPM**  
  
    1.  **En función de la credencial de usuario de confianza:** No se requiere ninguna configuración específica.  
  
    2.  **En función de EKCert de confianza:** El administrador debe obtener los certificados de la cadena de EKCert de fabricantes de TPM e importarlos a dos nuevos almacenes de certificados, creados por el administrador, en la CA que realizan la atestación de clave de TPM. Para obtener más información, consulte el [la configuración de CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) en este tema.  
  
    3.  **Confianza basada en EKPub:** El administrador debe obtener el EKPub para cada dispositivo que se necesitan certificados de atestación de TPM y agregarlos a la lista de permitidos EKPubs. Para obtener más información, consulte el [la configuración de CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) en este tema.  
  
    > [!NOTE]  
    > -   Esta característica requiere Windows 8.1 / Windows Server 2012 R2.  
    > -   No se admite la atestación de clave de TPM para tarjeta inteligente de otros fabricantes KSP. Se debe usar el proveedor de criptografía de Microsoft Platform KSP.  
    > -   Atestación de clave de TPM solo funciona para las claves RSA.  
    > -   Atestación de clave de TPM no se admite para una CA independiente.  
    > -   No se admite la atestación de clave de TPM [procesamiento no persistente de certificados](https://technet.microsoft.com/library/ff934598).  
  
## <a name="BKMK_DeploymentDetails"></a>Detalles de implementación  
  
### <a name="BKMK_ConfigCertTemplate"></a>Configurar una plantilla de certificado  
Para configurar la plantilla de certificado de atestación de clave de TPM, siga los pasos de configuración siguientes:  
  
1.  **Compatibilidad** ficha  
  
    En el **configuración de compatibilidad** sección:  
  
    -   Asegúrese de **Windows Server 2012 R2** está seleccionada para la **entidad de certificación**.  
  
    -   Asegúrese de **Windows 8.1 / Windows Server 2012 R2** está seleccionada para la **destinatario del certificado**.  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **Criptografía** ficha  
  
    Asegúrese de **Key Storage Provider** está seleccionada para la **categoría del proveedor** y **RSA** está seleccionada para la **nombre del algoritmo**. Asegúrese de **las solicitudes deben usar uno de los siguientes proveedores** está seleccionada y el **proveedor criptográfico de plataforma de Microsoft** está seleccionada en **proveedores**.  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **Atestación de clave** ficha  
  
    Se trata de una nueva pestaña para Windows Server 2012 R2:  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    Elegir un modo de atestación de las tres opciones posibles.  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **Ninguno:** Implica que no se debe usar la atestación de clave  
  
    -   **Requerido si el cliente es compatible con:** Permite a los usuarios en un dispositivo que no es compatible con la atestación de clave de TPM para continuar con la inscripción para ese certificado. Los usuarios que pueden realizar la atestación se se distinguen con una OID de directiva de emisión especial. Es posible que algunos dispositivos no pueda realizar la atestación debido a un TPM antiguo que no admiten la atestación de clave o el dispositivo no tiene un TPM de en absoluto.
  
    -   **Obligatorio:** Cliente *debe* realizar la atestación de clave de TPM, en caso contrario, se producirá un error de la solicitud de certificado.  
  
    A continuación, elija el modelo de confianza TPM. Nuevamente, existen tres opciones:  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **Credenciales de usuario:** Permitir que un usuario de autenticación garantizar un TPM válido al especificar sus credenciales de dominio.  
  
    -   **Certificado de aprobación:** Debe validar el EKCert del dispositivo a través de administradas por un administrador TPM certificados de CA intermedios a un certificado de CA raíz administradas por un administrador. Si elige esta opción, debe configurar EKCA y EKRoot almacenes en la CA emisora de certificados como se describe en el [la configuración de CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) en este tema.  
  
    -   **Clave de aprobación:** El EKPub del dispositivo debe aparecer en la lista administrada por el administrador PKI. Esta opción ofrece el mayor nivel de aseguramiento, pero requiere más esfuerzo administrativo. Si elige esta opción, debe configurar una lista EKPub en la CA emisora como se describe en el [la configuración de CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) en este tema.  
  
    Por último, decida qué directiva de emisión que se mostrarán en el certificado emitido. De forma predeterminada, cada tipo de aplicación tiene un identificador de objeto asociado (OID) que se insertará en el certificado si pasa ese tipo de aplicación, como se describe en la tabla siguiente. Tenga en cuenta que es posible elegir una combinación de métodos de cumplimiento. En este caso, la CA aceptará cualquiera de los métodos de atestación y la directiva de emisión OID reflejarán todos los métodos de atestación se realizó correctamente.  
  
    **OID de directiva de emisión**  
  
    |OID|Tipo de atestación de clave|Descripción|Nivel de seguridad|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|"EK comprobado":   Para la lista administrada por el Administrador de EK|Alto|  
    |1.3.6.1.4.1.311.21.31|Certificado de aprobación|"Certificado EK comprobado": Cuando se valida la cadena de certificados EK|Medio|  
    |1.3.6.1.4.1.311.21.32|Credenciales de usuario|"EK de confianza en uso": Para EK atestiguado por el usuario|Bajo|  
  
    Los OID se insertará en el certificado emitido si **incluyen directivas de emisión** está seleccionado (la configuración predeterminada).  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > Un uso posible de tener el OID presente en el certificado es limitar el acceso a VPN o a determinados dispositivos de redes inalámbricas. Por ejemplo, la directiva de acceso podría permitir la conexión (o acceso a una VLAN diferente) si OID 1.3.6.1.4.1.311.21.30 está presente en el certificado. Esto le permite limitar el acceso a dispositivos cuyo EK TPM está presente en la lista EKPUB.  
  
### <a name="BKMK_CAConfig"></a>Configuración de CA  
  
1.  **Configurar EKCA y EKROOT almacenes de certificados de una CA emisora**  
  
    Si eligió **certificado de aprobación** para la configuración de la plantilla, realice los pasos de configuración siguientes:  
  
    1.  Usar Windows PowerShell para crear dos nuevos almacenes de certificados en el servidor de certificación emisora (CA) que realizará la atestación de clave de TPM.  
  
    2.  Obtener el nivel intermedio y raíz de certificados de CA de fabricantes de que desea permitir en su entorno empresarial. No se deben importar dichos certificados en los almacenes de certificados creados previamente (EKCA y EKROOT) según corresponda.  
  
    El siguiente script de Windows PowerShell realiza estos dos pasos. En el ejemplo siguiente, el fabricante del TPM Fabrikam ha proporcionado un certificado raíz *FabrikamRoot.cer* y un certificado de CA emisora *Fabrikamca.cer*.  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **Configurar lista EKPUB si usa el tipo de atestación EK**  
  
    Si eligió **clave de aprobación** en la configuración de la plantilla, los pasos de configuración siguientes crear y configurar una carpeta en la CA emisora, que contiene los archivos de 0 bytes, cada una un nombre para el hash SHA-2 de un EK permitido. Esta carpeta sirve como una "lista de permitidos" de los dispositivos que tienen permiso para obtener certificados de atestación de clave TPM. Dado que debe agregar manualmente el EKPUB para cada dispositivo que requiere un certificado attested, proporciona la empresa con una garantía de los dispositivos que están autorizados a obtener certificados de atestación de TPM clave. La configuración de una entidad de certificación para este modo requiere dos pasos:  
  
    1.  **Cree la entrada de registro EndorsementKeyListDirectories:** Use la herramienta de línea de comandos Certutil para configurar las ubicaciones de carpeta donde EKpubs confianza se definen como se describe en la tabla siguiente.  
  
        |Operación|Sintaxis del comando|  
        |-------------|------------------|  
        |Agregar ubicaciones de carpeta|certutil.exe - setreg CA\EndorsementKeyListDirectories + "<folder>"|  
        |Quitar las ubicaciones de carpeta|certutil.exe - setreg CA\EndorsementKeyListDirectories-"<folder>"|  
  
        El EndorsementKeyListDirectories en el comando certutil es una configuración del registro como se describe en la tabla siguiente.  
  
        |Nombre de valor|Tipo|Datos|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< ruta de acceso LOCAL o UNC EKPUB permitir listas de elementos ><br /><br />Por ejemplo:<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories* contendrá una lista de rutas de acceso de sistema de archivos local o UNC cada señalando a una carpeta que la CA tiene acceso de lectura. Cada carpeta puede contener cero o más permiten enumerar las entradas, donde cada entrada es un archivo con un nombre que sea el SHA-2 hash de una confianza EKpub, sin extensión de archivo. 
        Crear o editar esta configuración de la clave del registro requiere un reinicio de la entidad de certificación, como opciones de configuración del registro de CA existentes. Sin embargo, las modificaciones realizadas en la opción de configuración surtirán efecto inmediatamente y no requerirá la entidad de certificación que se reinicie.  
  
        > [!IMPORTANT]  
        > Proteger las carpetas en la lista de la manipulación y acceso no autorizado mediante la configuración de permisos para que sólo los administradores autorizados leído y acceso de escritura. La cuenta de equipo de la entidad de certificación requiere acceso de solo lectura.  
  
    2.  **Rellenar la lista EKPUB:** Use el siguiente cmdlet de Windows PowerShell para obtener el hash de clave pública de la EK TPM mediante Windows PowerShell en cada dispositivo y, a continuación, enviar este hash de clave pública a la entidad de certificación y almacenarla en la carpeta EKPubList.  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>Solución de problemas  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Campos de atestación de clave no están disponibles en una plantilla de certificado  
Los campos de atestación de clave no están disponibles si la configuración de plantilla no cumple los requisitos para la atestación. Causas comunes son:  
  
1.  La configuración de compatibilidad no está configurada correctamente. Asegúrese de que están configurados como sigue:  
  
    1.  **Entidad de certificación**: **Windows Server 2012 R2**  
  
    2.  **Certificado de destinatario**: **Windows 8.1/Windows Server 2012 R2**  
  
2.  La configuración de cifrado no está configurada correctamente. Asegúrese de que están configurados como sigue:  
  
    1.  **Categoría del proveedor**: **Proveedor de almacenamiento de claves**  
  
    2.  **Nombre del algoritmo**: **RSA**  
  
    3.  **Proveedores de**: **Proveedor de cifrado de la plataforma de Microsoft**  
  
3.  Los valores de control de la solicitud no se ha configurado correctamente. Asegúrese de que están configurados como sigue:  
  
    1.  El **permitir que la clave privada se pueda exportar** opción no debe estar seleccionada.  
  
    2.  El **Archivar clave privada de cifrado de sujeto** opción no debe estar seleccionada.  
  
### <a name="verification-of-tpm-device-for-attestation"></a>Comprobación de un dispositivo para la atestación de TPM  
Use el cmdlet de Windows PowerShell, **confirmar CAEndorsementKeyInfo**, para comprobar que un dispositivo TPM específico se confía para atestación por entidades emisoras de certificados. Hay dos opciones: uno para comprobar la EKCert y otro para comprobar un EKPub. El cmdlet es ejecutar localmente en una entidad de certificación, o en CAs remoto mediante la comunicación remota de Windows PowerShell.  
  
1.  Para comprobar la confianza en un EKPub, realice los dos pasos siguientes:  
  
    1.  **Extraiga el EKPub desde el equipo cliente:** Se puede extraer el EKPub desde un equipo cliente a través de **Get TpmEndorsementKeyInfo**. Desde un símbolo del sistema con privilegios elevados, ejecute lo siguiente:  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **Comprobación de confianza en un EKCert en un equipo de CA:** Copie la cadena extraída (el hash SHA-2 de la EKPub) en el servidor (por ejemplo, por correo electrónico) y páselo al cmdlet Confirm-CAEndorsementKeyInfo. Tenga en cuenta que este parámetro debe tener 64 caracteres.  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  Para comprobar la confianza en un EKCert, realice los dos pasos siguientes:  
  
    1.  **Extraiga el EKCert desde el equipo cliente:** Se puede extraer el EKCert desde un equipo cliente a través de **Get TpmEndorsementKeyInfo**. Desde un símbolo del sistema con privilegios elevados, ejecute lo siguiente:  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```  
  
    2.  **Comprobación de confianza en un EKCert en un equipo de CA:** Copie el EKCert extraído (EkCert.cer) en la entidad de certificación (por ejemplo, a través de correo electrónico o xcopy). Por ejemplo, si copia el archivo de certificado de la carpeta "c:\diagnose" en el servidor de CA, ejecute lo siguiente para finalizar la comprobación:  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>Vea también  
[Información general de tecnología del módulo de plataforma segura](https://technet.microsoft.com/library/jj131725.aspx)  
[Recursos externos: Módulo de plataforma segura](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
