---
title: Comandos netsh para red de banda ancha móvil (MBN)
description: Usa netsh mbn para consultar y configurar los parámetros y las opciones de banda ancha móvil.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
author: apdutta
ms.date: 02/20/2020
ms.openlocfilehash: 478f87db4d520a133b3d70c0ed2dbb4e91db60d9
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80853738"
---
# <a name="netsh-mbn-commands"></a>Comandos netsh mbn


Usa **netsh mbn** para consultar y configurar los parámetros y las opciones de banda ancha móvil.

> [!TIP]
> Puedes obtener ayuda sobre el comando de netsh mbn con 
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh mbn /?

Los comandos netsh mbn disponibles son:

- [add](#add)
- [connect](#connect)
- [delete](#delete)
- [disconnect](#disconnect)
- [diagnose](#diagnose)
- [dump](#dump)
- [help](#help)
- [set](#set)
- [show](#show)

## <a name="add"></a>agregar

Agrega una entrada de configuración a una tabla.

Los comandos netsh mbn add disponibles son:

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

Agrega un perfil de configuración de DM al almacén de datos de perfil.

**Sintaxis**

```powershell
add dmprofile [interface=]<string> [name=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **name**      | Nombre del archivo XML de perfil. Es el nombre del archivo XML que contiene los datos del perfil.     | Requerido |


**Ejemplo**

```powershell
add dmprofile interface="Cellular" name="Profile1.xml"
```


### <a name="profile"></a>perfil

Agrega un perfil de red al almacén de datos de perfil.

**Sintaxis**

```powershell
add profile [interface=]<string> [name=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **name**      | Nombre del archivo XML de perfil. Es el nombre del archivo XML que contiene los datos del perfil.     | Requerido |


**Ejemplo**

```powershell
add profile interface="Cellular" name="Profile1.xml"
```


## <a name="connect"></a>connect

Se conecta a una red de banda ancha móvil.

**Sintaxis**

```powershell
connect [interface=]<string> [connmode=]tmp|name [name=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **connmode**  | Especifica cómo se proporcionan los parámetros de conexión. La conexión se puede solicitar mediante un archivo XML de perfil, o con el nombre de perfil para el archivo XML de perfil que se ha almacenado previamente en el almacén de datos de perfil de banda ancha móvil mediante el comando "netsh mbn add profile". En el caso anterior, el parámetro connmode debe contener "tmp" como valor. Mientras que en este último caso, será "name".                                       | Requerido |
| **name**      | Nombre del archivo XML de perfil. Es el nombre del archivo XML que contiene los datos del perfil.     | Requerido |


**Ejemplos**

```powershell
connect interface="Cellular" connmode=tmp name=d:\profile.xml
connect interface="Cellular" connmode=name name=MyProfileName
```


## <a name="delete"></a>Eliminar

Elimina una entrada de configuración de una tabla.

Los comandos netsh mbn delete disponibles son:

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

Elimina un perfil de configuración de DM del almacén de datos de perfil.

**Sintaxis**

```powershell
delete dmprofile [interface=]<string> [name=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **name**      | Nombre del archivo XML de perfil. Es el nombre del archivo XML que contiene los datos del perfil.     | Requerido |

**Ejemplo**

```powershell
delete dmprofile interface="Cellular" name="myProfileName"
```

### <a name="profile"></a>perfil

Elimina un perfil de red del almacén de datos de perfil.

**Sintaxis**

```powershell
delete profile [interface=]<string> [name=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **name**      | Nombre del archivo XML de perfil. Es el nombre del archivo XML que contiene los datos del perfil.     | Requerido |


**Ejemplo**

```powershell
delete profile interface="Cellular" name="myProfileName"
```

## <a name="diagnose"></a>diagnose

Ejecuta diagnósticos para problemas básicos de telefonía móvil.

**Sintaxis**

```powershell
diagnose [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
diagnose interface="Cellular"
```


## <a name="disconnect"></a>desconectar

Se desconecta de una red de banda ancha móvil.

**Sintaxis**

```powershell
disconnect [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
disconnect interface="Cellular"
```


## <a name="dump"></a>dump

Muestra un script de configuración. 

Crea un script que contiene la configuración actual.  Si se guarda en un archivo, este script se puede usar para restaurar la configuración modificada.

**Sintaxis**

```powershell
dump
```

## <a name="help"></a>ayuda

Muestra una lista de comandos.

**Sintaxis**

```powershell
help
```

## <a name="set"></a>set

Establece la información de configuración.

Los comandos netsh mbn set disponibles son:

- [acstate](#acstate)
- [dataenablement](#dataenablement)
- [dataroamcontrol](#dataroamcontrol)
- [enterpriseapnparams](#enterpriseapnparams)
- [highestconncategory](#highestconncategory)
- [powerstate](#powerstate)
- [profileparameter](#profileparameter)
- [slotmapping](#slotmapping)
- [tracing](#tracing)

### <a name="acstate"></a>acstate

Establece el estado de conexión automática de datos de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
set acstate [interface=]<string> [state=]autooff|autoon|manualoff|manualon
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **name**      | El estado de conexión automática que se va a establecer. Uno de los siguientes valores:<br>autooff: Token de conexión automática desactivado.<br>autoon: Token de conexión automática activado.<br>manualoff: Token de conexión manual desactivado.<br>manualon: Token de conexión manual activado. | Requerido |


**Ejemplo**

```powershell
set acstate interface="Cellular" state=autoon
```


### <a name="dataenablement"></a>dataenablement

Activa o desactiva los datos de banda ancha móvil para el conjunto de perfiles y la interfaz dados.

**Sintaxis**

```powershell
set dataenablement [interface=]<string> [profileset=]internet|mms|all [mode=]yes|no
```

**Parámetros**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **profileset** | Nombre del conjunto de perfiles.                                                                      | Requerido |
| **mode**       | Uno de los siguientes valores:<br>yes: Habilita el conjunto de perfiles de destino.<br>no: Deshabilita el conjunto de perfiles de destino.| Requerido |


**Ejemplo**

```powershell
set dataenablement interface="Cellular" profileset=mms mode=yes
```


### <a name="dataroamcontrol"></a>dataroamcontrol

Establece el estado de control de itinerancia de los datos de banda ancha móvil para el conjunto de perfiles y la interfaz dados.

**Sintaxis**

```powershell
set dataroamcontrol [interface=]<string> [profileset=]internet|mms|all [state=]none|partner|all
```

**Parámetros**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **profileset** | Nombre del conjunto de perfiles.                                                                      | Requerido |
| **mode**       | Uno de los siguientes valores:<br>none: Solo el operador local.<br>partner: Solo los operadores local y asociados.<br>all: Los operadores local, asociados y de itinerancia.| Requerido |


**Ejemplo**

```powershell
set dataroamcontrol interface="Cellular" profileset=mms mode=partner
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

Establece los parámetros de enterpriseAPN de datos de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
set enterpriseapnparams [interface=]<string> [allowusercontrol=]yes|no|nc [allowuserview=]yes|no|nc [profileaction=]add|delete|modify|nc
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **allowusercontrol** | Uno de los siguientes valores:<br>yes: Permitir al usuario final controlar enterpriseAPN.<br>no: No permitir al usuario final controlar enterpriseAPN.<br>nc: Sin cambios | Requerido |
| **allowuserview** |Uno de los siguientes valores:<br>yes: Permitir al usuario final ver enterpriseAPN.<br>no: No permitir al usuario final ver enterpriseAPN.<br>nc: Sin cambios. | Requerido |
| **profileaction** | Uno de los siguientes valores:<br>add: Se acaba de agregar un perfil de enterpriseAPN.<br>delete: Se acaba de eliminar un perfil de enterpriseAPN.<br>modify: Se acaba de modificar un perfil de enterpriseAPN.<br>nc: Sin cambios. | Requerido |


**Ejemplo**

```powershell
set enterpriseapnparams interface="Cellular" profileset=mms mode=yes
```


### <a name="highestconncategory"></a>highestconncategory

Establece la categoría de conexión más elevada de los datos de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
set highestconncategory [interface=]<string> [highestcc=]admim|user|operator|device
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **highestcc** | Uno de los siguientes valores:<br>admin: Perfiles aprovisionados por el administrador.<br>user: Perfiles aprovisionados por el usuario.<br>operator: Perfiles aprovisionados por el operador.<br>device: Perfiles aprovisionados por el dispositivo. | Requerido |


**Ejemplo**

```powershell
set highestconncategory interface="Cellular" highestcc=operator
```


### <a name="powerstate"></a>powerstate

Activa o desactiva la radio de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
set powerstate [interface=]<string> [state=]on|off
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **state**      | Uno de los siguientes valores:<br>on: Enciende el transceptor de radio.<br>off: Apaga el transceptor de radio. | Requerido |


**Ejemplo**

```powershell
set powerstate interface="Cellular" state=on
```


### <a name="profileparameter"></a>profileparameter

Establece parámetros en un perfil de red de banda ancha móvil.

**Sintaxis**

```powershell
set profileparameter [name=]<string> [[interface=]<string>] [[cost]=default|unrestricted|fixed|variable]
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nombre del perfil que se modificará. Si se especifica la interfaz, solo se modifica el perfil de esa interfaz. | Requerido |
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Opcional |
| **cost**      | Costo asociado al perfil.                                                             | Opcional |


**Observaciones**

Se debe especificar al menos un parámetro entre el nombre de la interfaz y el costo.


**Ejemplo**

```powershell
set profileparameter name="profile 1" cost=default
```


### <a name="slotmapping"></a>slotmapping

Establece la asignación de ranura del módem de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
set slotmapping [interface=]<string> [slotindex=]<integer>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **slotindex** | Índice de ranura que se va a establecer.                                                                         | Requerido |


**Ejemplo**

```powershell
set slotmapping interface="Cellular" slotindex=1
```


### <a name="tracing"></a>tracing

Habilita o deshabilita el seguimiento.

**Sintaxis**

```powershell
set tracing [mode=]yes|no
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **mode**      | Uno de los siguientes valores:<br>yes: Habilita el seguimiento para la banda ancha móvil.<br>no: Deshabilita el seguimiento para la banda ancha móvil.     | Requerido |


**Ejemplo**

```powershell
set tracing mode=yes
```

## <a name="show"></a>mostrar

Muestra información de red de banda ancha móvil.

Los comandos netsh mbn set disponibles son:

- [acstate](#acstate)
- [capability](#capability)
- [connection](#connection)
- [dataenablement](#dataenablement)
- [dataroamcontrol](#dataroamcontrol)
- [dmprofiles](#dmprofiles)
- [enterpriseapnparams](#enterpriseapnparams)
- [highestconncategory](#highestconncategory)
- [homeprovider](#homeprovider)
- [interfaces](#interfaces)
- [netlteattachinfo](#netlteattachinfo)
- [pin](#pin)
- [pinlist](#pinlist)
- [preferredproviders](#preferredproviders)
- [profiles](#profiles)
- [profilestate](#profilestate)
- [provisionedcontexts](#provisionedcontexts)
- [purpose](#purpose)
- [radio](#radio)
- [readyinfo](#readyinfo)
- [signal](#signal)
- [slotmapping](#slotmapping)
- [slotstatus](#slotstatus)
- [smsconfig](#smsconfig)
- [tracing](#tracing)
- [visibleproviders](#visibleproviders)

### <a name="acstate"></a>acstate  

Muestra el estado de conexión automática de datos de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
show acstate [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show acstate interface="Cellular"
```


### <a name="capability"></a>capability

Muestra la información de capacidad de la interfaz especificada.

**Sintaxis**

```powershell
show capability [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show capability interface="Cellular"
```


### <a name="connection"></a>connection

Muestra la información de conexión actual para la interfaz especificada.

**Sintaxis**

```powershell
show connection [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show connection interface="Cellular"
```


### <a name="dataenablement"></a>dataenablement

Muestra el estado de habilitación de datos de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
show dataenablement [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show dataenablement interface="Cellular"
```


### <a name="dataroamcontrol"></a>dataroamcontrol

Muestra el estado de control de itinerancia de datos de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
show dataroamcontrol [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show dataroamcontrol interface="Cellular"
```


### <a name="dmprofiles"></a>dmprofiles

Muestra una lista de perfiles de configuración de DM configurados en el sistema.

**Sintaxis**

```powershell
show dmprofiles [[name=]<string>] [[interface=]<string>]
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nombre del perfil que se mostrará.                                                               | Opcional |
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Opcional |

**Observaciones**
    
Muestra los datos de perfil o enumera los perfiles del sistema.

Si se proporciona el nombre del perfil, se mostrará el contenido del perfil. De lo contrario, se enumerarán los perfiles para la interfaz.

Si se proporciona el nombre de la interfaz, solo se mostrará el perfil especificado en la interfaz dada. De lo contrario, se mostrará el primer perfil coincidente.

**Ejemplo**

```powershell
show dmprofiles name="profile 1" interface="Cellular"
show dmprofiles name="profile 2"
show dmprofiles
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

Muestra los parámetros de enterpriseAPN de datos de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
show enterpriseapnparams [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show enterpriseapnparams interface="Cellular"
```


### <a name="highestconncategory"></a>highestconncategory

Muestra la categoría de conexión más elevada de los datos de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
show highestconncategory [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show highestconncategory interface="Cellular"
```


### <a name="homeprovider"></a>homeprovider

Muestra la información del proveedor principal para la interfaz especificada.

**Sintaxis**

```powershell
show homeprovider [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show homeprovider interface="Cellular"
```


### <a name="interfaces"></a>interfaces

Muestra una lista de interfaces de banda ancha móvil en el sistema. No hay parámetros para este comando.

**Sintaxis**

```powershell
show interfaces
```


### <a name="netlteattachinfo"></a>netlteattachinfo

Muestra la información de datos adjuntos de LTE de la red de banda ancha móvil para la interfaz dada.

**Sintaxis**

```powershell
show netlteattachinfo [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show netlteattachinfo interface="Cellular"
```

### <a name="pin"></a>pin      

Muestra la información de PIN para la interfaz especificada.

**Sintaxis**

```powershell
show pin [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show pin interface="Cellular"
```


### <a name="pinlist"></a>pinlist  

Muestra la información de la lista de PIN para la interfaz especificada.

**Sintaxis**

```powershell
show pinlist [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show pinlist interface="Cellular"
```


### <a name="preferredproviders"></a>preferredproviders

Muestra la lista de proveedores preferidos para la interfaz especificada.

**Sintaxis**

```powershell
show preferredproviders [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show preferredproviders interface="Cellular"
```


### <a name="profiles"></a>profiles 

Muestra una lista de perfiles configurados en el sistema.

**Sintaxis**

```powershell
show profiles [[name=]<string>] [[interface=]<string>] [[purpose=]<string>]
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nombre del perfil que se mostrará.                                                               | Opcional |
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Opcional |
| **purpose**   | Propósito | Opcional |

**Observaciones**

Si se proporciona el nombre del perfil, se mostrará el contenido del perfil. De lo contrario, se enumerarán los perfiles para la interfaz.

Si se proporciona el nombre de la interfaz, solo se mostrará el perfil especificado en la interfaz dada. De lo contrario, se mostrará el primer perfil coincidente.

Si se proporciona el propósito, solo se mostrarán los perfiles con el GUID de propósito coincidente.  De lo contrario, los perfiles no se filtrarán por propósito.  La cadena puede ser un GUID entre llaves o una de las siguientes cadenas: internet, supl, mms, ims o allhost.
    
**Ejemplo**

```powershell
show profiles interface="Cellular" purpose="{00000000-0000-0000-0000-000000000000}"
show profiles name="profile 1" interface="Cellular"
show profiles name="profile 2"
show profiles
```

### <a name="profilestate"></a>profilestate

Muestra el estado de un perfil de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
show profilestate [interface=]<string> [name=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |
| **name**      | Nombre del perfil. Es el nombre del perfil que tiene el estado que se va a mostrar.            | Requerido |

**Ejemplo**

```powershell
show profilestate interface="Cellular" name="myProfileName"
```

### <a name="provisionedcontexts"></a>provisionedcontexts

Muestra la información de contextos aprovisionados para la interfaz especificada.

**Sintaxis**

```powershell
show provisionedcontexts [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show provisionedcontexts interface="Cellular"
```


### <a name="purpose"></a>purpose  

Muestra los GUID del grupo de propósitos que se pueden usar para filtrar los perfiles en el dispositivo. No hay parámetros para este comando.

**Sintaxis**

```powershell
show purpose
```


### <a name="radio"></a>radio    

Muestra la información de estado de la radio para la interfaz especificada.

**Sintaxis**

```powershell
show radio [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show radio interface="Cellular"
```


### <a name="readyinfo"></a>readyinfo

Muestra la información de estado listo para la interfaz especificada.

**Sintaxis**

```powershell
show readyinfo [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show readyinfo interface="Cellular"
```


### <a name="signal"></a>signal   

Muestra la información de la señal para la interfaz especificada.

**Sintaxis**

```powershell
show signal [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show signal interface="Cellular"
```


### <a name="slotmapping"></a>slotmapping

Muestra la asignación de ranura del módem de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
show slotmapping [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show slotmapping interface="Cellular"
```


### <a name="slotstatus"></a>slotstatus

Muestra el estado de ranura del módem de banda ancha móvil para la interfaz especificada.

**Sintaxis**

```powershell
show slotstatus [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show slotstatus interface="Cellular"
```


### <a name="smsconfig"></a>smsconfig

Muestra la información de configuración de SMS para la interfaz especificada.

**Sintaxis**

```powershell
show smsconfig [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show smsconfig interface="Cellular"
```


### <a name="tracing"></a>tracing  

Muestra si el seguimiento de banda ancha móvil está habilitado o deshabilitado.

**Sintaxis**

```powershell
show tracing 
```


### <a name="visibleproviders"></a>visibleproviders

Muestra la lista de proveedores visibles para la interfaz especificada.

**Sintaxis**

```powershell
show visibleproviders [interface=]<string>
```

**Parámetros**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nombre de la interfaz. Es uno de los nombres de interfaz que se muestran en el comando "netsh mbn show interfaces". | Requerido |


**Ejemplo**

```powershell
show visibleproviders interface="Cellular"
```
