---
title: 'Máquinas virtuales blindadas para inquilinos: creación de datos de blindaje para definir una máquina virtual blindada'
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 26ff5e27494e2f42a0c8e4d28e2b9820f8d19e6a
ms.sourcegitcommit: 471464a674a53c468a2f1e28575c91245ce9badf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2019
ms.locfileid: "75548190"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>Máquinas virtuales blindadas para inquilinos: creación de datos de blindaje para definir una máquina virtual blindada

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Un archivo de datos de blindaje (también denominado archivo de datos de aprovisionamiento o archivo PDK) es un archivo cifrado que crea un inquilino o el propietario de la máquina virtual para proteger la información de configuración de máquina virtual importante, como la contraseña del administrador, RDP y otros certificados relacionados con la identidad o las credenciales de unión a un dominio, entre otra. En este tema se proporciona información sobre cómo crear un archivo de datos de blindaje. Antes de poder crear el archivo, debe obtener un disco de plantilla del proveedor de servicios de hosting o crear un disco de plantilla, como se describe en [máquinas virtuales blindadas para inquilinos: creación de un disco de plantilla (opcional)](guarded-fabric-tenant-creates-template-disk.md).

Para obtener una lista y un diagrama del contenido de un archivo de datos de blindaje, vea [¿Qué son los datos de blindaje y por qué es necesario?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

> [!IMPORTANT]
> Los pasos de esta sección se deben completar en un equipo independiente y de confianza fuera del tejido protegido. Normalmente, el propietario de la máquina virtual (inquilino) crea los datos de blindaje para sus máquinas virtuales, no los administradores del tejido.

Para preparar la creación de un archivo de datos de blindaje, realice los pasos siguientes:

- [Obtener un certificado para Conexión a Escritorio remoto](#optional-obtain-a-certificate-for-remote-desktop-connection)
- [Crear un archivo de respuesta](#create-an-answer-file)
- [Obtener el archivo de catálogo de firmas de volumen](#get-the-volume-signature-catalog-file)
- [Seleccionar tejidos de confianza](#select-trusted-fabrics)

Después, puede crear el archivo de datos de blindaje:

- [Creación de un archivo de datos de blindaje y adición de guardianes](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)

## <a name="optional-obtain-a-certificate-for-remote-desktop-connection"></a>Opta Obtener un certificado para Conexión a Escritorio remoto

Dado que los inquilinos solo pueden conectarse a sus máquinas virtuales blindadas mediante Conexión a Escritorio remoto u otras herramientas de administración remota, es importante asegurarse de que los inquilinos puedan comprobar que se conectan al punto de conexión derecho (es decir, no hay un "hombre en el centro"). interceptar la conexión).

Una manera de comprobar que se está conectando al servidor previsto es instalar y configurar un certificado para que Servicios de Escritorio remoto presente al iniciar una conexión. El equipo cliente que se conecta al servidor comprobará si confía en el certificado y mostrará una advertencia si no lo es. Por lo general, para asegurarse de que el cliente que se conecta confía en el certificado, los certificados RDP se emiten desde la PKI del inquilino. Puede encontrar más información sobre el [uso de certificados en servicios de escritorio remoto](https://technet.microsoft.com/library/dn781533.aspx) en TechNet.

 Como ayuda para decidir si necesita obtener un certificado RDP personalizado, tenga en cuenta lo siguiente:

- Si solo está probando máquinas virtuales blindadas en un entorno de laboratorio, **no** necesita un certificado RDP personalizado.
- Si la máquina virtual está configurada para unirse a un dominio de Active Directory, la entidad de certificación de la organización normalmente emitirá un certificado de equipo automáticamente y se usará para identificar el equipo durante las conexiones RDP. **No** necesita un certificado RDP personalizado.
- Si la máquina virtual no está unida a un dominio pero desea una manera de comprobar que se está conectando al equipo correcto al usar Escritorio remoto, **debería considerar la posibilidad** de usar certificados RDP personalizados.

> [!TIP]
> Al seleccionar un certificado RDP para incluirlo en el archivo de datos de blindaje, asegúrese de usar un certificado comodín. Un archivo de datos de blindaje se puede usar para crear un número ilimitado de máquinas virtuales. Dado que cada máquina virtual compartirá el mismo certificado, un certificado comodín garantiza que el certificado será válido independientemente del nombre de host de la máquina virtual.

## <a name="create-an-answer-file"></a>Crear un archivo de respuesta

Puesto que el disco de plantilla firmado en VMM está generalizado, los inquilinos deben proporcionar un archivo de respuesta para especializar sus máquinas virtuales blindadas durante el proceso de aprovisionamiento. El archivo de respuesta (a menudo denominado archivo de instalación desatendida) puede configurar la máquina virtual para su rol previsto; es decir, puede instalar características de Windows, registrar el certificado RDP creado en el paso anterior y realizar otras acciones personalizadas. También proporcionará la información necesaria para el programa de instalación de Windows, incluida la contraseña y la clave de producto del administrador predeterminado.

Para obtener información sobre cómo obtener y usar la función **New-ShieldingDataAnswerFile** para generar un archivo de respuesta (archivo Unattend. xml) para crear máquinas virtuales blindadas, consulte [generación de un archivo de respuesta mediante la función New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md). Con la función, puede generar más fácilmente un archivo de respuesta que refleje opciones como las siguientes:

- ¿La máquina virtual debe estar unida a un dominio al final del proceso de inicialización?
- ¿Va a usar una licencia por volumen o una clave de producto específica por máquina virtual?
- ¿Usa DHCP o una dirección IP estática?
- ¿Usará un certificado de Protocolo de escritorio remoto personalizado (RDP) que se usará para comprobar que la máquina virtual pertenece a su organización?
- ¿Desea ejecutar un script al final de la inicialización?

Los archivos de respuesta usados en los archivos de datos de blindaje se usarán en todas las máquinas virtuales creadas con el archivo de datos de blindaje. Por lo tanto, debe asegurarse de no codificar de forma rígida cualquier información específica de la máquina virtual en el archivo de respuesta. VMM admite algunas cadenas de sustitución (vea la tabla siguiente) en el archivo de instalación desatendida para controlar los valores de especialización que pueden cambiar de una máquina virtual a una máquina virtual. No es necesario utilizarlos. sin embargo, si están presentes, VMM se beneficiará de ellos.

Al crear un archivo Unattend. XML para máquinas virtuales blindadas, tenga en cuenta las siguientes restricciones:

- Si usa VMM para administrar el centro de recursos, el archivo de instalación desatendida debe hacer que la máquina virtual se apague después de configurarla. Esto permite que VMM sepa cuándo debe informar al inquilino de que la máquina virtual ha finalizado el aprovisionamiento y está lista para su uso. VMM volverá a encender automáticamente la máquina virtual una vez que detecta que se ha desactivado durante el aprovisionamiento.

- Asegúrese de habilitar RDP y la regla de Firewall correspondiente para poder acceder a la máquina virtual una vez configurada. No puede usar la consola VMM para acceder a máquinas virtuales blindadas, por lo que necesitará RDP para conectarse a la máquina virtual. Si prefiere administrar sus sistemas con la comunicación remota de Windows PowerShell, asegúrese de que WinRM está habilitado también.

- Las únicas cadenas de sustitución que se admiten en archivos de instalación desatendida de máquinas virtuales blindadas son las siguientes:

    | Elemento reemplazable | Cadena de sustitución |
    |-----------|-----------|
    | ComputerName        | @ComputerName@      |
    | Zona horaria            | @TimeZone@          |
    | ProductKey          | @ProductKey@        |
    | IPAddr4-1           | @IP4Addr-1@         |
    | IPAddr6-1           | @IP6Addr-1@         |
    | MACAddr-1           | @MACAddr-1@         |
    | Prefijo: 1-1          | @Prefix-1-1@        |
    | NextHop: 1-1         | @NextHop-1-1@       |
    | Prefijo: 1-2          | @Prefix-1-2@        |
    | NextHop: 1-2         | @NextHop-1-2@       |

    Si tiene más de una NIC, puede agregar varias cadenas de sustitución para la configuración de IP incrementando el primer dígito. Por ejemplo, para establecer la dirección IPv4, la subred y la puerta de enlace para 2 NIC, usaría las siguientes cadenas de sustitución:

    | Cadena de sustitución | Sustitución de ejemplo |
    |---------------------|----------------------|
    | @IP4Addr-1@         | 192.168.1.10/24      |
    | @MACAddr-1@         | Ethernet             |
    | @Prefix-1-1@        | 24                   |
    | @NextHop-1-1@       | 192.168.1.254        |
    | @IP4Addr-2@         | 10.0.20.30/24        |
    | @MACAddr-2@         | Ethernet 2           |
    | @Prefix-2-1@        | 24                   |
    | @NextHop-2-1@       | 10.0.20.1            |

Al usar cadenas de sustitución, es importante asegurarse de que las cadenas se rellenarán durante el proceso de aprovisionamiento de la máquina virtual. Si no se proporciona una cadena como @ProductKey@ en el momento de la implementación, lo que deja en blanco el nodo &lt;ProductKey&gt; en el archivo de instalación desatendida, se producirá un error en el proceso de especialización y no podrá conectarse a la máquina virtual.

Además, tenga en cuenta que las cadenas de sustitución relacionadas con redes hacia el final de la tabla solo se usan si está aprovechando grupos de direcciones IP estáticas de VMM. El proveedor de servicios de hosting debe ser capaz de indicarle si se requieren estas cadenas de sustitución. Para obtener más información acerca de las direcciones IP estáticas en las plantillas de VMM, consulte lo siguiente en la documentación de VMM:

- [Directrices para grupos de direcciones IP](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools)
- [Configuración de grupos de direcciones IP estáticas en el tejido de VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

Por último, es importante tener en cuenta que el proceso de implementación de la máquina virtual blindada solo cifrará la unidad del sistema operativo. Si implementa una máquina virtual blindada con una o varias unidades de datos, se recomienda encarecidamente que agregue un comando de instalación desatendida o una configuración de directiva de grupo en el dominio del inquilino para cifrar automáticamente las unidades de datos.

## <a name="get-the-volume-signature-catalog-file"></a>Obtener el archivo de catálogo de firmas de volumen

Los archivos de datos de blindaje también contienen información sobre los discos de plantilla en los que confía un inquilino. Los inquilinos adquieren las firmas de disco de los discos de plantilla de confianza en forma de un archivo de catálogo de firmas de volumen (VSC). Estas firmas se validan después cuando se implementa una nueva máquina virtual. Si ninguna de las firmas del archivo de datos de blindaje coincide con el disco de plantilla que intenta implementarse con la máquina virtual (es decir, se modificó o intercambió con otro disco potencialmente malintencionado), se producirá un error en el proceso de aprovisionamiento.

> [!IMPORTANT]
> Aunque el VSC garantiza que un disco no se ha manipulado, sigue siendo importante que el inquilino confíe en el disco en primer lugar. Si es el inquilino y el disco de plantilla lo proporciona su anfitrión, implemente una máquina virtual de prueba con ese disco de plantilla y ejecute sus propias herramientas (antivirus, detectores de vulnerabilidades, etc.) para validar el disco, de hecho, en un estado en el que confíe.

Hay dos formas de adquirir el VSC de un disco de plantilla:

1. El anfitrión (o inquilino, si el inquilino tiene acceso a VMM) usa los cmdlets de PowerShell de VMM para guardar el VSC y lo asigna al inquilino. Esto puede realizarse en cualquier equipo que tenga la consola VMM instalada y configurada para administrar el entorno VMM del tejido de hospedaje. Los cmdlets de PowerShell para guardar el VSC son los siguientes:

    ```powershell
    $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"

    $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk

    $vsc.WriteToFile(".\templateDisk.vsc")
    ```

2. El inquilino tiene acceso al archivo de disco de plantilla. Este puede ser el caso si el inquilino crea un disco de plantilla para cargarlo en un proveedor de servicios de hospedaje o si el inquilino puede descargar el disco de plantilla del proveedor de servicios de hosting. En este caso, sin VMM en la imagen, el inquilino ejecutaría el siguiente cmdlet (instalado con la característica herramientas de máquinas virtuales blindadas, parte de Herramientas de administración remota del servidor):

    ```powershell
    Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc
    ```

## <a name="select-trusted-fabrics"></a>Seleccionar tejidos de confianza

El último componente del archivo de datos de blindaje se relaciona con el propietario y los tutores de una máquina virtual. Los tutores se usan para designar tanto el propietario de una máquina virtual blindada como los tejidos protegidos en los que está autorizado a ejecutarse.

Para autorizar a un tejido de hospedaje a ejecutar una máquina virtual blindada, debe obtener los metadatos de protección del servicio de protección de host del proveedor de servicios de hosting. A menudo, el proveedor de servicios de Hosting le proporcionará estos metadatos a través de sus herramientas de administración. En un escenario empresarial, puede tener acceso directo para obtener los metadatos personalmente.

Usted o su proveedor de servicios de hosting pueden obtener los metadatos de protección del HGS mediante una de las siguientes acciones:

- Obtenga los metadatos de protección directamente desde HGS mediante la ejecución del siguiente comando de Windows PowerShell o vaya al sitio web y guarde el archivo XML que se muestra:

    ```powershell
    Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml
    ```

- Obtenga los metadatos de protección de VMM con los cmdlets de PowerShell de VMM:

    ```powershell
    $relecloudmetadata = Get-SCGuardianConfiguration
    $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8
    ```

Obtenga los archivos de metadatos de protección para cada tejido protegido en el que desee autorizar la ejecución de las máquinas virtuales blindadas antes de continuar.

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>Creación de un archivo de datos de blindaje y adición de protecciones mediante el Asistente para archivos de datos de blindaje

Ejecute el Asistente para archivos de datos de blindaje para crear un archivo de datos de blindaje (PDK). Aquí, agregará el certificado RDP, el archivo de instalación desatendida, los catálogos de firmas de volumen, el guardián del propietario y los metadatos del guardián descargados obtenidos en el paso anterior.

1. Instale **herramientas de administración remota del servidor herramientas de administración de características de &gt; &gt; herramientas de máquinas virtuales blindadas** en el equipo con administrador del servidor o el siguiente comando de Windows PowerShell:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

2. Abra el Asistente para archivos de datos de blindaje en la sección herramientas de administrador del menú Inicio o ejecute el siguiente archivo ejecutable **C:\\Windows\\System32\\ShieldingDataFileWizard. exe**.

3. En la primera página, use el segundo cuadro de selección de archivo para elegir una ubicación y un nombre de archivo para el archivo de datos de blindaje. Normalmente, debería asignar un nombre a un archivo de datos de blindaje después de la entidad que posee las máquinas virtuales creadas con los datos de blindaje (por ejemplo, HR, IT, Finance) y el rol de carga de trabajo que se está ejecutando (por ejemplo, servidor de archivos, servidor web o cualquier otro elemento configurado por el archivo de instalación desatendida). Deje el botón de radio establecido en **blindaje de los datos para las plantillas blindadas**.

    > [!NOTE]
    > En el Asistente para archivos de datos de blindaje, observará las dos opciones siguientes:
    >- **Protección de datos para plantillas blindadas**
    >- **Protección de datos de máquinas virtuales existentes y plantillas no blindadas**<br>
    > La primera opción se usa al crear nuevas máquinas virtuales blindadas a partir de plantillas blindadas. La segunda opción permite crear datos de blindaje que solo se pueden usar al convertir máquinas virtuales existentes o crear máquinas virtuales blindadas a partir de plantillas no blindadas.

    ![Asistente para archivos de datos de blindaje, selección de archivos](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

    Además, debe elegir si las máquinas virtuales creadas con este archivo de datos de blindaje se protegerán o se configurarán realmente en modo de "cifrado compatible". Para obtener más información sobre estas dos opciones, consulte [¿Cuáles son los tipos de máquinas virtuales que puede ejecutar un tejido protegido?](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run)

    > [!IMPORTANT]
    > Preste especial atención al siguiente paso, ya que define el propietario de las máquinas virtuales blindadas y en qué tejidos se autorizará la ejecución de las máquinas virtuales blindadas.<br>La posesión de la **protección de propietario** es necesaria para cambiar más adelante una máquina virtual blindada existente de **blindada** a **cifrado compatible** o viceversa.

4. El objetivo de este paso es dos veces:

    - Creación o selección de una protección de propietario que le represente como propietario de la máquina virtual

    - Importe el tutor que descargó desde el servicio de protección de host (o el suyo propio) del proveedor de hospedaje en el paso anterior

    Para designar un guardián de propietario existente, seleccione el guardián adecuado en el menú desplegable. En esta lista solo se mostrarán las protecciones instaladas en el equipo local con las claves privadas intactas. También puede crear su propia protección de propietario seleccionando **administrar protecciones locales** en la esquina inferior derecha y haciendo clic en **crear** y completando el asistente.

    A continuación, se importan los metadatos de Guardian descargados anteriormente de nuevo mediante la página **propietario y tutores** . Seleccione **administrar protecciones locales** en la esquina inferior derecha. Use la característica de **importación** para importar el archivo de metadatos de Guardian. Haga clic en **Aceptar** una vez que haya importado o agregado todos los tutores necesarios. Como práctica recomendada, los nombres de los tutores después del proveedor de servicios de hospedaje o del centro de recursos de empresa representan. Por último, seleccione todos los tutores que representan los centros de recursos en los que la máquina virtual blindada está autorizada a ejecutarse. No es necesario volver a seleccionar la protección de propietario. Haga clic en **siguiente** cuando termine.

    ![Asistente para archivos de datos de blindaje, propietario y tutores](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5. En la página calificadores de ID. de volumen, haga clic en **Agregar** para autorizar un disco de plantilla firmado en el archivo de datos de blindaje. Al seleccionar un VSC en el cuadro de diálogo, se mostrará información sobre el nombre, la versión y el certificado del disco que se usó para firmarlo. Repita este proceso para cada disco de plantilla que desee autorizar.

6. En la página **valores de especialización** , haga clic en **examinar** para seleccionar el archivo Unattend. XML que se usará para especializar las máquinas virtuales.

    Use el botón **Agregar** de la parte inferior para agregar cualquier archivo adicional al PDK que sea necesario durante el proceso de especialización. Por ejemplo, si el archivo de instalación desatendida está instalando un certificado RDP en la máquina virtual (como se describe en [generación de un archivo de respuesta mediante la función New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md)), debe agregar el archivo PFX del certificado RDP y el script RDPCertificateConfig. PS1 aquí. Tenga en cuenta que los archivos que especifique aquí se copiarán automáticamente en C:\\Temp\\ en la máquina virtual que se crea. El archivo de instalación desatendida debería esperar que los archivos estén en esa carpeta al hacer referencia a ellos mediante la ruta de acceso.

7. Revise las selecciones en la página siguiente y, a continuación, haga clic en **generar**.

8. Cierre el Asistente una vez completado.

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>Creación de un archivo de datos de blindaje y adición de guardianes mediante PowerShell

Como alternativa al Asistente para archivos de datos de blindaje, puede ejecutar [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps) para crear un archivo de datos de blindaje.

Todos los archivos de datos de blindaje deben configurarse con los certificados de propietario y guardián correctos para autorizar que las máquinas virtuales blindadas se ejecuten en un tejido protegido.
Puede comprobar si tiene algún tutor instalado localmente mediante la ejecución [de Get-HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps). Los guardianes de propietario tienen claves privadas, mientras que los guardianes de su centro de recursos normalmente no lo hacen.

Si necesita crear un guardián de propietario, ejecute el siguiente comando:

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

Este comando crea un par de certificados de firma y cifrado en el almacén de certificados del equipo local, en la carpeta "certificados locales de máquinas virtuales blindadas".
Necesitará los certificados de propietario y sus claves privadas correspondientes para desproteger una máquina virtual, por lo que debe asegurarse de que se realice una copia de seguridad de estos certificados y estén protegidos del robo.
Un atacante con acceso a los certificados de propietario puede usarlos para iniciar la máquina virtual blindada o cambiar la configuración de seguridad.

Si necesita importar información de protección desde un tejido protegido en el que desea ejecutar la máquina virtual (el centro de datos principal, los centros de datos de copia de seguridad, etc.), ejecute el siguiente comando para cada [archivo de metadatos recuperado de los tejidos protegidos](#select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> Si ha usado certificados autofirmados o los certificados registrados con HGS han expirado, puede que tenga que usar las marcas `-AllowUntrustedRoot` y/o `-AllowExpired` con el comando IMPORT-HgsGuardian para omitir las comprobaciones de seguridad.

También necesitará [obtener un catálogo de firmas de volumen](#get-the-volume-signature-catalog-file) para cada disco de plantilla que desee usar con este archivo de datos de blindaje y un [archivo de respuesta de datos de blindaje](#create-an-answer-file) para permitir que el sistema operativo complete automáticamente sus tareas de especialización.
Por último, decida si desea que la máquina virtual esté completamente blindada o simplemente habilitada para vTPM.
Use `-Policy Shielded` para una VM totalmente blindada o `-Policy EncryptionSupported` para una máquina virtual habilitada para vTPM que permita conexiones de consola básicas y PowerShell Direct.

Cuando todo esté listo, ejecute el siguiente comando para crear el archivo de datos de blindaje:

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

> [!TIP]
> Si usa un certificado RDP personalizado, claves SSH u otros archivos que deben incluirse con el archivo de datos de blindaje, use el parámetro `-OtherFile` para incluirlos. Puede proporcionar una lista separada por comas de rutas de acceso de archivo, como `-OtherFile "C:\source\myRDPCert.pfx", "C:\source\RDPCertificateConfig.ps1"`

En el comando anterior, el tutor denominado "Owner" (obtenido a través de Get-HgsGuardian) podrá cambiar la configuración de seguridad de la máquina virtual en el futuro, mientras que "EAST-US Datacenter" puede ejecutar la máquina virtual pero no cambiar su configuración.
Si tiene más de un guardián, separe los nombres de los tutores con comas como `'EAST-US Datacenter', 'EMEA Datacenter'`.
El calificador de ID. de volumen especifica si confía solo en la versión exacta (igual) del disco de plantilla o en versiones futuras (GreaterThanOrEquals).
El nombre del disco y el certificado de firma deben coincidir exactamente para que la comparación de versiones se considere en el momento de la implementación.
Puede confiar en más de un disco de plantilla proporcionando una lista separada por comas de calificadores de ID. de volumen para el parámetro `-VolumeIDQualifier`.
Por último, si tiene otros archivos que deben acompañar el archivo de respuesta con la máquina virtual, use el parámetro `-OtherFile` y proporcione una lista separada por comas de rutas de acceso de archivo.

Consulte la documentación del cmdlet para [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps) y [New-VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps) para obtener información sobre otras formas de configurar el archivo de datos de blindaje.

## <a name="see-also"></a>Consulta también

- [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
