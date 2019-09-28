---
title: Scwcmd ANALYZE
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 14426ae33144ae9bdd8f8154b4be74a3f088606b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371265"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Determina si un equipo cumple con una directiva. Los resultados se devuelven en un archivo. Xml. También acepta una lista de nombres de equipo como entrada. Para ver los resultados en el explorador, use **scwcmd View** y especifique **%WINDIR%\security\msscw\TransformFiles\scwanalysis.xsl** como la transformación. Xsl. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/m: @no__t 0ComputerName >|Especifica el nombre NetBIOS, el nombre DNS o la dirección IP del equipo que se va a analizar. Si se especifica el parámetro **/m** , también se debe especificar el parámetro **/p** .|
|/ou: \<OuName >|Especifica el nombre de dominio completo (FQDN) de una unidad organizativa (OU) en Active Directory Domain Services. Si se especifica el parámetro **/ou** , también se debe especificar el parámetro **/p** . Todos los equipos de la unidad organizativa se analizarán con la Directiva especificada.|
|/p: @no__t 0Policy >|Especifica la ruta de acceso y el nombre del archivo de directiva. XML que se va a usar para realizar el análisis.|
|/i: @no__t 0ComputerList >|Especifica la ruta de acceso y el nombre de un archivo. XML que contiene una lista de equipos junto con los archivos de directivas esperados. Todos los equipos del archivo. XML se analizarán con los archivos de directiva correspondientes. Un archivo. XML de ejemplo es%windir%\security\SampleMachineList.xml.|
|/o: @no__t 0ResultDir >|Especifica la ruta de acceso y el directorio donde se deben guardar los archivos de resultados de análisis. El valor predeterminado es el directorio actual.|
|/u: @no__t 0UserName >|Especifica una credencial de usuario alternativa para usar al realizar el análisis en un equipo remoto. El valor predeterminado es el usuario que ha iniciado sesión.|
|/PW: \<Password >|Especifica una credencial de usuario alternativa para usar al realizar el análisis en un equipo remoto. El valor predeterminado es la contraseña del usuario que ha iniciado sesión.|
|/t: \<Threads >|Especifica el número de operaciones de análisis pendientes simultáneas que se deben mantener durante el análisis (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/l|Hace que se registre el proceso de análisis. Se generará un archivo de registro para cada equipo que se está analizando. Los archivos de registro se almacenarán en el mismo directorio que los archivos de resultados. Use la opción **/o** para especificar el directorio de los archivos de resultados.|
|/e|Registra un evento en el registro de eventos de la aplicación si se encuentra un error de coincidencia.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Scwcmd. exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Example

Para analizar una directiva de seguridad con el archivo webpolicy. XML, escriba:
```
scwcmd analyze /p:webpolicy.xml

```
Para analizar una directiva de seguridad en el equipo denominado WebServer en el archivo webpolicy. XML con las credenciales de la cuenta WebAdmin, escriba:
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
Para analizar una directiva de seguridad con el archivo webpolicy. XML, con un máximo de 100 subprocesos y generar los resultados en un archivo denominado resultados en el recurso compartido resultserver, escriba:
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
Para analizar una directiva de seguridad para la unidad organizativa WebServers en el archivo webpolicy. XML con las credenciales del ID. de servidor, escriba:
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)