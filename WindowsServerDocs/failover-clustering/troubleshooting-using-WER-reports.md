---
title: Solución de problemas de un clúster de conmutación por error mediante el informe de errores de Windows
description: Solución de problemas de un clúster de conmutación por error mediante informes de WER, con detalles específicos sobre cómo recopilar los informes y diagnosticar problemas comunes.
keywords: Clúster de conmutación por error, los informes WER, diagnósticos, clúster, Windows Error Reporting
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.author: vpetter
ms.topic: article
author: vpetter
ms.date: 03/27/2018
ms.localizationpriority: ''
ms.openlocfilehash: 0b0c75f8e2d09a1fc17374428c48fb856465bb5a
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469537"
---
# <a name="troubleshooting-a-failover-cluster-using-windows-error-reporting"></a>Solución de problemas de un clúster de conmutación por error mediante el informe de errores de Windows 

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server

Informe de errores de Windows (WER) es una infraestructura de comentarios basado en eventos flexible diseñada para ayudar a los administradores avanzados o soporte técnico de nivel 3 para recopilar información acerca de los problemas de hardware y software que Windows pueden detectar, notificar la información a Microsoft, y proporcionar a los usuarios con ninguna solución disponible. Esto [referencia](https://docs.microsoft.com/powershell/module/windowserrorreporting/) proporciona descripciones y sintaxis de todos los cmdlets de WindowsErrorReporting.

La información sobre cómo solucionar problemas presentados a continuación le resultará útil para la solución de problemas avanzada que se han transferido y que pueden requerir datos que se enviarán a Microsoft para evaluar las prioridades.

## <a name="enabling-event-channels"></a>Habilitación de los canales de eventos

Cuando se instala Windows Server, muchos de los canales de eventos están habilitados de forma predeterminada. Pero en ocasiones, al diagnosticar un problema, necesitamos poder habilitar algunos de estos canales de eventos, ya que lo ayudará a evaluar las prioridades y diagnosticar problemas del sistema.

Puede habilitar los canales de eventos adicionales en cada nodo del servidor en el clúster según sea necesario; Sin embargo, este enfoque presenta dos problemas:

1. Tendrá que acordarse de habilitar los mismos canales de eventos en cada nodo de servidor nuevo que agregue al clúster.
2. Al diagnosticar, puede ser tedioso habilitar los canales de eventos específicos, reproduzca el error y repita este proceso hasta que la causa raíz.

Para evitar estos problemas, puede permitir que los canales de eventos en el inicio de clúster. La lista de los canales de eventos habilitados en el clúster se puede configurar mediante la propiedad pública **EnabledEventLogs**. De forma predeterminada, se habilitan los siguientes canales de eventos:

```powershell
PS C:\Windows\system32> (get-cluster).EnabledEventLogs
```

A continuación presentamos un ejemplo de la salida:
```
Microsoft-Windows-Hyper-V-VmSwitch-Diagnostic,4,0xFFFFFFFD
Microsoft-Windows-SMBDirect/Debug,4
Microsoft-Windows-SMBServer/Analytic
Microsoft-Windows-Kernel-LiveDump/Analytic
```

El **EnabledEventLogs** propiedad es un elemento multistring, donde cada cadena tiene el formato: **máscara de palabra clave de nombre de canal, el nivel de registro,** . El **máscara de palabra clave** puede ser un valor hexadecimal (prefijo 0 x), octal (prefijo 0) o número de número decimal (sin prefijo). Por ejemplo, para agregar un nuevo canal de eventos a la lista y para configurar ambos **nivel de registro** y **máscara de palabra clave** puede ejecutar:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,321"
```

Si desea establecer el **nivel de registro** pero mantener la **máscara de palabra clave** en su valor predeterminado, puede usar cualquiera de los comandos siguientes:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,"
```

Si desea mantener el **nivel de registro** en su valor predeterminado, pero establezca la **máscara de palabra clave** puede ejecutar el comando siguiente:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,0xf1"
```

Si desea conservar ambas versiones del **nivel de registro** y **máscara de palabra clave** en sus valores predeterminados, puede ejecutar cualquiera de los siguientes comandos:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,"
```

Estos canales de eventos se habilitará en cada nodo del clúster cuando se inicia el servicio de clúster o siempre que el **EnabledEventLogs** se cambia la propiedad.

## <a name="gathering-logs"></a>Recopilando registros

Después de haber habilitado los canales de eventos, puede usar el **DumpLogQuery** para recopilar registros. La propiedad de tipo de recurso público **DumpLogQuery** es un valor mutistring. Cada cadena es un [consulta XPATH como se describe aquí](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85).aspx).

Para solucionar el problema, si tiene que recopilar los canales de eventos adicionales, puede una modificación la **DumpLogQuery** propiedad agregando consultas adicionales o modificar la lista.

Para ello, primero pruebe la consulta XPATH con el [get-WinEvent](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Diagnostics/Get-WinEvent?view=powershell-5.1) cmdlet de PowerShell:

```powershell
get-WinEvent -FilterXML "<QueryList><Query><Select Path='Microsoft-Windows-GroupPolicy/Operational'>*[System[TimeCreated[timediff(@SystemTime) &gt;= 600000]]]</Select></Query></QueryList>"
```

A continuación, anexe la consulta para el **DumpLogQuery** propiedad del recurso:

```powershell
(Get-ClusterResourceType -Name "Physical Disk".DumpLogQuery += "<QueryList><Query><Select Path='Microsoft-Windows-GroupPolicy/Operational'>*[System[TimeCreated[timediff(@SystemTime) &gt;= 600000]]]</Select></Query></QueryList>"
```

Y si desea obtener una lista de las consultas para usar, ejecute:
```powershell
(Get-ClusterResourceType -Name "Physical Disk").DumpLogQuery
```

## <a name="gathering-windows-error-reporting-reports"></a>Obtener informes de informe de errores de Windows

Informes de Error de Windows se almacenan en **%ProgramData%\Microsoft\Windows\WER**

Dentro de la **WER** carpeta, el **ReportsQueue** carpeta contiene informes que están esperando para cargarse en Watson.

```powershell
PS C:\Windows\system32> dir c:\ProgramData\Microsoft\Windows\WER\ReportQueue
```

A continuación presentamos un ejemplo de la salida:
```
Volume in drive C is INSTALLTO
Volume Serial Number is 4031-E397

Directory of C:\ProgramData\Microsoft\Windows\WER\ReportQueue

<date>  <time>    <DIR>          .
<date>  <time>    <DIR>          ..
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_02d10a3f
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_0588dd06
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_10d55ef5
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_13258c8c
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_13a8c4ac
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_13dcf4d3
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_1721a0b0
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_1839758a
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_1d4131cb
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_23551d79
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_2468ad4c
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_255d4d61
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_cab_08289734
<date>  <time>    <DIR>          Critical_Physical Disk_64acaf7e4590828ae8a3ac3c8b31da9a789586d4_00000000_cab_1d94712e
<date>  <time>    <DIR>          Critical_Physical Disk_ae39f5243a104f21ac5b04a39efeac4c126754_00000000_003359cb
<date>  <time>    <DIR>          Critical_Physical Disk_ae39f5243a104f21ac5b04a39efeac4c126754_00000000_cab_1b293b17
<date>  <time>    <DIR>          Critical_Physical Disk_b46b8883d892cfa8a26263afca228b17df8133d_00000000_cab_08abc39c
<date>  <time>    <DIR>          Kernel_166_1234dacd2d1a219a3696b6e64a736408fc785cc_00000000_cab_19c8a127
               0 File(s)              0 bytes
              20 Dir(s)  23,291,658,240 bytes free
```

Dentro de la **WER** carpeta, el **ReportsArchive** carpeta contiene los informes que ya se han cargado a Watson. Se eliminan datos en estos informes, pero el **Report.wer** archivo continúa.

```powershell
PS C:\Windows\system32> dir C:\ProgramData\Microsoft\Windows\WER\ReportArchive
```

A continuación presentamos un ejemplo de la salida:
```
Volume in drive C is INSTALLTO
Volume Serial Number is 4031-E397

Directory of c:\ProgramData\Microsoft\Windows\WER\ReportArchive

<date>  <time>    <DIR>          .
<date>  <time>    <DIR>          ..
<date>  <time>    <DIR>          Critical_powershell.exe_7dd54f49935ce48b2dd99d1c64df29a5cfb73db_00000000_cab_096cc802
               0 File(s)              0 bytes
               3 Dir(s)  23,291,658,240 bytes free

```

Informe de errores de Windows proporciona muchas opciones para personalizar la experiencia de informes de problemas. Para obtener más información, consulte el informe de errores de Windows [documentación](https://msdn.microsoft.com/library/windows/desktop/bb513638(v=vs.85).aspx).


## <a name="troubleshooting-using-windows-error-reporting-reports"></a>Solución de problemas con informes de Windows Error Reporting

### <a name="physical-disk-failed-to-come-online"></a>Disco físico no se pudo poner en línea

Para diagnosticar este problema, vaya a la carpeta de informes WER:

```powershell
PS C:\Windows\system32> dir C:\ProgramData\Microsoft\Windows\WER\ReportArchive\Critical_PhysicalDisk_b46b8883d892cfa8a26263afca228b17df8133d_00000000_cab_08abc39c
```

A continuación presentamos un ejemplo de la salida:
```
Volume in drive C is INSTALLTO
Volume Serial Number is 4031-E397

<date>  <time>    <DIR>          .
<date>  <time>    <DIR>          ..
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_1.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_10.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_11.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_12.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_13.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_14.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_15.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_16.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_17.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_18.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_19.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_2.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_20.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_21.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_22.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_23.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_24.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_25.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_26.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_27.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_28.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_29.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_3.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_30.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_31.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_32.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_33.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_34.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_35.evtx
<date>  <time>         2,166,784 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_36.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_37.evtx
<date>  <time>            33,194 Report.wer
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_38.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_39.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_4.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_40.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_41.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_5.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_6.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_7.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_8.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_9.evtx
<date>  <time>             7,382 WERC263.tmp.WERInternalMetadata.xml
<date>  <time>            59,202 WERC36D.tmp.csv
<date>  <time>            13,340 WERC38D.tmp.txt
```

A continuación, inicie la clasificación de la **Report.wer** archivo: esto indicará dónde está el error.

```
EventType=Failover_clustering_resource_error 
<skip>
Sig[0].Name=ResourceType
Sig[0].Value=Physical Disk
Sig[1].Name=CallType
Sig[1].Value=ONLINERESOURCE
Sig[2].Name=RHSCallResult
Sig[2].Value=5018
Sig[3].Name=ApplicationCallResult
Sig[3].Value=999
Sig[4].Name=DumpPolicy
Sig[4].Value=5225058577
DynamicSig[1].Name=OS Version
DynamicSig[1].Value=10.0.17051.2.0.0.400.8
DynamicSig[2].Name=Locale ID
DynamicSig[2].Value=1033
DynamicSig[27].Name=ResourceName
DynamicSig[27].Value=Cluster Disk 10
DynamicSig[28].Name=ReportId
DynamicSig[28].Value=8d06c544-47a4-4396-96ec-af644f45c70a
DynamicSig[29].Name=FailureTime
DynamicSig[29].Value=2017//12//12-22:38:05.485
```

Puesto que no se pudo poner en línea el recurso, se han recopilado ningún volcados de memoria, pero el informe del informe de errores de Windows recopilar registros. Si abre todos **.evtx** archivos utilizando Microsoft Message Analyzer, verá toda la información que se recopilaron con las siguientes consultas a través del canal del sistema, canal de la aplicación, los canales de diagnóstico de clúster de conmutación por error y algunos otros canales genéricos.

```powershell
PS C:\Windows\system32> (Get-ClusterResourceType -Name "Physical Disk").DumpLogQuery
```

A continuación presentamos un ejemplo de la salida:
```
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Kernel-PnP/Configuration">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-ReFS/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Ntfs/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Ntfs/WHC">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Storport/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Storport/Health">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Storport/Admin">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-ClassPnP/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-ClassPnP/Admin">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-ScmBus/Certification">*[System[TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-ScmBus/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-PmemDisk/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-NvdimmN/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-INvdimm/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-VirtualNvdimm/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Disk/Admin">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Disk/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-ScmDisk0101/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Partition/Diagnostic">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Volume/Diagnostic">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-VolumeSnapshot-Driver/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-FailoverClustering-Clusport/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-FailoverClustering-ClusBflt/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-StorageSpaces-Driver/Diagnostic">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-StorageManagement/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-StorageSpaces-Driver/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Tiering/Admin">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Hyper-V-VmSwitch-Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Hyper-V-VmSwitch-Diagnostic">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
```

Analizador de mensajes le permite capturar, mostrar y analizar el tráfico de mensajería de protocolo. También le permite seguimiento y evaluar los eventos del sistema y otros mensajes de componentes de Windows. Puede descargar [Microsoft Message Analyzer desde aquí](https://www.microsoft.com/download/details.aspx?id=44226). Cuando se cargan los registros en el analizador de mensajes, verá los siguientes proveedores y los mensajes de los canales de registro.

![Cargar registros en el analizador de mensajes](media/troubleshooting-using-WER-reports/loading-logs-into-message-analyzer.png)

También puede agrupar por los proveedores para obtener la vista siguiente:

![Registros agrupados por los proveedores](media/troubleshooting-using-WER-reports/logs-grouped-by-providers.png)

Para identificar el motivo por el error en el disco, vaya a los eventos en **FailoverClustering y diagnóstico** y **FailoverClustering/DiagnosticVerbose**. A continuación, ejecute la consulta siguiente: **EventLog.EventData["LogString"] contiene el "Disco de clúster de 10"** .  Esto le facilitará asigne a la salida siguiente:

![Resultado de consulta de registro de ejecución](media/troubleshooting-using-WER-reports/output-of-running-log-query.png)


### <a name="physical-disk-timed-out"></a>Tiempo de espera de disco físico

Para diagnosticar este problema, vaya a la carpeta de informes WER. La carpeta contiene archivos de registro y archivos de volcado para **RHS**, **clussvc.exe**y del proceso que hospeda el "**smphost**" del servicio, tal como se muestra a continuación:

```powershell
PS C:\Windows\system32> dir C:\ProgramData\Microsoft\Windows\WER\ReportArchive\Critical_PhysicalDisk_64acaf7e4590828ae8a3ac3c8b31da9a789586d4_00000000_cab_1d94712e
```

A continuación presentamos un ejemplo de la salida:
```
Volume in drive C is INSTALLTO
Volume Serial Number is 4031-E397

<date>  <time>    <DIR>          .
<date>  <time>    <DIR>          ..
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_1.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_10.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_11.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_12.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_13.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_14.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_15.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_16.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_17.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_18.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_19.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_2.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_20.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_21.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_22.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_23.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_24.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_25.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_26.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_27.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_28.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_29.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_3.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_30.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_31.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_32.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_33.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_34.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_35.evtx
<date>  <time>         2,166,784 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_36.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_37.evtx
<date>  <time>        28,340,500 memory.hdmp
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_38.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_39.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_4.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_40.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_41.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_5.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_6.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_7.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_8.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_9.evtx
<date>  <time>         4,466,943 minidump.0f14.mdmp
<date>  <time>         1,735,776 minidump.2200.mdmp
<date>  <time>            33,890 Report.wer
<date>  <time>            49,267 WER69FA.tmp.mdmp
<date>  <time>             5,706 WER70A2.tmp.WERInternalMetadata.xml
<date>  <time>            63,206 WER70E0.tmp.csv
<date>  <time>            13,340 WER7100.tmp.txt
```

A continuación, inicie la clasificación de la **Report.wer** archivo: esto indicará qué llamada o el recurso se bloquea.

```
EventType=Failover_clustering_resource_timeout_2
<skip>
Sig[0].Name=ResourceType
Sig[0].Value=Physical Disk
Sig[1].Name=CallType
Sig[1].Value=ONLINERESOURCE
Sig[2].Name=DumpPolicy
Sig[2].Value=5225058577
Sig[3].Name=ControlCode
Sig[3].Value=18
DynamicSig[1].Name=OS Version
DynamicSig[1].Value=10.0.17051.2.0.0.400.8
DynamicSig[2].Name=Locale ID
DynamicSig[2].Value=1033
DynamicSig[26].Name=ResourceName
DynamicSig[26].Value=Cluster Disk 10
DynamicSig[27].Name=ReportId
DynamicSig[27].Value=75e60318-50c9-41e4-94d9-fb0f589cd224
DynamicSig[29].Name=HangThreadId
DynamicSig[29].Value=10008
```

La lista de servicios y procesos que se recopilan en un volcado de memoria se controla mediante la propiedad siguiente: **PS C:\Windows\system32> (Get-ClusterResourceType -Name "Physical Disk").DumpServicesSmphost**

Para determinar por qué ha ocurrido la falta de respuesta, abra los archivos de disco. A continuación, ejecute la consulta siguiente: **EventLog.EventData["LogString"] contiene el "Disco de clúster de 10"** esto dará asigne a la salida siguiente:

![Resultado de consulta de registro de ejecución 2](media/troubleshooting-using-WER-reports/output-of-running-log-query-2.png)

Esto nos podemos cross-examine con el subproceso de la **memory.hdmp** archivo:

```
# 21  Id: 1d98.2718 Suspend: 0 Teb: 0000000b`f1f7b000 Unfrozen
# Child-SP          RetAddr           Call Site
00 0000000b`f3c7ec38 00007ff8`455d25ca ntdll!ZwDelayExecution+0x14 
01 0000000b`f3c7ec40 00007ff8`2ef19710 KERNELBASE!SleepEx+0x9a 
02 0000000b`f3c7ece0 00007ff8`3bdf7fbf clusres!ResHardDiskOnlineOrTurnOffMMThread+0x2b0 
03 0000000b`f3c7f960 00007ff8`391eed34 resutils!ClusWorkerStart+0x5f 
04 0000000b`f3c7f9d0 00000000`00000000 vfbasics+0xed34
```