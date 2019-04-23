---
title: Uso de PowerShell de la extensión
description: Uso de PowerShell en el SDK de Windows Admin Center (proyecto Honolulu) de la extensión
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ae5004104150c510a56c06161c9280e029968298
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867606"
---
# <a name="using-powershell-in-your-extension"></a>Uso de PowerShell de la extensión #

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Vamos allá más detallada en el SDK de extensiones de Windows Admin Center: vamos a hablar acerca de cómo agregar comandos de PowerShell para la extensión.

## <a name="powershell-in-typescript"></a>PowerShell en TypeScript ##

El proceso de compilación gulp tiene un paso de generar que va a realizar cualquier ". ps1" que se coloca en la carpeta "/ src/recursos/scripts" y compilarlas en la clase "scripts de powershell" en la carpeta "/ src/generado".

>[!NOTE] 
> No actualizar manualmente los archivos "strings.ts" ni "powershell-scripts.ts". Se sobrescribirá cualquier cambio realizado en la siguiente generar.

### <a name="adding-your-own-powershell-script"></a>Agregar su propio script de PowerShell ##

Vamos a agregar más información acerca de cómo agregar su propio script de PowerShell en breve.

### <a name="managing-powershell-sessions"></a>Administración de sesiones de PowerShell ###

Al trabajar con PowerShell en Windows Admin Center, las sesiones son un componente necesario del proceso de ejecución de scripts. Para ejecutar scripts en los servidores administrados remotos, PowerShell usa los espacios de ejecución. Windows Admin Center incluye administración y creación de espacio de ejecución en un objeto PowerShellSession para administrar la vigencia y habilitar la reutilización del espacio de ejecución para la ejecución secuencial de la secuencia de comandos.

Todos los componentes que se necesita para crear una referencia a un objeto de sesión que se crea mediante la clase AppContextService mediante tres opciones distintas:
<!-- I don't 100% get this part - it looks like you're adding 3 arguments - nodeName, <session key>, and <PowerShellSessionRequestOptions>. I got that from looking at the examples, not the text. We need to rework those paras explaining. -->
``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName);
```

Proporcionando el nombre del nodo al método createSession una nueva sesión de PowerShell se crea, utiliza y, a continuación, se destruye inmediatamente tras la finalización de la llamada de PowerShell. Esta funcionalidad es buena para llamadas de uso único, pero el uso repetido puede afectar al rendimiento. Una sesión tarda aproximadamente 1 segundo para crear, reciclar continuamente por lo que las sesiones puede provocar una ralentización.

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>');
```

El primer parámetro opcional es la \<clave de sesión\> parámetro. Esto crea una sesión con clave que se puede buscar y reutilizar, incluso en componentes (es decir, que Component1 puede crear una sesión con la clave "SME-ROCKS" y Component2 puede usar esa misma sesión).  

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>', <PowerShellSessionRequestOptions>);
```
<!-- The second optional parameter is \<PowerShellSessionRequestOptions\> that does ... ? -->
### <a name="common-patterns"></a>Patrones comunes ###

En la mayoría de los casos, crear una sesión con clave en el **ngOnInit** método y, a continuación, elimínela en un **ngOnDestroy**. Siga este patrón cuando hay varios scripts de PowerShell en un componente, pero la sesión subyacente que es no se comparten entre los componentes.

Para obtener mejores resultados, asegúrese de crear la sesión se administra dentro de los componentes en lugar de servicios: Esto permite garantizar esa duración y limpieza puede administrarse correctamente.