---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: Reconocimiento de dominio de error
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: f5c64bb8f8b7d4b8d13c76c4e94cfcf52ee32c30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821476"
---
# <a name="fault-domain-awareness-in-windows-server-2016"></a>Conocimiento de dominio de error en Windows Server 2016

> Se aplica a: Windows Server 2016

Los clústeres de conmutación por error permiten que varios servidores funcionen conjuntamente para proporcionar alta disponibilidad, o dicho de otro modo, para proporcionar tolerancia de errores de nodo. Pero las empresas de hoy demandan cada vez mayor disponibilidad de su infraestructura. Para lograr un tiempo de actividad similar a la nube, incluso en situaciones muy improbables como errores de chasis, interrupciones del bastidor o desastres naturales, deberá estar protegido. Por este motivo los clústeres de conmutación por error en Windows Server 2016 presenta chasis, bastidor y sitio también tolerancia a errores.

Los dominios de error y la tolerancia a errores son conceptos relacionados. Un dominio de error es un conjunto de componentes de hardware que comparten un único punto de error. Para ser tolerante a errores en un nivel determinado, se necesitan varios dominios de error en ese nivel. Por ejemplo, para ser tolerante a errores en bastidor, los servidores y los datos deben distribuirse entre varios bastidores.

En este breve vídeo se presenta información general de dominios de error en Windows Server 2016:  
[![Haga clic en esta imagen para ver una descripción general de los dominios de error en Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

## <a name="benefits"></a>Ventajas
- **Espacios de almacenamiento, incluidos espacios de almacenamiento directo, utiliza dominios de error para maximizar la seguridad de los datos.**  
    La resistencia en Espacios de almacenamiento es conceptualmente similar a RAID distribuido y definido por software. Varias copias de todos los datos se mantienen sincronizadas, y si se produce un error de hardware y una copia se pierde, se vuelven a copiar las otras para restaurar la resistencia. Para lograr la mejor resistencia posible, se deben mantener las copias en dominios de error independientes.

- **El [servicio de mantenimiento](health-service-overview.md) usa dominios para proporcionar alertas más útiles de error.**  
    Cada dominio de error se puede asociar con metadatos de ubicación, que se incluirán automáticamente en las alertas posteriores. Estos descriptores pueden ayudar al personal de mantenimiento o de operaciones y reducir los errores al eliminar la ambigüedad de hardware.  

- **Ajuste de los clústeres utiliza dominios de error para la afinidad de almacenamiento.** El ajuste de los clústeres permite que servidores lejanos se unan a un clúster común. Para optimizar el rendimiento, las aplicaciones o máquinas virtuales debe ejecutarse en servidores que están cercanos a los que proporcionan el almacenamiento. Conocimiento de dominio de error permite esta afinidad de almacenamiento.   

## <a name="levels-of-fault-domains"></a>Niveles de dominios de error  
Hay cuatro niveles canónicos de dominios de error: sitio, bastidor, chasis y nodo. Los nodos se detectan automáticamente; cada nivel adicional es opcional. Por ejemplo, si su implementación no usa servidores blade, el nivel de chasis puede ser importante para usted.  

![Diagrama de los distintos niveles de dominios de error](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Uso  
Puede usar PowerShell o XML para especificar los dominios de error. Ambos enfoques son equivalentes y proporcionan una funcionalidad completa.

>[!IMPORTANT]
> Especifique los dominios de error antes de habilitar Espacios de almacenamiento directo, si es posible. Esto permite que la configuración automática prepare el grupo, niveles y configuración como la resistencia y el número de columnas, para la tolerancia a errores del bastidor o del chasis. Una vez creados el grupo y los volúmenes, los datos no se moverán con carácter retroactivo en respuesta a los cambios en la topología del dominio de error. Para mover los nodos entre chasis o bastidores después de habilitar Espacios de almacenamiento directo, primero debe expulsar el nodo y sus unidades del grupo mediante `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Definir dominios de error con PowerShell
Windows Server 2016 presenta los siguientes cmdlets para trabajar con dominios de error:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Este breve vídeo muestra el uso de estos cmdlets.
[![Haga clic en esta imagen para ver un vídeo corto sobre el uso de los cmdlets de dominio de error de clúster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Use `Get-ClusterFaultDomain` para ver la topología de dominio de error actual. Esto mostrará todos los nodos del clúster, además de los chasis, bastidores o sitios que haya creado. Puede filtrarlos mediante parámetros como **-Type** o **-Name**, pero no son necesarios.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Use `New-ClusterFaultDomain` para crear nuevos chasis, bastidores o sitios. El `-Type` y `-Name` parámetros son obligatorios. Los valores posibles de `-Type` son `Chassis`, `Rack`, y `Site`. El `-Name` puede ser cualquier cadena. (Para `Node` dominios de error de tipo, el nombre deben ser el nombre del nodo real, como se establece automáticamente).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server no puede y no comprueba que los dominios de error que cree se corresponden con cualquier cosa del mundo real, físico. (Esto puede parecer obvio, pero es importante comprender). Si, en el mundo físico, los nodos están en un bastidor, la creación de dos `-Type Rack` dominios de error en el software no proporciona mágicamente la tolerancia a errores del bastidor. Son responsables de garantizar que la topología que se crea con estos cmdlets coincide con la disposición real del hardware.

Use `Set-ClusterFaultDomain` para mover un dominio de error a otro. Los términos "principal" y "secundario" se utilizan normalmente para describir esta relación de anidamiento. El `-Name` y `-Parent` parámetros son obligatorios. En `-Name`, proporcione el nombre del dominio de error que está moviendo; en `-Parent`, proporcione el nombre del destino. Para mover varios dominios de error a la vez, liste sus nombres.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Cuando se mueve los dominios de error, sus elementos secundarios se mueven con ellos. En el ejemplo anterior, si el bastidor A es el elemento principal de server01.contoso.com, este último no necesita moverse separadamente al sitio Shanghai: aún está allí en virtud de su elemento principal, igual que en el mundo físico.

Puede ver las relaciones de elementos primarios y secundarios en la salida de `Get-ClusterFaultDomain`, en el `ParentName` y `ChildrenNames` columnas.

También puede usar `Set-ClusterFaultDomain` para modificar determinadas propiedades de otros dominios de error. Por ejemplo, puede proporcionar opcional `-Location` o `-Description` los metadatos de cualquier dominio de error. Si se proporciona, esta información se incluirá en las alertas de hardware procedentes del servicio de mantenimiento. También puede cambiar el nombre de dominios de error mediante el `-NewName` parámetro. No cambie el nombre `Node` escriba dominios de error.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Use `Remove-ClusterFaultDomain` para quitar el chasis, bastidores o sitios que ha creado. El parámetro `-Name` es obligatorio. No se puede quitar un dominio de error que contiene elementos secundarios: en primer lugar, quite los elementos secundarios o muévalos fuera mediante `Set-ClusterFaultDomain`. Para mover un dominio de error fuera de todos los demás dominios de error, establezca su `-Parent` en la cadena vacía (""). No puede quitar `Node` escriba dominios de error. Para quitar varios dominios de error a la vez, liste sus nombres.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Definición de dominios de error con marcado XML
Los dominios de error se especifican mediante una sintaxis basada en XML. Se recomienda usar el editor de texto que prefiera, como Visual Studio Code (disponible gratuitamente *[aquí](https://code.visualstudio.com/)*) o el Bloc de notas, para crear un documento XML que puede guardar y reutilizar.  

Este breve vídeo muestra el uso de marcado de XML para especificar los dominios de error.

[![Haga clic en esta imagen para ver un vídeo corto sobre cómo usar XML para especificar los dominios de error](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

En PowerShell, ejecute el siguiente cmdlet: `Get-ClusterFaultDomainXML`. Se devuelve la especificación del dominio de error actual para el clúster, como XML. Esto refleja cada detectados `<Node>`, rodeado de apertura y cierre `<Topology>` etiquetas.  

Ejecute lo siguiente para guardar esta salida en un archivo.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Abra el archivo y agregue `<Site>`, `<Rack>`, y `<Chassis>` etiquetas para especificar cómo se distribuyen estos nodos entre sitios, bastidores y chasis. Cada etiqueta debe identificarse mediante un **nombre** único. Para los nodos, debe mantener el nombre del nodo tal como se muestra de forma predeterminada.  

> [!IMPORTANT]  
> Mientras que todas las etiquetas adicionales son opcionales, deben adherirse a la jerarquía transitiva de Sitio &gt; Bastidor &gt; Chasis &gt; Nodo y debe cerrarse correctamente.  
Además del nombre, de forma libre `Location="..."` y `Description="..."` descriptores pueden agregarse a cualquier etiqueta.  

#### <a name="example-two-sites-one-rack-each"></a>Por ejemplo: Dos sitios, una estantería cada uno  

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

Para establecer la nueva especificación de dominio de error, guarde el código XML y ejecute el siguiente comando en PowerShell.  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

Esta guía presenta solo dos ejemplos, pero la `<Site>`, `<Rack>`, `<Chassis>`, y `<Node>` etiquetas pueden mezclar y combinar de muchas otras formas para reflejar la topología física de la implementación, cualquiera que sea. Esperamos que estos ejemplos muestren la flexibilidad de estas etiquetas y el valor de descriptores de ubicación de forma libre para eliminar la ambigüedad entre ellos.  

### <a name="optional-location-and-description-metadata"></a>Opcional: Ubicación y una descripción de metadatos

Puede proporcionar opcional **ubicación** o **descripción** los metadatos de cualquier dominio de error. Si se proporciona, esta información se incluirá en las alertas de hardware procedentes del servicio de mantenimiento. Este breve vídeo muestra el valor de agregar los descriptores de este tipo.

[![Haga clic para ver un breve vídeo que muestra el valor de agregar los descriptores de ubicación para los dominios de error](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Vea también  
-   [Windows Server 2016](../get-started/windows-server-2016.md)  
-   [Espacios de almacenamiento directo en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md) 
