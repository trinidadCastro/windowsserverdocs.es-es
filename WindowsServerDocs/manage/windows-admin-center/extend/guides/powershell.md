---
title: Uso de PowerShell de la extensión
description: Uso de PowerShell en la extensión SDK del centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.openlocfilehash: 2dccdeebafc5adc7ea391b0e8d62176a62189606
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944895"
---
# <a name="using-powershell-in-your-extension"></a>Uso de PowerShell de la extensión #

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Vamos a profundizar en el SDK de extensiones del centro de administración de Windows. vamos a hablar sobre cómo agregar comandos de PowerShell a la extensión.

## <a name="powershell-in-typescript"></a>PowerShell en TypeScript ##

El proceso de compilación de Gulp tiene un paso de generación que tomará cualquier ```{!ScriptName}.ps1``` que se coloque en la ```\src\resources\scripts``` carpeta y los compilará en la ```powershell-scripts``` clase bajo la ```\src\generated``` carpeta.

>[!NOTE]
> No actualice manualmente ```powershell-scripts.ts``` ni los ```strings.ts``` archivos. Cualquier cambio que realice se sobrescribirá en la siguiente generación.

## <a name="running-a-powershell-script"></a>Ejecución de un script de PowerShell ##
Cualquier script que desee ejecutar en un nodo se puede colocar en ```\src\resources\scripts\{!ScriptName}.ps1``` .
>[!IMPORTANT]
> Los cambios efectuados en un ```{!ScriptName}.ps1``` archivo no se reflejarán en el proyecto hasta que se haya ```gulp generate``` ejecutado.

La API funciona creando primero una sesión de PowerShell en los nodos de destino, creando el script de PowerShell con los parámetros que se deben pasar y ejecutando el script en las sesiones que se crearon.

Por ejemplo, tenemos este script ```\src\resources\scripts\Get-NodeName.ps1``` :
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
A continuación, crearemos el script de PowerShell con un parámetro de entrada:
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Por último, necesitamos ejecutar ese script en la sesión que hemos creado:
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
Ahora se debe suscribir a la función observable que acabamos de crear. Colóquelo en la ubicación en la que necesita llamar a la función para ejecutar el script de PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
Al proporcionar el nombre de nodo al método createSession, se crea una nueva sesión de PowerShell, se usa y, a continuación, se destruye inmediatamente después de la finalización de la llamada de PowerShell.

### <a name="key-options"></a>Opciones clave ###
Hay algunas opciones disponibles cuando se llama a la API de PowerShell. Cada vez que se crea una sesión, se puede crear con o sin una clave.

**Clave:** Esto crea una sesión con clave que se puede buscar y reutilizar, incluso entre los componentes (lo que significa que Component1 puede crear una sesión con la clave "SME-ROCKs" y Component2 puede usar esa misma sesión). Si se proporciona una clave, la sesión creada debe desecharse llamando a Dispose () como se hizo en el ejemplo anterior. No se debe mantener una sesión sin que se elimine durante más de 5 minutos.
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

Sin **llave:** Se creará automáticamente una clave para la sesión. Esta sesión con se eliminará automáticamente después de 3 minutos. El uso de la entrada sin llave permite que la extensión recicle el uso de cualquier espacio de ejecución que ya esté disponible en el momento de la creación de una sesión. Si no hay ningún espacio de ejecución disponible, se creará uno nuevo. Esta funcionalidad es adecuada para las llamadas de un solo uso, pero el uso repetido puede afectar al rendimiento. Una sesión tarda aproximadamente un segundo en crearse, de modo que las sesiones que se reciclan continuamente pueden provocar ralentizaciones.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
o
``` ts
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
En la mayoría de los casos, cree una sesión con clave en el ```ngOnInit()``` método y, a continuación, deseche en ```ngOnDestroy()``` . Siga este patrón cuando haya varios scripts de PowerShell en un componente, pero la sesión subyacente no se comparta entre los componentes.
Para obtener los mejores resultados, asegúrese de que la creación de la sesión se administra dentro de componentes en lugar de servicios. esto ayuda a garantizar que la duración y la limpieza se pueden administrar correctamente.

Para obtener los mejores resultados, asegúrese de que la creación de la sesión se administra dentro de componentes en lugar de servicios. esto ayuda a garantizar que la duración y la limpieza se pueden administrar correctamente.

### <a name="powershell-stream"></a>Secuencia de PowerShell ###
Si tiene un script de ejecución prolongada y los datos se generan de forma progresiva, un flujo de PowerShell le permitirá procesar los datos sin tener que esperar a que finalice el script. Se llamará al siguiente objeto observable () en cuanto se reciban los datos.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>Scripts de larga ejecución ###
Si tiene un script de larga ejecución que le gustaría ejecutar en segundo plano, se puede enviar un elemento de trabajo. La puerta de enlace realizará el seguimiento del estado del script y las actualizaciones del estado se pueden enviar a una notificación.
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
> Para que se muestre el progreso, la escritura se debe incluir en el script que ha escrito. Por ejemplo:
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!' -percentComplete 95
>```

#### <a name="workitem-options"></a>Opciones de elemento de trabajo ####

| function | Explicación |
| ----- | ----------- |
| enviar () | Envía el elemento de trabajo.
| submitAndWait() | Enviar el elemento de trabajo y esperar a que se complete la ejecución
| Wait () | Esperar a que se complete el elemento de trabajo existente
| consulta () | Consulta de un elemento de trabajo existente por identificador
| buscar () | Busque el elemento de trabajo existente por TargetNodeName, ModuleName o typeId.

### <a name="powershell-batch-apis"></a>API de batch de PowerShell ###
Si necesita ejecutar el mismo script en varios nodos, se puede usar una sesión de PowerShell de batch. Por ejemplo:
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
| runSingleCommand | Ejecutar un solo comando en todos los nodos de la matriz
| Ejecutar | Ejecutar comando correspondiente en el nodo emparejado
| cancel | Cancelar el comando en todos los nodos de la matriz
