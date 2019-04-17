---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: "Atestación de clave de TPM"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d08efe825e10d526763a1654c7e090427719c51
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="tpm-key-attestation"></a>Atestación de clave de TPM

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, Senior ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de soporte técnico al cliente de Microsoft y está destinada a arquitectos de sistemas que buscan explicaciones técnicas más profundos de características y soluciones de Windows Server 2012 R2 que normalmente que proporcionan los temas en TechNet y administradores experimentados. Sin embargo, no ha sufrido las mismas fases de edición, forma parte del lenguaje que parezca que menos refinada que lo que normalmente se encuentra en TechNet.  
  
## <a name="overview"></a>Introducción  
Aunque la compatibilidad para teclas protegido de TPM ha existido desde Windows 8, se han producido ningún mecanismo para entidades de certificación certificar criptográficamente que la clave privada solicitante realmente está protegida por un módulo de plataforma segura (TPM). Esta actualización permite que una entidad de certificación para realizar esa atestación y para reflejar ese atestación en el certificado emitido.  
  
> [!NOTE]  
> Este artículo se supone que el lector está familiarizado con el concepto de plantilla de certificado (como referencia, consulta [plantillas de certificado](https://technet.microsoft.com/library/cc730705.aspx)). También se supone que el lector está familiarizado con cómo configurar la CA de empresa para emitir certificados basados en plantillas de certificado (como referencia, consulta [lista de comprobación: configurar la CA para emitir y administrar certificados](https://technet.microsoft.com/library/cc771533.aspx)).  
  
### <a name="terminology"></a>Terminología  
  
|Término|Definición|  
|--------|--------------|  
|EK|Clave de aprobación. Se trata de una clave asimétrica contenida dentro del TPM (insertado en el momento de fabricación). La EK es única para cada TPM y poder identificarla. No se puede cambiar o quitar la EK.|  
|EKpub|Hace referencia a la clave pública de la EK.|  
|EKPriv|Hace referencia a la clave privada de la EK.|  
|EKCert|Certificado EK. Un certificado emitido por el fabricante TPM para EKPub. No todos los TPM disponen de EKCert.|  
|TPM|Módulo de plataforma segura. Un TPM está diseñado para proporcionar funciones de relacionadas con la seguridad basada en hardware. Un chip TPM es un procesador de criptografía seguro diseñado para llevar a cabo operaciones criptográficas. El chip incluye varios mecanismos de seguridad física para que sea resistente a las alteraciones y software malintencionado no puede altere las funciones de seguridad del TPM.|  
  
### <a name="background"></a>En segundo plano  
A partir de Windows 8, un módulo de plataforma segura (TPM) puede usarse para proteger la clave privada de un certificado. Plataforma de criptografía de proveedor de clave de almacenamiento proveedor (KSP de Microsoft) se habilita esta característica. Hay dos problemas con la implementación:  

-   No había ninguna garantía de que una clave está realmente protegida por un TPM (alguien fácilmente imite un KSP de software como un KSP del TPM con credenciales de administrador local).

-   No era posible limitar la lista de TPM que se pueden proteger la empresa certificados emitido (en caso de que el Administrador de PKI desea controlar los tipos de dispositivos que pueden usarse para obtener certificados en el entorno).  

### <a name="tpm-key-attestation"></a>Atestación de clave de TPM  
Atestación de clave de TPM es la capacidad de la entidad que solicita un certificado criptográficamente demostrar a una entidad de certificación que la clave RSA en la solicitud de certificado está protegida por TPM "a" o "del" que confíe en la entidad de certificación. El modelo de confianza TPM se describe con mayor detalle en la [Introducción a la implementación](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) sección más adelante en este tema.  
  
### <a name="why-is-tpm-key-attestation-important"></a>¿Por qué es importante la atestación de clave de TPM?  
Un certificado de usuario con una clave atestiguan TPM proporciona mayor garantía de seguridad, una copia de seguridad por la imposibilidad de exportación, protección contra ataques de repetición y aislamiento de las claves proporcionados por el TPM.  
  
Con atestación de clave de TPM, ahora es posible un nuevo paradigma de administración: un administrador puede definir el conjunto de dispositivos que los usuarios pueden usar para acceder a recursos corporativos (por ejemplo, VPN o punto de acceso inalámbrico) y tienen **seguro** garantiza que ningún otro dispositivo puede usarse para acceder a ellos. Este nuevo paradigma de control de acceso es **seguro** dado que está vinculado a un *hardware enlazado* identidad de usuario, lo que es más seguro que una credencial basado en software.
  
### <a name="how-does-tpm-key-attestation-work"></a>¿Cómo funciona la atestación de clave de TPM?  
En general, la atestación de clave de TPM se basa en los siguientes pilares:  
  
1.  Cada TPM se entrega con una única clave asimétrica, llama el *clave de aprobación* (EK), que se grabará por el fabricante. Nos referimos a la parte pública de esta clave como *EKPub* y la clave privada asociada como *EKPriv*. Algunos chips de TPM también tienen un certificado EK emitido por el fabricante de la EKPub. Nos referimos a este certificado como *EKCert*.  
  
2.  Una CA establece la confianza en el TPM, ya sea mediante EKPub o EKCert.  
  
3.  Un usuario se demuestra a la entidad de certificación que la clave RSA para que se solicita el certificado criptográficamente está relacionada con la EKPub y que el usuario posee el EKpriv.  
  
4.  La CA emite un certificado con una directiva de emisión especial OID para indicar que la clave es atestiguada ahora estar protegido por un TPM.  
  
## <a name="BKMK_DeploymentOverview"></a>Introducción a la implementación  
En esta implementación, se supone que se configura una CA de empresa de Windows Server 2012 R2. Además, los clientes (Windows 8.1) está configurados para inscribirse en esa entidad emisora de certificados de empresa mediante plantillas de certificados. 

Hay tres pasos para implementar la atestación de clave de TPM:  
  
1.  **Planear el modelo de confianza TPM:** el primer paso es decidir qué modelo de confianza TPM para usar. Hay 3 formas compatibles para hacer esto:  
  
    -   **Confianza en función de las credenciales de usuario:** la CA de empresa confía en el EKPub proporcionado por el usuario como parte de la solicitud de certificado y se realiza ninguna validación que no sean de credenciales de dominio del usuario.  
  
    -   **En función de confianza de EKCert:** la CA de empresa valida la cadena de EKCert que se proporciona como parte de la solicitud de certificado con una lista que administrados por el Administrador de *aceptables cadenas de certificado EK*. Las cadenas aceptables están definidos por el fabricante y se expresan mediante dos almacenes de certificados personalizado en la CA emisora (una tienda para intermedios) y otro para los certificados de CA de raíz. Este modo de confianza significa que **todos los** TPM de un fabricante determinado es de confianza. Ten en cuenta que en este modo, el TPM en uso en el entorno debe contener EKCerts.
  
    -   **En función de confianza de EKPub:** la CA de empresa valida que el EKPub proporcionado como parte de la solicitud de certificado aparece en una lista controlada por el Administrador de permitido EKPubs. Esta lista se expresa como un directorio de archivos donde el nombre de cada archivo en este directorio es el hash SHA-2 de la EKPub permitido. Esta opción ofrece el nivel de seguridad más alto, pero requiere más trabajo administrativo, porque cada dispositivo se identifica por separado. En este modelo de confianza, solo los dispositivos que han tenido EKPub sus del TPM agregado a la lista de aplicaciones permitidas de EKPubs tienen permiso para inscribir un certificado atestiguan TPM.  
  
    Dependiendo de cuál es el método se usa, la entidad de certificación aplicará una directiva de emisión diferentes OID para el certificado emitido. Para obtener más información acerca de la directiva de emisión OID, consulta la tabla de OID de directiva de emisión de la [configurar una plantilla de certificado](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) sección en este tema.  
  
    Ten en cuenta que es posible elegir una combinación de modelos de confianza TPM. En este caso, la CA aceptará cualquiera de los métodos de atestación y la directiva de emisión que OID reflejará todos los métodos de atestación que se realice correctamente.  
  
2.  **Configurar la plantilla de certificado:** se describe la configuración de la plantilla de certificado en el [detalles de implementación](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) sección en este tema. En este artículo no abarca cómo se asigna esta plantilla de certificado a la CA de empresa o cómo inscribirse se concede acceso a un grupo de usuarios. Para obtener más información, consulta [lista de comprobación: configurar la CA para emitir y administrar certificados](https://technet.microsoft.com/library/cc771533.aspx).  
  
3.  **Configurar la CA para el modelo de confianza TPM**  
  
    1.  **Confianza en función de las credenciales de usuario:** se requiere ninguna configuración específica.  
  
    2.  **En función de confianza de EKCert:** el administrador debe obtener los certificados de la cadena de EKCert de fabricantes TPM e importarlos a dos almacenes de certificados nuevo, creados por el Administrador de la CA que realizan la atestación de clave de TPM. Para obtener más información, consulta el [configuración de la CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) sección en este tema.  
  
    3.  **En función de confianza de EKPub:** el administrador debe obtener el EKPub para cada dispositivo que necesitarán atestiguan TPM certificados y agregarlos a la lista de permitidos EKPubs. Para obtener más información, consulta el [configuración de la CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) sección en este tema.  
  
    > [!NOTE]  
    > -   Esta característica requiere Windows 8.1 o Windows Server 2012 R2.  
    > -   No se admite la atestación de clave de TPM de tarjeta inteligente de terceros KSP. Deben usarse KSP de proveedor de criptografía de plataforma de Microsoft.  
    > -   Atestación de clave de TPM solo funciona para las claves RSA.  
    > -   Atestación de clave de TPM no se admite para una CA independiente.  
    > -   Atestación de clave de TPM no admite [procesamiento del certificado no persistentes](https://technet.microsoft.com/library/ff934598).  
  
## <a name="BKMK_DeploymentDetails"></a>Detalles de implementación  
  
### <a name="BKMK_ConfigCertTemplate"></a>Configurar una plantilla de certificado  
Para configurar la plantilla de certificado de atestación de clave de TPM, realiza los siguientes pasos de configuración:  
  
1.  **Compatibilidad** pestaña  
  
    En la **configuración de compatibilidad** sección:  
  
    -   Asegúrate de **Windows Server 2012 R2** está seleccionado para la **entidad de certificación**.  
  
    -   Asegúrate de **de Windows 8.1 o Windows Server 2012 R2** está seleccionado para la **destinatario de certificado**.  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **Criptografía** pestaña  
  
    Asegúrate de **proveedor de almacenamiento de claves** está activada para la **categoría de proveedor** y **RSA** está activada para la **nombre del algoritmo**. Asegúrate de **las solicitudes deben usar uno de los siguientes proveedores de** está seleccionado y el **proveedor de criptografía de plataforma de Microsoft** está seleccionada en **proveedores**.  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **La atestación de clave** pestaña  
  
    Se trata de una pestaña nueva para Windows Server 2012 R2:  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    Elige un modo de atestación de las tres opciones posibles.  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **None:** implica que no debe usarse la atestación de clave  
  
    -   **Es necesario, si el cliente es compatible con:** permite a los usuarios en un dispositivo que no es compatible con el TPM clave atestación para seguir el proceso de inscripción para ese certificado. Los usuarios que pueden realizar atestación se deben distinguirse con una directiva de emisión especial OID. Es posible que algunos dispositivos no podrá realizar atestación debido a un TPM antiguos que no es compatible con atestación de clave o el dispositivo no disponga de un TPM en absoluto.
  
    -   **Obligatorio:** cliente *debe* realizar la atestación de clave de TPM, lo contrario, se producirá un error de la solicitud de certificado.  
  
    A continuación, elegir el modelo de confianza TPM. Nuevamente, hay tres opciones:  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **Las credenciales de usuario:** permitir que un usuario autenticación atestiguar un TPM válido especificando sus credenciales de dominio.  
  
    -   **Certificado de aprobación:** debe validar la EKCert del dispositivo a través de administradas por un administrador TPM certificados de CA intermedias a un certificado de CA de raíz controlada por el administrador. Si eliges esta opción, debes configurar EKCA y EKRoot almacenes en la CA emisora de certificados, como se describe en la [configuración de la CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) sección en este tema.  
  
    -   **Clave de aprobación:** la EKPub del dispositivo debe aparecer en la lista de controlada por el administrador PKI. Esta opción ofrece el nivel de seguridad más alto, pero requiere más trabajo administrativo. Si eliges esta opción, debes configurar una lista de EKPub de la CA emisora como se describe en la [configuración de la CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) sección en este tema.  
  
    Por último, decide qué directiva de emisión para mostrar en el certificado emitido. De manera predeterminada, cada tipo de aplicación tiene un identificador de objeto asociado (OID) que se inserta en el certificado si pasa ese tipo de aplicación, como se describe en la siguiente tabla. Ten en cuenta que es posible elegir una combinación de métodos de la aplicación. En este caso, la CA aceptará cualquiera de los métodos de atestación y la directiva de emisión OID reflejará todos los métodos de atestación que se realizó correctamente.  
  
    **OID de directiva de emisión**  
  
    |OID|Tipo de atestación de clave|Descripción|Nivel de seguridad|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|"EK verificado": administrado por el Administrador de la lista de EK|Alto|  
    |1.3.6.1.4.1.311.21.31|Certificado de aprobación|"Verificado de certificado EK": cuando se valida la cadena de certificados EK|Mediano|  
    |1.3.6.1.4.1.311.21.32|Credenciales de usuario|"EK de confianza en uso": para EK atestiguan por el usuario|Baja|  
  
    El OID se insertarán en el certificado emitido si **incluir directivas de emisión** está activada (la configuración predeterminada).  
  
    ![Atestación de clave de TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > Uno de los posible usos de tener el OID presente en el certificado es limitar el acceso a la VPN o ciertos dispositivos de redes inalámbricas. Por ejemplo, la directiva de acceso podría permitir conexión (o el acceso a una VLAN diferente), si está presente en el certificado OID 1.3.6.1.4.1.311.21.30. Esto te permite limitar el acceso a dispositivos cuyo EK TPM está presente en la lista EKPUB.  
  
### <a name="BKMK_CAConfig"></a>Configuración de entidad emisora  
  
1.  **EKCA y EKROOT almacenes de certificados en una entidad emisora del programa de instalación**  
  
    Si has elegido **certificado de aprobación** para la configuración de plantilla, realiza los siguientes pasos de configuración:  
  
    1.  Usar Windows PowerShell para crear dos nuevos almacenes de certificados en el servidor de la entidad de (certificación CA) de certificación que realizará la atestación de clave de TPM.  
  
    2.  Obtener intermedios y raíz certificados de fabricantes que desea permitir en el entorno empresarial. Dichos certificados deben importarse a los almacenes de certificados creado anteriormente (EKCA y EKROOT) según corresponda.  
  
    El siguiente script de Windows PowerShell realiza estos dos pasos. En el siguiente ejemplo, el fabricante del TPM Fabrikam proporcionó un certificado raíz *FabrikamRoot.cer* y un certificado de CA *Fabrikamca.cer*.  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **Lista de EKPUB del programa de instalación si usando el tipo de atestación EK**  
  
    Si has elegido **clave de aprobación** en la configuración de plantilla, los siguientes pasos de configuración crear y configurar una carpeta de la CA emisora, que contiene los archivos de 0 bytes, cada uno con nombre para el valor de hash SHA-2 de un EK permitido. Esta carpeta actúa como una "lista de permitidos" de los dispositivos que tienen permiso para obtener certificados atestiguan clave TPM. Dado que se debe agregar manualmente la EKPUB para cada dispositivo que requiere un certificado atestado, proporciona la empresa con la garantía de los dispositivos que tienen autorización para obtener certificados claves atestados de TPM. Configurar una CA de este modo, requiere dos pasos:  
  
    1.  **Crear la entrada del registro EndorsementKeyListDirectories:** usar la herramienta de línea de comandos de Certutil para configurar las ubicaciones de la carpeta donde se definen EKpubs confianza tal como se describe en la siguiente tabla.  
  
        |Operación|Sintaxis de comandos|  
        |-------------|------------------|  
        |Agregar ubicaciones de carpeta|certutil.exe - setreg CA\EndorsementKeyListDirectories + "<folder>"|  
        |Quitar ubicaciones de carpeta|certutil.exe - setreg CA\EndorsementKeyListDirectories-"<folder>"|  
  
        El EndorsementKeyListDirectories en certutil comando es un valor del registro como se describe en la siguiente tabla.  
  
        |Nombre de valor|Tipo|Datos|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< ruta de acceso LOCAL o UNC EKPUB permitir listas ><br /><br />Ejemplo:<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories* contendrá una lista de rutas de acceso de sistema de archivos local o UNC cada apuntando a una carpeta que la entidad de certificación tiene acceso de lectura. Cada carpeta puede contener cero o más permiten entradas de la lista, donde cada entrada es un archivo cuyo nombre es el SHA-2 hash de una confianza EKpub, con ninguna extensión de archivo. 
        Crear o modificar esta configuración de la clave del registro requiere un reinicio de la CA, igual que las opciones de configuración del registro de CA existentes. Sin embargo, las modificaciones realizadas en la opción de configuración surtirán efecto inmediatamente y no requiere la CA que reiniciarse.  
  
        > [!IMPORTANT]  
        > Proteger las carpetas en la lista contra alteraciones y el acceso no autorizado mediante la configuración de permisos para que solo los administradores de autorizados leído y acceso de escritura. La cuenta de equipo de la CA requiere acceso de sólo lectura.  
  
    2.  **Rellena la lista EKPUB:** usar el siguiente cmdlet de Windows PowerShell para obtener el hash de clave público de la EK TPM mediante Windows PowerShell en cada dispositivo y, a continuación, enviar este público clave hash para la entidad de certificación y almacenarlo en la carpeta EKPubList.  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublickKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>Solución de problemas  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Campos de atestación de clave no están disponibles en una plantilla de certificado  
Los campos de la atestación de clave no están disponibles si la configuración de plantilla no cumplen los requisitos de certificación. Razones comunes son:  
  
1.  La configuración de compatibilidad no está configurada correctamente. Asegúrate de que estén configurados como sigue:  
  
    1.  **Entidad de certificación**: **Windows Server 2012 R2**  
  
    2.  **Destinatario de certificado**: **Windows 8.1 o Windows Server 2012 R2**  
  
2.  La configuración de cifrado no está configurada correctamente. Asegúrate de que estén configurados como sigue:  
  
    1.  **Categoría de proveedor**: **proveedor de almacenamiento de claves**  
  
    2.  **Nombre del algoritmo**: **RSA**  
  
    3.  **Proveedores de**: **proveedor criptográfico de la plataforma de Microsoft**  
  
3.  La configuración de control de la solicitud no se configura correctamente. Asegúrate de que estén configurados como sigue:  
  
    1.  La **permitir que la clave privada se pueda exportar** opción no debe estar seleccionada.  
  
    2.  La **Archivar clave privada de cifrado de sujeto** opción no debe estar seleccionada.  
  
### <a name="verification-of-tpm-device-for-attestation"></a>Comprobación del dispositivo TPM para la certificación  
Usa el cmdlet de Windows PowerShell, **confirmar CAEndorsementKeyInfo**, para comprobar que un dispositivo específico de TPM es de confianza para la certificación por entidades emisoras de certificados. Existen dos opciones: uno para comprobar la EKCert y otro para comprobar una EKPub. El cmdlet es ejecutar localmente en una entidad de certificación, o en entidades emisoras remoto mediante el uso de Windows PowerShell remoto.  
  
1.  Para comprobar la relación de confianza en una EKPub, realiza los siguientes dos pasos:  
  
    1.  **Extraer el EKPub desde el equipo cliente:** EKPub el pueden extraerse de un equipo cliente a través de **Get-TpmEndorsementKeyInfo**. Desde un símbolo del sistema con privilegios elevados, ejecuta lo siguiente:  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **Comprueba la confianza en una EKCert en un equipo de entidad emisora de certificados:** copiar la cadena extraída (el hash SHA-2 de la EKPub) en el servidor (por ejemplo, a través de correo electrónico) y pasarlo al cmdlet confirmar CAEndorsementKeyInfo. Ten en cuenta que este parámetro debe ser de 64 caracteres.  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  Para comprobar la relación de confianza en una EKCert, realiza los siguientes dos pasos:  
  
    1.  **Extraer el EKCert desde el equipo cliente:** EKCert el pueden extraerse de un equipo cliente a través de **Get-TpmEndorsementKeyInfo**. Desde un símbolo del sistema con privilegios elevados, ejecuta lo siguiente:  
  
        ```  
        PS C:>\$a= Get- TpmEndorsementKeyInfo  
        PS C:>\$a.manufacturerCertificates|Export-Certificate c:\myEkcert.cer  
        ```  
  
    2.  **Comprueba la confianza en una KCert en un equipo de entidad emisora de certificados:** copiar el EKCert extraído (EkCert.cer) a la entidad de certificación (por ejemplo, a través de correo electrónico o xcopy). Por ejemplo, si se copia el archivo de certificado de la carpeta "c:\diagnose" en el servidor de entidad emisora de certificados, ejecuta lo siguiente para finalizar la comprobación:  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>Consulta también  
[Información general sobre la tecnología de módulo de plataforma segura](https://technet.microsoft.com/library/jj131725.aspx)  
[Recurso externo: Módulo de plataforma segura](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
  


