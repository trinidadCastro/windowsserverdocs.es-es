---
title: Error de directiva de QoS y mensajes de eventos
description: Este tema proporciona una lista de mensajes de error y eventos de directiva de calidad de servicio (QoS) en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e2e890a7947d4f7de09159d7de606c0542f45045
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-error-and-event-messages"></a>Error de directiva de QoS y mensajes de eventos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Siguiente es los mensajes de error y eventos que están asociados a la directiva QoS.  
  
## <a name="informational-messages"></a>Mensajes informativos  

Siguiente es una lista de mensajes de información de QoS directiva.

|||  
|-|-|  
|**Id. de mensaje**|16500|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglés|  
|**Mensaje**|Directivas QoS de equipo se actualizaron correctamente. Se detectó ningún cambio.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16501|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglés|  
|**Mensaje**|Directivas QoS de equipo se actualizaron correctamente. Cambios en la directiva detectados.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16502|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglés|  
|**Mensaje**|Directivas QoS de usuario se actualizaron correctamente. Se detectó ningún cambio.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16503|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglés|  
|**Mensaje**|Directivas QoS de usuario se actualizaron correctamente. Cambios en la directiva detectados.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16504|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento TCP entrante se actualizan correctamente. No se especifica el valor por ninguna directiva QoS. Se aplicará el valor predeterminado del equipo local.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16505|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento TCP entrante se actualizan correctamente. El valor es el nivel de 0 (rendimiento mínimo).|  
  
|||  
|-|-|  
|**Id. de mensaje**|16506|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento TCP entrante se actualizan correctamente. El valor es el nivel 1.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16507|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento TCP entrante se actualizan correctamente. El valor es el nivel 2.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16508|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento TCP entrante se actualizan correctamente. El valor es el nivel 3 (rendimiento máximo).|  
  
|||  
|-|-|  
|**Id. de mensaje**|16509|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para marcado DSCP invalida actualizado correctamente. No se especifica el valor. Las aplicaciones pueden establecer valores DSCP independientemente de las directivas de QoS.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16510|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para marcado DSCP invalida actualizado correctamente. Las solicitudes de marcado de DSCP de la aplicación se omitirán. Solo las directivas de QoS pueden establecer valores DSCP.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16511|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para marcado DSCP invalida actualizado correctamente. Las aplicaciones pueden establecer valores DSCP independientemente de las directivas de QoS.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16512|  
|**Gravedad**|Informativo|  
|**Nombre**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Idioma**|Inglés|  
|**Mensaje**|Se ha deshabilitado la aplicación selectiva de directivas de QoS basada en la categoría de la red de dominio. Las directivas de QoS se aplicarán a todas las interfaces de red.|  
  
## <a name="warning-messages"></a>Mensajes de advertencia

Siguiente es una lista de mensajes de advertencia directiva QoS.

|||  
|-|-|  
|**Id. de mensaje**|16600|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_TEST_1|  
|**Idioma**|Inglés|  
|**Mensaje**|EQOS: *** Testing\ * \ * \ * [, con una cadena] "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16601|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_TEST_2|  
|**Idioma**|Inglés|  
|**Mensaje**|EQOS: *** Testing\ * \ * \ * [, con dos cadenas, es string1] "%2" [, string2 es] "%3".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16602|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Idioma**|Inglés|  
|**Mensaje**|La directiva de equipo QoS "%2" tiene un número de versión no válida. Esta directiva no se aplicarán.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16603|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Idioma**|Inglés|  
|**Mensaje**|El usuario directiva QoS "%2" tiene un número de versión no válido. Esta directiva no se aplicarán.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16604|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglés|  
|**Mensaje**|La directiva de equipo QoS "%2" no especificar un tipo de valor o el Acelerador DSCP. Esta directiva no se aplicarán.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16605|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglés|  
|**Mensaje**|La directiva de usuario QoS "%2" no especificar un tipo de valor o el Acelerador DSCP. Esta directiva no se aplicarán.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16606|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglés|  
|**Mensaje**|Se excedió el número máximo de las directivas de QoS de equipo. No se apliquen las directivas de QoS de posteriores del equipo y la directiva de QoS "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16607|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglés|  
|**Mensaje**|Se excedió el número máximo de las directivas de QoS de usuario. No se apliquen las directivas de QoS de posteriores del usuario y la directiva de QoS "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16608|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Idioma**|Inglés|  
|**Mensaje**|La directiva de equipo QoS "%2" potencialmente entra en conflicto con otras directivas QoS. Consulta la documentación de las reglas acerca de qué se aplica la directiva.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16609|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Idioma**|Inglés|  
|**Mensaje**|La directiva de usuario QoS "%2" potencialmente entra en conflicto con otras directivas QoS. Consulta la documentación de las reglas acerca de qué se aplica la directiva.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16610|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglés|  
|**Mensaje**|Se omitió la directiva de equipo QoS "%2" porque no se pueden procesar la ruta de acceso de la aplicación. La ruta de acceso de la aplicación puede no ser válido, contienen una letra de unidad no válida o una unidad de red asignada.|  
  
|||  
|-|-|  
|**Id. de mensaje**|16611|  
|**Gravedad**|Advertencia|  
|**Nombre**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglés|  
|**Mensaje**|Se omitió la directiva de usuario QoS "%2" porque no se pueden procesar la ruta de acceso de la aplicación. La ruta de acceso de la aplicación puede no ser válido, contienen una letra de unidad no válida o una unidad de red asignada.|  
  
## <a name="error-messages"></a>Mensajes de error  

Siguiente es una lista de mensajes de error de directiva QoS.

|||  
|-|-|  
|**Id. de mensaje**|16700|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudo actualizar las directivas de QoS de equipo. Código de error: "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16701|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudo actualizar las directivas de QoS de usuario. Código de error: "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16702|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudo abrir la clave raíz de nivel de equipo para las directivas de QoS QoS. Código de error: "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16703|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudo abrir la clave raíz de nivel de usuario para las directivas de QoS QoS. Código de error: "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16704|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglés|  
|**Mensaje**|Un equipo directiva QoS supera la longitud máxima del nombre permitido. La directiva conflictiva aparece bajo la clave de raíz de la directiva QoS de nivel de la máquina, con el índice "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16705|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglés|  
|**Mensaje**|Un usuario directiva QoS supera la longitud máxima del nombre permitido. La directiva conflictiva aparece bajo la clave de raíz de la directiva QoS de nivel de usuario, con el índice "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16706|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglés|  
|**Mensaje**|Una directiva de QoS equipo tiene un nombre de longitud cero. La directiva conflictiva aparece bajo la clave de raíz de la directiva QoS de nivel de la máquina, con el índice "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16707|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglés|  
|**Mensaje**|Un usuario directiva QoS tiene un nombre de longitud cero. La directiva conflictiva aparece bajo la clave de raíz de la directiva QoS de nivel de usuario, con el índice "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16708|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudo abrir la subclave del registro para un equipo directiva QoS QoS. La directiva aparece bajo la clave de raíz de la directiva QoS de nivel de la máquina, con el índice "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16709|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudo abrir la subclave del registro para que un usuario directiva QoS QoS. La directiva aparece bajo la clave de raíz de la directiva QoS de nivel de usuario, con el índice "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16710|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Idioma**|Inglés|  
|**Mensaje**|Error de QoS al leer o validar el campo "%2", la directiva de equipo QoS "%3".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16711|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Idioma**|Inglés|  
|**Mensaje**|Error de QoS al leer o validar el campo "%2" para que el usuario directiva QoS "%3".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16712|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Idioma**|Inglés|  
|**Mensaje**|QoS no se pudo leer o establecer el código de entrada TCP rendimiento nivel, error: "%2".|  
  
|||  
|-|-|  
|**Id. de mensaje**|16713|  
|**Gravedad**|Error|  
|**Nombre**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Idioma**|Inglés|  
|**Mensaje**|Error al leer o establecer el marcado de DSCP de QoS invalidar la configuración, el código de error: "%2".|  

El siguiente tema en esta guía, encontrarás [preguntas más frecuentes de QoS directiva](qos-policy-faq.md).

El primer tema de esta guía, encontrarás [directiva de calidad de servicio (QoS)](qos-policy-top.md).
