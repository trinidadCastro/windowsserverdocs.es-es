---
title: Analizar scwcmd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce3428b281ede582ed781afbdee9dea495b52ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873916"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Determina si un equipo es conforme a una directiva. Los resultados se devuelven en un archivo .xml. También acepta una lista de nombres de equipo como entrada. Para ver los resultados en el explorador, use **scwcmd vista** y especifique **%windir%\security\msscw\TransformFiles\scwanalysis.xsl** como la transformación XSL. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ m:\<nombreDeEquipo >|Especifica el nombre NetBIOS, nombre DNS o dirección IP del equipo para analizar. Si el **/m** parámetro se especifica, el **/p** también debe especificarse el parámetro.|
|/ou:\<OuName>|Especifica el nombre de dominio completo (FQDN) de una unidad organizativa (OU) en Active Directory Domain Services. Si el **/OU** parámetro se especifica, el **/p** también debe especificarse el parámetro. Todos los equipos de la unidad organizativa se analizará en la directiva especificada.|
|/ p:\<directiva >|Especifica la ruta de acceso y el nombre del archivo de directiva .xml que se usará para realizar el análisis.|
|/ i:\<lista de equipos >|Especifica la ruta de acceso y el nombre de un archivo .xml que contiene una lista de equipos junto con sus archivos de directiva esperado. Todos los equipos en el archivo .xml se analizará en sus archivos de directiva correspondientes. Un archivo .xml de ejemplo es % windir%\security\SampleMachineList.xml.|
|/ o:\<ResultDir >|Especifica la ruta de acceso y el directorio donde se deben guardar los archivos de resultados del análisis. El valor predeterminado es el directorio actual.|
|/ u:\<nombre de usuario >|Especifica una credencial de usuario alternativa que utilizará al realizar el análisis en un equipo remoto. El valor predeterminado es el inicio de sesión de usuario.|
|/ pw:\<contraseña >|Especifica una credencial de usuario alternativa que utilizará al realizar el análisis en un equipo remoto. El valor predeterminado es la contraseña del usuario con sesión iniciada.|
|/ t:\<subprocesos >|Especifica el número de operaciones simultáneas análisis pendientes que deben mantenerse durante el análisis (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/l|Hace que el proceso de análisis que se registre. Se generará un archivo de registro para cada equipo que se está analizando. En el mismo directorio que los archivos de resultados se almacenarán los archivos de registro. Use la **/o** opción para especificar el directorio para los archivos de resultados.|
|/e|Registrar un evento en el registro de eventos de aplicación si se encuentra un error de coincidencia.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Ejemplos

Para analizar una directiva de seguridad contra los webpolicy.xml de archivo, escriba:
```
scwcmd analyze /p:webpolicy.xml

```
Para analizar una directiva de seguridad en el equipo llamado servidor Web en el archivo de webpolicy.xml con las credenciales de la cuenta de webadmin, escriba:
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
Para analizar una directiva de seguridad en el archivo webpolicy.xml, con un máximo de 100 subprocesos y los resultados en un archivo denominado resultados en el recurso compartido de resultserver, escriba:
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
Para analizar una directiva de seguridad para la OU WebServers contra el webpolicy.xml archivo utilizando las credenciales de administrador de dominio, escriba:
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)