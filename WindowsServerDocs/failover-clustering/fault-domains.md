---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: Reconocimiento de dominio de errores
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: f5c64bb8f8b7d4b8d13c76c4e94cfcf52ee32c30
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="fault-domain-awareness-in-windows-server-2016"></a>Reconocimiento de dominio de errores en Windows Server 2016

> Se aplica a: Windows Server 2016

Clústeres de conmutación por error permite varios servidores funcionan juntos para ofrecer una alta disponibilidad – o dicho de otro modo, para proporcionar la tolerancia del nodo. Sin embargo, las empresas de hoy piden cada vez mayor disponibilidad de su infraestructura. Para lograr el tiempo de actividad de la forma de nube, incluso muy poco probable repeticiones como errores de chasis, las interrupciones de bastidor o desastres naturales deben estar protegidas contra. Por eso clústeres de conmutación por error en Windows Server 2016 presenta chasis, bastidor y tolerancia de sitio también.

Dominios de error y tolerancia a errores son conceptos estrechamente relacionadas. Un dominio de error es un conjunto de componentes de hardware que comparten un único punto de error. Para ser tolerancia hasta cierto punto, debes varios dominios de error en ese mismo nivel. Por ejemplo, para que sea el bastidor tolerancia, los servidores y los datos deben distribuirse a través de varios bastidores.

En este breve vídeo presenta una visión general de dominios de error en Windows Server 2016:  
[![Haz clic en esta imagen para ver una descripción general de dominios de error en Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

## <a name="benefits"></a>Ventajas
- **Espacios de almacenamiento, incluidos los espacios de almacenamiento directa, usa los dominios de error para maximizar la seguridad de los datos.**  
    Resistencia en espacios de almacenamiento conceptualmente es similar a RAID distribuido, definida por el software. Varias copias de todos los datos se mantienen sincronizadas, y si se produce un error de hardware y se pierde una copia, otras personas se vuelven a copiar para restaurar el estado de resistencia. Para lograr la mejor resistencia posible, deben mantenerse copias en dominios de error independiente.

- **La [Service Health](health-service-overview.md) usa dominios para proporcionar más útiles alertas a errores.**  
    Cada dominio de error puede asociarse con metadatos de ubicación, que se incluirá automáticamente en las alertas posteriores. Estos descriptores pueden ayudar a las operaciones o personal de mantenimiento y eliminar la ambigüedad de hardware para reducir errores.  

- **Ampliar clústeres usa dominios de error para afinidad de almacenamiento.** Stretch clústeres permite que los servidores remotos para unirse a un clúster comunes. Para mejorar el rendimiento, aplicaciones o máquinas virtuales debe ejecutarse en servidores cercanas a los que proporcionan su almacenamiento. Reconocimiento de dominio de error permite este afinidad de almacenamiento.   

## <a name="levels-of-fault-domains"></a>Niveles de dominios de error  
Hay cuatro niveles canónicos de dominios de error - sitio, bastidor, chasis y nodo. Los nodos que se detectan automáticamente; cada nivel adicional es opcional. Por ejemplo, si la implementación no usa servidores blade, el nivel de chasis no puede tenga sentido para TI.  

![Diagrama de los diferentes niveles de dominios de error](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Uso de  
Puedes usar PowerShell o XML para especificar los dominios de error. Ambos enfoques son equivalentes y proporcionan la funcionalidad completa.

>[!IMPORTANT]
> Especifica los dominios de error antes de habilitar espacios de almacenamiento directa, si es posible. Esto permite la configuración automática preparar la agrupación, niveles y opciones de configuración como resistencia y el número de columnas, chasis o bastidor tolerancia a errores. Una vez creados el grupo y volúmenes, datos no retroactiva moverá en respuesta a cambios a la topología de dominio de errores. Para mover los nodos entre bastidores o de chasis después de habilitar directa de los espacios de almacenamiento, primero debes expulsa el nodo y sus unidades del grupo mediante `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Definir los dominios de error con PowerShell
Windows Server 2016 presenta los siguientes cmdlets para trabajar con los dominios de error:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

En este breve vídeo se muestra el uso de estos cmdlets.
[![Haz clic en esta imagen para ver un vídeo breve sobre el uso de los cmdlets de dominio de error de clúster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Usa `Get-ClusterFaultDomain`para ver la topología de dominio de error actual. Esto enumera todos los nodos del clúster, además de cualquier chasis, estanterías o sitios que has creado. Puedes filtrar usar parámetros como **-tipo** o **-nombre**, pero no son necesarios.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Usa `New-ClusterFaultDomain`para crear nuevas imágenes, bastidores o sitios. La `-Type`y `-Name`parámetros se requieren. Los posibles valores para `-Type`son `Chassis`, `Rack`, y `Site`. La `-Name`puede ser cualquier cadena. (Para `Node`dominios de error de tipo, el nombre deben ser el nombre del nodo real, según se establece automáticamente).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server no puede y no se comprueba que los dominios de error que creas se corresponden con nada en el mundo real, físico. (Esto puede resultar obvio, pero es importante comprender). Si, en el mundo real, los nodos son todas en un bastidor, a continuación, crear dos `-Type Rack`dominios de error en el software no proporciona mágicamente bastidor tolerancia a errores. Usted es responsable de garantizar la topología que creas mediante estos cmdlets coincide con la disposición real de tu hardware.

Usa `Set-ClusterFaultDomain`para mover un dominio de error a otro. Los términos "principal" y "secundaria" se usan normalmente para describir esta relación de anidación. La `-Name`y `-Parent`parámetros se requieren. En `-Name`, proporcionar el nombre del dominio errores que se mueve; en `-Parent`, proporcionar el nombre del destino. Para mover varios dominios de error a la vez, enumera los nombres.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Cuando se mueve dominios de error, sus elementos secundarios se mueven con ellos. En el ejemplo anterior, si un es el elemento primario de server01.contoso.com, este último no por separado es necesario mover al sitio Shanghái: aún está en virtud de su elemento primario está allí, como en el mundo real.

Puedes ver las relaciones de elementos primarios y secundarios en la salida de `Get-ClusterFaultDomain`, en la `ParentName`y `ChildrenNames`columnas.

También puedes usar `Set-ClusterFaultDomain`para modificar algunas otras propiedades de dominios de error. Por ejemplo, puedes proporcionar opcional `-Location`o `-Description`metadatos para cualquier dominio de error. Si se ha indicado, se incluirá esta información en hardware alertas desde el servicio de estado. También puede cambiar el nombre de dominios de error con la `-NewName`parámetro. No cambiar el nombre `Node`escribe dominios de error.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Usa `Remove-ClusterFaultDomain`quitar chasis, bastidores o los sitios que has creado. La `-Name`parámetro es necesario. No se puede quitar un dominio de error que contiene los elementos secundarios: en primer lugar, quite los elementos secundarios o moverlos fuera con `Set-ClusterFaultDomain`. Para mover un dominio de error fuera de otros dominios de errores, establece su `-Parent`en una cadena vacía (""). No se puede quitar `Node`escribe dominios de error. Para quitar varios dominios de error a la vez, enumera los nombres.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Definir los dominios de error con formato XML
Dominios de error pueden especificarse mediante una sintaxis de XML y de inspiración. Te recomendamos que uses el editor de texto, como código de Visual Studio (disponible de forma gratuita *[aquí](https://code.visualstudio.com/)*) o en el Bloc de notas, para crear un documento XML que puedes guardar y volver a usar.  

En este breve vídeo se muestra el uso de marcado XML para especificar los dominios de error.

[![CHaga clic en esta imagen para ver un vídeo breve sobre cómo usar XML para especificar los dominios de error](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

En PowerShell, ejecuta el siguiente cmdlet:`Get-ClusterFaultDomainXML`. Esto devuelve la especificación de dominio de error actual para el clúster, como XML. Esto refleja cada descubierto `<Node>`, encapsulado en de apertura y cierre `<Topology>`etiquetas.  

Ejecuta el siguiente procedimiento para guardar este resultado en un archivo.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Abre el archivo y agregar `<Site>`, `<Rack>`, y `<Chassis>`etiquetas para especificar cómo se distribuyen estos nodos entre sitios, bastidores y chasis. Cada etiqueta debe identificarse mediante un único **nombre **. Para los nodos, debe mantener el nombre del nodo que se rellenen de forma predeterminada.  

> [!IMPORTANT]  
> Mientras que todas las etiquetas adicionales son opcionales, deben cumplir con el sitio transitivo &gt;bastidor &gt;chasis &gt;jerarquía de nodos y debe cerrarse correctamente.  
Además de nombre, de forma libre `Location="..."`y `Description="..."`descriptores pueden agregarse a cualquier etiqueta.  

#### <a name="example-two-sites-one-rack-each"></a>Ejemplo: Dos sitios, cada uno bastidor  

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

Para establecer la nueva especificación de dominio de errores, guardar el XML y ejecuta las siguientes acciones en PowerShell.  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

Esta guía presenta tan solo dos ejemplos, pero la `<Site>`, `<Rack>`, `<Chassis>`, y `<Node>`etiquetas pueden mezclar y combinar de muchas formas adicionales para reflejar la topología física de su implementación, cualquiera que sea. Esperamos que estos ejemplos ilustran la flexibilidad de estas etiquetas y el valor de descriptores de ubicación de forma libre para eliminar la ambigüedad entre ellos.  

### <a name="optional-location-and-description-metadata"></a>Opcional: Metadatos de ubicación y una descripción

Puedes proporcionar opcional **ubicación** o **descripción** metadatos para cualquier dominio de error. Si se ha indicado, se incluirá esta información en hardware alertas desde el servicio de estado. En este breve vídeo se muestra el valor de agregar estos descriptores.

[![CHaga clic para ver un vídeo breve demostrar el valor de agregar descriptores de ubicación a dominios de error](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Consulta también  
-   [Windows Server 2016](../get-started/windows-server-2016.md)  
-   [Espacios de almacenamiento dirigen en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md) 
