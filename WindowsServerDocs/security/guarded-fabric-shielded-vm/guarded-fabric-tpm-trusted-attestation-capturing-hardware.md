---
title: Capturar la información en modo TPM requerida por HGS
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 04/01/2019
ms.openlocfilehash: c1d169147c6b09c8a238163a961c192e3e483728
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155951"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>Autorizar hosts protegidos mediante la atestación basada en TPM

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

El modo TPM usa un identificador de TPM (también denominado identificador de plataforma o clave de aprobación \[ EKpub \] ) para empezar a determinar si un host determinado está autorizado como "protegido". Este modo de atestación utiliza medidas de arranque seguro y de integridad de código para garantizar que un host de Hyper-V determinado se encuentre en un estado correcto y ejecute solo código de confianza. Para que la atestación entienda qué es y no es correcto, debe capturar los siguientes artefactos:

1.  Identificador de TPM (EKpub)

    -  Esta información es única para cada host de Hyper-V

2.  Línea de base de TPM (medidas de arranque)

    -  Esto es aplicable a todos los hosts de Hyper-V que se ejecutan en la misma clase de hardware

3.  Directiva de integridad de código (un permitidos de archivos binarios permitidos)

    -  Esto es aplicable a todos los hosts de Hyper-V que comparten hardware y software común.

Se recomienda capturar la Directiva de línea de base y de CI desde un "host de referencia" que sea representativo de cada clase única de configuración de hardware de Hyper-V en el centro de recursos. A partir de la versión 1709 de Windows Server, las directivas de CI de ejemplo se incluyen en C:\Windows\schemas\CodeIntegrity\ExamplePolicies.

## <a name="versioned-attestation-policies"></a>Directivas de atestación de versiones

Windows Server 2019 presenta un nuevo método para la atestación, denominado *atestación V2*, donde debe existir un certificado TPM para agregar EKPUB a HGS. El método de atestación v1 usado en Windows Server 2016 le permitía invalidar esta comprobación de seguridad mediante la especificación de la marca-Force al ejecutar Add-HgsAttestationTpmHost u otros cmdlets de atestación de TPM para capturar los artefactos. A partir de Windows Server 2019, se usa la atestación V2 de forma predeterminada y es necesario especificar la marca-PolicyVersion v1 al ejecutar Add-HgsAttestationTpmHost si necesita registrar un TPM sin un certificado. La marca-Force no funciona con la atestación V2.

Un host solo puede atestiguar si todos los artefactos (EKPub + Directiva de línea base de TPM + Directiva de CI) usan la misma versión de atestación. La atestación V2 se prueba primero y, si se produce un error, se usa la atestación v1. Esto significa que si necesita registrar un identificador de TPM mediante la atestación v1, también debe especificar la marca-PolicyVersion V1 para usar la atestación v1 al capturar la línea base de TPM y crear la Directiva de CI. Si la base de referencia de TPM y la Directiva de CI se crearon con la atestación V2 y posteriormente necesita agregar un host protegido sin un certificado de TPM, debe volver a crear cada artefacto con la marca-PolicyVersion v1.

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>Capturar el identificador de TPM (identificador de plataforma o EKpub) de cada host

1.  En el dominio del tejido, asegúrese de que el TPM de cada host está listo para su uso; es decir, el TPM se inicializa y se obtiene la propiedad. Puede comprobar el estado del TPM si abre la consola de administración de TPM (TPM. msc) o si ejecuta **Get-TPM** en una ventana de Windows PowerShell con privilegios elevados. Si el TPM no está en el estado **listo** , tendrá que inicializarlo y establecer su propiedad. Esto puede hacerse en la consola de administración de TPM o mediante la ejecución de **Initialize-TPM**.

2.  En cada host protegido, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios elevados para obtener su EKpub. En `<HostName>` , sustituya el nombre de host único por un elemento que sea adecuado para identificar este host (puede ser su nombre de host o el nombre usado por un servicio de inventario de tejido (si está disponible). Para mayor comodidad, asigne un nombre al archivo de salida con el nombre del host.

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  Repita los pasos anteriores para cada host que se convertirá en un host protegido y asegúrese de asignar un nombre único a cada archivo XML.

4.  Proporcione los archivos XML resultantes al administrador de HGS.

5.  En el dominio HGS, abra una consola de Windows PowerShell con privilegios elevados en un servidor HGS y ejecute el siguiente comando. Repita el comando para cada uno de los archivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Si se produce un error al agregar un identificador de TPM con respecto a un certificado de clave de aprobación que no es de confianza (EKCert), asegúrese de que los [certificados raíz del TPM de confianza se han agregado](guarded-fabric-install-trusted-tpm-root-certificates.md) al nodo HGS.
    > Además, algunos proveedores de TPM no usan EKCerts.
    > Puede comprobar si falta un EKCert abriendo el archivo XML en un editor como el Bloc de notas y comprobando si hay un mensaje de error que indica que no se encontró EKCert.
    > Si este es el caso y confía en que el TPM del equipo es auténtico, puede usar el `-Force` parámetro para agregar el identificador de host a HGS. En Windows Server 2019, también debe usar el `-PolicyVersion v1` parámetro al utilizar `-Force` . Esto crea una directiva coherente con el comportamiento de Windows Server 2016 y requerirá que use `-PolicyVersion v1` al registrar también la Directiva de CI y la línea de base de TPM.

## <a name="create-and-apply-a-code-integrity-policy"></a>Crear y aplicar una directiva de integridad de código

Una directiva de integridad de código ayuda a garantizar que solo se permita la ejecución de los archivos ejecutables en los que confíe en un host.
Se impide la ejecución de malware y otros ejecutables fuera de los archivos ejecutables de confianza.

Cada host protegido debe tener una directiva de integridad de código aplicada para ejecutar máquinas virtuales blindadas en modo TPM.
Las directivas de integridad de código exactas se especifican agregándolas a HGS.
Las directivas de integridad de código se pueden configurar para aplicar la Directiva, bloqueando cualquier software que no cumpla la Directiva o simplemente auditar (registrar un evento cuando se ejecute software no definido en la Directiva).

A partir de la versión 1709 de Windows Server, las directivas de integridad de código de ejemplo se incluyen con Windows en C:\Windows\schemas\CodeIntegrity\ExamplePolicies. Se recomiendan dos directivas para Windows Server:

- **AllowMicrosoft**: permite todos los archivos firmados por Microsoft. Esta Directiva se recomienda para las aplicaciones de servidor como SQL o Exchange, o si el servidor es supervisado por agentes publicados por Microsoft.
- **DefaultWindows_Enforced**: solo permite archivos distribuidos en Windows y no permite otras aplicaciones publicadas por Microsoft, como Office. Esta Directiva se recomienda para los servidores que ejecutan solo roles de servidor integrados y características como Hyper-V.

Se recomienda crear primero la Directiva de CI en modo auditoría (registro) para ver si falta algo y, a continuación, aplicar la Directiva para las cargas de trabajo de producción del host.

Si usa el cmdlet [New-CIPolicy](/powershell/module/configci/new-cipolicy?view=win10-ps) para generar su propia Directiva de integridad de código, deberá decidir los niveles de regla que se usarán.
Se recomienda un nivel principal de **publicador** con la reserva para el **hash**, lo que permite que la mayoría del software firmado digital se actualice sin cambiar la Directiva de CI.
El nuevo software escrito por el mismo publicador también se puede instalar en el servidor sin cambiar la Directiva de CI.
Se aplicará un algoritmo hash a los archivos ejecutables que no estén firmados digitalmente: las actualizaciones de estos archivos requerirán la creación de una nueva Directiva de CI.
Para obtener más información sobre los niveles de regla de directiva de CI disponibles, vea [implementar directivas de integridad de código: reglas de directivas y reglas de archivos](/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) y ayuda de cmdlet.

1.  En el host de referencia, genere una nueva Directiva de integridad de código. Los siguientes comandos crean una directiva en el nivel de **publicador** con la reserva para el **hash**. A continuación, convierte el archivo XML en el formato de archivo binario Windows y HGS deben aplicar y medir la Directiva de CI, respectivamente.

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >El comando anterior crea una directiva de CI solo en modo auditoría. No impedirá que los binarios no autorizados se ejecuten en el host. Solo debe utilizar directivas aplicadas en producción.

2.  Mantenga el archivo de directiva de integridad de código (archivo XML) donde puede encontrarlo fácilmente. Tendrá que editar este archivo posteriormente para aplicar la Directiva de CI o fusionar mediante combinación los cambios de las actualizaciones futuras realizadas en el sistema.

3.  Aplique la Directiva de CI al host de referencia:

    1.  Ejecute el siguiente comando para configurar el equipo para usar la Directiva de CI. También puede implementar la Directiva de CI con [Directiva de grupo](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy) o [System Center Virtual Machine Manager](/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm).

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  Reinicie el host para aplicar la Directiva.

3.  Pruebe la Directiva de integridad de código mediante la ejecución de una carga de trabajo típica. Esto puede incluir máquinas virtuales en ejecución, agentes de administración del tejido, agentes de copia de seguridad o herramientas de solución de problemas en la máquina. Compruebe si hay infracciones de integridad de código y actualice la Directiva de CI si es necesario.

4.  Cambie la Directiva de CI a modo forzado mediante la ejecución de los siguientes comandos en el archivo XML de directiva de CI actualizado.

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  Aplique la Directiva de CI a todos los hosts (con una configuración de hardware y software idéntica) mediante los siguientes comandos:

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }

    Restart-Computer
    ```

    >[!NOTE]
    >Tenga cuidado al aplicar directivas de CI a los hosts y al actualizar cualquier software en estos equipos. Cualquier controlador de modo kernel que no sea compatible con la Directiva de CI puede impedir que se inicie la máquina.

6.  Proporcione el archivo binario (en este ejemplo, HW1CodeIntegrity \_ obligatorio. p7b) al administrador de HGS.

7.  En el dominio HGS, copie la Directiva de integridad de código en un servidor HGS y ejecute el siguiente comando.

    En `<PolicyName>` , especifique un nombre para la Directiva de CI que describe el tipo de host al que se aplica. Un procedimiento recomendado consiste en asignarle un nombre después de la marca y el modelo de su equipo y cualquier configuración de software especial que se ejecute en él.<br>Para `<Path>` , especifique la ruta de acceso y el nombre de archivo de la Directiva de integridad de código.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>Capturar la línea de base de TPM para cada clase de hardware única

Se requiere una línea base de TPM para cada clase de hardware única en el tejido del centro de recursos. Vuelva a usar un "host de referencia".

1. En el host de referencia, asegúrese de que está instalado el rol de Hyper-V y la característica de compatibilidad de Hyper-V de protección de host.

    >[!WARNING]
    >La característica de compatibilidad de Hyper-V de protección de host permite la protección basada en la virtualización de la integridad de código que puede ser incompatible con algunos dispositivos. Se recomienda encarecidamente probar esta configuración en el laboratorio antes de habilitar esta característica. Si no lo haces pueden producirse errores inesperados, incluida la pérdida de datos o un error de pantalla azul (también llamado error grave).

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. Para capturar la Directiva de línea de base, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios elevados.

    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >Tendrá que usar la marca **-SkipValidation** si el host de referencia no tiene habilitado el arranque seguro, se aplicará una directiva de integridad de código, un sistema de seguridad basado en virtualización y una directiva de integridad de código. Estas validaciones están diseñadas para que conozca los requisitos mínimos para ejecutar una máquina virtual blindada en el host. El uso de la marca-SkipValidation no cambia la salida del cmdlet; simplemente silencia los errores.

3.  Proporcione la línea de base de TPM (archivo TCGlog) al administrador de HGS.

4.  En el dominio HGS, copie el archivo TCGlog en un servidor HGS y ejecute el siguiente comando. Normalmente, asignará un nombre a la Directiva después de la clase de hardware que representa (por ejemplo, "revisión del modelo del fabricante").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Confirmar la atestación](guarded-fabric-confirm-hosts-can-attest-successfully.md)