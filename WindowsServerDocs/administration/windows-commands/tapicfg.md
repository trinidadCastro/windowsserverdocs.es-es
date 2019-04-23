---
title: tapicfg
description: Obtenga información sobre cómo administrar una partición de directorio de aplicaciones de TAPI.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6e89b1e3d7638fbcbe0140658d2d2a9af1ccd852
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886516"
---
# <a name="tapicfg"></a>tapicfg

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea, quita, o una partición de directorio de aplicaciones de TAPI se muestra o establece una partición de directorio de aplicaciones de TAPI predeterminada. Los clientes de TAPI 3.1 pueden usar la información en esta partición de directorio de aplicación con el servicio de ubicación de servicio de directorio para buscar y comunicarse con los directorios TAPI. También puede usar **tapicfg** para crear o quitar puntos de conexión de servicio, que permiten a los clientes TAPI localizar eficazmente las particiones de directorio en un dominio. Para obtener más información, vea la sección Comentarios. Para ver la sintaxis del comando, haga clic en un comando. 
-   [tapicfg install](#BKMK_install)
-   [tapicfg remove](#BKMK_remove)
-   [tapicfg publishscp](#BKMK_publishscp)
-   [tapicfg removescp](#BKMK_removescp)
-   [tapicfg show](#BKMK_show)
-   [tapicfg makedefault](#BKMK_makedefault)

## <a name="BKMK_install"></a>instalación de tapicfg
Crea una partición de directorio de aplicaciones de TAPI.

### <a name="syntax"></a>Sintaxis
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|instalar /directory:\<PartitionName >|Obligatorio. Especifica el nombre DNS de la partición de directorio de aplicaciones de TAPI para crearse. Este nombre debe ser un nombre de dominio completo.|
|/server: \<DCName>|Especifica el nombre DNS del controlador de dominio en el que se crea la partición de directorio de aplicaciones de TAPI. Si no se especifica el nombre del controlador de dominio, se usa el nombre del equipo local.|
|/forcedefault|Especifica que este directorio es la partición de directorio de aplicación de TAPI predeterminada para el dominio. Puede haber varias particiones de directorio en un dominio.<br /><br />Si este directorio es la primera partición de directorio de aplicación de TAPI creada en el dominio, automáticamente se establece de forma predeterminada, independientemente de si usa la **/forcedefault** opción.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_remove"></a>quitar tapicfg
Quita una partición de directorio de aplicaciones de TAPI.

### <a name="syntax"></a>Sintaxis
```
tapicfg remove /directory:<PartitionName>
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|quitar /directory:\<PartitionName >|Obligatorio. Especifica el nombre DNS de la partición de directorio de aplicaciones de TAPI va a quitar. Tenga en cuenta que este nombre debe ser un nombre de dominio completo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_publishscp"></a>tapicfg publishscp
Crea un punto de conexión de servicio para publicar una partición de directorio de aplicaciones de TAPI.

### <a name="syntax"></a>Sintaxis
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|publishscp /directory:\<PartitionName >|Obligatorio. Especifica el nombre DNS de la partición de directorio de aplicaciones de TAPI que elija la conexión de servicio va a publicar.|
|/ Domain:\<DomainName >|Especifica el nombre DNS del dominio en el que el servicio punto de conexión se crea. Si no se especifica el nombre de dominio, se usa el nombre del dominio local.|
|/forcedefault|Especifica que este directorio es la partición de directorio de aplicación de TAPI predeterminada para el dominio. Puede haber varias particiones de directorio en un dominio.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_removescp"></a>tapicfg removescp
Quita un punto de conexión de servicio para una partición de directorio de aplicaciones de TAPI.

### <a name="syntax"></a>Sintaxis
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|removescp /directory:\<PartitionName >|Obligatorio. Especifica el nombre DNS de la partición de directorio de aplicaciones de TAPI para el que se quita un punto de conexión de servicio.|
|/domain: \<DomainName>|Especifica el nombre DNS del dominio del que se va a quitar el punto de conexión de servicio. Si no se especifica el nombre de dominio, se usa el nombre del dominio local.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_show"></a>tapicfg show
Muestra los nombres y ubicaciones de las particiones de directorio en el dominio.

### <a name="syntax"></a>Sintaxis
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/defaultonly|Muestra los nombres y ubicaciones de solo la partición de directorio de forma predeterminada TAPI aplicación en el dominio.|
|/domain: \<DomainName>|Especifica el nombre DNS del dominio para el que se muestran las particiones de directorio. Si no se especifica el nombre de dominio, se usa el nombre del dominio local.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_makedefault"></a>tapicfg makedefault
Establece la partición de directorio de aplicación de TAPI predeterminada para el dominio.

### <a name="syntax"></a>Sintaxis
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|makedefault /directory:\<PartitionName >|Obligatorio. Especifica el nombre DNS de la partición de directorio de aplicaciones de TAPI establecer como la partición predeterminada para el dominio. Tenga en cuenta que este nombre debe ser un nombre de dominio completo. Especifica el nombre DNS del dominio para el que la partición de directorio de aplicaciones de TAPI se establece como valor predeterminado. Si no se especifica el nombre de dominio, se usa el nombre del dominio local.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
Debe ser miembro del grupo Administradores de empresas en active directory puede ejecutar **tapicfg instalar** (para crear una partición de directorio de aplicaciones de TAPI) o **tapicfg quitar** (para quitar una aplicación de TAPI partición de directorio).

Esta herramienta de línea de comandos se puede ejecutar en cualquier equipo que sea miembro del dominio.

Solo se muestran correctamente el texto proporcionado por el usuario (por ejemplo, los nombres de particiones de directorio de aplicaciones, servidores y dominios TAPI) con caracteres internacionales o Unicode si están instaladas las fuentes adecuadas y compatibilidad con idiomas.

Todavía puede usar los servidores del servicio de ubicación de Internet (ILS) de su organización, si se precisa para admitir determinadas aplicaciones, ya que los clientes TAPI que ejecutan Windows XP o un sistema operativo Windows Server 2003 pueden consultar servidores ILS o aplicación de TAPI particiones de directorio.

Puede usar **tapicfg** para crear o quitar puntos de conexión de servicio. Si se cambia el nombre de la partición de directorio de aplicaciones de TAPI por cualquier motivo (por ejemplo, si cambia el nombre del dominio en el que reside), debe quitar el punto de conexión de servicio existente y crear uno nuevo que contiene el nuevo nombre DNS del directorio de aplicación TAPI partición que se va a publicarse. En caso contrario, los clientes TAPI son no puede localizar y tener acceso a la partición de directorio de aplicaciones de TAPI. También puede quitar un punto de conexión de servicio para fines de mantenimiento o seguridad (por ejemplo, si no desea exponer los datos TAPI en una partición de directorio de aplicación TAPI específica).

## <a name="examples"></a>Ejemplos
Para crear una partición de directorio de aplicación TAPI denominada tapifiction.testdom.microsoft.com en un servidor denominado testdc.testdom.microsoft.com y vuelva a establecerla como la partición de directorio de aplicación de TAPI predeterminada para el nuevo dominio, escriba:
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
Para mostrar el nombre de la partición de directorio de aplicaciones de TAPI predeterminada para el nuevo dominio, escriba:
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
