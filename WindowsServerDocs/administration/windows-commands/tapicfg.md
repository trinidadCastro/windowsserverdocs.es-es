---
title: tapicfg
description: Obtenga información acerca de cómo administrar una partición de directorio de aplicaciones TAPI.
ms.topic: reference
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: fc44ebc15c130dab34a1aff60aba7926b4cbbcb4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635464"
---
# <a name="tapicfg"></a>tapicfg

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea, quita o muestra una partición de directorio de aplicaciones TAPI, o establece una partición de directorio de aplicaciones TAPI predeterminada. Los clientes TAPI 3,1 pueden usar la información de esta partición de directorio de aplicaciones con el servicio de localizador de servicios de directorio para buscar y comunicarse con directorios TAPI. También puede usar **Tapicfg** para crear o quitar puntos de conexión de servicio, que permiten a los clientes TAPI localizar de forma eficaz las particiones de directorio de aplicaciones TAPI en un dominio. Para obtener más información, vea la sección Comentarios. Para ver la sintaxis del comando, haga clic en un comando.
-   [Tapicfg (instalación)](#BKMK_install)
-   [Tapicfg Remove](#BKMK_remove)
-   [Tapicfg publishscp](#BKMK_publishscp)
-   [Tapicfg removescp](#BKMK_removescp)
-   [Tapicfg show](#BKMK_show)
-   [Tapicfg makedefault](#BKMK_makedefault)

## <a name="tapicfg-install"></a><a name="BKMK_install"></a>Tapicfg (instalación)
Crea una partición de directorio de aplicaciones TAPI.

### <a name="syntax"></a>Sintaxis
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|instalar/Directory:\<PartitionName>|Necesario. Especifica el nombre DNS de la partición de directorio de aplicaciones TAPI que se va a crear. Este nombre debe ser un nombre de dominio completo.|
|/Server \<DCName>|Especifica el nombre DNS del controlador de dominio en el que se crea la partición del directorio de aplicaciones TAPI. Si no se especifica el nombre del controlador de dominio, se utiliza el nombre del equipo local.|
|/forcedefault|Especifica que este directorio es la partición del directorio de aplicaciones TAPI predeterminada para el dominio. Puede haber varias particiones de directorio de aplicaciones TAPI en un dominio.<p>Si este directorio es la primera partición del directorio de aplicaciones TAPI creada en el dominio, se establece automáticamente como valor predeterminado, con independencia de si se usa la opción **/forcedefault** .|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="tapicfg-remove"></a><a name="BKMK_remove"></a>Tapicfg Remove
Quita una partición de directorio de aplicaciones TAPI.

### <a name="syntax"></a>Sintaxis
```
tapicfg remove /directory:<PartitionName>
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|quitar/Directory:\<PartitionName>|Necesario. Especifica el nombre DNS de la partición de directorio de aplicaciones TAPI que se va a quitar. Tenga en cuenta que este nombre debe ser un nombre de dominio completo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="tapicfg-publishscp"></a><a name="BKMK_publishscp"></a>Tapicfg publishscp
Crea un punto de conexión de servicio para publicar una partición de directorio de aplicaciones TAPI.

### <a name="syntax"></a>Sintaxis
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|publishscp/Directory:\<PartitionName>|Necesario. Especifica el nombre DNS de la partición del directorio de aplicaciones TAPI que publicará el punto de conexión de servicio.|
|/Domain\<DomainName>|Especifica el nombre DNS del dominio en el que se crea el punto de conexión de servicio. Si no se especifica el nombre de dominio, se utiliza el nombre del dominio local.|
|/forcedefault|Especifica que este directorio es la partición del directorio de aplicaciones TAPI predeterminada para el dominio. Puede haber varias particiones de directorio de aplicaciones TAPI en un dominio.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="tapicfg-removescp"></a><a name="BKMK_removescp"></a>Tapicfg removescp
Quita un punto de conexión de servicio para una partición de directorio de aplicaciones TAPI.

### <a name="syntax"></a>Sintaxis
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|removescp/Directory:\<PartitionName>|Necesario. Especifica el nombre DNS de la partición de directorio de aplicaciones TAPI para la que se quita un punto de conexión de servicio.|
|/Domain \<DomainName>|Especifica el nombre DNS del dominio desde el que se quita el punto de conexión de servicio. Si no se especifica el nombre de dominio, se utiliza el nombre del dominio local.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="tapicfg-show"></a><a name="BKMK_show"></a>Tapicfg show
Muestra los nombres y las ubicaciones de las particiones de directorio de aplicaciones TAPI del dominio.

### <a name="syntax"></a>Sintaxis
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/defaultonly|Muestra los nombres y las ubicaciones de solo la partición del directorio de aplicaciones TAPI predeterminada en el dominio.|
|/Domain \<DomainName>|Especifica el nombre DNS del dominio para el que se muestran las particiones de directorio de aplicaciones TAPI. Si no se especifica el nombre de dominio, se utiliza el nombre del dominio local.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="tapicfg-makedefault"></a><a name="BKMK_makedefault"></a>Tapicfg makedefault
Establece la partición del directorio de aplicaciones TAPI predeterminado para el dominio.

### <a name="syntax"></a>Sintaxis
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|makedefault/Directory:\<PartitionName>|Necesario. Especifica el nombre DNS del conjunto de particiones del directorio de aplicaciones TAPI como la partición predeterminada del dominio. Tenga en cuenta que este nombre debe ser un nombre de dominio completo. Especifica el nombre DNS del dominio para el que se establece la partición del directorio de aplicaciones TAPI como valor predeterminado. Si no se especifica el nombre de dominio, se utiliza el nombre del dominio local.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
Debe ser miembro del grupo administradores de empresas en Active Directory para ejecutar **Tapicfg install** (para crear una partición de directorio de aplicaciones TAPI) o **Tapicfg Remove** (para quitar una partición de directorio de aplicaciones TAPI).

Esta herramienta de línea de comandos puede ejecutarse en cualquier equipo que sea miembro del dominio.

El texto proporcionado por el usuario (como los nombres de las particiones de directorio de aplicaciones TAPI, los servidores y los dominios) con caracteres internacionales o Unicode solo se muestra correctamente si se instalan las fuentes y la compatibilidad con idiomas correspondientes.

Puede seguir usando los servidores del servicio de ubicación de Internet (ILS) en su organización, si se necesita ILS para admitir determinadas aplicaciones, ya que los clientes TAPI que ejecuten Windows XP o un sistema operativo Windows Server 2003 pueden consultar servidores ILS o particiones de directorio de aplicaciones TAPI.

Puede usar **Tapicfg** para crear o quitar puntos de conexión de servicio. Si se cambia el nombre de la partición del directorio de aplicaciones TAPI por cualquier motivo (por ejemplo, si cambia el nombre del dominio en el que reside), debe quitar el punto de conexión de servicio existente y crear uno nuevo que contenga el nuevo nombre DNS de la partición del directorio de la aplicación TAPI que se va a publicar. De lo contrario, los clientes TAPI no podrán encontrar ni tener acceso a la partición de directorio de aplicaciones TAPI. También puede quitar un punto de conexión de servicio para fines de mantenimiento o seguridad (por ejemplo, si no desea exponer datos TAPI en una partición de directorio de aplicaciones TAPI específica).

## <a name="examples"></a>Ejemplos
Para crear una partición de directorio de aplicaciones TAPI denominada tapifiction.testdom.microsoft.com en un servidor denominado testdc.testdom.microsoft.com y, a continuación, establézcalo como la partición del directorio de aplicaciones TAPI predeterminada para el nuevo dominio, escriba:
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
Para mostrar el nombre de la partición de directorio de aplicaciones TAPI predeterminada para el nuevo dominio, escriba:
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
