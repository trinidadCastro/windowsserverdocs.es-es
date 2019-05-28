---
ms.assetid: 8b15d44e-e4e6-4510-aa91-cc7ec7161b0a
title: El papel del motor de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b69277cdedd697605f57aa4cf7214f5b65bb2e81
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188469"
---
# <a name="the-role-of-the-claims-engine"></a>El papel del motor de notificaciones
En su nivel más alto, el motor de notificaciones en servicios de federación de Active Directory \(AD FS\) es una regla\-según el motor que se dedica a servir y procesar las solicitudes de notificación para el servicio de federación. El motor de notificaciones es la única entidad dentro del Servicio de federación que se encarga de ejecutar cada uno de los conjuntos de reglas en todas las relaciones de confianza federadas que ha configurado y de entregar el resultado de salida a la canalización de notificaciones.  
  
Mientras que la canalización de notificaciones es más un concepto lógico del extremo\-a\-terminar el proceso de flujo de notificaciones, las reglas son un elemento administrativo reales que puede usar para personalizar el flujo de notificaciones durante el proceso de ejecución de reglas de notificación de notificación. Para más información sobre el proceso de canalización, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
Como se muestra en la siguiente ilustración, el hecho de aceptar notificaciones entrantes \(reglas de aceptación\), autorizar a los solicitantes de notificaciones \(las reglas de autorización\) y emitir notificaciones salientes \( reglas de emisión\) a través de la notificación de las reglas en todas las relaciones de confianza federada en su organización se realiza mediante el motor de notificaciones.  
  
![Roles de AD FS](media/adfs2_enginepipeline.gif)  
  
## <a name="claim-rules-execution-process"></a>Proceso de ejecución de reglas de notificación  
Cuando se configura una relación de confianza de proveedor de notificaciones o de confianza en su organización con reglas de notificación, Establece la regla de notificación\(s\) esa confianza actuar como equipo selector para las notificaciones entrantes al invocar el motor de notificaciones para aplicar las necesarias lógica en las reglas de notificación para determinar si se emiten notificaciones y qué notificaciones emitirá.  
  
En la siguiente sección se describen los pasos que se producen en el motor durante el flujo de notificaciones a través del proceso de ejecución de reglas de notificación. Cada uno de los pasos que se detallan a continuación se produce en cada fase que se describe en el proceso de canalización de notificaciones. Estos pasos incluyen:  
  
-   Paso 1: inicialización.  
  
-   Paso 2: ejecución.  
  
-   Paso 3: resultado de la ejecución.  
  
Para más información sobre el proceso de canalización, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="step-1--initialization"></a>Paso 1: inicialización.  
En el primer paso del proceso de ejecución de reglas de notificación, el motor de notificaciones acepta notificaciones entrantes agregándolas primero al *conjunto de notificaciones de entrada*. Un conjunto de notificaciones de entrada es similar a una memoria caché en cuanto a que la memoria se usa para almacenar temporalmente datos únicamente cuando un proceso requiere que los datos estén disponibles para su recuperación. Los datos del conjunto de notificaciones de entrada se descartan cuando finaliza la ejecución de la regla.  
  
#### <a name="adding-a-claim-to-the-input-claim-set-for-a-rule-set"></a>Adición de una notificación al conjunto de notificaciones de entrada de un conjunto de reglas  
El motor de notificaciones crea el conjunto de notificaciones de entrada cuando necesita almacenar temporalmente datos de notificación en la memoria mientras procesa la lógica asociada a un conjunto de reglas de notificación. El motor de notificaciones copia todas las notificaciones entrantes al conjunto de notificaciones de entrada donde se pueden recuperar mediante la primera regla del conjunto de reglas.  
  
Por ejemplo, en la ilustración siguiente, el motor de notificaciones lee las notificaciones de A y B a partir de las notificaciones entrantes y las copia en el conjunto de notificaciones de entrada. Cuando se encuentran en el conjunto de notificaciones de entrada, el motor de notificaciones recupera y procesa las notificaciones A y B como entradas para la lógica en la primera regla del conjunto de reglas de notificación.  
  
![Roles de AD FS](media/adfs2_context1.gif)  
  
Todas las reglas de un conjunto de reglas de notificación comparten el mismo conjunto de notificaciones de entrada. Cada regla de ese conjunto puede agregarse al conjunto de notificaciones de entrada compartida, por lo que afectará a todas las reglas sucesivas del conjunto.  
  
### <a name="step-2--execution"></a>Paso 2: ejecución.  
En este paso del proceso de las reglas de notificación, estas se procesan cuando el motor de notificaciones recorre cronológicamente todas las reglas dentro de un conjunto de reglas determinado, una a una. Cada regla dentro de un conjunto de reglas solo se ejecuta una vez y se ejecuta en el orden en que aparecen de arriba a abajo como se muestra en el cuadro de diálogo Editar reglas de notificación en el complemento Administración de AD FS\-en. La regla de notificación que se encuentra en la parte superior del conjunto de reglas se procesa en primer lugar y, luego, se procesan las reglas posteriores hasta que todas se hayan ejecutado.  
  
Tal y como se define en el lenguaje de reglas de notificación, una regla de notificación consta de dos partes: la condición y la instrucción de emisión. Conjunto de notificaciones de los procesos primera del motor de notificaciones conjunto para determinar si cumple la condición especificada dentro de la regla para las notificaciones contenidas en la entrada de notificaciones de la parte de la condición mediante el uso de los datos en la entrada \(las notificaciones que coinciden con la regla condición se conocen como notificaciones coincidentes\). Si no se encuentran notificaciones coincidentes, el motor de notificaciones ejecuta la instrucción de emisión de la regla para cada conjunto de las notificaciones coincidentes. La instrucción de emisión de la regla puede realizar cualquiera de las siguientes tareas con notificaciones coincidentes:  
  
1.  Copiar una notificación coincidente en el conjunto de notificaciones de salida.  
  
2.  Transformar los campos de notificación y crear una nueva notificación o bien solo en el conjunto de notificaciones de entrada, o bien en los conjuntos de notificaciones de salida y de evaluación.  
  
3.  Usar la notificación coincidente\(s\) como clave para buscar más información de un almacén de atributos para crear la nueva notificación\(s\) en simplemente la entrada de conjunto de notificaciones o tanto de entrada y salida en conjuntos de notificaciones.  
  
#### <a name="adding-a-claim-to-the-output-claim-set-for-a-rule-set"></a>Adición de una notificación al conjunto de notificaciones de salida para un conjunto de reglas  
El *conjunto de notificaciones de salida* es una ubicación de memoria que inicialmente está vacía y tiene una gran importancia, ya que el motor de notificaciones solo devolverá notificaciones que residan en el conjunto de notificaciones de salida cuando finalice el proceso de ejecución. Esto significa que las notificaciones que residan únicamente en el conjunto de notificaciones de entrada y no en el conjunto de notificaciones de salida se omitirán cuando llegue el momento de calcular el conjunto final de notificaciones salientes.  
  
#### <a name="adding-a-claim-to-both-claim-sets-for-a-rule-set"></a>Adición de una notificación a ambos conjuntos de notificaciones para un conjunto de reglas  
A medida que se procesa una regla, las notificaciones se agregan solo al conjunto de notificaciones de entrada, o tanto al de entrada como al de salida, según la instrucción que se use en la instrucción de emisión de la regla. El lenguaje de reglas de notificación se refiere a estas instrucciones como *add* o *issue*.  
  
Si se usa la instrucción *add* , las notificaciones se agregan solo al conjunto de notificaciones de entrada y dichas notificaciones existirán únicamente para los fines de la ejecución y dejarán de existir una vez finalice la misma. Si se usa la instrucción *issue*, las notificaciones se agregan tanto al conjunto de notificaciones de entrada como al de salida y se devuelven en el conjunto de notificaciones de salida cuando finalice la ejecución. Para más información sobre estas instrucciones, consulte [The Role of the Claim Rule Language](The-Role-of-the-Claim-Rule-Language.md).  
  
Si la parte de la condición de una regla dentro de un conjunto de reglas no coincide con ninguna notificación del conjunto de notificaciones de entrada, la parte de la instrucción de emisión de la regla se omite y, por tanto, no se agrega ninguna notificación al conjunto de notificaciones de salida ni al de entrada. En la siguiente ilustración y sus pasos correspondientes se muestra lo que sucede cuando el motor de notificaciones ejecuta una regla de transformación:  
  
![Roles de AD FS](media/adfs2_context2.gif)  
  
1.  El motor de notificaciones agrega las notificaciones entrantes al conjunto de notificaciones de entrada.  
  
2.  Cuando se ejecuta la primera regla, ve las notificaciones A y B, que en ese momento son las únicas notificaciones en el conjunto de notificaciones de entrada, y procesa la parte condicional de la lógica de la regla 1.  
  
3.  Puesto que la notificación está presente en el conjunto de notificaciones de entrada, la condición de la regla se determina como verdadera \(coincidente con la notificación A\) y se agrega una nueva notificación C tanto la entrada de conjunto de notificaciones y de salida.  
  
4.  Regla 2 ahora puede usar las notificaciones A, B y C \(todas las notificaciones de la entrada de conjunto de notificaciones\) como entrada para procesar su lógica.  
  
Para más información sobre la transformación de notificaciones, consulte [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).  
  
### <a name="step-3--execution-result"></a>Paso 3: resultado de la ejecución.  
La fase final de la ejecución del conjunto de reglas de notificación comienza cuando se han ejecutado todas las reglas dentro de un conjunto de reglas determinado y el conjunto final de notificaciones está presente en el conjunto de notificaciones de salida. En ese momento, el motor de notificaciones devuelve el contexto del conjunto de notificaciones de salida como el resultado de la ejecución del conjunto de reglas. A partir de ese punto, es la canalización de notificaciones la que asume y mueve este resultado final a la siguiente fase en su proceso.  
  
## <a name="sending-the-execution-output-to-the-claims-pipeline"></a>Envío del resultado de la ejecución a la canalización de notificaciones  
Cuando el motor de notificaciones procesa un conjunto de reglas, dicho conjunto de reglas tiene sus propias ubicaciones dedicadas en memoria para sus conjuntos de notificaciones de entrada y salida. Esto significa que los conjuntos de notificaciones de entrada y salida que usa un conjunto de reglas son independientes de los conjuntos de entrada y salida que se utilizan en otro conjunto de reglas.  
  
Después de haberse ejecutado todo el proceso para un conjunto de reglas determinado \(los pasos 1, 2 y 3\), el recién emitido notificaciones salientes \(contenido de la salida del conjunto de notificaciones\) se usará como entrada para el siguiente conjunto de reglas en la canalización de notificaciones. Esto permite que las notificaciones fluyan desde la salida de un conjunto de reglas a la entrada de otro, tal y como se muestra en la ilustración siguiente.  
  
![Roles de AD FS](media/adfs2_enginecontexts.gif)  
  
> [!NOTE]  
> Aunque el conjunto de reglas de emisión también es una fase crítica de la canalización, en la ilustración anterior no se muestra únicamente por motivos de simplificación. Para ver una ilustración en la que se muestra el conjunto de reglas de emisión y cómo encaja en la canalización de notificaciones, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
En este caso, la canalización usa la salida de las reglas de aceptación para llevar el conjunto final de notificaciones producidas por las reglas de aceptación a la segunda fase de la canalización, que es el procesamiento de las reglas de autorización. En este momento, todo el proceso de ejecución de reglas de notificación \(los pasos 1, 2 y 3 anteriores\) ejecutaría de nuevo para el conjunto de reglas de autorización. Este ciclo continúa hasta que la regla de emisión establecida \(la fase final de la canalización\) se ha completado.  
  
Cuando las notificaciones salientes finalizadas se hayan devuelto desde el motor para el conjunto de reglas de emisión, se empaquetarán en un token SAML y el Servicio de federación enviará el token de vuelta al cliente.  
  
## <a name="processing-authorization-rules"></a>Procesamiento de las reglas de autorización  
Si el conjunto de reglas de notificación que se ejecuta durante el paso 2 del proceso de ejecución de reglas de notificación consta de las reglas de autorización \(que tienen una entrada diferente y conjuntos de reglas de aceptación o emisión de notificaciones de salida\), a continuación, los se ejecutarán las reglas de autorización para determinar si un solicitante del token está autorizado para obtener un token de seguridad para un determinado usuario de confianza del servicio de federación basada en notificaciones del solicitante.  
  
El objetivo de las reglas de autorización es emitir una notificación de denegación o de permiso, según si el usuario está habilitado o no para obtener un token para el usuario autenticado proporcionado. Como se muestra en la siguiente ilustración, el resultado de la ejecución de autorización se utiliza la canalización para determinar si se ejecuta el conjunto de reglas de emisión o no, según la presencia o ausencia de la y\/o denegar la notificación, pero la autorización salida de ejecución propia no se utiliza como entrada para el conjunto de reglas de notificación.  
  
![Roles de AD FS](media/adfs2_authorization.gif)  
  
Para más información sobre la autorización de notificaciones, consulte [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).  
  

