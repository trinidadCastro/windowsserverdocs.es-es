---
title: Scwcmd configure
description: Artículo de referencia de * * * *-
ms.topic: reference
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: eff97910907aca9db2f8e8f40c15058a21e2fac3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637010"
---
# <a name="scwcmd-configure"></a>Scwcmd: configurar

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Aplica una directiva de seguridad generada por el Asistente para configuración de seguridad (SCW) a un equipo. Esta herramienta de línea de comandos también acepta una lista de nombres de equipo como entrada.

## <a name="syntax"></a>Sintaxis

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/m\<ComputerName>|Especifica el nombre NetBIOS, el nombre DNS o la dirección IP del equipo que se va a configurar. Si se especifica el parámetro **/m** , también se debe especificar el parámetro **/p** .|
|/ou\<OuName>|Especifica el nombre de dominio completo (FQDN) de una unidad organizativa (OU) en Active Directory Domain Services. Si se especifica el parámetro **/ou** , también se debe especificar el parámetro **/p** . Todos los equipos de la unidad organizativa se analizarán según la Directiva especificada.|
|/p\<Policy>|Especifica la ruta de acceso y el nombre del archivo de directiva. XML que se va a usar para realizar la configuración.|
|/i\<ComputerList>|Especifica la ruta de acceso y el nombre de un archivo. XML que contiene una lista de equipos junto con los archivos de directivas esperados. Todos los equipos del archivo. XML se configurarán según sus archivos de directiva correspondientes. Un archivo. XML de ejemplo es% WINDIR% \security\SampleMachineList.xml.|
|/u\<UserName>|Especifica una credencial de usuario alternativa para usar al configurar un equipo remoto. El valor predeterminado es el usuario que ha iniciado sesión.|
|/PW\<Password>|Especifica una credencial de usuario alternativa para usar al configurar un equipo remoto. El valor predeterminado es la contraseña del usuario que ha iniciado sesión.|
|/t:\<Threads>|Especifica el número de operaciones de configuración pendientes simultáneas que se deben mantener durante el proceso de configuración (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Ejemplos

Para configurar una directiva de seguridad en el webpolicy.xml de archivos, escriba:
```
scwcmd configure /p:webpolicy.xml
```
Para configurar una directiva de seguridad para el equipo en 172.16.0.0 con el webpolicy.xml de archivos mediante las credenciales de la cuenta WebAdmin, escriba:
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
Para configurar una directiva de seguridad en todos los equipos de la lista campusmachines.xml con un máximo de 100 subprocesos, escriba:
```
scwcmd configure /i:campusmachines.xml /t:100
```
Para configurar una directiva de seguridad en todos los equipos de la unidad organizativa WebServers con el webpolicy.xml de archivos mediante las credenciales de la cuenta de el servidor de archivos, escriba:
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)