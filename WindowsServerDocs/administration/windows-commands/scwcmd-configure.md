---
title: scwcmd configure
description: Artículo de referencia para el comando scwcmd configure, que aplica una directiva de seguridad generada por el Asistente para configuración de seguridad (SCW) a un equipo.
ms.topic: reference
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a57d2142f8fc7fd788a5669c5318ff6444c34734
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388592"
---
# <a name="scwcmd-configure"></a>scwcmd configure

> Se aplica a: Windows Server 2012 R2 y Windows Server 2012

Aplica una directiva de seguridad generada por el Asistente para configuración de seguridad (SCW) a un equipo. Esta herramienta de línea de comandos también acepta una lista de nombres de equipo como entrada.

## <a name="syntax"></a>Sintaxis

```
scwcmd configure [[[/m:<computername> | /ou:<OuName>] /p:<policy>] | /i:<computerlist>] [/u:<username>] [/pw:<password>] [/t:<threads>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /m`<computername>` | Especifica el nombre NetBIOS, el nombre DNS o la dirección IP del equipo que se va a configurar. Si se especifica el parámetro **/m** , también se debe especificar el parámetro **/p** . |
| /ou`<OuName>` | Especifica el nombre de dominio completo (FQDN) de una unidad organizativa (OU) en Active Directory Domain Services. Si se especifica el parámetro **/ou** , también se debe especificar el parámetro **/p** . Todos los equipos de la unidad organizativa se configurarán con la Directiva especificada. |
| /p`<policy>` | Especifica la ruta de acceso y el nombre del archivo de directiva. XML que se va a usar para realizar la configuración. |
| /i`<computerlist>` | Especifica la ruta de acceso y el nombre de un archivo. XML que contiene una lista de equipos junto con los archivos de directivas esperados. Todos los equipos del archivo. XML se analizarán con los archivos de directiva correspondientes. Un archivo. XML de ejemplo es `%windir%\security\SampleMachineList.xml` . |
| /u`<username>` | Especifica una credencial de usuario alternativa que se va a usar al realizar la configuración en un equipo remoto. El valor predeterminado es el usuario que ha iniciado sesión. |
| /PW`<password>` | Especifica una credencial de usuario alternativa que se va a usar al realizar la configuración en un equipo remoto. El valor predeterminado es la contraseña del usuario que ha iniciado sesión. |
| /t:`<threads>` | Especifica el número de operaciones de configuración pendientes simultáneas que se deben mantener durante el análisis. El intervalo de valores es 1-1000, con un valor predeterminado de 40. |
| /l | Hace que se registre el proceso de análisis. Se generará un archivo de registro para cada equipo que se está analizando. Los archivos de registro se almacenarán en el mismo directorio que los archivos de resultados. Use la opción **/o** para especificar el directorio de los archivos de resultados. |
| /e | Registra un evento en el registro de eventos de la aplicación si se encuentra un error de coincidencia. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para configurar una directiva de seguridad en el *webpolicy.xml*de archivos, escriba:

```
scwcmd configure /p:webpolicy.xml
```

Para configurar una directiva de seguridad para el equipo en *172.16.0.0* con el *webpolicy.xml* de archivos mediante las credenciales de la cuenta *WebAdmin* , escriba:

```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```

Para configurar una directiva de seguridad en todos los equipos de la lista *campusmachines.xml* con un *máximo de 100 subprocesos*, escriba:

```
scwcmd configure /i:campusmachines.xml /t:100
```

Para configurar una directiva de seguridad para la *unidad organizativa WebServers* con el *webpolicy.xml* de archivos mediante *las credenciales* del ID. de servidor, escriba:

```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando scwcmd ANALYZE](scwcmd-analyze.md)

- [comando scwcmd Register](scwcmd-register.md)

- [comando de restauración de scwcmd](scwcmd-rollback.md)

- [scwcmd transform (comando)](scwcmd-transform.md)

- [comando scwcmd View](scwcmd-view.md)