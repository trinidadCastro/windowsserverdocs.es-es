---
ms.assetid: 8b15d44e-e4e6-4510-aa91-cc7ec7161b0a
title: El rol del motor de notificaciones
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 930b6f8034f17d8902104419042f944b82e90b4f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claims-engine"></a>El rol del motor de notificaciones
En su nivel más alto, el motor de notificaciones de los servicios de federación de Active Directory \(AD FS\) es un motor basado en rule\ dedicado a proporcionar y procesamiento de solicitudes de notificación para el servicio de federación. El motor de notificaciones es la entidad única dentro de los servicios de federación de que se encarga de ejecute cada uno de los conjuntos de reglas en todas las relaciones de confianza federada que ha configurado y la entrega el resultado a la canalización de reclamaciones.  
  
Mientras la canalización de notificaciones es más lógico concepto de notificaciones el proceso de end\-to\-end para hacer fluir reglas de notificación son un elemento administrativo real que puedes usar para personalizar el flujo de una reclamación durante el proceso de ejecución de las reglas de notificación. Para obtener más información sobre el proceso de la canalización, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
Como se muestra en la siguiente ilustración, la ley de Aceptar \(acceptance rules\) notificaciones entrantes, autorizar notificaciones solicitantes \(authorization rules\) y emitir notificaciones salientes \(issuance rules\) a través de las reglas de notificación a través de todas las relaciones de confianza federada en la organización se realiza mediante el motor de notificaciones.  
  
![Roles de AD FS](media/adfs2_enginepipeline.gif)  
  
## <a name="claim-rules-execution-process"></a>Proceso de ejecución de las reglas de notificación  
Cuando se configura una confianza de proveedor de reclamaciones o de usuario de confianza confiar en la organización con reglas de notificación, la set\(s\) de regla de notificación para que actúen de confianza como un equipo de selector para notificaciones entrantes al invocar el motor de notificaciones para aplicar la lógica necesaria en las reglas de notificación para determinar si va a emitir cualquier reclamación y que dice emitir.  
  
La siguiente sección describe los pasos que se producen por el motor durante el flujo de una reclamación a través del proceso de ejecución de reglas de notificación. Cada uno de los pasos como se describe a continuación se produce para cada fase que se describe en el proceso de la canalización de reclamaciones. Se incluyen estos pasos:  
  
-   Paso 1: inicialización  
  
-   Paso 2: ejecución  
  
-   Paso 3: resultado de ejecución  
  
Para obtener más información sobre el proceso de la canalización, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="step-1--initialization"></a>Paso 1: inicialización  
En el primer paso del proceso de ejecución de reglas de notificación, el motor de reclamaciones acepta notificaciones entrantes, agrégalos a la *conjunto de notificaciones de entrada*. Un conjunto de solicitud de entrada es similar de una memoria caché en la memoria que se usa para almacenar temporalmente datos únicamente cuando un proceso requerido requiere que estarán disponibles para la recuperación de los datos. El conjunto de solicitud de entrada se descartan los datos una vez finalizada la ejecución de la regla.  
  
#### <a name="adding-a-claim-to-the-input-claim-set-for-a-rule-set"></a>Agregar una reclamación a la solicitud de entrada establecido para un conjunto de reglas  
El conjunto de solicitud de entrada se crea mediante el motor de notificaciones cuando sea necesario almacenar temporalmente datos de notificación en la memoria mientras se procesa la lógica asociada a un conjunto de reglas de notificación. El motor de reclamaciones copia todas las notificaciones entrantes al conjunto de solicitud de entrada que se pueden recuperar por la primera regla en el conjunto de reglas.  
  
Por ejemplo, en la siguiente ilustración, el motor de notificaciones lee las notificaciones de A y B desde la entrada de notificaciones y las copia en el conjunto de solicitud de entrada. Después de que se encuentran en el conjunto de solicitud de entrada, el motor de reclamaciones recupera y procesa reclamaciones A y B como entrada para la lógica en la primera regla en el conjunto de reglas de notificación.  
  
![Roles de AD FS](media/adfs2_context1.gif)  
  
Todas las reglas en un conjunto de reglas de Reclamación comparten el mismo conjunto de solicitud de entrada. Cada regla en ese conjunto puede agregar al conjunto de solicitud de entrada compartida, por lo tanto, afectar a todas las reglas posteriores en el conjunto.  
  
### <a name="step-2--execution"></a>Paso 2: ejecución  
En este paso del proceso de reglas de notificación, se procesan las reglas de notificación cuando el motor de reclamaciones cronológicamente recorre todas las reglas dentro de un conjunto de regla particular uno en uno. Cada regla dentro de un conjunto de reglas solo se ejecuta una vez y se ejecuta en el orden en que aparecen de arriba a abajo según se muestra en el cuadro de diálogo Editar reglas de notificación en la administración de AD FS snap\ en. La regla de Reclamación establecido en la parte superior de la regla se procesa primero y, a continuación, se procesan las reglas posteriores hasta que todas las reglas se han ejecutado.  
  
Como se define en el idioma de la regla de notificación, una regla de solicitud consta de dos partes, condición y emisión declaración. Conjunto de notificaciones de los procesos primera motor de reclamaciones conjunto para determinar si la condición especificada en la regla es válida para las notificaciones de la entrada de notificaciones de la parte de la condición mediante el uso de los datos en la entrada \ (las notificaciones que coinciden con la condición de regla se conocen como un claims\ coincidente). Si se encuentra cualquier reclamación coincidente, el motor de reclamaciones ejecuta la declaración de emisión de la regla para cada conjunto de las notificaciones coincidentes. La declaración de emisión de la regla puede realizar cualquiera de las siguientes tareas con reclamaciones coincidentes:  
  
1.  Copia una reclamación de coincidencia en el conjunto de solicitud de salida  
  
2.  Transformar los campos de la reclamación y crear una nueva notificación solo el conjunto de solicitud de entrada o en la evaluación y salida los conjuntos de notificación.  
  
3.  Usa el claim\(s\) coincidente como clave para buscar más información de un almacén de atributo para crear nuevas claim\(s\) en cualquiera solo el conjunto de solicitud de entrada o en la entrada y salida de los conjuntos de notificación.  
  
#### <a name="adding-a-claim-to-the-output-claim-set-for-a-rule-set"></a>Agregar una reclamación a la solicitud de salida establecido para un conjunto de reglas  
La *conjunto de notificaciones de salida* es una ubicación en la memoria que inicialmente está vacío y es importante porque el motor de reclamaciones, solo devolverá los créditos que residen en la solicitud de salida establecer después de finalizar el proceso de ejecución. Esto significa que cualquier reclamación que se encuentran solo en la solicitud de entrada y no en el conjunto de solicitud de salida se omitirá cuando llega el momento de calcular el conjunto final de notificaciones salientes.  
  
#### <a name="adding-a-claim-to-both-claim-sets-for-a-rule-set"></a>Agregar una reclamación a ambos conjuntos de notificaciones para un conjunto de reglas  
Cuando se procesa una regla, las notificaciones son agregado en la entrada conjunto de notificaciones o conjunto de notificaciones en la entrada y salida en función de la instrucción que se usa en la declaración de emisión de la regla de conjunto de solicitud. El idioma de la regla de Reclamación hace referencia a estas instrucciones como *agregar* o *problema*.  
  
Si la *agregar* se usa la declaración, se agregan las notificaciones solo al conjunto de solicitud de entrada y las notificaciones existirán únicamente para los fines de la ejecución y dejarán de existir una vez finalizada la ejecución. Si la *problema* se usa la declaración, se agregan las notificaciones para el conjunto de solicitud de entrada y el conjunto de solicitud de salida y las notificaciones se devuelven en la solicitud de salida establece una vez finalizada la ejecución. Para obtener más información acerca de estas instrucciones, consulta [el rol del lenguaje de regla reclamación](The-Role-of-the-Claim-Rule-Language.md).  
  
Si la parte de la condición de una regla dentro de un conjunto de reglas no coincide con cualquier reclamación en el conjunto de solicitud de entrada, se omitirá la parte de la declaración de emisión de la regla y, por consiguiente, no hay notificaciones se agregan al conjunto de Reclamación de salida o al conjunto de solicitud de entrada. La siguiente ilustración y los pasos correspondientes mostrarán lo que sucede cuando el motor de notificaciones ejecuta una regla de transformación:  
  
![Roles de AD FS](media/adfs2_context2.gif)  
  
1.  Notificaciones entrantes se agregan a la solicitud de entrada establecida por el motor de notificaciones.  
  
2.  Cuando se ejecuta la primera regla, ve las notificaciones A y B, que son en ese momento que conjunto de notificaciones de las notificaciones solo en la entrada, y procesa la parte de la lógica de la regla en la regla 1 condicional.  
  
3.  Dado que la notificación A está presente en el conjunto de solicitud de entrada, se determina el estado de la regla para ser verdad \ (coincidencia de la reclamación A\) y se agrega una nueva notificación C conjunto de notificaciones para la entrada y el conjunto de notificaciones de la salida.  
  
4.  La regla 2 ahora puedes usar las notificaciones A, B y C \ (todas las reclamaciones en la entrada reclamar Set) como entrada para el procesamiento de su lógica.  
  
Para obtener más información acerca de la transformación de notificaciones, consulta [cuándo usar una regla de Reclamación transformar](When-to-Use-a-Transform-Claim-Rule.md).  
  
### <a name="step-3--execution-result"></a>Paso 3: resultado de ejecución  
La fase final de la ejecución de conjunto de reglas de Reclamación comienza una vez que todas las reglas se han ejecutado dentro de un conjunto de reglas determinado y el conjunto final de una reclamación está presente en el conjunto de solicitud de salida. En este punto, el motor de reclamaciones devuelve el contexto de la solicitud de salida establece como el resultado de la ejecución del conjunto de reglas. A partir de este punto es la canalización de notificaciones que se encarga y mueve el resultado final a la siguiente etapa de su proceso.  
  
## <a name="sending-the-execution-output-to-the-claims-pipeline"></a>Enviar la información de ejecución a la canalización de notificaciones  
Cuando las notificaciones del motor procesos un conjunto de reglas, que el conjunto de reglas tiene sus propio dedicados ubicaciones en la memoria para su entrada y salida reclamar conjuntos. Esto significa que la entrada y salida reclamar juegos utilizados por un conjunto de reglas son independientes de la entrada y salida reclamar conjuntos usados en otro conjunto de reglas.  
  
Una vez ejecutado todo el proceso para un conjunto de reglas de dar \ (los pasos 1, 2 y 3\), las notificaciones salientes recién emitidas \ (contenido de la Set Reclamación de salida) que se usará como entrada para el siguiente conjunto de reglas en la canalización de reclamaciones. Esto permite reclamaciones de flujo de la salida de un conjunto de reglas para la entrada de otro conjunto de reglas, tal como se muestra en la ilustración siguiente.  
  
![Roles de AD FS](media/adfs2_enginecontexts.gif)  
  
> [!NOTE]  
> Aunque el conjunto de reglas de emisión también es una etapa crítica en la canalización, la ilustración anterior muestran no solo para fines de simplificar la ilustración. Para obtener una ilustración que muestra el conjunto de reglas de emisión y cómo encaja en la canalización de notificaciones, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
En este caso, se usa el resultado de las reglas de aceptación por la canalización para hacer fluir el conjunto final de una reclamación producida por las reglas de aceptación para la segunda fase de la canalización, que es el procesamiento de reglas de autorización. En este punto, todo el reclamar el proceso de ejecución de las reglas \ (above\ los pasos 1, 2 y 3) se ejecute de nuevo para el conjunto de reglas de autorización. Este ciclo continúa hasta que la regla de emisión establece \ (la fase final en el pipeline\) se ha completado.  
  
Una vez que las notificaciones salientes finalizadas se han devuelto desde el motor del conjunto de reglas de emisión, se empaquetan en un token SAML y los servicios de federación de enviará el token al cliente.  
  
## <a name="processing-authorization-rules"></a>Reglas de procesamiento de autorización  
Si el conjunto de reglas de notificación que se ejecuta durante el paso 2 del proceso de ejecución de las reglas de Reclamación consta de las reglas de autorización \ (que tiene una entrada diferente y la salida reclamar conjuntos de aceptación o emisión rules\), a continuación, se ejecutarán esas reglas de autorización para determinar si un solicitante token está autorizado para obtener un token de seguridad para un determinado usuario de confianza del servicio de federación en función de notificaciones del solicitante.  
  
El objetivo de reglas de autorización es emitir una permitir o denegar reclamación en función de si el usuario es para poder obtener un token para el usuario de confianza determinado o no. Como se muestra en la siguiente ilustración, el resultado de la ejecución de autorización se usa la canalización para determinar si se ejecuta el conjunto de reglas de emisión o no, en función de presencia o ausencia de la y\ permitir / o denegar la solicitud, pero no se utiliza la información de ejecución de autorización sí como una entrada para el conjunto de reglas de notificación.  
  
![Roles de AD FS](media/adfs2_authorization.gif)  
  
Para obtener más información acerca de la autorización de notificaciones, consulta [cuándo usar una regla de solicitud de autorización](When-to-Use-an-Authorization-Claim-Rule.md).  
  

