---
title: Uso de PowerShell de la extensión
description: Uso de PowerShell de la extensión SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b1be4fe7639d913243cc28371dff9e98e0f5827e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296727"
---
# Uso de PowerShell de la extensión #

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Vamos a profundizar más en el SDK de extensiones de Windows Admin Center: Hablemos sobre cómo agregar comandos de PowerShell para la extensión.

## PowerShell en TypeScript ##

El proceso de compilación gulp tiene un paso de generar que va a realizar cualquier ```{!ScriptName}.ps1``` que se coloca en el ```\src\resources\scripts``` carpeta y compilarlos en el ```powershell-scripts``` clase bajo el ```\src\generated``` carpeta.

>[!NOTE] 
> No actualizar manualmente el ```powershell-scripts.ts``` ni la ```strings.ts``` archivos. Se sobrescribirá cualquier cambio realizado en la siguiente generar.

## Ejecutar un Script Powerhell ##
Los scripts que desea ejecutar en un nodo se pueden colocar en ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Los cambios que se realicen en un ```{!ScriptName}.ps1``` archivo no se reflejarán en el proyecto, a menos que generar 

La API funciona creando primero una sesión de PowerShell en los nodos que está destinados a, crear el script de PowerShell con los parámetros que deben pasarse en y, a continuación, ejecuta el script en las sesiones que se crearon.

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
Por último, necesitamos ejecutar comandos en la sesión que creamos:
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
Ahora necesitaremos para suscribirse a la función observable que acabamos de crear. Esto coloca donde debe llamar a la función para ejecutar el script de PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
Proporcionando el nombre del nodo al método createSession, que se crea una nueva sesión de PowerShell, usa y, a continuación, se destruye inmediatamente tras la finalización de la llamada de PowerShell. 

### Opciones de teclas ###
Algunas opciones están disponibles cuando se llama a la API de PowerShell. Cada vez que se crea una sesión se puede crear con o sin una clave. 

**Clave:** Esto crea una sesión con clave que se puede buscar y volver a usar, incluso en componentes (es decir, que Component1 puede crear una sesión con la clave "SME ROCAS" y Component2 usar esa misma sesión). Si se proporciona una clave, la sesión que se crea debe eliminarse de llamar a Dispose() como ocurría en el ejemplo anterior. No debe mantenerse en una sesión sin que se va a eliminarse de más de cinco minutos. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Sin llave:** Se creará automáticamente una clave de la sesión. En esta sesión con eliminarse automáticamente después de 3 minutos. Sin llaves permite la extensión para el uso de cualquier tipo que ya está disponible en el momento de creación de una sesión de reciclaje. Si hay ningún espacio disponible que se creará una nueva. Esta funcionalidad es buena para las llamadas de uso único, pero usa repetido puede afectar al rendimiento. Una sesión tarda aproximadamente 1 segundo para crear, por lo tanto, continuamente reciclaje sesiones puede causar retrasos.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
o bien 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
En la mayoría de los casos, crear una sesión con clave en el ```ngOnInit()``` método y, a continuación, elimínela en ```ngOnDestroy()```. Sigue este patrón cuando hay varios scripts de PowerShell en un componente, pero la sesión subyacente que es no se comparten entre los componentes.
Para obtener resultados óptimos, asegúrate de creación de la sesión se administra dentro de los componentes en lugar de servicios: Esto ayuda a garantiza que del ciclo de vida y limpieza puede administrarse correctamente.

Para obtener resultados óptimos, asegúrate de creación de la sesión se administra dentro de los componentes en lugar de servicios: Esto ayuda a garantiza que del ciclo de vida y limpieza puede administrarse correctamente.

### Secuencia de PowerShell ###
Si tienes un script y los datos de larga duración se va a producir progresivamente, que una secuencia de PowerShell te permitirá procesar los datos sin tener que esperar a la secuencia de comandos Finalizar. Se llamará el next() observable tan pronto como se reciben datos.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### Scripts de ejecución larga ###
Si tienes un script de larga duración que te gustaría ejecutar en segundo plano, se puede enviar un elemento de trabajo. El estado de la secuencia de comandos se realizará la puerta de enlace y se pueden enviar actualizaciones al estado a una notificación. 
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
> Para que el progreso que se muestre, Write-Progress deben incluirse en la secuencia de comandos que ha escrito. Por ejemplo:
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### Opciones de elemento de trabajo ####

| function | Explicación |
| ----- | ----------- |
| Submit() | Envía el elemento de trabajo 
| submitAndWait() | Enviar el elemento de trabajo y espera a que la finalización de su ejecución
| Wait() | Esperar para el trabajo existente elemento para finalizar
| Query() | Consulta de un elemento de trabajo existente por Id.
| Find() | Busca y existente de elemento de trabajo por TargetNodeName, nombreMódulo o typeId.

### Las API de PowerShell lote ###
Si necesitas ejecutar la misma secuencia en varios nodos, puede usarse una sesión de PowerShell de lote. Por ejemplo:
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


#### Opciones de PowerShellBatch ####
| opción | Explicación |
| ----- | ----------- |
| runSingleCommand | Ejecutar un único comando en todos los nodos de la matriz 
| ejecutar | Ejecuta el comando correspondiente en el nodo emparejado
| Cancelar | Cancelar el comando en todos los nodos de la matriz