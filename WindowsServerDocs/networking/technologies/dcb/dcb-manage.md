---
title: Administrar el protocolo de puente del centro de datos (DCB)
description: En este tema se proporcionan instrucciones sobre cómo usar los comandos de Windows PowerShell para administrar el protocolo de puente del centro de datos en Windows Server 2016.
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 12/08/2020
ms.openlocfilehash: 51f4b70dfc81010bfb2989f925a9f6508b6eb1c1
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949931"
---
# <a name="manage-data-center-bridging-dcb"></a>Administrar el protocolo de puente del centro de datos (DCB)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporcionan instrucciones sobre cómo usar los comandos de Windows PowerShell para configurar el protocolo de puente \( del centro de datos DCB \) en un \- adaptador de red compatible con DCB que está instalado en un equipo que ejecuta Windows Server 2016 o Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Instalar DCB en Windows Server 2016 o Windows 10

Para obtener información sobre los requisitos previos para usar y cómo instalar DCB, consulte [instalación del Protocolo de puente del centro de datos (DCB) en Windows Server 2016 o Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configuraciones de DCB

Antes de Windows Server 2016, toda la configuración de DCB se aplicaba universalmente a todos los adaptadores de red que admitían DCB.

En Windows Server 2016, puede aplicar las configuraciones de DCB al almacén de directivas global o a los almacenes de directivas individuales \( \) . Cuando se aplican directivas individuales, se invalidan todas las configuraciones de directivas globales.

Las configuraciones de clase de tráfico, PFC y asignación de prioridad de aplicación en el nivel de sistema no se aplican a los adaptadores de red hasta que haga lo siguiente.

1. Desactive el bit DCBX en false

2. Habilite DCB en los adaptadores de red. Vea [habilitar y mostrar la configuración de DCB en adaptadores de red](#bkmk_enabledcb).

>[!NOTE]
>Si desea configurar DCB desde un conmutador a través de DCBX, consulte [configuración de DCBX](#dcb-configuration-on-network-adapters).

El bit interesado de DCBX se describe en la especificación de DCB. Si el bit que se encuentra en un dispositivo se establece en true, el dispositivo está dispuesto a aceptar configuraciones desde un dispositivo remoto a través de DCBX. Si el bit dispuesto en un dispositivo se establece en false, el dispositivo rechazará todos los intentos de configuración de los dispositivos remotos y aplicará solo las configuraciones locales.

Si DCB no está instalado en Windows Server 2016, el valor del bit interesado es irrelevante en lo que respecta al sistema operativo, ya que el sistema operativo no tiene ninguna configuración local aplicable a los adaptadores de red. Una vez instalado DCB, el valor predeterminado del bit interesado es true. Este diseño permite a los adaptadores de red mantener las configuraciones que puedan haber recibido de sus homólogos remotos.

Si un adaptador de red no es compatible con DCBX, nunca recibirá las configuraciones de un dispositivo remoto. Recibe las configuraciones del sistema operativo, pero solo después de que el bit dispuesto de DCBX esté establecido en false.

## <a name="set-the-willing-bit"></a>Establecer el bit dispuesto

Para aplicar configuraciones de sistema operativo de clase de tráfico, PFC y asignación de prioridad de aplicación en los adaptadores de red, o simplemente invalidar las configuraciones de los dispositivos remotos \, si hay \, puede ejecutar el siguiente comando.

>[!NOTE]
>Los nombres de comandos de Windows PowerShell de DCB incluyen "QoS" en lugar de "DCB" en la cadena de nombre. Esto se debe a que QoS y DCB están integrados en Windows Server 2016 para proporcionar una experiencia de administración de QoS sin problemas.

```powershell
    Set-NetQosDcbxSetting -Willing $FALSE

    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

Para mostrar el estado de la configuración de bits que desea, puede usar el siguiente comando:

```powershell
    Get-NetQosDcbxSetting

    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global
```

## <a name="dcb-configuration-on-network-adapters"></a>Configuración de DCB en adaptadores de red

Habilitar DCB en un adaptador de red le permite ver la configuración propagada desde un conmutador al adaptador de red.

Las configuraciones de DCB incluyen los pasos siguientes.

1.  Configure los valores de DCB en el nivel del sistema, que incluye:

    a. Administración de clases de tráfico

    b. Configuración de control de flujo de prioridad (PFC)

    c. Asignación de prioridad de aplicación

    d. Configuración de DCBX

2. Configure DCB en el adaptador de red.

##  <a name="dcb-traffic-class-management"></a>Administración de clases de tráfico de DCB

A continuación se muestran ejemplos de comandos de Windows PowerShell para la administración de clases de tráfico.

### <a name="create-a-traffic-class"></a>Crear una clase de tráfico

Puede usar el comando **New-NetQosTrafficClass** para crear una clase de tráfico.

```powershell
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS

    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
```

De forma predeterminada, todos los valores p de 802.1 se asignan a una clase de tráfico predeterminada, que tiene un 100% del ancho de banda del vínculo físico. El comando **New-NetQosTrafficClass** crea una nueva clase de tráfico, a la que se asigna cualquier paquete etiquetado con el valor de prioridad 4 de 802.1 p. El algoritmo de selección \( de transmisión TSA \) es ETS y tiene un 30% del ancho de banda.

Puede crear hasta 7 clases de tráfico nuevas. Al incluir la clase de tráfico predeterminada, puede haber como máximo 8 clases de tráfico en el sistema. Sin embargo, es posible que un adaptador de red compatible con DCB no admita tantas clases de tráfico en el hardware. Si crea más clases de tráfico de las que se pueden incluir en un adaptador de red y habilita DCB en ese adaptador de red, el controlador de minipuerto informa de un error al sistema operativo. El error se registra en el registro de eventos.

La suma de las reservas de ancho de banda para todas las clases de tráfico creadas no puede superar el 99% del ancho de banda. La clase de tráfico predeterminada siempre tiene al menos un 1% del ancho de banda reservado para sí mismo.

### <a name="display-traffic-classes"></a>Mostrar clases de tráfico

Puede usar el comando **Get-NetQosTrafficClass** para ver las clases de tráfico.

```powershell
    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global
```

### <a name="modify-a-traffic-class"></a>Modificar una clase de tráfico

Puede usar el comando **set-NetQosTrafficClass** para crear una clase de tráfico.

```powershell
    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50
```

Después, puede usar el comando **Get-NetQosTrafficClass** para ver la configuración.

```powershell
    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global
```

Después de crear una clase de tráfico, puede cambiar su configuración de manera independiente. Entre los valores que puede cambiar se incluyen:

1. Asignación de ancho de banda (-BandwidthPercentage)

2. TSA (-Algorithm)

3. Asignación de prioridad (-prioridad)

### <a name="remove-a-traffic-class"></a>Quitar una clase de tráfico

Puede usar el comando **Remove-NetQosTrafficClass** para eliminar una clase de tráfico.

>[!IMPORTANT]
>No se puede quitar la clase de tráfico predeterminada.

```powershell
    Remove-NetQosTrafficClass -Name SMB

You can then use the **Get-NetQosTrafficClass** command to view settings.

    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
```

Después de quitar una clase de tráfico, el valor de 802.1 p asignado a esa clase de tráfico se reasigna a la clase de tráfico predeterminada. Cualquier ancho de banda reservado para una clase de tráfico se devuelve a la asignación de clase de tráfico predeterminada cuando se quita la clase de tráfico.

## <a name="per-network-interface-policies"></a>Directivas de interfaz de Per-Network

En todos los ejemplos anteriores se establecen directivas globales. A continuación se muestran ejemplos de cómo puede establecer y obtener directivas por NIC.

El campo "PolicySet" cambia de global a AdapterSpecific. Cuando se muestran las directivas de AdapterSpecific, también se muestran el índice de interfaz \( \) y el nombre de interfaz \( ifAlias \) .

```powershell
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

## <a name="priority-flow-control-settings"></a>Configuración de control de flujo de prioridad:

A continuación se muestran ejemplos de comandos para la configuración de control de flujo de prioridad. Esta configuración también se puede especificar para adaptadores individuales.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Habilitar y mostrar el control de flujo de prioridad para casos de uso global y de interfaz específicos

```powershell
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

### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Deshabilitar el control de flujo de prioridad (global e interfaz específica)

```powershell
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

A continuación se muestran ejemplos de asignación de prioridad.

### <a name="create-qos-policy"></a>Crear Directiva de QoS

```powershell
PS C:\> New-NetQosPolicy -Name "SMB Policy" -SMB -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
Template       : SMB
PriorityValue  : 4
```

El comando anterior crea una nueva Directiva para SMB. : SMB es un filtro de bandeja de entrada que coincide con el puerto TCP 445 (reservado para SMB). Si un paquete se envía al puerto TCP 445, lo etiquetará el sistema operativo con el valor 4 de 802.1 p antes de que el paquete se pase a un controlador de minipuerto de red.

Además de: SMB, otros filtros predeterminados incluyen-iSCSI (puerto TCP 3260),-NFS (puerto TCP 2049),-LiveMigration (puerto TCP 6600 correspondiente),-FCOE (coincidencia de EtherType 0x8906) y – NetworkDirect.

NetworkDirect es una capa abstracta que creamos sobre cualquier implementación de RDMA en un adaptador de red. – NetworkDirect debe ir seguido de un puerto de red directo.

Además de los filtros predeterminados, puede clasificar el tráfico por el nombre del archivo ejecutable de la aplicación (como en el primer ejemplo siguiente), o por la dirección IP, el puerto o el protocolo (como se muestra en el segundo ejemplo):

**Por nombre de archivo ejecutable**

```powershell
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1
```

**Por protocolo o puerto de dirección IP**

```powershell
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

### <a name="display-qos-policy"></a>Mostrar la directiva QoS

```powershell
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
Template       : SMB
JobObject      :
PriorityValue  : 4
```

### <a name="modify-qos-policy"></a>Modificar la Directiva de QoS

Puede modificar las directivas de QoS como se muestra a continuación.

```powershell
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

### <a name="remove-qos-policy"></a>Quitar Directiva de QoS

```powershell
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="dcb-configuration-on-network-adapters"></a>Configuración de DCB en adaptadores de red

La configuración de DCB en los adaptadores de red es independiente de la configuración de DCB en el nivel de sistema descrito anteriormente.

Independientemente de si DCB está instalado en Windows Server 2016, siempre puede ejecutar los siguientes comandos.

Si configura DCB desde un conmutador y confía en DCBX para propagar las configuraciones a los adaptadores de red, puede examinar qué configuraciones se reciben y se aplican a los adaptadores de red desde el sistema operativo después de habilitar DCB en los adaptadores de red.

###  <a name="enable-and-display-dcb-settings-on--network-adapters"></a><a name="bkmk_enabledcb"></a>Habilitar y mostrar la configuración de DCB en los adaptadores de red

```powershell
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

### <a name="disable-dcb-on-network-adapters"></a>Deshabilitar DCB en adaptadores de red

```powershell
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

## <a name="windows-powershell-commands-for-dcb"></a><a name="bkmk_wps"></a>Comandos de Windows PowerShell para DCB

Hay comandos de Windows PowerShell de DCB para Windows Server 2016 y Windows Server 2012 R2. Puede usar todos los comandos para Windows Server 2012 R2 en Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016 comandos de Windows PowerShell para DCB

En el siguiente tema de Windows Server 2016 se proporcionan las descripciones y la sintaxis de los cmdlets de Windows PowerShell para todos los \( \) \( \) cmdlets específicos de QoS Quality of Service de calidad de servicio \- . Enumera los cmdlets por orden alfabético según el verbo que aparece al principio del cmdlet.

- [Módulo DcbQoS](/powershell/module/dcbqos/)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 R2 comandos de Windows PowerShell para DCB

En el siguiente tema de Windows Server 2012 R2 se proporcionan las descripciones y la sintaxis de los cmdlets de Windows PowerShell para todos los \( \) \( \) cmdlets específicos de QoS Quality of Service de calidad de servicio \- . Enumera los cmdlets por orden alfabético según el verbo que aparece al principio del cmdlet.

- [Cmdlets de Calidad de servicio (QoS) del Protocolo de puente del centro de datos (DCB) en Windows PowerShell](/powershell/module/dcbqos/)
