---
title: Máquinas virtuales blindadas para inquilinos - creación de los datos de blindaje para definir una máquina virtual blindada
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: d1d269ecdbfd4803c51da4817b62caf01d2091ae
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469622"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>Máquinas virtuales blindadas para inquilinos - creación de los datos de blindaje para definir una máquina virtual blindada

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Un archivo de datos de blindaje (también denominado archivo de datos de aprovisionamiento o archivo PDK) es un archivo cifrado que crea un inquilino o el propietario de la máquina virtual para proteger la información de configuración de máquina virtual importante, como la contraseña del administrador, RDP y otros certificados relacionados con la identidad o las credenciales de unión a un dominio, entre otra. En este tema se proporciona información sobre cómo crear un archivo de datos de blindaje. Antes de poder crear el archivo, debe obtener un disco de plantilla desde su proveedor de servicios de hospedaje, o crear un disco de plantilla, como se describe en [máquinas virtuales blindadas para inquilinos: creación de un disco de plantilla (opcional)](guarded-fabric-tenant-creates-template-disk.md).

Para obtener una lista y un diagrama del contenido de un archivo de datos de blindaje, consulte [lo es con los datos de blindaje y por qué es necesario?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

> [!IMPORTANT]
> Los pasos descritos en esta sección deben realizarse en una máquina del inquilino que ejecuta Windows Server 2016. Esa máquina no debe formar parte de un tejido protegido (es decir, debe no ser configurado para utilizar un clúster HGS).

Para preparar la creación de un archivo de datos de blindaje, realice los pasos siguientes:

- [Obtener un certificado para la conexión a Escritorio remoto](#obtain-a-certificate-for-remote-desktop-connection)
- [Crear un archivo de respuesta](#create-an-answer-file)
- [Obtener el archivo de catálogo de firmas de volumen](#get-the-volume-signature-catalog-file)
- [Seleccione los tejidos de confianza](#select-trusted-fabrics)

A continuación, puede crear el archivo de datos de blindaje:

- [Cree un archivo de datos de blindaje y agregue tutores](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)


## <a name="obtain-a-certificate-for-remote-desktop-connection"></a>Obtener un certificado para la conexión a Escritorio remoto

Dado que los inquilinos solo pueden conectarse a sus máquinas virtuales blindadas con conexión a Escritorio remoto u otras herramientas de administración remota, es importante asegurarse de que los inquilinos pueden comprobar que se está conectando al extremo derecho (es decir, no hay un tipo "man in the middle" interceptar la conexión).

Es una forma de verificar que se está conectando al servidor adecuado instalar y configurar un certificado para los servicios de escritorio remoto presentar cuando se inicia una conexión. El equipo cliente conectarse al servidor comprobará si confía en el certificado y mostrará una advertencia si no es así. Por lo general, para asegurarse de que confía en el cliente que se conecta el certificado, se emiten certificados RDP de PKI del inquilino. Para obtener más información acerca de [mediante certificados en los servicios de escritorio remoto](https://technet.microsoft.com/library/dn781533.aspx) puede encontrarse en TechNet.

> [!NOTE]
> Al seleccionar un certificado RDP para incluir en el archivo de datos de blindaje, asegúrese de usar un certificado comodín. Un archivo de datos de blindaje se puede usar para crear un número ilimitado de máquinas virtuales. Puesto que cada máquina virtual compartirán el mismo certificado, un certificado comodín garantiza que el certificado sea válido, independientemente del nombre de host de la máquina virtual.

Si va a evaluar las máquinas virtuales blindadas y no están aún esté listo para solicitar un certificado de la entidad emisora de certificados, puede crear un certificado autofirmado en la máquina del inquilino, ejecute el comando de Windows PowerShell siguiente (donde *contoso.com*  es el dominio del inquilino):

``` powershell
$rdpCertificate = New-SelfSignedCertificate -DnsName '\*.contoso.com'
$password = ConvertTo-SecureString -AsPlainText 'Password1' -Force
Export-PfxCertificate -Cert $RdpCertificate -FilePath .\rdpCert.pfx -Password $password
```

## <a name="create-an-answer-file"></a>Crear un archivo de respuesta

Puesto que se ha generalizado el disco plantilla firmada en VMM, los inquilinos deben proporcionar un archivo de respuesta para sus máquinas virtuales blindadas se especializan durante el proceso de aprovisionamiento. El archivo de respuesta (a menudo denominado archivo de instalación desatendida) puede configurar la máquina virtual para su rol deseado: es decir, puede instalar las características de Windows, además de registrar el certificado RDP creado en el paso anterior y realizar otras acciones personalizadas. También proporcionará la información necesaria para la instalación de Windows, incluida la clave de producto y la contraseña del Administrador de forma predeterminada.

Para obtener información acerca de cómo obtener y utilizar el **New-ShieldingDataAnswerFile** función para generar un archivo de respuesta (archivo Unattend.xml) para crear máquinas virtuales blindadas, consulte [generar un archivo de respuesta mediante el Función nueva-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md). La función puede generar más fácilmente un archivo de respuesta que refleja las opciones como las siguientes:

- ¿La máquina virtual está pensada para unirse al dominio en el final del proceso de inicialización?
- ¿Va a usar una licencia por volumen o la clave de producto específica por máquina virtual?
- ¿Usa DHCP o dirección IP estática?
- ¿Usará un certificado de protocolo de escritorio remoto (RDP) que se usará para demostrar que la máquina virtual pertenece a su organización?
- ¿Desea ejecutar un script al final de la inicialización?
- ¿Se usa un servidor de Desired State Configuration (DSC) para continuar con la configuración?

Archivos de respuesta usados en los archivos de datos de blindaje se usará en todas las máquinas virtuales creadas con ese archivo de datos de blindaje. Por lo tanto, debe asegurarse de que no codificar de forma rígida cualquier información específica de la máquina virtual en el archivo de respuesta. VMM admite algunas cadenas de sustitución (consulte la tabla siguiente) en el archivo de instalación desatendida para controlar los valores de especialización que pueden cambiar de máquina virtual a máquina virtual. No es necesario para poder utilizarlos; Sin embargo, si están presentes VMM sacarán provecho de ellas.

Al crear un archivo unattend.xml para máquinas virtuales blindadas, tenga en cuenta las siguientes restricciones:

-   Debe dar como resultado el archivo de instalación desatendida en la máquina virtual que se ha desactivado después de que se ha configurado. Esto es para permitir que VMM saber cuándo debe notificar al inquilino que la máquina virtual de terminado el aprovisionamiento y está lista para su uso. VMM automáticamente power la máquina virtual en cuanto detecta que se ha desactivado durante el aprovisionamiento.

-   Se recomienda que configure un certificado RDP para asegurarse de que se conecta a la máquina virtual de la derecha y no otro equipo configurado para un ataque man-in-the-middle.

-   No olvide habilitar RDP y la correspondiente regla de firewall para que pueda acceder a la máquina virtual después de que se ha configurado. No se puede usar la consola VMM para máquinas virtuales blindada de acceso, por lo que necesitará RDP para conectarse a la máquina virtual. Si prefiere administrar sus sistemas con comunicación remota de Windows PowerShell, asegúrese de que WinRM está habilitado, demasiado.

-   Las cadenas de sustitución solo admitidas archivos de instalación desatendida de máquina virtual blindados son los siguientes:

| Elemento reemplazable | Cadena de sustitución |
|-----------|-----------|
| ComputerName        | @ComputerName@      |
| TimeZone            | @TimeZone@          |
| ProductKey          | @ProductKey@        |
| IPAddr4-1           | @IP4Addr-1@         |
| IPAddr6-1           | @IP6Addr-1@         |
| MACAddr-1           | @MACAddr-1@         |
| Prefijo de-1-1          | @Prefix-1-1@        |
| NextHop-1-1         | @NextHop-1-1@       |
| Prefijo de-1-2          | @Prefix-1-2@        |
| NextHop-1-2         | @NextHop-1-2@       |

Al usar las cadenas de sustitución, es importante asegurarse de que las cadenas se rellenará durante el proceso de aprovisionamiento de VM. Si una cadena como @ProductKey@ no se proporciona en tiempo de implementación, dejando el &lt;ProductKey&gt; nodo en el archivo de instalación desatendida en blanco, se producirá un error en el proceso de especialización y no será posible conectarse a la máquina virtual.

Además, tenga en cuenta que solo se usan las cadenas de sustitución relacionados con las redes hacia el final de la tabla si saca provecho de grupos de direcciones IP estáticas de VMM. El proveedor de servicios de hospedaje debe indicarle si estas cadenas de sustitución son necesarias. Para obtener más información acerca de las direcciones IP estáticas en las plantillas de VMM, consulte lo siguiente en la documentación de VMM:

- [Directrices para grupos de direcciones IP](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools) 
- [Configurar grupos de direcciones IP estáticas en el tejido de VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

Por último, es importante tener en cuenta que el proceso de implementación de máquinas virtuales blindado únicamente cifrará la unidad del sistema operativo. Si implementa una máquina virtual blindada con uno o más unidades de datos, se recomienda agregar un comando de instalación desatendida o la configuración de directiva de grupo en el dominio del inquilino para cifrar automáticamente las unidades de datos.

## <a name="get-the-volume-signature-catalog-file"></a>Obtener el archivo de catálogo de firmas de volumen

Archivos de datos de blindaje también contienen información sobre los discos de plantilla que se confía en un inquilino. Los inquilinos adquieren las firmas de disco de los discos de plantilla de confianza en forma de un archivo de catálogo (VSC) de firma de volumen. Estas firmas, a continuación, se validan cuando se implementa una nueva máquina virtual. Si ninguna de las firmas en el archivo de datos de blindaje coincide con el disco de plantilla intentando implementarse con la máquina virtual (es decir, que se modificó o intercambiar con un disco diferente y posiblemente malintencionado), el proceso de aprovisionamiento se producirá un error.

> [!IMPORTANT]
> Mientras la VSC garantiza que un disco no se ha alterado, es importante para el inquilino debe confiar en primer lugar en el disco. Si se encuentra el inquilino y el disco de plantilla se proporciona mediante el servicio de hospedaje, implementar una máquina virtual con ese disco de plantilla de prueba y ejecutan sus propias herramientas (antivirus, detectores de vulnerabilidades etc.) para validar el disco es, de hecho, en un estado de confianza.

Hay dos maneras de adquirir la VSC de un disco de plantilla:

-  El proveedor de hospedaje (o inquilino, si el inquilino tiene acceso a VMM) usa los cmdlets de PowerShell de VMM para guardar la VSC y le da al inquilino. Esto puede realizarse en cualquier equipo con la consola VMM instalado y configurado para administrar el entorno de VMM del tejido de hospedaje. Los cmdlets de PowerShell para guardar la VSC son:

        $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"
    
        $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk
    
        $vsc.WriteToFile(".\templateDisk.vsc")

-  El inquilino tiene acceso al archivo de disco de plantilla. Esto puede ocurrir si el inquilino crea un disco de plantilla que va a cargar en un proveedor de hospedaje de servicio o si el inquilino puede descargar el disco de plantilla del proveedor de hospedaje. En este caso, sin VMM en la imagen, el inquilino de ejecutar el siguiente cmdlet (instalado con la característica herramientas de máquinas virtuales blindadas, parte de herramientas de administración remota del servidor):

        Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc

## <a name="select-trusted-fabrics"></a>Seleccione los tejidos de confianza

El último componente en el archivo de datos de blindaje se relaciona con el propietario y obtener una copia de una máquina virtual. Protecciones se utilizan para designar tanto el propietario de una máquina virtual blindada y los tejidos protegidos en el que está autorizado para ejecutarse.

Para autorizar a un tejido de hospedaje para ejecutar una máquina virtual blindada, debe obtener los metadatos del guardián desde el servicio guardián de Host del proveedor de hospedaje servicio. A menudo, el proveedor de servicios de hospedaje le proporcionará estos metadatos a través de sus herramientas de administración. En un escenario empresarial, puede tener acceso directo para obtener los metadatos usted mismo.

Usted o su proveedor de servicio de hospedaje puede obtener los metadatos del guardián de HGS realizando una de las siguientes acciones:

-  Obtener los metadatos del guardián directamente desde HGS, ejecutando el siguiente comando de Windows PowerShell, o diríjase al sitio Web y guardar el archivo XML que se muestra:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

-  Obtener los metadatos del guardián desde VMM mediante los cmdlets de PowerShell de VMM:

        $relecloudmetadata = Get-SCGuardianConfiguration

        $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8

Obtener archivos de metadatos de la protección para cada tejido protegido que desea autorizar a las máquinas virtuales blindadas para ejecutar en antes de continuar.

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>Cree un archivo de datos de blindaje y agregue a tutores mediante el Asistente para archivos de datos de blindaje

Ejecute el Asistente de archivo de datos de blindaje para crear un archivo de blindaje (PDK) de datos. En este caso, deberá agregar el certificado RDP, archivo, los catálogos de firma de volumen, protección de propietario de instalación desatendida y obtienen los metadatos del guardián descargado en el paso anterior.

1.  Instalar **herramientas de administración remota del servidor &gt; herramientas de administración de la característica &gt; herramientas VM blindadas** en el equipo con el administrador del servidor o el siguiente comando de Windows PowerShell:

        Install-WindowsFeature RSAT-Shielded-VM-Tools

2.  Abrir el Asistente de archivo de datos de blindaje de la sección Herramientas de administrador en el menú Inicio o puede ejecutar el siguiente archivo ejecutable **C:\\Windows\\System32\\ShieldingDataFileWizard.exe**.

3.  En la primera página, utilice el segundo cuadro de selección de archivo para elegir una ubicación y un nombre para el archivo de datos de blindaje. Normalmente, podría denominar un archivo de datos de blindaje de una vez creada la entidad que posee todas las máquinas virtuales con esos datos de blindaje (por ejemplo, recursos humanos, TI, Finanzas) y el rol de carga de trabajo se está ejecutando (por ejemplo, servidor de archivos, servidor web o cualquier otra cosa configurado por el archivo de instalación desatendida). Deje el botón de radio establecido en **blindaje datos para las plantillas blindadas**.

    > [!NOTE]
    > En el Asistente de archivo de datos de blindaje se muestran las dos opciones siguientes:
    >- **Datos de blindaje para plantillas blindadas**
    >- **Los datos de blindaje para máquinas virtuales existentes y las plantillas no blindada**<br>
    > La primera opción se usa al crear nuevas máquinas virtuales blindadas desde plantillas blindadas. La segunda opción permite crear datos de blindaje que solo se pueden usar al convertir archivos existentes de las máquinas virtuales o crear las máquinas virtuales blindadas desde las plantillas no blindada.

    ![Asistente para archivos de datos de blindaje, selección de archivos](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

       Además, debe elegir si las máquinas virtuales creadas con este archivo de datos de blindaje se realmente blindada o configurado en "modo"cifrado admitido. Para obtener más información acerca de estas dos opciones, consulte [cuáles son los tipos de máquinas virtuales que puede ejecutar un tejido protegido?](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run).

    > [!IMPORTANT]
    > Prestar atención especial con el paso siguiente, ya que define el propietario de las máquinas virtuales blindadas y que los tejidos tendrá autorización para ejecutarse en las máquinas virtuales blindadas.<br>Posesión de **guardian propietario** es necesario para cambiar más adelante una máquina virtual blindada existente desde **blindadas** a **admite el cifrado** o viceversa.
    
4.  El objetivo de este paso es doble:

    - Cree o seleccione a un guardián propietario que se representa como el propietario de la máquina virtual

    - Importar la protección que ha descargado desde el proveedor de hospedaje (o su propio) servicio guardián de Host en el paso anterior

    Para designar a un guardián propietario existente, seleccione a la protección adecuada en el menú desplegable. Solo los tutores instalados en el equipo local con las claves privadas intactas aparecerán en esta lista. También puede crear su propia protección propietario seleccionando **administrar Local tutores** en la parte inferior esquina derecha y haga clic en **crear** y finalización del asistente.

    A continuación, se importación los metadatos del guardián descargado anteriormente utilizando de nuevo el **propietario y tutores** página. Seleccione **administrar Local tutores** desde la esquina inferior derecha. Use la **importar** característica para importar el archivo de metadatos de protección. Haga clic en **Aceptar** una vez que haya importado o agregado todas las protecciones necesarias. Como práctica recomendada, asigne el nombre a tutores después del hospedaje servicio proveedor o enterprise centro de datos que representan. Por último, seleccione a todas las protecciones que representan los centros de datos en el que la máquina virtual blindada está autorizada a ejecutarse. No es necesario volver a seleccionar a la protección de propietario. Haga clic en **siguiente** una vez finalizado.

    ![Asistente para archivos de datos, propietario y protecciones de blindaje](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5.  En la página de calificadores de Id. de volumen, haga clic en **agregar** para autorizar a un disco de plantilla firmada en su archivo de datos de blindaje. Cuando se selecciona un VSC en el cuadro de diálogo, se mostrará información sobre el nombre del disco, versión y el certificado que se usó para firmarlo. Repita este proceso para cada disco de plantilla que desea autorizar.

6.  En el **especialización valores** página, haga clic en **examinar** para seleccionar el archivo unattend.xml que se usará para especializar las máquinas virtuales.

    Use la **agregar** situado en la parte inferior para agregar los archivos adicionales para el PDK que son necesarios durante el proceso de especialización. Por ejemplo, si el archivo de instalación desatendida es instalar un certificado RDP en la máquina virtual (como se describe en [generar un archivo de respuesta mediante la función New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md)), debe agregar el archivo RDPCert.pfx hace referenciado en la instalación desatendida de archivo aquí. Tenga en cuenta que se configurará automáticamente y los archivos que especifique aquí se copian en C:\\temp\\ en la máquina virtual que se crea. El archivo de instalación desatendida debe saber que los archivos estén en esa carpeta al hacer referencia a ellos mediante la ruta de acceso.

7.  Revise las selecciones en la página siguiente y, a continuación, haga clic en **generar**.

8.  Cuando se ha completado, cierre al asistente.

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>Cree un archivo de datos de blindaje y agregue a tutores mediante PowerShell

Como alternativa para el Asistente para archivos de datos de blindaje, puede ejecutar [New ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps) para crear un archivo de datos de blindaje.

Todos los archivos de datos de blindaje deben configurarse con los certificados de protección y propietario correcto para autorizar a las máquinas virtuales blindadas para ejecutarse en un tejido protegido.
Puede comprobar si tiene cualquier tutores instalados localmente mediante la ejecución [Get HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps). Protecciones de propietario tienen las claves privadas mientras tutores de su centro de datos, normalmente no.

Si necesita crear a una protección de propietario, ejecute el siguiente comando:

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

Este comando crea un par de certificados de firma y cifrado en el almacén de certificados del equipo local en la carpeta "Certificates Local VM blindadas".
Necesitará los certificados de propietario y sus claves privadas correspondientes para unshield una máquina virtual, así que asegúrese de estos certificados son una copia de seguridad y protegidos contra robo.
Un atacante con acceso a los certificados de propietario puede usarlos para iniciar la máquina virtual blindada o cambiar su configuración de seguridad.

Si tiene que importar la información de protección de un tejido protegido en el que desea ejecutar la máquina virtual (centro de datos principal, los centros de datos de copia de seguridad, etc.), ejecute el siguiente comando para cada [recuperar el archivo de metadatos de los tejidos protegidos ](#select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> Si utiliza certificados autofirmados o certificados registrados con HGS han expirado, deberá usar el `-AllowUntrustedRoot` o `-AllowExpired` marcas con el comando Import-HgsGuardian para omitir las comprobaciones de seguridad.

También deberá [obtener un catálogo de firmas de volumen](#get-the-volume-signature-catalog-file) para cada disco de plantilla que desea usar con este archivo de datos de blindaje y una [archivo de respuesta de datos de blindaje](#create-an-answer-file) para permitir que el sistema operativo para completar su especialización de tareas automáticamente.
Por último, decida si desea que la máquina virtual sea totalmente blindada o simplemente habilitadas para vTPM.
Use `-Policy Shielded` para una máquina virtual blindada completamente o `-Policy EncryptionSupported` ha habilitado un vTPM PowerShell Direct y máquina virtual que permita las conexiones de consola básica.

Una vez que todo esté listo, ejecute el siguiente comando para crear el archivo de datos de blindaje:

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

En el comando anterior, el guardián denominado "Owner" (obtenido de Get-HgsGuardian) podrán cambiar la configuración de seguridad de la máquina virtual en el futuro, mientras que 'EAST-US Datacenter' pueden ejecutar la máquina virtual pero no cambiar su configuración.
Si tiene más de una protección, como los nombres de las protecciones con comas independiente `'EAST-US Datacenter', 'EMEA Datacenter'`.
El calificador de Id. de volumen Especifica si se puede confiar solo la versión exacta (igual) del disco de plantilla o en versiones futuras (GreaterThanOrEquals) también.
El nombre del disco y el certificado de firma deben coincidir exactamente para que la comparación de versiones en cuenta durante la implementación.
Puede confiar en más de un disco de plantilla, ya que proporciona una lista separada por comas de volumen calificadores de Id. para el `-VolumeIDQualifier` parámetro.
Por último, si tiene otros archivos que necesitan para acompañar el archivo de respuesta con la máquina virtual, use el `-OtherFile` parámetro y proporcionar una lista separada por comas de rutas de acceso.

Consulte la documentación del cmdlet de [New ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps) y [New VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps) para obtener información sobre otras maneras de configurar su archivo de datos de blindaje.

## <a name="see-also"></a>Vea también

- [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
