---
title: Uso de PowerShell de la extensión
description: Uso de PowerShell en el SDK de Windows Admin Center (proyecto Honolulu) de la extensión
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7375732fd464519cd1533043d271065e488fd46a
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621357"
---
# <a name="using-powershell-in-your-extension"></a>Uso de PowerShell de la extensión #

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Vamos allá más detallada en el SDK de extensiones de Windows Admin Center: vamos a hablar acerca de cómo agregar comandos de PowerShell para la extensión.

## <a name="powershell-in-typescript"></a>PowerShell en TypeScript ##

El proceso de compilación gulp tiene un paso de generar que va a realizar cualquier ```{!ScriptName}.ps1``` que se coloca en el ```\src\resources\scripts``` carpeta y compilarlos en la ```powershell-scripts``` clase bajo el ```\src\generated``` carpeta.

>[!NOTE] 
> No se actualiza manualmente el ```powershell-scripts.ts``` ni ```strings.ts``` archivos. Se sobrescribirá cualquier cambio realizado en la siguiente generar.

## <a name="running-a-powershell-script"></a>Ejecutar un Script de PowerShell ##
Los scripts que desea ejecutar en un nodo pueden colocarse en ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Los cambios que realice en un ```{!ScriptName}.ps1``` no se reflejarán en el proyecto hasta el archivo ```gulp generate``` se ha ejecutado.

La API funciona creando primero una sesión de PowerShell en los nodos de destino, crear el script de PowerShell con los parámetros que deben pasarse en y, a continuación, ejecutar el script en las sesiones que se crearon.

Por ejemplo, tenemos este script ```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Se creará una sesión de PowerShell para el nodo de destino:
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
A continuación, se creará el script de PowerShell con un parámetro de entrada:
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Por último, necesitamos ejecutar ese script en la sesión que se creó:
``` ts
  public ngOnInit(): void {
    this.session = this.appContextService.powerShell.createAutomaticSession('{!TargetNode}');
  }

  public getNodeName(): Observable<any> {
    const script = PowerShell.createScript(PowerShellScripts.Get_NodeName.script, { stringFormat: 'The name of the node is {0}!'});
    return this.appContextService.powerShell.run(this.session, script)
    .pipe(
        map(
        response => {
            if (response && response.results) {
                return response.results;
            }
            return 'no response';
        }
      ) 
    );
  }

  public ngOnDestroy(): void {
    this.session.dispose()
  }

```
Ahora necesitamos suscribirse a la función observable que acabamos de crear. Coloque la donde debe llamar a la función para ejecutar el script de PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
Proporcionando el nombre del nodo al método createSession una nueva sesión de PowerShell se crea, utiliza y, a continuación, se destruye inmediatamente tras la finalización de la llamada de PowerShell. 

### <a name="key-options"></a>Opciones de clave ###
Algunas opciones están disponibles cuando se llama a la API de PowerShell. Cada vez que se crea una sesión se puede crear con o sin una clave. 

**Clave:** Esto crea una sesión con clave que se puede buscar y reutilizar, incluso en componentes (es decir, que Component1 puede crear una sesión con la clave "SME-ROCKS" y Component2 puede usar esa misma sesión). Si se proporciona una clave, la sesión que se crea debe eliminarse de Dispose() que realiza la llamada como se hizo en el ejemplo anterior. No se debe mantener una sesión sin desecharlos de más de 5 minutos. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Sin llaves:** Automáticamente se creará una clave para la sesión. Esta sesión con se elimine automáticamente después de 3 minutos. El uso sin llaves, permite la extensión reciclar el uso de cualquier espacio de ejecución que ya está disponible en el momento de creación de una sesión. Si no hay espacio de ejecución está disponible en el que se creará una nueva. Esta funcionalidad es buena para llamadas de uso único, pero el uso repetido puede afectar al rendimiento. Una sesión tarda aproximadamente 1 segundo para crear, reciclar continuamente por lo que las sesiones puede provocar una ralentización.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
o bien 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
En la mayoría de los casos, crear una sesión con clave en el ```ngOnInit()``` método y, a continuación, elimínela en ```ngOnDestroy()```. Siga este patrón cuando hay varios scripts de PowerShell en un componente, pero la sesión subyacente que es no se comparten entre los componentes.
Para obtener mejores resultados, asegúrese de crear la sesión se administra dentro de los componentes en lugar de servicios: Esto permite garantizar esa duración y limpieza puede administrarse correctamente.

Para obtener mejores resultados, asegúrese de crear la sesión se administra dentro de los componentes en lugar de servicios: Esto permite garantizar esa duración y limpieza puede administrarse correctamente.

### <a name="powershell-stream"></a>Stream de PowerShell ###
Si tiene una larga secuencia de comandos y los datos se genera de forma progresiva, que una secuencia de PowerShell, podrá procesar los datos sin tener que esperar a que finalice la secuencia de comandos. Se llamará el observable next() en cuanto se reciben datos.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>Scripts de ejecución prolongada ###
Si tiene una larga secuencia de comandos que le gustaría ejecutar en segundo plano, se puede enviar un elemento de trabajo. Se realizará un seguimiento del estado de la secuencia de comandos de la puerta de enlace y el estado de las actualizaciones se pueden enviar a una notificación. 
```ts
const workItem: WorkItemSubmitRequest = {
    typeId: 'Long Running Script',
    objectName: 'My long running service',
    powerShellScript: script,
    
    //in progress notifications
    inProgressTitle: 'Executing long running request',
    startedMessage: 'The long running request has been started',
    progressMessage: 'Working on long running script – {{ percent }} %',

    //success notification
    successTitle: 'Successfully executed a long running script!',
    successMessage: '{{objectName}} was successful',
    successLinkText: 'Bing',
    successLink: 'http://www.bing.com',
    successLinkType: NotificationLinkType.Absolute,

    //error notification
    errorTitle: 'Failed to execute long running script',
    errorMessage: 'Error: {{ message }}'

    nodeRequestOptions: {
       logAudit: true,
       logTelemetry: true
    }
};

return this.appContextService.workItem.submit('{!TargetNode}', workItem);
```

>[!NOTE] 
> Para que se mostrará el progreso, Write-Progress deben incluirse en la secuencia de comandos que se ha escrito. Por ejemplo:
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### <a name="workitem-options"></a>Opciones de elemento de trabajo ####

| function | Explicación |
| ----- | ----------- |
| submit() | Envía el elemento de trabajo 
| submitAndWait() | Enviar el elemento de trabajo y esperar la finalización de su ejecución.
| wait() | Espere a que un trabajo existente elemento completar
| Query() | Consulta de un elemento de trabajo existente por Id.
| find() | Busque y existente de elemento de trabajo con TargetNodeName, ModuleName o typeId.

### <a name="powershell-batch-apis"></a>API de Batch PowerShell ###
Si tiene que ejecutar la misma secuencia de comandos en varios nodos, puede usar una sesión de PowerShell de batch. Por ejemplo:
```ts
const batchSession = this.appContextService.powerShell.createBatchSession(
    ['{!TargetNode1}', '{!TargetNode2}', sessionKey);
  this.appContextService.powerShell.runBatchSingleCommand(batchSession, command).subscribe((responses: PowerShellBatchResponseItem[]) => {
    for (const response of responses) { 
      if (response.error || response.errors) {
        //handle error
      } else {
        const results = response.properties && response.properties.results;
        //response.nodeName
        //results[0]
      }
    }
     },
     Error => { /* handle error */ });  

```


#### <a name="powershellbatch-options"></a>Opciones de PowerShellBatch ####
| Opción | Explicación |
| ----- | ----------- |
| runSingleCommand | Ejecutar un comando único en todos los nodos de la matriz 
| ejecutar | Ejecute el comando correspondiente en el nodo emparejado
| Cancelar | Cancelar el comando en todos los nodos de la matriz
