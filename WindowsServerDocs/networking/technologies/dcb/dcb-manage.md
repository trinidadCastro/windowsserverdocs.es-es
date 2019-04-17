---
title: Administrar el centro de datos puente (DCB)
description: En este tema proporciona instrucciones sobre cómo usar los comandos de Windows PowerShell para administrar el puente de centro de datos en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbe9e5e4af2ddd834b5b8f38e9ffd1b403e92793
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="manage-data-center-bridging-dcb"></a>Administrar el centro de datos puente (DCB)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema proporciona instrucciones sobre cómo usar los comandos de Windows PowerShell para configurar \(DCB\) puente de centro de datos en un adaptador de red compatibles con DCB\ que está instalado en un equipo que ejecute Windows Server 2016 o Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Instalar DCB en Windows Server 2016 o Windows 10

Para obtener información sobre los requisitos previos para usar y cómo instalar DCB, consulta [instalar datos centro puente (DCB) en Windows Server 2016 o Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configuraciones de DCB 

Antes de Windows Server 2016, toda la configuración DCB aplicó universalmente a todos los adaptadores de red que admiten DCB. 

En Windows Server 2016, puedes aplicar configuraciones DCB a la tienda de la directiva Global o a Store\(s\) de directiva individuales. Cuando se aplican directivas individuales reemplaza toda la configuración de directiva Global.

Las configuraciones de asignación de prioridad de tráfico clase, PFC y la aplicación en el nivel del sistema no se aplica en adaptadores de red hasta que hagas lo siguiente.

1. Activa el bit dispuestos DCBX a "false"

2. Habilitar DCB en los adaptadores de red. Consulta [habilitar y mostrar la configuración de DCB en adaptadores de red](#bkmk_enabledcb).

>[!NOTE]
>Si quieres configurar DCB desde un conmutador a través de DCBX, consulta [configuración DCBX](#BKMK_DCBX_Settings)

El bit dispuestos DCBX se describe en la especificación de DCB. Si el bit dispuesto en un dispositivo se establece en true, el dispositivo está dispuesto a aceptar las configuraciones de un dispositivo remoto a través de DCBX. Si el bit dispuesto en un dispositivo se establece en "false", el dispositivo rechazar todos los intentos de configuración de dispositivos remotos y aplicar solo a la configuración local.

Si DCB no está instalado en Windows Server 2016 el valor del bit dispuesto es irrelevante en cuanto el sistema operativo es porque el sistema operativo no tiene ninguna configuración local se aplican a los adaptadores de red. Después de instala DCB, el valor predeterminado del bit dispuesto es true. Este diseño permite adaptadores de red mantener cualquier configuraciones que haya recibido de sus colegas remotos.

Si un adaptador de red no admite DCBX, nunca recibirá configuraciones desde un dispositivo remoto. Recibe las configuraciones del sistema operativo, pero solo después el DCBX dispuestos bits se establece en false.

## <a name="set-the-willing-bit"></a>Establecer el bit dispuesto

Para aplicar configuraciones del sistema operativo de clase de tráfico, PFC y asignación de prioridad de aplicaciones en adaptadores de red, o simplemente invalidar la configuración de dispositivos remotos \: si hay alguna \, puedes ejecutar el comando siguiente.

>[!NOTE]
>Los nombres de comandos de Windows PowerShell de DCB incluyen "QoS" en lugar de "DCB" en la cadena de nombre. Esto es porque QoS y DCB están integradas en Windows Server 2016 para proporcionar una experiencia de administración de QoS sin problemas.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Para mostrar el estado del bit dispuesto configuración, puedes usar el siguiente comando:

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configuración de DCB en adaptadores de red

Habilitar DCB en un adaptador de red permite ver la configuración que se propagan desde un conmutador para el adaptador de red.

Las configuraciones de DCB incluyen los siguientes pasos.

1.  Configurar DCB en el nivel de sistema, que incluye:

    Un archivo. Administración de la clase de tráfico
    
    b. Configuración de Control (PFC) del flujo de prioridad
    
    c. Asignación de prioridad de aplicaciones
    
    d. Configuración de DCBX

2. Configurar DCB en el adaptador de red.



##  <a name="dcb-traffic-class-management"></a>Administración de la clase de tráfico DCB

Siguientes son ejemplos de comandos de Windows PowerShell para la administración de la clase de tráfico.

### <a name="create-a-traffic-class"></a>Crear una clase de tráfico

Puedes usar la **nueva NetQosTrafficClass** comando para crear una clase de tráfico.

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

De manera predeterminada, todos los valores de 802.1X p se asignan a una clase de tráfico predeterminado, que es 100% del ancho de banda de vínculo físico. La **nueva NetQosTrafficClass** comando crea una nueva clase de tráfico, se asigna a qué cualquier paquete que se etiqueta con prioridad p 802.1X valor 4. El algoritmo de selección de transmisión de \(TSA\) tiene NET y 30% del ancho de banda.

Puedes crear hasta 7 nuevas clases de tráfico. Como la clase de tráfico de forma predeterminada, puede haber al menos 8 clases de tráfico en el sistema. Sin embargo, un adaptador de red compatibles con DCB podría no admitir el tráfico que muchas clases en el hardware. Si creas más clases de tráfico que se pueden aplicar en un adaptador de red y habilitar DCB en ese adaptador de red, el controlador de minipuerto notifica un error al sistema operativo. El error se registra en el registro de eventos.

La suma de las reservas de ancho de banda para todas las clases de tráfico creado no puede exceder 99% del ancho de banda. La clase de tráfico predeterminado siempre tiene al menos un 1% del ancho de banda reservado por sí mismos.

### <a name="display-traffic-classes"></a>Mostrar clases de tráfico

Puedes usar la **Get NetQosTrafficClass** comando para ver las clases de tráfico.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modificar una clase de tráfico

Puedes usar la **conjunto NetQosTrafficClass** comando para crear una clase de tráfico. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

Puedes usar la **Get NetQosTrafficClass** comando para ver la configuración.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Después de crear una clase de tráfico, puedes cambiar la configuración de forma independiente. La configuración que se puede cambiar incluye:

1. Ancho de banda asignación \(-BandwidthPercentage\)

2. TSA (\-Algorithm\)

3. Prioridad asignación \(-Priority\)

### <a name="remove-a-traffic-class"></a>Quitar una clase de tráfico

Puedes usar la **quitar NetQosTrafficClass** comando para eliminar una clase de tráfico.

>[!IMPORTANT]
>No se puede quitar la clase de tráfico de forma predeterminada.


    Remove-NetQosTrafficClass -Name SMB

Puedes usar la **Get NetQosTrafficClass** comando para ver la configuración.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Después de quitar una clase de tráfico, el valor de 802.1X p asignado para que la clase de tráfico se reasigna a la clase de tráfico de forma predeterminada. Cualquier ancho de banda que se ha reservado para una clase de tráfico se devuelve a la asignación de clase de tráfico predeterminada cuando se quita la clase de tráfico.

## <a name="per-network-interface-policies"></a>Directivas de la interfaz de red

Todos los ejemplos anteriores de establecen directivas Global. Siguientes son ejemplos de cómo puede establecer y obtener las directivas por NIC. 

El campo "PolicySet" cambia de Global a AdapterSpecific. Cuando se muestran las directivas de AdapterSpecific, también se muestran los \(ifIndex\) índice de la interfaz y el nombre de la interfaz \(ifAlias\).

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

A continuación se muestran ejemplos de comandos de configuración de Control de flujo de prioridad. Estas opciones de configuración también se pueden especificar para adaptadores individuales.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Habilitar y Control de flujo de prioridad de pantalla para globales e interfaz específicos de casos de uso

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


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Deshabilitar el Control de flujo de la prioridad (Global y específicos de la interfaz)

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

##  <a name="application-priority-assignment"></a>Asignación de prioridad de aplicaciones

Siguientes son ejemplos de asignación de prioridad.

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

El comando anterior, crea una nueva directiva de SMB. – SMB es un filtro de bandeja de entrada que coincida con el puerto TCP 445 (reservado para SMB). Si se envía un paquete para el puerto TCP 445 se etiquetarán por el sistema operativo con valor p 802.1X de 4 antes de que el paquete se pasa a un controlador de minipuerto de red.

Además – SMB, otros filtros predeterminados incluyen: iSCSI (coincidente el puerto TCP 3260), - NFS (coincidente el puerto TCP 2049), - LiveMigration (coincidente el puerto TCP 6600), - FCOE (coincidencia EtherType 0x8906) y – NetworkDirect.

NetworkDirect es un nivel abstracto que creamos sobre cualquier implementación RDMA en un adaptador de red. – NetworkDirect debe ir seguida de un puerto de red directa.

Además de los filtros de forma predeterminada, puedes clasificar tráfico por nombre del archivo ejecutable de la aplicación (como en el primer ejemplo), o por la dirección IP, puerto o protocolo (como se muestra en el segundo ejemplo):

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

Puedes modificar las directivas de QoS tal como se muestra a continuación.


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

### <a name="remove-qos-policy"></a>Quitar la directiva de QoS

```
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y  

```

## <a name="dcb-configuration-on-network-adapters"></a>Configuración de DCB en adaptadores de red

Configuración DCB en adaptadores de red es independiente de la configuración de DCB en el nivel de sistema que se describió anteriormente. 

Independientemente de si DCB está instalado en Windows Server 2016, siempre puede ejecutar los siguientes comandos. 

Si configura DCB desde un conmutador y se basan en DCBX para propagar las configuraciones para adaptadores de red, puedes examinar qué configuraciones se reciben y se aplica en los adaptadores de red desde el lado del sistema operativo después de habilitar DCB en los adaptadores de red.

###  <a name="bkmk_enabledcb"></a>Habilitar y mostrar la configuración de DCB en adaptadores de red

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

### <a name="disable-dcb-on-network-adapters"></a>Deshabilitar DCB en adaptadores de red

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

Hay comandos DCB Windows PowerShell para Windows Server 2016 y Windows Server 2012 R2. Puedes usar todos los comandos de Windows Server 2012 R2 en Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandos de PowerShell de Windows de Windows Server 2016 para DCB

El siguiente tema para Windows Server 2016 proporciona descripciones de cmdlet de Windows PowerShell y la sintaxis para todos los \(DCB\) puente de centro de datos calidad de servicio \ (QoS\) \-specific cmdlets. Enumera los cmdlets en orden alfabético según el verbo al principio del cmdlet.

- [Módulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandos de Windows Server 2012 R2 Windows PowerShell para DCB

El siguiente tema para Windows Server 2012 R2 proporciona descripciones de cmdlet de Windows PowerShell y la sintaxis para todos los \(DCB\) puente de centro de datos calidad de servicio \ (QoS\) \-specific cmdlets. Enumera los cmdlets en orden alfabético según el verbo al principio del cmdlet.

- [Centro de datos de calidad (DCB) de los Cmdlets de servicio (QoS) en Windows PowerShell de puente](https://technet.microsoft.com/library/hh967440.aspx)
