---
title: Introducción a Windows Admin Center
description: Introducción a Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: f4fd9f69e75ed80bbdb345b4041c2337c65ec2e6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296657"
---
# Empezar a trabajar con Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## Windows Admin Center instalado en Windows 10

> [!IMPORTANT]
> Debes ser miembro del grupo del administrador local para usar Windows Admin Center en Windows 10

### Seleccionar un certificado de cliente

La primera vez que se abre Windows Admin Center en Windows 10, asegúrate de seleccionar el certificado de *Cliente de Windows Admin Center* (en caso contrario obtendrás un mensaje de error HTTP 403 diciendo "no podrá acceder a esta página").

En Microsoft Edge, cuando se le pida con este cuadro de diálogo:
 
1. Haz clic en **más opciones**

    ![](../media/launch-cert-1.png)

2. Selecciona el certificado de **Cliente de Windows Admin Center** con la etiqueta y haz clic en **Aceptar**

    ![](../media/launch-cert-2.png)

3. Asegúrese de **Que permitir el acceso siempre** está activada y haga clic en **Permitir**

    ![](../media/launch-cert-3.png)

## Conectarse a nodos administrados y clústeres

Después de completar la instalación de Windows Admin Center, puedes agregar servidores o clústeres para administrar desde la página principal de información general.

 **Agregar un único servidor o en un clúster como un nodo administrado**

 1. Haga clic en **+ Agregar** **todas las conexiones**.

    ![](../media/launch/addserver0.png)

 2. Elige si quieres agregar una conexión de servidores, clústeres de conmutación por error o clúster hiperconvergido:
    
    ![](../media/launch/addserver1.png)

 3. Escribe el nombre del servidor o clúster para administrar y hacer clic en **Enviar**. El servidor o clúster se agregará a tu lista de conexión en la página información general.

    ![](../media/launch/addserver2.png)

   **-- O BIEN --**

**Varios servidores de importación masiva**

 1. En la página de **Agregar la conexión del servidor** , elige la pestaña de **Servidores de importación** .

    ![](../media/launch/import-servers.png)

 2. Haga clic en **Examinar** y selecciona un archivo de texto que contiene una coma o nueva línea separados, lista de nombres de dominio completos de los servidores que desea agregar.

    **-- O BIEN --**

**Agregar servidores mediante la búsqueda de Active Directory**

 1. En la página de **Agregar la conexión del servidor** , elige la pestaña de **Búsqueda en Active Directory** .

    ![](../media/launch/search-ad.png)

 2. Escribe los criterios de búsqueda y haz clic en la **búsqueda**. Se admite caracteres comodín (*).

 3. Una vez completada la búsqueda: selecciona uno o varios de los resultados, opcionalmente, agregar etiquetas y haz clic en **Agregar**.

## Autenticar con el nodo administrado ##

Windows Admin Center es compatible con varios mecanismos para la autenticación con un nodo administrado. El inicio de sesión único es el valor predeterminado.

**El inicio de sesión único**

Puedes usar las credenciales de Windows actuales para autenticarse en el nodo administrado. Este es el valor predeterminado y Windows Admin Center intenta el inicio de sesión cuando agregas un servidor. 

**Sesión único cuando se implementa como un servicio en Windows Server**

Si has instalado Windows Admin Center en Windows Server, configuración adicional es necesaria para el inicio de sesión único.  [Configurar el entorno para delegación](..\configure\user-access-control.md)

**-- O BIEN --**

**Usar *Administrar como* para especificar las credenciales**

En **Todas las conexiones**, selecciona un servidor de la lista y elegir **Administrar como** para especificar las credenciales que se usará para autenticarse en el nodo administrado:

![](../media/launch-use-6.png)

Si Windows Admin Center se ejecuta en modo de servicio en Windows Server, pero no tienes configurada la delegación de Kerberos, debe volver a escribir las credenciales de Windows:

![](../media/launch-use-7.png)

Las credenciales se pueden aplicar a todas las conexiones, que se almacenarán en esa sesión específica del explorador. Si vuelve a cargar el explorador, debes volver a escribir sus credenciales **Administrar como** .

**Solución de contraseña de administrador local (LAPS)**

Si tu entorno usa [LAPS](https://technet.microsoft.com/mt227395.aspx), puedes usar las credenciales de LAPS para autenticarse en el nodo administrado. **Si usas este escenario, por favor** [proporcionar comentarios](http://aka.ms/WACFeedback).

## Uso de etiquetas para organizar las conexiones

Puedes usar etiquetas para identificar y filtrar servidores relacionados en la lista de conexión.  Esto te permite ver un subconjunto de los servidores en la lista de conexión.  Esto es especialmente útil si tienes un gran número de conexiones.

### Edición de etiquetas

* Selecciona un servidor o en varios servidores en la lista de todas las conexiones
* En **Todas las conexiones**, haz clic en **Editar etiquetas**

![](../media/launch/tags-5.png)

El panel de **Edición de etiquetas de conexión** te permite modificar, agregar o quitar etiquetas de su conexiones seleccionadas:

* Para agregar una etiqueta de nueva a tu conexiones seleccionadas, selecciona **Agregar etiqueta** y escribe el nombre de la etiqueta que quieras usar.

* Para etiquetar las conexiones seleccionadas con un nombre de etiqueta existente, activa la casilla situada junto al nombre de la etiqueta que quieras aplicar.

* Para quitar una etiqueta de conexiones de todos los seleccionados, desactiva la casilla junto a la etiqueta que quieres quitar.

* Si se aplica una etiqueta a un subconjunto de las conexiones seleccionadas, se muestra la casilla de verificación en un estado intermedio. Puedes hacer clic en la casilla para activarla y la etiqueta se aplican a todas las conexiones seleccionadas o haga clic de nuevo para desactivarla y quitar la etiqueta de todas las conexiones seleccionadas.

![](../media/launch/tags-6.png)

### Filtrar las conexiones por etiqueta

Una vez que se han agregado etiquetas para las conexiones de servidor de uno o más, puedes ver las etiquetas en la lista de conexión y filtrar la lista de conexión mediante las etiquetas.

* Para filtrar por una etiqueta, selecciona el icono de filtro situado junto al cuadro de búsqueda.
![](../media/launch/tags-7.png)
* Puedes seleccionar "o", "y" o "no" para modificar el comportamiento de filtro de las etiquetas seleccionadas.
![](../media/launch/tags-8.png)

## Usar PowerShell para importar o exportar las conexiones (con etiquetas)

> Se aplica a: Windows Admin Center Preview

Versión preliminar de Windows Admin Center incluye un módulo de PowerShell para importar o exportar la lista de conexión.

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### Formato de archivo CSV para la importación de conexiones

El formato del archivo CSV comienza con los encabezados de cuatro ```"name","type","tags","groupId"```, seguido por cada conexión en una nueva línea.

**nombre** es el FQDN de la conexión.

**tipo** es el tipo de conexión. Para las conexiones de predeterminada que se incluye con Windows Admin Center, tienes que usar uno de los siguientes:

| Tipo de conexión | Cadena de conexión |
|------|-------------------------------|
| Windows Server | msft.SME.Connection type.server |
| PC con Windows 10 | cliente de type.windows msft.SME.Connection |
| Clúster de conmutación por error | msft.SME.Connection type.cluster |
| Clúster Hiperconvergido | msft.SME.Connection-type.hyper--clústeres |

**las etiquetas** son separados por la canalización.

**groupId** se usa para conexiones compartidas. Usa el valor ```global``` en esta columna para establecer una conexión compartida.

### Archivo CSV de ejemplo para la importación de conexiones

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## Conexiones de importación RDCman

Usar el script siguiente para exportar conexiones guardadas en [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) a un archivo. A continuación, puede importar el archivo en Windows Admin Center, mantener la jerarquía de agrupación de RDCMan mediante etiquetas. ¡Pruébalo!

1. Copia y pega el código siguiente en la sesión de PowerShell:

   ```powershell
   #Helper function for RdgToWacCsv
   function AddServers {
    param (
    [Parameter(Mandatory = $true)]
    [Xml.XmlLinkedNode]
    $node,
    [Parameter()]
    [String[]]
    $tags,
    [Parameter(Mandatory = $true)]
    [String]
    $csvPath
    )
    if ($node.LocalName -eq 'server') {
        $serverName = $node.properties.name
        $tagString = $tags -join "|"
        Add-Content -Path $csvPath -Value ('"'+ $serverName + '","msft.sme.connection-type.server","'+ $tagString +'"')
    } 
    elseif ($node.LocalName -eq 'group' -or $node.LocalName -eq 'file') {
        $groupName = $node.properties.name
        $tags+=$groupName
        $currNode = $node.properties.NextSibling
        while ($currNode) {
            AddServers -node $currNode -tags $tags -csvPath $csvPath
            $currNode = $currNode.NextSibling
        }
    } 
    else {
        # Node type isn't relevant to tagging or adding connections in WAC
    }
    return
   }

   <#
   .SYNOPSIS
   Convert an .rdg file from Remote Desktop Connection Manager into a .csv that can be imported into Windows Admin Center, maintaining groups via server tags. This will not modify the existing .rdg file and will create a new .csv file

    .DESCRIPTION
    This converts an .rdg file into a .csv that can be imported into Windows Admin Center.

    .PARAMETER RDGfilepath
    The path of the .rdg file to be converted. This file will not be modified, only read.

    .PARAMETER CSVdirectory
    Optional. The directory you wish to export the new .csv file. If not provided, the new file is created in the same directory as the .rdg file.

    .EXAMPLE
    C:\PS> RdgToWacCsv -RDGfilepath "rdcmangroup.rdg"
    #>
   function RdgToWacCsv {
    param(
        [Parameter(Mandatory = $true)]
        [String]
        $RDGfilepath,
        [Parameter(Mandatory = $false)]
        [String]
        $CSVdirectory
    )
    [xml]$RDGfile = Get-Content -Path $RDGfilepath
    $node = $RDGfile.RDCMan.file
    if (!$CSVdirectory){
        $csvPath = [System.IO.Path]::GetDirectoryName($RDGfilepath) + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    } else {
        $csvPath = $CSVdirectory + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    }
    New-item -Path $csvPath
    Add-Content -Path $csvPath -Value '"name","type","tags"'
    AddServers -node $node -csvPath $csvPath
    Write-Host "Converted $RDGfilepath `nOutput: $csvPath"
   }
   ```

2. Para crear una. Archivo CSV, ejecute el siguiente comando:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importar resultante. Archivo CSV en Windows Admin Center y todo su jerarquía de agrupación de RDCMan se representará mediante etiquetas en la lista de conexión. Para obtener más información, consulte [Usar PowerShell para importar o exportar las conexiones (con etiquetas)](#use-powershell-to-import-or-export-your-connections-with-tags).

## Scripts de PowerShell de vista que se usa en Windows Admin Center

Una vez que has conectado a un servidor, un clúster o un equipo, puedes ver los scripts de PowerShell que potencia las acciones de la interfaz de usuario disponibles en Windows Admin Center. Desde dentro de una herramienta, haz clic en el icono de PowerShell en la barra de la aplicación superior. Selecciona un comando de interés en la lista desplegable para navegar a la secuencia de comandos de PowerShell correspondiente.

![](../media/launch/showscript.png)