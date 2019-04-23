---
title: Capturar información de modo de TPM requerido HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 12/12/2018
ms.openlocfilehash: 82171eee10a06cad6bb3ac30e8f771086975c242
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841666"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>Autorizar a los hosts protegidos con la atestación de TPM

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Modo TPM utiliza un identificador TPM (también denominada clave de aprobación o de identificador de plataforma \[EKpub\]) para empezar a determinar si un host concreto está autorizado como "protegido". Este modo de atestación utiliza medidas de integridad de arranque seguro y el código para asegurarse de que un determinado host de Hyper-V está en un estado correcto y que ejecuta solo el código de confianza. En orden para la atestación comprender qué es y no es correcto, debe capturar los siguientes artefactos:

1.  Identificador TPM (EKpub)

    -  Esta información es única para cada host de Hyper-V

2.  Línea de base TPM (mediciones de arranque)

    -  Esto es aplicable a todos los hosts de Hyper-V que se ejecutan en la misma clase de hardware

3.  Directiva de integridad de código (una lista blanca de binarios permitidos)

    -  Esto es aplicable a todos los hosts de Hyper-V que comparten el hardware y software comunes

Se recomienda que capturan la línea base y la directiva de CI desde un host de"referencia" representativos de cada clase de configuración de hardware de Hyper-V dentro de su centro de datos única. A partir de Windows Server versión 1709, las directivas de CI de ejemplo se incluyen C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 

## <a name="versioned-attestation-policies"></a>Directivas de atestación con control de versiones

Windows Server 2019 presenta un nuevo método de atestación, llamado *v2 atestación*, donde un certificado TPM debe estar presente con el fin de agregar el EKPub a HGS. El método de atestación v1 usado en Windows Server 2016 permite invalidar esta comprobación de seguridad especificando-marca Force al ejecutar Add-HgsAttestationTpmHost u otros cmdlets de atestación de TPM para capturar los artefactos. A partir de Windows Server 2019, atestación v2 se usa de forma predeterminada y debe especificar la marca de v1 - PolicyVersion al ejecutar Add-HgsAttestationTpmHost si necesita registrar un TPM sin un certificado. -Force marca no funciona con la atestación de v2. 

Un host puede atestiguar solo si todos los artefactos (EKPub + TPM previsto + directiva de CI) usan la misma versión de atestación. Primero se intenta la atestación de v2, y si se produce un error, se usa la atestación de v1. Esto significa que si tiene que registrar un identificador TPM mediante el uso de atestación de v1, deberá especificar también el indicador de v1 - PolicyVersion para usar la atestación de v1 al capturar la línea de base TPM y crear la directiva de CI. Si la línea de base TPM y la directiva de CI se crearon con la atestación de v2 y más adelante, deberá agregar un host protegido sin un certificado TPM, deberá volver a crear cada artefacto con el indicador de v1 - PolicyVersion. 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>Capture el identificador TPM (identificador de plataforma o EKpub) para cada host

1.  En el dominio de fabric, asegúrese de que el TPM en cada host está listo para su uso: es decir, se inicializa el TPM y obtener la propiedad. Puede comprobar el estado de TPM, abra la consola de administración de TPM (tpm.msc) o mediante la ejecución de **Get Tpm** en una ventana de Windows PowerShell con privilegios elevados. Si el TPM no está en el **listo** de estado, debe inicializarlo y establezca su propiedad. Esto puede hacerse en la consola de administración de TPM o ejecutando **inicializar Tpm**.

2.  En cada host protegido, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios elevados para obtener su EKpub. Para `<HostName>`, sustituya el nombre de host único con algo adecuado para identificar este host, esto puede ser su nombre de host o el nombre utilizado por un servicio de inventario de tejido (si está disponible). Para mayor comodidad, el nombre del archivo de salida con el nombre del host.

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  Repita los pasos anteriores para cada host que se convertirá en un host protegido, asegúrese de asignar un nombre único de cada archivo XML.

4.  Proporcione los archivos XML resultantes para el Administrador de HGS.

5.  En el dominio HGS, abra una consola de Windows PowerShell con privilegios elevados en un servidor HGS y ejecute el siguiente comando. Repita el comando para cada uno de los archivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Si se produce un error al agregar un identificador TPM con respecto a un certificado de clave de aprobación (EKCert) que no se confía, asegúrese de que el [se han agregado certificados de raíz TPM de confianza](guarded-fabric-install-trusted-tpm-root-certificates.md) en el nodo HGS.
    > Además, algunos proveedores TPM no use EKCerts.
    > Puede comprobar si falta una EKCert abriendo el archivo XML en un editor como Bloc de notas y buscando un mensaje de error que indica ninguna EKCert se encontró.
    > Si es así, y confía en que el TPM en el equipo es auténtico, puede usar el `-Force` parámetro para agregar el identificador de host a HGS. En Windows Server 2019, deberá usar también el `-PolicyVersion v1` al usar `-Force`. Esto crea una directiva coherente con el comportamiento de Windows Server 2016 y será necesario usar `-PolicyVersion v1` al registrar la directiva de CI y también la línea de base TPM.

## <a name="create-and-apply-a-code-integrity-policy"></a>Crear y aplicar una directiva de integridad de código

Una directiva de integridad de código ayuda a garantizar que sólo los archivos ejecutables de que confianza para ejecutarse en un host se pueden ejecutar. Malware y otros ejecutables fuera de los archivos ejecutables de confianza se impedirá la ejecución.

Cada host protegido debe tener una directiva de integridad de código aplica para ejecutar máquinas virtuales blindadas en el modo TPM. Especifique las directivas de integridad de código exacto que confía agregando HGS. Directivas de integridad de código pueden configurarse para aplicar la directiva, cualquier software que no cumple con la directiva, o simplemente auditar de bloqueo (un evento cuando se ejecuta software que no está definido en la directiva de registro). 

A partir de Windows Server versión 1709, las directivas de integridad de código de ejemplo se incluyen con Windows en C:\Windows\schemas\CodeIntegrity\ExamplePolicies. Se recomiendan dos directivas para Windows Server:

- **AllowMicrosoft**: Permite que todos los archivos firmados por Microsoft. Esta directiva se recomienda para las aplicaciones de servidor, como SQL o Exchange, o si el servidor está supervisado por agentes publicados por Microsoft.
- **DefaultWindows_Enforced**: Permite que sólo los archivos que se incluye en Windows y no permiten que otras aplicaciones publicadas por Microsoft, como Office. Para los servidores que ejecutan solo roles de servidor integrado y características como Hyper-V, se recomienda esta directiva. 

Se recomienda que primero cree la directiva de CI en modo de auditoría (registro) para ver si falta nada y después aplicar la directiva para hospedar cargas de trabajo de producción. 

Si usas el [New CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) cmdlet para generar su propia directiva de integridad de código, debe decidir los niveles de regla usar. Se recomienda un nivel primario de **Publisher** con reserva **Hash**, lo que permite más digitalmente firmado software actualizarse sin necesidad de cambiar la directiva de CI.
Nuevo software escrito por el mismo editor también puede instalarse en el servidor sin cambiar la directiva de CI.
Se resolverán los archivos ejecutables que no estén firmados digitalmente, las actualizaciones a estos archivos, deberá crear una nueva directiva de CI.
Para obtener más información acerca de los niveles de regla de directiva de CI disponibles, consulte [implementar directivas de integridad de código: reglas de directivas y reglas de archivo](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) y ayuda de cmdlet.

1.  En el host de referencia, generar una nueva directiva de integridad de código. Los siguientes comandos crean una directiva en el **Publisher** nivel con reserva **Hash**. A continuación, se convierte el archivo XML al formato de archivo binario Windows y HGS necesitan aplicar y medir la directiva de CI, respectivamente.

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >El comando anterior crea una directiva de CI en solo en modo auditoría. No bloqueará los archivos binarios no autorizados se ejecuten en el host. Solo debe usar las directivas aplicadas en producción.

2.  Mantener el archivo de directivas de integridad de código (archivo XML), donde puede encontrarlo fácilmente. Deberá modificar este archivo más adelante para aplicar la directiva de CI o mezcla en cambios en futuras actualizaciones realizadas en el sistema.

3.  Aplicar la directiva de CI a su host de referencia:

    1.  Copie el archivo binario de directiva de CI (HW1CodeIntegrity.p7b) en la siguiente ubicación en el host de referencia (el nombre de archivo debe coincidir exactamente con):<br>
        **C:\\Windows\\System32\\CodeIntegrity\\SIPolicy.p7b**

    2.  Reinicie el host para aplicar la directiva.

3.  Probar la directiva de integridad de código mediante la ejecución de una carga de trabajo típica. Esto puede incluir la ejecución de máquinas virtuales, los agentes de administración del tejido, los agentes de copia de seguridad, o herramientas en el equipo para solucionar problemas. Compruebe si hay infracciones de integridad de código y actualizar la directiva de CI si es necesario.

4.  Cambie la directiva de CI a modo forzado ejecutando los comandos siguientes en el archivo actualizado de XML de directiva de CI.

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  Aplicar la directiva de CI a todos los hosts (con la configuración de hardware y software idéntica) mediante los siguientes comandos:

    ```powershell
    Copy-Item -Path '<Path to HW1CodeIntegrity\_enforced.p7b>' -Destination 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'

    Restart-Computer
    ```

    >[!NOTE]
    >Tenga cuidado al aplicar las directivas de CI a hosts y al actualizar cualquier software en estos equipos. Los controladores de modo kernel que no son compatibles con la directiva de CI pueden impedir que se inicia la máquina. 

6.  Proporcione el archivo binario (en este ejemplo, HW1CodeIntegrity\_enforced.p7b) para el Administrador de HGS.

7.  En el dominio HGS, copie la directiva de integridad de código a un servidor HGS y ejecute el siguiente comando.

    Para `<PolicyName>`, especifique un nombre para la directiva de CI que describe el tipo de host se aplica a. Una práctica recomendada es un nombre después de la marca y modelo de la máquina y cualquier configuración especial de software se ejecute en él.<br>Para `<Path>`, especifique la ruta de acceso y el nombre de la directiva de integridad de código.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>Captura de la línea de base TPM para cada clase única de hardware

Una línea de base TPM se requiere para cada clase de hardware en el tejido del centro de datos única. Usar un host de"referencia" de nuevo. 

1. En el host de referencia, asegúrese de que el rol Hyper-V y la característica de compatibilidad de Hyper-V de guardián de Host están instalados.

    >[!WARNING]
    >La característica de compatibilidad de Hyper-V de guardián de Host permite protección basada en virtualización de integridad de código que puede no ser compatible con algunos dispositivos. Se recomienda esta configuración de pruebas en su laboratorio antes de habilitar esta característica. Si no lo haces pueden producirse errores inesperados, incluida la pérdida de datos o un error de pantalla azul (también llamado error grave).

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. Para capturar la directiva de línea de base, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios elevados.
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >Deberá usar el **- SkipValidation** marca indica si el host de referencia no tiene el arranque seguro habilitado, un IOMMU presente, habilitado de seguridad basada en virtualización y ejecución o aplicar una directiva de integridad de código. Estas validaciones están diseñadas para que sea consciente de los requisitos mínimos de la ejecución de una máquina virtual blindada en el host. Con la marca - SkipValidation no cambia la salida del cmdlet; simplemente silencia los errores.

3.  Proporcione la línea de base TPM (archivo TCGlog) al administrador HGS.

4.  En el dominio HGS, copie el archivo TCGlog en un servidor HGS y ejecute el siguiente comando. Normalmente, se denominará la directiva después de la clase de hardware que representa (por ejemplo, "Revisión de modelo del fabricante").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Confirmar atestación](guarded-fabric-confirm-hosts-can-attest-successfully.md)
