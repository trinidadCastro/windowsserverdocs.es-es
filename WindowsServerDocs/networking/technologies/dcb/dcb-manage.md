---
title: Administrar el centro de datos (DCB) de protocolo de puente
description: Este tema proporciona instrucciones sobre cómo usar los comandos de Windows PowerShell para administrar el puente del centro de datos en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daed746fe798ae253956d0977827d0e205bb8b3e
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034578"
---
# <a name="manage-data-center-bridging-dcb"></a>Administrar el centro de datos (DCB) de protocolo de puente

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema proporciona instrucciones sobre cómo usar los comandos de Windows PowerShell para configurar el protocolo de puente del centro de datos \(DCB\) en un DCB\-adaptador de red compatible que se instala en un equipo que ejecuta Windows Server 2016 o Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Instalar DCB en Windows Server 2016 o Windows 10

Para obtener información sobre los requisitos previos para utilizar y cómo instalar DCB, vea [instalar Data Center Bridging (DCB) en Windows Server 2016 o Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configuraciones de DCB 

Antes de Windows Server 2016, toda la configuración de DCB se aplica universalmente a todos los adaptadores de red admiten DCB. 

En Windows Server 2016, se pueden aplicar configuraciones de DCB al Store de la directiva Global o a Store individuales de directiva\(s\). Cuando se aplican las directivas individuales invalidan todas las configuraciones de directiva Global.

Las configuraciones de asignación de prioridad de tráfico PFC, clase y la aplicación en el nivel de sistema no se aplica en los adaptadores de red hasta que lo haga lo siguiente.

1. Activar el bit dispuesto DCBX en false

2. Habilite DCB en los adaptadores de red. Consulte [habilitar y mostrar la configuración de DCB en los adaptadores de red](#bkmk_enabledcb).

>[!NOTE]
>Si desea configurar DCB desde un conmutador a través de DCBX, consulte [configuración DCBX](#dcb-configuration-on-network-adapters).

El bit dispuesto DCBX se describe en la especificación de DCB. Si el bit dispuesto en un dispositivo se establece en true, el dispositivo está dispuesto a aceptar las configuraciones de un dispositivo remoto a través de DCBX. Si el bit dispuesto en un dispositivo se establece en false, el dispositivo rechazará todos los intentos de configuración de dispositivos remotos y aplicar solo a las configuraciones locales.

Si DCB no está instalado en Windows Server 2016 el valor del bit dispuesto es irrelevante en lo que se refiere el sistema operativo porque el sistema operativo no tiene ninguna configuración local se aplican a los adaptadores de red. Después de instala DCB, el valor predeterminado del bit dispuesto es true. Este diseño permite a los adaptadores de red mantener cualquier configuraciones recibieron de sus colegas remotos.

Si un adaptador de red no es compatible con DCBX, nunca recibirá las configuraciones de un dispositivo remoto. Recibe las configuraciones del sistema operativo, pero solo después de la DCBX dispuesto bit se establece en false.

## <a name="set-the-willing-bit"></a>Establecer el bit dispuesto

Para aplicar configuraciones de sistema operativo de clase de tráfico, PFC y asignación de prioridad de la aplicación en los adaptadores de red, o simplemente reemplazar las configuraciones de dispositivos remotos \, si existe alguna \, puede ejecutar el comando siguiente.

>[!NOTE]
>Los nombres de comando de PowerShell de Windows de DCB incluyen "QoS" en lugar de "DCB" en la cadena de nombre. Esto es porque QoS y DCB están integrados en Windows Server 2016 para proporcionar una experiencia de administración de calidad de servicio.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Para mostrar el estado del bit dispuesto configuración, puede usar el comando siguiente:

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configuración de DCB en los adaptadores de red

Habilitación de DCB en un adaptador de red le permite ver la configuración que se propagan desde un conmutador para el adaptador de red.

DCB, figuran los pasos siguientes.

1.  Configure las opciones de DCB en el nivel del sistema, que incluye:

    a. Administración de la clase de tráfico
    
    b. Configuración de Control (PFC) del flujo de prioridad
    
    c. Asignación de prioridad de aplicación
    
    d. Configuración de DCBX

2. Configurar DCB del adaptador de red.



##  <a name="dcb-traffic-class-management"></a>Administración de la clase de tráfico de DCB

Estos son ejemplos de comandos de Windows PowerShell para administración de la clase de tráfico.

### <a name="create-a-traffic-class"></a>Crear una clase de tráfico

Puede usar el **New-NetQosTrafficClass** comando para crear una clase de tráfico.

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

De forma predeterminada, todos los valores de 802.1p se asignan a una clase de tráfico de forma predeterminada, que tiene el 100% del ancho de banda del vínculo físico. El **New-NetQosTrafficClass** comando crea una nueva clase de tráfico, se asigna para que cualquier paquete que esté etiquetada con la prioridad de 802.1p valor 4. El algoritmo de selección de transmisión \(TSA\) es ETS y tiene 30% del ancho de banda.

Puede crear hasta 7 nuevas clases de tráfico. Incluida la clase de tráfico de forma predeterminada, puede haber a lo sumo 8 clases de tráfico en el sistema. Sin embargo, un adaptador de red compatibles con DCB podría no admitir que muchas clases en el hardware de tráfico. Si crea más clases de tráfico que se puede incluir en un adaptador de red y habilite DCB en los que el adaptador de red, el controlador de minipuerto notifica un error en el sistema operativo. El error se registra en el registro de eventos.

La suma de las reservas de ancho de banda para todas las clases de tráfico creados no puede tener más de 99% del ancho de banda. La clase de tráfico predeterminado siempre tiene al menos un 1% del ancho de banda reservado para sí mismo.

### <a name="display-traffic-classes"></a>Mostrar las clases de tráfico

Puede usar el **Get-NetQosTrafficClass** comando para ver las clases de tráfico.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modificar una clase de tráfico

Puede usar el **Set-NetQosTrafficClass** comando para crear una clase de tráfico. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

A continuación, puede usar el **Get-NetQosTrafficClass** comando para ver la configuración.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Después de crear una clase de tráfico, puede cambiar su configuración de forma independiente. Los valores que puede cambiar se incluyen:

1. Asignación de ancho de banda \(- BandwidthPercentage\)

2. TSA (\-algoritmo\)

3. Asignación de prioridad \(-prioridad\)

### <a name="remove-a-traffic-class"></a>Quitar una clase de tráfico

Puede usar el **Remove-NetQosTrafficClass** comando para eliminar una clase de tráfico.

>[!IMPORTANT]
>No se puede quitar la clase de tráfico de forma predeterminada.


    Remove-NetQosTrafficClass -Name SMB

A continuación, puede usar el **Get-NetQosTrafficClass** comando para ver la configuración.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Después de quitar una clase de tráfico, el valor de 802.1p se asigna a que la clase de tráfico se reasigna a la clase de tráfico de forma predeterminada. Ancho de banda que se ha reservado para una clase de tráfico se devuelve a la asignación de clase de tráfico de forma predeterminada cuando se quita la clase de tráfico.

## <a name="per-network-interface-policies"></a>Directivas de la interfaz de red

Todos los ejemplos anteriores establecen las directivas globales. Estos son algunos ejemplos de cómo puede establecer y obtener las directivas por NIC. 

El campo "PolicySet" cambia de Global a AdapterSpecific. Cuando se muestran las directivas de AdapterSpecific, el índice de interfaz \(ifIndex\) y nombre de la interfaz \(ifAlias\) también se muestran.

```
PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              AdapterSpecific  4       M1


PS C:\> New-NetQosTrafficClass -Name SMBGlobal -BandwidthPercentage 30 -Priority 4 -Algorithm ETS

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBGlobal   ETS       30           4                Global


PS C:\> New-NetQosTrafficClass -Name SMBforM1 -BandwidthPercentage 30 -Priority 4 -Algorithm ETS -Interfac


Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          Global
SMBGlobal   ETS       30           4                Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          AdapterSpecific  4       M1
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


```

## <a name="priority-flow-control-settings"></a>Configuración de Control de flujo de prioridad:

Estos son algunos ejemplos de comandos para la configuración de Control de flujo de prioridad. También se puede especificar esta configuración para adaptadores individuales.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Habilitar y Control de flujo de prioridad de visualización para globales y específicas de interfaz de casos de uso

```
PS C:\> Enable-NetQosFlowControl -Priority 4
PS C:\> Enable-NetQosFlowControl -Priority 3 -InterfaceAlias M1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          True       Global
5          False      Global
6          False      Global
7          False      Global

PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          True       AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1  

```


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Deshabilitar el Control de flujo de prioridad (globales y específicos de la interfaz)

```
PS C:\> Disable-NetQosFlowControl -Priority 4
PS C:\> Disable-NetQosFlowControl -Priority 3 -InterfaceAlias m1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          False      Global
5          False      Global
6          False      Global
7          False      Global


PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          False      AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1  

```

##  <a name="application-priority-assignment"></a>Asignación de prioridad de aplicación

Estos son algunos ejemplos de asignación de prioridad.

### <a name="create-qos-policy"></a>Crear directiva QoS

```
PS C:\> New-NetQosPolicy -Name "SMB Policy" -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

El comando anterior crea una nueva directiva para SMB. SMB: es un filtro de la Bandeja de entrada que coincide con el puerto TCP 445 (reservado para SMB). Si un paquete se envía al puerto TCP 445 se etiquetarán por el sistema operativo con el valor de 802.1p de 4 antes de que el paquete se pasa a un controlador de minipuerto de la red.

: SMB, además de otros filtros predeterminados incluyen: iSCSI (coincidencia el puerto TCP 3260), - NFS (coincidencia el puerto TCP 2049), - LiveMigration (búsqueda de coincidencias el puerto TCP 6600), - FCOE (coincidencia de EtherType 0x8906) y – NetworkDirect.

NetworkDirect es una capa abstracta que creamos sobre cualquier implementación de RDMA en un adaptador de red. NetworkDirect – debe ir seguido por un puerto de red directo.

Además de los filtros de forma predeterminada, se puede clasificar el tráfico por el nombre del archivo ejecutable de la aplicación (como en el primer ejemplo), o por dirección IP, puerto o protocolo (como se muestra en el segundo ejemplo):

**Por el nombre del archivo ejecutable**

```
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

```


**Dirección IP puerto o protocolo**

```
PS C:\> New-NetQosPolicy -Name "Network Management" -IPDstPrefixMatchCondition 10.240.1.0/24 -IPProtocolMatchCondition both -NetworkProfile all -PriorityValue8021Action 7

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

```

### <a name="display-qos-policy"></a>Mostrar la directiva de QoS

```
PS C:\> Get-NetQosPolicy

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

### <a name="modify-qos-policy"></a>Modificar la directiva de QoS

Puede modificar las directivas de QoS, tal como se muestra a continuación.


```
PS C:\> Set-NetQosPolicy -Name "Network Management" -IPSrcPrefixMatchCondition 10.235.2.0/24 -IPProtocolMatchCondition both -PriorityValue8021Action 7
PS C:\> Get-NetQosPolicy

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPSrcPrefix    : 10.235.2.0/24
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7


```

### <a name="remove-qos-policy"></a>Quitar directiva de QoS

```
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y  

```

## <a name="dcb-configuration-on-network-adapters"></a>Configuración de DCB en los adaptadores de red

Configuración de DCB en los adaptadores de red es independiente de configuración de DCB a nivel de sistema que se ha descrito anteriormente. 

Independientemente de si DCB está instalado en Windows Server 2016, siempre puede ejecutar los comandos siguientes. 

Si configura DCB de un conmutador y se basan en DCBX para propagar las configuraciones de adaptadores de red, puede examinar qué configuraciones se reciben y se aplican en los adaptadores de red desde el lado del sistema operativo después de habilitar DCB en los adaptadores de red.

###  <a name="bkmk_enabledcb"></a>Habilitar y mostrar la configuración de DCB en los adaptadores de red

```
PS C:\> Enable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos

Name                       : M1
Enabled                    : True
Capabilities               :                       Hardware     Current
                                                   --------     -------
                             MacSecBypass        : NotSupported NotSupported
                             DcbxSupport         : None         None
                             NumTCs(Max/ETS/PFC) : 8/8/8        8/8/8

OperationalTrafficClasses  : TC TSA    Bandwidth Priorities
                             -- ---    --------- ----------
                              0 ETS    70%       0-3,5-7
                              1 ETS    30%       4

OperationalFlowControl     : All Priorities Disabled
OperationalClassifications : Protocol  Port/Type Priority
                             --------  --------- --------
                             Default             1


```

### <a name="disable-dcb-on-network-adapters"></a>Deshabilitar DCB en los adaptadores de red

```
PS C:\> Disable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos M1

Name         : M1
Enabled      : False
Capabilities :                       Hardware     Current
                                     --------     -------
               MacSecBypass        : NotSupported NotSupported
               DcbxSupport         : None         None
               NumTCs(Max/ETS/PFC) : 8/8/8        0/0/0  

```
## <a name="bkmk_wps"></a>Comandos de Windows PowerShell para DCB

Hay comandos DCB Windows PowerShell para Windows Server 2016 y Windows Server 2012 R2. Puede usar todos los comandos de Windows Server 2012 R2 en Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandos de Windows Server 2016 Windows PowerShell para DCB

El siguiente tema para Windows Server 2016 proporciona descripciones de cmdlet de Windows PowerShell y la sintaxis para todos los datos de puente del centro de \(DCB\) calidad de servicio \(QoS\)\-cmdlets específicos. Enumera los cmdlets por orden alfabético según el verbo que aparece al principio del cmdlet.

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows PowerShell comandos de Windows Server 2012 R2 para DCB

El siguiente tema para Windows Server 2012 R2 proporciona descripciones de cmdlet de Windows PowerShell y la sintaxis para todos los datos de puente del centro de \(DCB\) calidad de servicio \(QoS\)\-cmdlets específicos. Enumera los cmdlets por orden alfabético según el verbo que aparece al principio del cmdlet.

- [Centro de datos (DCB) Cmdlets de calidad de servicio (QoS) en Windows PowerShell de protocolo de puente](https://technet.microsoft.com/library/hh967440.aspx)
