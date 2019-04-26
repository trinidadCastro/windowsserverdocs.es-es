---
title: Administrar el Registro de inventario de software
description: Describe cómo administrar el registro de inventario de Software
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 812173d1-2904-42f4-a9e2-de19effec201
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 095cd2ad799857b789943b4f477aa9e6a8c3ae50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815186"
---
# <a name="manage-software-inventory-logging"></a>Administrar el Registro de inventario de software

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Este documento describe cómo administrar el registro de inventario de Software, una característica que ayuda a los administradores de centros de datos a registrar fácilmente los datos de administración de activos de software de Microsoft para sus implementaciones con el tiempo. En este documento se describe cómo administrar el Registro de inventario de software. Antes de usar el registro de inventario de Software con Windows Server 2012 R2, asegúrese de que la actualización Windows [KB 3000850](https://support.microsoft.com/kb/3000850) y [KB 3060681](https://support.microsoft.com/kb/3060681) están instalados en cada sistema que necesitan realizar un inventario. No hay actualizaciones de Windows son necesarios para Windows Server 2016. Esta característica se ejecuta localmente en cada servidor para que se incluya en el inventario. No recopila datos de servidores remotos.  

La característica de registro de inventario de Software también puede agregarse a dos versiones de Windows Server anteriores a Windows Server 2012 R2. Puede instalar las actualizaciones siguientes para agregar funcionalidad de registro de inventario de Software para Windows Server 2012 y Windows Server 2008 R2 SP1:

- **Windows Server 2012 (Standard o Datacenter Edition)** 

> [!NOTE] 
> Asegúrese de tener [WMF 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) instalado antes de aplicar el paquete de actualización siguiente.

-  Paquete de actualización de WMF 4.0 para Windows Server 2012: [KB 3119938](https://support.microsoft.com/en-us/kb/3119938)

- **Windows Server 2008 R2 SP1**

> [!NOTE] 
> Asegúrese de tener [WMF 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) instalado antes de aplicar el paquete de actualización siguiente.


- Requiere [.NET Framework 4.5](https://www.microsoft.com/en-us/download/details.aspx?id=30653)


- Paquete de actualización de WMF 4.0 para Windows Server 2008 R2: [KB 3109118](https://support.microsoft.com/en-us/kb/3109118)


Hay dos métodos principales para realizar un inventario con esta característica:  
  
1.  Iniciar la funcionalidad de registro de SIL para recopilar orígenes de datos de SIL y reenviar la carga a través de la red a un destino (URI) concreto cada hora.  
  
2.  Consultar manualmente los datos de SIL mediante WMI o PowerShell en cualquier intervalo.  
  
El inicio del registro de SIL implica una cierta planificación y previsión, pero aporta ventajas considerables en comparación con la consulta manual de los datos. El uso del registro de SIL tiene tres ventajas principales para los administradores de centros de datos:  
  
-   Es posible recopilar un historial (registro) continuo con el tiempo, lo que permite informes completos y flexibles de un único origen.  
  
-   Se pueden superar los retos de detección de equipos comunes que comparten muchas herramientas de inventario.  
  
-   Es posible superar los retos que plantean los límites de confianza y cubrir la necesidad de los privilegios de usuarios elevados habituales de muchas herramientas de inventario a la vez que se mantiene un nivel de seguridad, ya que los datos se cifran mediante HTTPS con SSL.  
  
El Registro de inventario de software está instalado de forma predeterminada, pero el registro no se inicia forma predeterminada. Toda la configuración del Registro de inventario de Software se realiza con cmdlets de PowerShell. Solo hay unas pocas opciones de configuración del Registro de inventario de software. En este documento se describen estas opciones y su finalidad prevista, así como los cmdlets que se usan para la recopilación de datos (si se usa el segundo método anterior).  
  
**En este documento**  
  
Las opciones de configuración que se abordan en este documento son:  
  
-   [Iniciar y detener el Software de registro de inventario](manage-software-inventory-logging.md#BKMK_Step1)  
  
-   [Registro de inventario de software con el tiempo](manage-software-inventory-logging.md#BKMK_Step2)  
  
-   [Mostrar los datos de registro de inventario de Software](manage-software-inventory-logging.md#BKMK_Step3)  
  
-   [Eliminación de datos registrados por el registro de inventario de Software](manage-software-inventory-logging.md#BKMK_Step4)  
  
-   [Copia de seguridad y restauración de datos registrados por el registro de inventario de Software] administrar-software-inventario-logging.md #BKMK_Step5)  
  
-   [Lectura de datos registrado y publicado por registro de inventario de Software](manage-software-inventory-logging.md#BKMK_Step6)  
  
-   [Seguridad del registro de inventario de software](manage-software-inventory-logging.md#BKMK_Step7)  
  
-   [Trabajar con la configuración de fecha y hora en el registro de inventario de Software de Windows Server](manage-software-inventory-logging.md#BKMK_Step8)  
  
-   [Habilitar y configurar el inventario de Software de registro en un disco duro Virtual montado](manage-software-inventory-logging.md#BKMK_Step10)  
  
-   [Introducción al uso de inventario de Software de registro en Windows Server 2012 R2 sin KB 3000850](manage-software-inventory-logging.md#BKMK_Step11)  
  
-   [Usar el registro de inventario de Software en un entorno de Windows Server 2012 R2 Hyper-V sin KB 3000850](manage-software-inventory-logging.md#BKMK_Step12)  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulte uso de Cmdlets.

  
## <a name="BKMK_Step1"></a>Iniciar y detener el Software de registro de inventario  
Registro de inventario de software de recopilación diaria y hacia delante a través de la red deben habilitarse en un equipo que ejecuta Windows Server 2012 R2 al inventario de software de registro.  
  
> [!NOTE]  
> Puede usar el cmdlet de PowerShell **[Get-SilLogging](https://technet.microsoft.com/library/dn283396.aspx)** para recuperar información sobre el servicio del Registro de inventario de software, incluido si está en ejecución o detenido.  
  
#### <a name="to-start-software-inventory-logging"></a>Para iniciar el Registro de inventario de software  
  
1.  Inicie sesión en el servidor con una cuenta que tenga privilegios de administrador local.  
  
2.  Ejecute PowerShell como administrador.  
  
3.  En el símbolo del sistema de PowerShell, escriba **[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx)**  
  
> [!NOTE]  
> Es posible establecer el destino sin tener que configurar una huella digital del certificado, pero si lo hace, se producirá un error en los reenvíos y los datos se almacenarán localmente durante un valor predeterminado de hasta 30 días (después del cual se eliminará). Una vez que se establezca un hash de certificado válido para el destino (y se instale el correspondiente certificado válido en el almacén LocalMachine o Personal), los datos almacenados localmente se reenviarán al destino siempre y cuando el destino esté configurado para aceptar estos datos con este certificado (consulte [Software Inventory Logging Aggregator](Software-Inventory-Logging-Aggregator.md) para obtener más información).  
  
#### <a name="to-stop-software-inventory-logging"></a>Para detener el Registro de inventario de software  
  
1.  Inicie sesión en el servidor con una cuenta que tenga privilegios de administrador local.  
  
2.  Ejecute PowerShell como administrador.  
  
3.  En el símbolo del sistema de PowerShell, escriba **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)**  
  
## <a name="configuring-software-inventory-logging"></a>Configuración del Registro de inventario de software  
Hay tres pasos para configurar el Registro de inventario de software con el fin de reenviar datos de un servidor de agregación con el transcurso del tiempo:  
  
1.  Use **Set-SilLogging – TargetUri** para especificar la dirección web de su servidor de agregación (debe comenzar por "https://?).  
  
2.  Use **Set-SilLogging –CertificateThumbprint** para especificar el valor de hash de huella digital del certificado SSL válido que se va a usar para autenticar las transmisiones de datos a su servidor de agregación (el servidor de agregación deberá configurarse para aceptar hash).  
  
3.  Instale un certificado SSL válido (para su red) en el **Almacén LocalMachine o Personal** (o **/LocalMachine/MY**) del servidor local desde el que se van a reenviar datos.  
  
Es conveniente realizar estos pasos antes de usar **Start-SilLogging**.  Si quiere usarlos después de usar **Start-SilLogging**, basta con detener e iniciar de nuevo SIL.  O bien, puede usar el cmdlet Publish-SilData para asegurarse de que el servidor de agregación tenga un complemento completo de los datos para este servidor.  
  
Si quiere obtener una guía completa para configurar el marco SIL en su conjunto, vea [Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md).  En particular, si **Publish-SilData** produce un error, o si de algún modo el registro SIL produce un error, vea la sección de solución de problemas.  
  
## <a name="BKMK_Step2"></a>Registro de inventario de software con el tiempo  
Si un administrador inició el Registro de inventario de software, comenzará la recopilación y el reenvío cada hora de los datos al servidor de agregación (URI de destino). El primer reenvío será un conjunto de datos completo de los mismos datos que [Get-SilData](https://technet.microsoft.com/library/dn283388.aspx) recupera y muestra en la consola en un momento dado. A partir de ese momento, en cada intervalo, SIL realizará una comprobación de los datos y reenviará solo una pequeña confirmación de identificación al servidor de agregación de destino si no hay ningún cambio en los datos desde la última recopilación. Si algún valor ha cambiado, SIL enviará de nuevo un conjunto de datos completo.  
  
> [!IMPORTANT]  
> Si en cualquier intervalo el URI de destino no es accesible o la transferencia de datos a través de la red no se realiza por algún motivo, los datos recopilados se almacenarán localmente durante un valor predeterminado de hasta 30 días (después de cual se eliminará). En el siguiente reenvío correcto de datos al servidor de agregación de destino, todos los datos almacenados localmente se reenviarán y se eliminarán los datos almacenados en la memoria caché local.  
  
## <a name="BKMK_Step3"></a>Mostrar los datos de registro de inventario de Software  
Además de los cmdlets de PowerShell que se describen en la sección anterior, es posible usar seis cmdlets adicionales para recopilar datos del Registro de inventario de software:  
  
-   **[Get-SilComputer](https://technet.microsoft.com/library/dn283392.aspx)**: Muestra los valores en momentos concretos de datos relacionados con el sistema operativo y un servidor concreto, así como el FQDN o el nombre de host del host físico, si está disponible.  
  
-   **[Get-SilComputerIdentity (KB 3000850)](https://technet.microsoft.com/library/dn858074.aspx)**: Muestra los identificadores que usa SIL para servidores individuales.  
  
-   **[Get-SilData](https://technet.microsoft.com/library/dn283388.aspx)**: Muestra una recopilación en momentos concretos de todos los datos del Registro de inventario de software.  
  
-   **[Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)**: Muestra la identidad en momentos concretos de todo el software instalado en el equipo.  
  
-   **[Get-SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx)**: Muestra el número total de solicitudes de dispositivos cliente únicos y las solicitudes del servidor de los dos días anteriores.  
  
-   **[Get-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx)**: Muestra la lista en momentos concretos de todas las actualizaciones de Windows instaladas en el equipo.  
  
Un escenario de caso de uso típico de los cmdlets del Registro de inventario de software sería un administrador que consulta el Registro de inventario de software en busca de la recopilación de un momento concreto de todos los datos del Registro de inventario de software mediante [Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx).  
  
**Ejemplo de salida**  
  
```  
PS C:\> Get-SilData   
  
ID                 : 961FF8A1-8549-4BEC-8DF6-3B3E32C26FFA  
UUID               : B49ACB4C-7D9C-4806-9917-AE750BB3DA84  
VMGUID             : E84CCCBD-0D0F-486B-A424-9780C7CF92E4  
Name               : Server01Guest.Test.Contoso.com  
HypervisorHostName : Server01.Test.Contoso.com  
  
ID          : {F0C3E5D1-1ADE-321E-8167-68EF0DE699A5}  
Name        : Microsoft Visual C++ 2010  x86 Redistributable - 10.0.40219  
InstallDate : 12/5/2013  
Publisher   : Microsoft Corporation  
Version     : 10.0.40219  
  
ID          : {89F4137D-6C26-4A84-BDB8-2E5A4BB71E00}  
Name        : Microsoft Silverlight  
InstallDate : 3/20/2014  
Publisher   : Microsoft Corporation  
Version     : 5.1.30214.0  
  
ChassisSerialNumber       : 4452-0564-0284-2290-0113-6804-05  
CollectedDateTime         : 10/27/2014 4:01:33 PM  
Model                     : Virtual Machine  
Name                      : Server01Guest.Test.Contoso.com  
NumberOfCores             : 1  
NumberOfLogicalProcessors : 1  
NumberOfProcessors        : 1  
OSName                    : Microsoft Windows Server 2012 R2 Datacenter  
OSSku                     : 8  
OSSuite                   : 400  
OSSuiteMask               : 400  
OSVersion                 : 6.3.9600  
ProcessorFamily           : 179  
ProcessorManufacturer     : GenuineIntel  
ProcessorName             : Intel(R) Xeon(R) CPU           E5440  @ 2.83GHz  
SystemManufacturer        : Microsoft Corporation  
  
```  
  
> [!NOTE]  
> El resultado de este cmdlet es el mismo que el de todos los demás cmdlets **Get-Sil** para esta característica combinados, pero se ofrece a la consola de forma asincrónica, por lo que puede que el orden de los objetos no sea siempre el mismo.  
>   
> No es necesario que el Registro de inventario de software se haya iniciado para poder usar los cmdlets **Get-Sil**.  
  
## <a name="BKMK_Step4"></a>Eliminación de datos registrados por el registro de inventario de Software  
El Registro de inventario de software no pretende ser un componente crítico. Está diseñado para tener un impacto mínimo en las operaciones del sistema local, si bien manteniendo un gran nivel de confiabilidad. Esto también permite al administrador elimine manualmente la base de datos de registro de inventario de Software y archivos (todos los archivos en el directorio \Windows\System32\LogFiles\SIL) auxiliares para satisfacer las necesidades operativas.  
  
#### <a name="to-delete-data-logged-by-software-inventory-logging"></a>Para eliminar los datos que el Registro de inventario de software ha registrado  
  
1.  En PowerShell, detenga el Registro de inventario de software con el comando **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)** .  
  
2.  Abra el Explorador de Windows.  
  
3.  Vaya a **\Windows\System32\Logfiles\SIL\**  
  
4.  Elimine todos los archivos de esa carpeta.  
  
## <a name="BKMK_Step5"></a>Copia de seguridad y restauración de datos registrados por el registro de inventario de Software  
El Registro de inventario de software almacenará temporalmente colecciones de datos cada hora si no pueden realizar los reenvíos a través de la red. Los archivos de registro se almacenan en el directorio \Windows\System32\LogFiles\SIL\. Se pueden realizar copias de seguridad de estos datos del Registro de inventario de software con las copias de seguridad de servidor programadas periódicamente.  
  
> [!IMPORTANT]  
> Si por alguna razón es necesario realizar una reparación de la instalación o una actualización del sistema operativo, se perderán los archivos de registro almacenados localmente.  Si estos datos son fundamentales para las operaciones, se recomienda realizar copias de seguridad antes de la instalación del nuevo sistema operativo. Después de reparar o actualizar, restaure simplemente en la misma ubicación.  
  
> [!NOTE]  
> Si por alguna razón la administración de la retención de la duración de los datos que registró localmente Sil es importante, esto se puede configurar cambiando el valor del registro aquí: \HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging. El valor predeterminado es '30' para 30 días.  
  
## <a name="BKMK_Step6"></a>Lectura de datos registrado y publicado por registro de inventario de Software  
Los datos que registró SIL, pero que se almacenaron localmente (si se produjo un error en el reenvío al URI de destino) o los datos que se reenviaron correctamente al servidor de agregación de destino se almacenan en un archivo binario (para los datos de cada día). Para mostrar estos datos en PowerShell, use el cmdlet [Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx) .  
  
## <a name="BKMK_Step7"></a>Seguridad del registro de inventario de software  
Se requieren privilegios administrativos en el servidor local para recuperar correctamente datos de WMI del Registro de inventario de software y las API de PowerShell.  
  
Para aprovechar correctamente la capacidad completa de la característica del Registro de inventario de software para reenviar datos a un punto de agregación de forma continua en el tiempo (en intervalos de una hora), un administrador necesita usar certificados de cliente a fin de garantizar las sesiones SSL seguras para la transferencia de datos a través de HTTPS. Aquí encontrará una introducción básica a la autenticación HTTPS: [Autenticación HTTPS](https://technet.microsoft.com/library/cc736680(v=WS.10).aspx).  
  
Los datos almacenados localmente en un equipo Windows Server (solo se produce si la característica se inicia, pero no se tiene acceso al destino por algún motivo) solamente estarán disponibles con privilegios administrativos en el servidor local.  
  
## <a name="BKMK_Step8"></a>Trabajar con la configuración de fecha y hora en el registro de inventario de Software de Windows Server 2012 R2  
  
-   Al usar [Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay para configurar la hora a la que se ejecuta el registro de SIL, se debe especificar una fecha y hora. Se establecerá la fecha del calendario y no se producirá el registro hasta que se llegue a esa fecha, en la hora del sistema local.  
  
-   ¿Cuando se usa [Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx), o [Get-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx), "InstallDate? mostrará siempre 12:00:00 A.M., un valor sin sentido.  
  
-   ¿Cuando se usa [Get-SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx), "SampleDate? mostrará siempre 11:59:00 p. M., un valor sin sentido.  La fecha se corresponde con los datos pertinentes para estas consultas de cmdlet.  
  
## <a name="BKMK_Step10"></a>Habilitar y configurar el inventario de Software de registro en un disco duro Virtual montado  
El Registro de inventario de software también admite la configuración y habilitación de máquinas virtuales sin conexión. Los usos prácticos de esto están diseñados para cubrir tanto la instalación "de imagen dorada" para la implementación a gran escala a lo largo de los centros de datos, así como la configuración de imágenes de usuario final que van desde una implementación local a una en la nube.  
  
Para admitir estos usos, el Registro de inventario de software tiene entradas de registro asociadas con cada opción configurable.  Estos valores del registro pueden encontrarse en \HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging.  
  
|||||  
|-|-|-|-|  
|**Función**|**Nombre del valor**|**Datos**|**Cmdlet correspondiente (disponible solo en el sistema operativo en ejecución)**|  
|Iniciar o detener la característica|CollectionState|1 o 0|[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx), [Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)|  
|Especifica el punto de agregación de destino en la red|TargetUri|string|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TargetURI|  
|Especifica la huella digital del certificado o el hash del certificado usado para la autenticación SSL del servidor web de destino|CertificateThumbprint|string|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -CertificateThumbprint|  
|Especifica la fecha y hora en que debe iniciarse la característica (si el valor establecido es futuro, según la hora del sistema local)|CollectionTime|Default:  2000-01-01T03:00:00|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay|  
  
Para modificar estos valores en un VHD sin conexión (sin que el sistema operativo de la máquina virtual se ejecute), primero se debe montar un VHD y, después, podrán usarse los comandos siguientes para realizar cambios:  
  
-   [carga de registro](https://technet.microsoft.com/library/cc742053.aspx)  
  
-   [reg delete](https://technet.microsoft.com/library/cc742145.aspx)  
  
-   [Agregar registro](https://technet.microsoft.com/library/cc742162.aspx)  
  
-   [reg unload](https://technet.microsoft.com/library/cc742043.aspx)  
  
El Registro de inventario de software comprobará estos valores cuando se inicie el sistema operativo y se ejecutará en consecuencia.  
  
## <a name="BKMK_Step11"></a>Introducción al uso de inventario de Software de registro en Windows Server 2012 R2 sin KB 3000850  
Se realizaron los cambios siguientes en la configuración predeterminada y la capacidad del Registro de inventario de software con [KB 3000850](https://support.microsoft.com/kb/3000850):  
  
-   El intervalo predeterminado para la recopilación y el reenvío a través de la red cuando se inicia el registro de SIL cambió de diario a cada hora (de forma aleatoria en cada hora).  
  
-   Se redujo la carga de datos predeterminado para incluir solo los objetos de Get-SilComputer, Get-SilComputerIdentity y Get-SilSoftware.  
  
-   Se eliminó el invitado de la comunicación del canal de host en entornos de Hyper-V.  
  
## <a name="BKMK_Step12"></a>Usar el registro de inventario de Software en un entorno de Windows Server 2012 R2 Hyper-V sin KB 3000850  
  
> [!NOTE]  
> Esta funcionalidad se elimina con la instalación de la actualización [KB 3000850](https://support.microsoft.com/kb/3000850) .  
  
Cuando se usa el registro de inventario de Software en un host de Windows Server 2012 R2 Hyper-V, es posible recuperar los datos SIL de los invitados de Windows Server 2012 R2 que se ejecutan localmente, si se ha iniciado el registro de SIL en el invitado (s). Sin embargo, esto solo es posible cuando se usan los cmdlets Get-SilData y Publish-SilData Powershell y solo es posible con WIndows Server 2012 R2 en el host e invitado.  El objetivo de esta capacidad es permitir que los administradores del centro de datos ofrezcan máquinas virtuales invitadas a los inquilinos u otras entidades de una gran empresa, a fin de capturar datos de inventario de software en el host del hipervisor y posteriormente reenviar todos esos datos a un agregador (o un URI de destino).  
  
A continuación se muestran dos ejemplos de máquina virtual con el registro de SIL iniciado a qué el resultado en la consola tendría el siguiente aspecto (muy abreviada) en un host de Windows Server 2012 R2 Hyper-V que ejecuta a un invitado de Windows Server 2012 R2 de PowerShell.  Puede observar que el primer ejemplo, que usa únicamente Get-SilData, dará como resultado todos los datos desde el host, como se esperaba.  También se incluyen todos los datos de SIL del invitado, pero en un formato contraído.  Para expandir y ver los datos del invitado, corte y pegue el fragmento de código que se usa en el segundo ejemplo de más abajo.  Los objetos de datos del SIL del invitado siempre tendrán el GUID de la máquina virtual asociado dentro del objeto.  
  
> [!NOTE]  
> Dado que los datos del SIL se muestran como resultados en la consola, al usar el cmdlet Get-SilData, en las secuencias de datos, los objetos no siempre se mostrarán en la salida en un orden predecible.  En los dos ejemplos siguientes, se ha aplicado formato de color al texto (azul para los datos del host físico y verde para los datos del invitado virtual) solo como una herramienta ilustrativa para este documento.  
  
**Ejemplo de salida 1**  
  
![](../media/software-inventory-logging/SILHyper-VExample1.png)  
  
**Ejemplo de salida 2** (con la función Expand-Sildat)  
  
![](../media/software-inventory-logging/SILHyper-VExample2.png)  
  
## <a name="see-also"></a>Vea también  
[Empezar a trabajar con Software de registro de inventario](get-started-with-software-inventory-logging.md)  
[Agregador de registro de inventario de software](software-inventory-logging-aggregator.md)  
[Cmdlets de registro de inventario de software en Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)  
[Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx)  
[Export-BinaryMiLog](https://technet.microsoft.com/library/dn262591.aspx)  
  

