---
title: Configurar scwcmd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31838ac7299cc30a7b7dde3beb47669df772487c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857506"
---
# <a name="scwcmd-configure"></a>Scwcmd: configure

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Se aplica una directiva de seguridad generados por el Asistente de configuración de seguridad de SCW en un equipo. Esta herramienta de línea de comandos también acepta una lista de nombres de equipo como entrada.

## <a name="syntax"></a>Sintaxis

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ m:\<nombreDeEquipo >|Especifica el nombre NetBIOS, nombre DNS o dirección IP del equipo para configurar. Si el **/m** parámetro se especifica, el **/p** también debe especificarse el parámetro.|
|/ou:\<OuName>|Especifica el nombre de dominio completo (FQDN) de una unidad organizativa (OU) en Active Directory Domain Services. Si el **/OU** parámetro se especifica, el **/p** también debe especificarse el parámetro. Todos los equipos de la unidad organizativa se analizarán según la directiva especificada.|
|/ p:\<directiva >|Especifica la ruta de acceso y el nombre del archivo de directiva .xml que se usará para realizar la configuración.|
|/ i:\<lista de equipos >|Especifica la ruta de acceso y el nombre de un archivo .xml que contiene una lista de equipos junto con sus archivos de directiva esperado. Todos los equipos en el archivo .xml se configurará según sus archivos de directiva correspondientes. Un archivo .xml de ejemplo es % windir%\security\SampleMachineList.xml.|
|/ u:\<nombre de usuario >|Especifica una credencial de usuario alternativo debe usar al configurar un equipo remoto. El valor predeterminado es el inicio de sesión de usuario.|
|/ pw:\<contraseña >|Especifica una credencial de usuario alternativo debe usar al configurar un equipo remoto. El valor predeterminado es la contraseña del usuario con sesión iniciada.|
|/ t:\<subprocesos >|Especifica el número de operaciones simultáneas de configuración pendientes que deben mantenerse durante el proceso de configuración (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Ejemplos

Para configurar una directiva de seguridad contra los webpolicy.xml de archivo, escriba:
```
scwcmd configure /p:webpolicy.xml
```
Para configurar una directiva de seguridad para el equipo en 172.16.0.0 contra el webpolicy.xml archivo utilizando las credenciales de cuenta webadmin, escriba:
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
Para configurar una directiva de seguridad en todos los equipos en los campusmachines.xml lista con un máximo de 100 subprocesos, escriba:
```
scwcmd configure /i:campusmachines.xml /t:100
```
Para configurar una directiva de seguridad en todos los equipos de la OU WebServers contra el webpolicy.xml archivo utilizando las credenciales de la cuenta de administrador de dominio, escriba:
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)