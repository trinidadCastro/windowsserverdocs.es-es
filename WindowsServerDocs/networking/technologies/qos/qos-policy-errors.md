---
title: Error de la directiva de QoS y mensajes de eventos
description: En este tema se proporciona una lista de mensajes de error y eventos de directiva de calidad de servicio (QoS) en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 774d9473beed1da861c6827357710133aeaca15e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824056"
---
# <a name="qos-policy-error-and-event-messages"></a>Error de la directiva de QoS y mensajes de eventos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

A continuación es los mensajes de error y eventos que están asociados con la directiva de QoS.  
  
## <a name="informational-messages"></a>Mensajes informativos  

Siguiente es una lista de los mensajes informativos de directiva de QoS.

|||  
|-|-|  
|**MessageId**|16500|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglés|  
|**Mensaje**|Directivas QoS de equipo se actualizó correctamente. No se detectó ningún cambio.|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglés|  
|**Mensaje**|Directivas QoS de equipo se actualizó correctamente. Cambios en la directiva detectados.|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglés|  
|**Mensaje**|Directivas QoS de usuario se actualizaron correctamente. No se detectó ningún cambio.|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglés|  
|**Mensaje**|Directivas QoS de usuario se actualizaron correctamente. Cambios en la directiva detectados.|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se ha actualizado correctamente. No se especifica el valor de directiva de QoS. Se aplicará el valor predeterminado del equipo local.|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se ha actualizado correctamente. Establecer el valor es el nivel 0 (rendimiento mínimo).|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se ha actualizado correctamente. Establecer el valor es el nivel 1.|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se ha actualizado correctamente. Establecer valor es el nivel 2.|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se ha actualizado correctamente. Establecer valor es el nivel 3 (rendimiento máximo).|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el marcado de DSCP invalidaciones se actualizaron correctamente. No se especifica el valor de configuración. Las aplicaciones pueden establecer los valores DSCP independientemente de las directivas de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el marcado de DSCP invalidaciones se actualizaron correctamente. Se omitirán las solicitudes de marcado de DSCP de la aplicación. Solo las directivas de QoS pueden establecer valores DSCP.|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Idioma**|Inglés|  
|**Mensaje**|La configuración avanzada de QoS para el marcado de DSCP invalidaciones se actualizaron correctamente. Las aplicaciones pueden establecer los valores DSCP independientemente de las directivas de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Gravedad**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Idioma**|Inglés|  
|**Mensaje**|Se deshabilitó la aplicación selectiva de las directivas de QoS basada en la categoría de la red de dominio. Directivas de QoS se aplicarán a todas las interfaces de red.|  
  
## <a name="warning-messages"></a>Mensajes de advertencia

Siguiente es una lista de mensajes de advertencia de directiva de QoS.

|||  
|-|-|  
|**MessageId**|16600|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Idioma**|Inglés|  
|**Mensaje**|EQOS: *** pruebas\*\*\*[, con una cadena] "%2".|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Idioma**|Inglés|  
|**Mensaje**|EQOS: *** pruebas\*\*\*[, con dos cadenas, string1 es] "%2" [, string2 es] "%3".|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Idioma**|Inglés|  
|**Mensaje**|El equipo "%2" de la directiva de calidad de servicio tiene un número de versión no válida. No se aplicará esta directiva.|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Idioma**|Inglés|  
|**Mensaje**|El usuario de la directiva de QoS "%2" tiene un número de versión no válida. No se aplicará esta directiva.|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglés|  
|**Mensaje**|El equipo "%2" de la directiva de QoS no especificar un tipo de valor o el Acelerador DSCP. No se aplicará esta directiva.|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglés|  
|**Mensaje**|El usuario "%2" de directiva de QoS no especifica una tasa de limitación o de valor DSCP. No se aplicará esta directiva.|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglés|  
|**Mensaje**|Había superado el número máximo de directivas de QoS de equipo. No se aplicará la directiva de calidad de servicio "%2" y las directivas de QoS posteriores de equipos.|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglés|  
|**Mensaje**|Había superado el número máximo de directivas QoS de usuario. No se aplicará la directiva de calidad de servicio "%2" y las directivas de QoS de usuario siguientes.|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Idioma**|Inglés|  
|**Mensaje**|El equipo "%2" de la directiva de QoS potencialmente entra en conflicto con otras directivas de QoS. Consulte la documentación de las reglas sobre los cuales se aplicará la directiva.|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Idioma**|Inglés|  
|**Mensaje**|El usuario "%2" de directiva de QoS potencialmente entra en conflicto con otras directivas de QoS. Consulte la documentación de las reglas sobre los cuales se aplicará la directiva.|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglés|  
|**Mensaje**|El equipo "%2" de la directiva de QoS se omitió porque no se puede procesar la ruta de acceso de la aplicación. La ruta de acceso de la aplicación es posible que no es válido, contener una letra de unidad no válida o contiene una unidad de red asignada.|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Gravedad**|Advertencia|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglés|  
|**Mensaje**|El usuario "%2" de directiva de QoS se omitió porque no se puede procesar la ruta de acceso de la aplicación. La ruta de acceso de la aplicación es posible que no es válido, contener una letra de unidad no válida o contiene una unidad de red asignada.|  
  
## <a name="error-messages"></a>Mensajes de error  

Siguiente es una lista de mensajes de error de directiva de QoS.

|||  
|-|-|  
|**MessageId**|16700|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudieron actualizar las directivas de QoS de equipo. Código de error: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudieron actualizar las directivas de QoS de usuario. Código de error: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Idioma**|Inglés|  
|**Mensaje**|QoS no se pudo abrir la clave raíz de nivel de equipo para las directivas de QoS. Código de error: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Idioma**|Inglés|  
|**Mensaje**|QoS no se pudo abrir la clave raíz de nivel de usuario para las directivas de QoS. Código de error: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglés|  
|**Mensaje**|Una directiva QoS de equipo supera la longitud máxima permitida. La directiva incorrecta aparece bajo la clave de raíz de la directiva QoS de nivel de equipo, con el índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglés|  
|**Mensaje**|Una directiva QoS de usuario supera la longitud máxima permitida. La directiva incorrecta aparece bajo la clave de raíz de la directiva QoS de nivel de usuario, con el índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglés|  
|**Mensaje**|Una directiva QoS de equipo tiene un nombre de longitud cero. La directiva incorrecta aparece bajo la clave de raíz de la directiva QoS de nivel de equipo, con el índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglés|  
|**Mensaje**|Una directiva QoS de usuario tiene un nombre de longitud cero. La directiva incorrecta aparece bajo la clave de raíz de la directiva QoS de nivel de usuario, con el índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Idioma**|Inglés|  
|**Mensaje**|QoS no se pudo abrir la subclave del registro para una directiva QoS de equipo. La directiva aparece bajo la clave de raíz de la directiva QoS de nivel de equipo, con el índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Idioma**|Inglés|  
|**Mensaje**|QoS no se pudo abrir la subclave del registro para una directiva QoS de usuario. La directiva aparece bajo la clave de raíz de la directiva QoS de nivel de usuario, con el índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Idioma**|Inglés|  
|**Mensaje**|QoS no se pudo leer ni validar el campo "%2" para el equipo "%3" de la directiva de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Idioma**|Inglés|  
|**Mensaje**|QoS no se pudo leer ni validar el campo "%2" para el usuario "%3" de directiva de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Idioma**|Inglés|  
|**Mensaje**|QoS no se pudo leer ni establecer código de entrada TCP rendimiento nivel, error: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Gravedad**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Idioma**|Inglés|  
|**Mensaje**|No se pudo leer ni establecer el marcado de DSCP de QoS reemplazar la configuración, código de error: "%2".|  

El siguiente tema en esta guía, consulte [preguntas más frecuentes de QoS directiva](qos-policy-faq.md).

Para el primer tema de esta guía, consulte [directiva de calidad de servicio (QoS)](qos-policy-top.md).
