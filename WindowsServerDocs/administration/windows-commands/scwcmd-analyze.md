---
title: scwcmd analyze
description: Artículo de referencia para el comando scwcmd Analyze, que determina si un equipo cumple con una directiva.
ms.topic: reference
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0d891262ebf04b1b8e604bc4756a3ca05888f8fa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388611"
---
# <a name="scwcmd-analyze"></a>scwcmd analyze

> Se aplica a: Windows Server 2012 R2 y Windows Server 2012

Determina si un equipo cumple con una directiva. Los resultados se devuelven en un archivo. Xml.

Este comando también acepta una lista de nombres de equipo como entrada. Para ver los resultados en el explorador, use **scwcmd View** y especifique `%windir%\security\msscw\TransformFiles\scwanalysis.xsl` como la transformación. Xsl.

## <a name="syntax"></a>Sintaxis

```
scwcmd analyze [[[/m:<computername> | /ou:<OuName>] /p:<policy>] | /i:<computerlist>] [/o:<resultdir>] [/u:<username>] [/pw:<password>] [/t:<threads>] [/l] [/e]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /m`<computername>` | Especifica el nombre NetBIOS, el nombre DNS o la dirección IP del equipo que se va a analizar. Si se especifica el parámetro **/m** , también se debe especificar el parámetro **/p** . |
| /ou`<OuName>` | Especifica el nombre de dominio completo (FQDN) de una unidad organizativa (OU) en Active Directory Domain Services. Si se especifica el parámetro **/ou** , también se debe especificar el parámetro **/p** . Todos los equipos de la unidad organizativa se analizarán con la Directiva especificada. |
| /p`<policy>` | Especifica la ruta de acceso y el nombre del archivo de directiva. XML que se va a usar para realizar el análisis. |
| /i`<computerlist>` | Especifica la ruta de acceso y el nombre de un archivo. XML que contiene una lista de equipos junto con los archivos de directivas esperados. Todos los equipos del archivo. XML se analizarán con los archivos de directiva correspondientes. Un archivo. XML de ejemplo es `%windir%\security\SampleMachineList.xml` . |
| /o`<resultdir>` | Especifica la ruta de acceso y el directorio donde se deben guardar los archivos de resultados de análisis. El valor predeterminado es el directorio actual. |
| /u`<username>` | Especifica una credencial de usuario alternativa para usar al realizar el análisis en un equipo remoto. El valor predeterminado es el usuario que ha iniciado sesión. |
| /PW`<password>` | Especifica una credencial de usuario alternativa para usar al realizar el análisis en un equipo remoto. El valor predeterminado es la contraseña del usuario que ha iniciado sesión. |
| /t:`<threads>` | Especifica el número de operaciones de análisis pendientes simultáneas que se deben mantener durante el análisis. El intervalo de valores es 1-1000, con un valor predeterminado de 40. |
| /l | Hace que se registre el proceso de análisis. Se generará un archivo de registro para cada equipo que se está analizando. Los archivos de registro se almacenarán en el mismo directorio que los archivos de resultados. Use la opción **/o** para especificar el directorio de los archivos de resultados. |
| /e | Registra un evento en el registro de eventos de la aplicación si se encuentra un error de coincidencia. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para analizar una directiva de seguridad con el *webpolicy.xml*de archivos, escriba:

```
scwcmd analyze /p:webpolicy.xml
```

Para analizar una directiva de seguridad en el equipo denominado *WebServer* en el *webpolicy.xml* de archivos mediante las credenciales de la cuenta *WebAdmin* , escriba:

```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin
```

Para analizar una directiva de seguridad con el *webpolicy.xml*de archivo, con un *máximo de 100 subprocesos*y generar los resultados en un archivo denominado resultados en el recurso compartido *resultserver* , escriba:

```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results
```

Para analizar una directiva de seguridad para la *unidad organizativa WebServers* con el *webpolicy.xml* de archivos mediante *las credenciales* del ID. de servidor, escriba:

```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando scwcmd configure](scwcmd-configure.md)

- [comando scwcmd Register](scwcmd-register.md)

- [comando de restauración de scwcmd](scwcmd-rollback.md)

- [scwcmd transform (comando)](scwcmd-transform.md)

- [comando scwcmd View](scwcmd-view.md)