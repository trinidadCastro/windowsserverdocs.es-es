---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: Reconocimiento de dominio de error
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: 439f898b7c96ecc3d2f380509fe86d528aa737c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361143"
---
# <a name="fault-domain-awareness"></a>Reconocimiento de dominio de error

> Se aplica a: Windows Server 2019 y Windows Server 2016

Los clústeres de conmutación por error permiten que varios servidores funcionen conjuntamente para proporcionar alta disponibilidad, o dicho de otro modo, para proporcionar tolerancia de errores de nodo. Pero las empresas de hoy día demandan una disponibilidad cada vez mayor de su infraestructura. Para lograr un tiempo de actividad similar a la nube, incluso en situaciones muy improbables como errores de chasis, interrupciones del bastidor o desastres naturales, deberá estar protegido. Este es el motivo por el que los clústeres de conmutación por error en Windows Server 2016 introdujeron también el chasis, el bastidor y la tolerancia a errores del sitio.

## <a name="fault-domain-awareness"></a>Reconocimiento de dominio de error

Los dominios de error y la tolerancia a errores son conceptos relacionados. Un dominio de error es un conjunto de componentes de hardware que comparten un único punto de error. Para ser tolerante a errores en un nivel determinado, se necesitan varios dominios de error en ese nivel. Por ejemplo, para ser tolerante a errores en bastidor, los servidores y los datos deben distribuirse entre varios bastidores.

Este breve vídeo presenta una introducción a los dominios de error en Windows Server 2016:  
[@no__t: 1Click esta imagen para ver información general de los dominios de error en Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

### <a name="fault-domain-awareness-in-windows-server-2019"></a>Reconocimiento de dominios de error en Windows Server 2019

El reconocimiento de dominios de error está disponible en Windows Server 2019, pero está deshabilitado de forma predeterminada y debe habilitarse a través del registro de Windows.

Para habilitar el reconocimiento de dominios de error en Windows Server 2019, vaya al registro de Windows y establezca el valor de (get-cluster). Clave del registro AutoAssignNodeSite en 1.

```Registry
    (Get-Cluster).AutoAssignNodeSite=1
```

Para deshabilitar el reconocimiento de dominios de error en Windows 2019, vaya al registro de Windows y establezca el valor de (get-cluster). Clave del registro AutoAssignNodeSite en 0.

```Registry
    (Get-Cluster).AutoAssignNodeSite=0
```

## <a name="benefits"></a>Ventajas
- **Los espacios de almacenamiento, incluido Espacios de almacenamiento directo, usan dominios de error para maximizar la seguridad de los datos.**  
    La resistencia en Espacios de almacenamiento es conceptualmente similar a RAID distribuido y definido por software. Varias copias de todos los datos se mantienen sincronizadas, y si se produce un error de hardware y una copia se pierde, se vuelven a copiar las otras para restaurar la resistencia. Para lograr la mejor resistencia posible, se deben mantener las copias en dominios de error independientes.

- **El [servicio de mantenimiento](health-service-overview.md) utiliza dominios de error para proporcionar alertas más útiles.**  
    Cada dominio de error se puede asociar con metadatos de ubicación, que se incluirán automáticamente en las alertas posteriores. Estos descriptores pueden ayudar al personal de mantenimiento o de operaciones y reducir los errores al eliminar la ambigüedad de hardware.  

- **La agrupación en clústeres de stretch utiliza dominios de error para la afinidad de almacenamiento.** El ajuste de los clústeres permite que servidores lejanos se unan a un clúster común. Para optimizar el rendimiento, las aplicaciones o máquinas virtuales debe ejecutarse en servidores que están cercanos a los que proporcionan el almacenamiento. El reconocimiento de dominios de error habilita esta afinidad de almacenamiento.   

## <a name="levels-of-fault-domains"></a>Niveles de dominios de error  
Hay cuatro niveles canónicos de dominios de error: sitio, bastidor, chasis y nodo. Los nodos se detectan automáticamente; cada nivel adicional es opcional. Por ejemplo, si su implementación no usa servidores blade, el nivel de chasis puede ser importante para usted.  

![Diagrama de los distintos niveles de dominios de error](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Uso  
Puede usar PowerShell o el marcado XML para especificar los dominios de error. Ambos enfoques son equivalentes y proporcionan una funcionalidad completa.

>[!IMPORTANT]
> Especifique los dominios de error antes de habilitar Espacios de almacenamiento directo, si es posible. Esto permite que la configuración automática prepare el grupo, niveles y configuración como la resistencia y el número de columnas, para la tolerancia a errores del bastidor o del chasis. Una vez creados el grupo y los volúmenes, los datos no se moverán con carácter retroactivo en respuesta a los cambios en la topología del dominio de error. Para mover los nodos entre chasis o bastidores después de habilitar Espacios de almacenamiento directo, primero debe expulsar el nodo y sus unidades del grupo mediante `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Definición de dominios de error con PowerShell
Windows Server 2016 presenta los siguientes cmdlets para trabajar con dominios de error:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Este breve vídeo muestra el uso de estos cmdlets.
[@no__t: 1Click esta imagen para ver un vídeo corto sobre el uso de los cmdlets de dominio de error de clúster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Use `Get-ClusterFaultDomain` para ver la topología de dominio de error actual. Esto mostrará todos los nodos del clúster, además de los chasis, bastidores o sitios que haya creado. Puede filtrarlos mediante parámetros como **-Type** o **-Name**, pero no son necesarios.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Use `New-ClusterFaultDomain` para crear nuevos chasis, bastidores o sitios. Los parámetros `-Type` y `-Name` son obligatorios. Los valores posibles para `-Type` son `Chassis`, `Rack` y `Site`. El `-Name` puede ser cualquier cadena. (En el caso de los dominios de error de tipo `Node`, el nombre debe ser el nombre de nodo real, tal como se estableció automáticamente).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server no puede y no comprueba que los dominios de error que cree se correspondan con nada del mundo físico real. (Esto puede parecer obvio, pero es importante entenderlo). Si, en el mundo físico, los nodos están en un bastidor, la creación de dos `-Type Rack` dominios de error en el software no proporciona mágicamente la tolerancia a errores del bastidor. Son responsables de garantizar que la topología que se crea con estos cmdlets coincide con la disposición real del hardware.

Use `Set-ClusterFaultDomain` para desplace un dominio de error a otro. Los términos "principal" y "secundario" se utilizan normalmente para describir esta relación de anidamiento. Los parámetros `-Name` y `-Parent` son obligatorios. En `-Name`, proporcione el nombre del dominio de error que se está moviendo; en `-Parent`, proporcione el nombre del destino. Para mover varios dominios de error a la vez, liste sus nombres.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Cuando se mueve los dominios de error, sus elementos secundarios se mueven con ellos. En el ejemplo anterior, si el bastidor A es el elemento principal de server01.contoso.com, este último no necesita moverse separadamente al sitio Shanghai: aún está allí en virtud de su elemento principal, igual que en el mundo físico.

Puede ver las relaciones de elementos primarios y secundarios en la salida de `Get-ClusterFaultDomain`, en las columnas `ParentName` y `ChildrenNames`.

También puede usar `Set-ClusterFaultDomain` para modificar algunas otras propiedades de los dominios de error. Por ejemplo, puede proporcionar metadatos opcionales `-Location` o `-Description` para cualquier dominio de error. Si se proporciona, esta información se incluirá en las alertas de hardware procedentes del servicio de mantenimiento. También puede cambiar el nombre de los dominios de error mediante el parámetro `-NewName`. No cambie el nombre de los dominios de error de tipo `Node`.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Use `Remove-ClusterFaultDomain` para quitar el chasis, los bastidores o los sitios que ha creado. El parámetro `-Name` es obligatorio. No se puede quitar un dominio de error que contenga elementos secundarios: en primer lugar, quite los elementos secundarios o muévalos fuera de mediante `Set-ClusterFaultDomain`. Para desplace un dominio de error fuera de todos los demás dominios de error, establezca su `-Parent` en la cadena vacía (""). No se pueden quitar los dominios de error de tipo `Node`. Para quitar varios dominios de error a la vez, liste sus nombres.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Definición de dominios de error con marcado XML
Los dominios de error se especifican mediante una sintaxis basada en XML. Se recomienda usar el editor de texto que prefiera, como Visual Studio Code (disponible gratuitamente *[aquí](https://code.visualstudio.com/)* ) o el Bloc de notas, para crear un documento XML que puede guardar y reutilizar.  

Este breve vídeo muestra el uso de marcado de XML para especificar los dominios de error.

[@no__t: 1Click esta imagen para ver un vídeo corto sobre cómo usar XML para especificar los dominios de error](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

En PowerShell, ejecute el siguiente cmdlet: `Get-ClusterFaultDomainXML`. Se devuelve la especificación del dominio de error actual para el clúster, como XML. Esto refleja todos los @no__t detectados, ajustados en etiquetas de apertura y cierre `<Topology>`.  

Ejecute lo siguiente para guardar esta salida en un archivo.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Abra el archivo y agregue las etiquetas `<Site>`, `<Rack>` y `<Chassis>` para especificar cómo se distribuyen estos nodos entre los sitios, los bastidores y el chasis. Cada etiqueta debe identificarse mediante un **nombre** único. Para los nodos, debe mantener el nombre del nodo tal como se muestra de forma predeterminada.  

> [!IMPORTANT]  
> Mientras que todas las etiquetas adicionales son opcionales, deben adherirse a la jerarquía transitiva de Sitio &gt; Bastidor &gt; Chasis &gt; Nodo y debe cerrarse correctamente.  
Además del nombre, los descriptores de forma libre `Location="..."` y `Description="..."` se pueden agregar a cualquier etiqueta.  

#### <a name="example-two-sites-one-rack-each"></a>Ejemplo: Dos sitios, un bastidor cada uno  

```XML
<Topology>  
  <Site Name="SEA" Location="Contoso HQ, 123 Example St, Room 4010, Seattle">  
    <Rack Name="A01" Location="Aisle A, Rack 01">  
      <Node Name="Server01" Location="Rack Unit 33" />  
      <Node Name="Server02" Location="Rack Unit 35" />  
      <Node Name="Server03" Location="Rack Unit 37" />  
    </Rack>  
  </Site>  
  <Site Name="NYC" Location="Regional Datacenter, 456 Example Ave, New York City">  
    <Rack Name="B07" Location="Aisle B, Rack 07">  
      <Node Name="Server04" Location="Rack Unit 20" />  
      <Node Name="Server05" Location="Rack Unit 22" />  
      <Node Name="Server06" Location="Rack Unit 24" />  
    </Rack>  
  </Site>  
</Topology> 
``` 

#### <a name="example-two-chassis-blade-servers"></a>Ejemplo: dos chasis, servidores blade  
```XML
<Topology>  
  <Rack Name="A01" Location="Contoso HQ, Room 4010, Aisle A, Rack 01">  
    <Chassis Name="Chassis01" Location="Rack Unit 2 (Upper)" >  
      <Node Name="Server01" Location="Left" />  
      <Node Name="Server02" Location="Right" />  
    </Chassis>  
    <Chassis Name="Chassis02" Location="Rack Unit 6 (Lower)" >  
      <Node Name="Server03" Location="Left" />  
      <Node Name="Server04" Location="Right" />  
    </Chassis>  
  </Rack>  
</Topology>  
```

Para establecer la nueva especificación de dominio de error, guarde el archivo XML y ejecute lo siguiente en PowerShell.  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

En esta guía se presentan solo dos ejemplos, pero las etiquetas `<Site>`, `<Rack>`, `<Chassis>` y `<Node>` se pueden mezclar y coincidir de muchas maneras adicionales para reflejar la topología física de la implementación, lo que pueda haber. Esperamos que estos ejemplos muestren la flexibilidad de estas etiquetas y el valor de descriptores de ubicación de forma libre para eliminar la ambigüedad entre ellos.  

### <a name="optional-location-and-description-metadata"></a>Opcional: Metadatos de ubicación y descripción

Puede proporcionar metadatos de **Ubicación** o **Descripción** opcionales para cualquier dominio de error. Si se proporciona, esta información se incluirá en las alertas de hardware procedentes del servicio de mantenimiento. En este breve vídeo se muestra el valor de agregar estos descriptores.

[![Click para ver un vídeo corto en el que se muestra el valor de agregar descriptores de ubicación a dominios de error](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Vea también  
- [Introducción a Windows Server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19)  
- [Introducción a Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics)  
-   [Información general de Espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md) 
