---
title: Mensajes de eventos y errores de la directiva QoS
description: En este tema se proporciona una lista de mensajes de error y de eventos para la Directiva de calidad de servicio (QoS) en Windows Server 2016.
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 5b1c181bba776b8b42433dfb6f407c6c5a584019
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948631"
---
# <a name="qos-policy-error-and-event-messages"></a>Mensajes de eventos y errores de la directiva QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

A continuación se muestran los mensajes de error y de eventos asociados a la Directiva de QoS.

## <a name="informational-messages"></a>Mensajes informativos

A continuación se muestra una lista de mensajes informativos de la Directiva de QoS.

|||
|-|-|
|**MessageId**|16500|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|
|**Lenguaje**|Inglés|
|**Mensaje**|Las directivas QoS de equipo se actualizaron correctamente. No se detectó ningún cambio.|

|||
|-|-|
|**MessageId**|16501|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|
|**Lenguaje**|Inglés|
|**Mensaje**|Las directivas QoS de equipo se actualizaron correctamente. Se detectaron cambios en la Directiva.|

|||
|-|-|
|**MessageId**|16502|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|
|**Lenguaje**|Inglés|
|**Mensaje**|Las directivas QoS de usuario se actualizaron correctamente. No se detectó ningún cambio.|

|||
|-|-|
|**MessageId**|16503|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|
|**Lenguaje**|Inglés|
|**Mensaje**|Las directivas QoS de usuario se actualizaron correctamente. Se detectaron cambios en la Directiva.|

|||
|-|-|
|**MessageId**|16504|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|
|**Lenguaje**|Inglés|
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se actualizó correctamente. La directiva QoS no especifica el valor de configuración. Se aplicará el valor predeterminado del equipo local.|

|||
|-|-|
|**MessageId**|16505|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|
|**Lenguaje**|Inglés|
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se actualizó correctamente. El valor de configuración es nivel 0 (rendimiento mínimo).|

|||
|-|-|
|**MessageId**|16506|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|
|**Lenguaje**|Inglés|
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se actualizó correctamente. El valor de configuración es el nivel 1.|

|||
|-|-|
|**MessageId**|16507|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|
|**Lenguaje**|Inglés|
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se actualizó correctamente. El valor de configuración es nivel 2.|

|||
|-|-|
|**MessageId**|16508|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|
|**Lenguaje**|Inglés|
|**Mensaje**|La configuración avanzada de QoS para el nivel de rendimiento de TCP de entrada se actualizó correctamente. El valor de configuración es el nivel 3 (rendimiento máximo).|

|||
|-|-|
|**MessageId**|16509|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|
|**Lenguaje**|Inglés|
|**Mensaje**|La configuración avanzada de QoS para las invalidaciones de marcado de DSCP se actualizó correctamente. No se ha especificado el valor de configuración. Las aplicaciones pueden establecer valores de DSCP independientemente de las directivas de QoS.|

|||
|-|-|
|**MessageId**|16510|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|
|**Lenguaje**|Inglés|
|**Mensaje**|La configuración avanzada de QoS para las invalidaciones de marcado de DSCP se actualizó correctamente. Se omitirán las solicitudes de marcado de DSCP de aplicación. Solo las directivas QoS pueden establecer valores de DSCP.|

|||
|-|-|
|**MessageId**|16511|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|
|**Lenguaje**|Inglés|
|**Mensaje**|La configuración avanzada de QoS para las invalidaciones de marcado de DSCP se actualizó correctamente. Las aplicaciones pueden establecer valores de DSCP independientemente de las directivas de QoS.|

|||
|-|-|
|**MessageId**|16512|
|**Gravedad**|Informativo|
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|
|**Lenguaje**|Inglés|
|**Mensaje**|Se ha deshabilitado la aplicación selectiva de directivas QoS basadas en la categoría red de dominio. Las directivas de QoS se aplicarán a todas las interfaces de red.|

## <a name="warning-messages"></a>Mensajes de advertencia

A continuación se muestra una lista de mensajes de advertencia de la Directiva de QoS.

|||
|-|-|
|**MessageId**|16600|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|
|**Lenguaje**|Inglés|
|**Mensaje**|EQOS: * * _Testing \_ \* \* [, con una cadena] "%2".|

|||
|-|-|
|**MessageId**|16601|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|
|**Lenguaje**|Inglés|
|**Mensaje**|EQOS: * * _Testing \_ \* \* [, con dos cadenas, String1 es] "%2" [, cadena2 es] "%3".|

|||
|-|-|
|**MessageId**|16602|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|
|**Lenguaje**|Inglés|
|**Mensaje**|La directiva QoS de equipo "%2" tiene un número de versión no válido. Esta Directiva no se aplicará.|

|||
|-|-|
|**MessageId**|16603|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|
|**Lenguaje**|Inglés|
|**Mensaje**|La directiva QoS de usuario "%2" tiene un número de versión no válido. Esta Directiva no se aplicará.|

|||
|-|-|
|**MessageId**|16604|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|
|**Lenguaje**|Inglés|
|**Mensaje**|La directiva QoS de equipo "%2" no especifica un valor de DSCP o una velocidad de limitación. Esta Directiva no se aplicará.|

|||
|-|-|
|**MessageId**|16605|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|
|**Lenguaje**|Inglés|
|**Mensaje**|La directiva QoS de usuario "%2" no especifica un valor de DSCP o una tasa de limitación. Esta Directiva no se aplicará.|

|||
|-|-|
|**MessageId**|16606|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|
|**Lenguaje**|Inglés|
|**Mensaje**|Se superó el número máximo de directivas de QoS de equipo. No se aplicará la directiva QoS "%2" y las directivas QoS de equipo posteriores.|

|||
|-|-|
|**MessageId**|16607|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|
|**Lenguaje**|Inglés|
|**Mensaje**|Se superó el número máximo de directivas de QoS de usuario. No se aplicará la directiva QoS "%2" y las directivas QoS de usuario subsiguientes.|

|||
|-|-|
|**MessageId**|16608|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|
|**Lenguaje**|Inglés|
|**Mensaje**|La directiva QoS de equipo "%2" puede estar en conflicto con otras directivas de QoS. Consulte la documentación sobre las reglas sobre qué directiva se aplicará.|

|||
|-|-|
|**MessageId**|16609|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|
|**Lenguaje**|Inglés|
|**Mensaje**|La directiva QoS de usuario "%2" puede estar en conflicto con otras directivas QoS. Consulte la documentación sobre las reglas sobre qué directiva se aplicará.|

|||
|-|-|
|**MessageId**|16610|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|
|**Lenguaje**|Inglés|
|**Mensaje**|Se omitió la directiva QoS de equipo "%2" porque no se puede procesar la ruta de acceso de la aplicación. Es posible que la ruta de acceso de la aplicación no sea válida, contenga una letra de unidad no válida o contenga una unidad de red asignada.|

|||
|-|-|
|**MessageId**|16611|
|**Gravedad**|Advertencia|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|
|**Lenguaje**|Inglés|
|**Mensaje**|Se omitió la directiva QoS de usuario "%2" porque no se puede procesar la ruta de acceso de la aplicación. Es posible que la ruta de acceso de la aplicación no sea válida, contenga una letra de unidad no válida o contenga una unidad de red asignada.|

## <a name="error-messages"></a>mensajes de error

A continuación se muestra una lista de mensajes de error de la directiva QoS.

|||
|-|-|
|**MessageId**|16700|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|
|**Lenguaje**|Inglés|
|**Mensaje**|No se pudieron actualizar las directivas QoS del equipo. Código de error: "%2".|

|||
|-|-|
|**MessageId**|16701|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|
|**Lenguaje**|Inglés|
|**Mensaje**|No se pudieron actualizar las directivas QoS de usuario. Código de error: "%2".|

|||
|-|-|
|**MessageId**|16702|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|
|**Lenguaje**|Inglés|
|**Mensaje**|QoS no pudo abrir la clave raíz de nivel de equipo para las directivas QoS. Código de error: "%2".|

|||
|-|-|
|**MessageId**|16703|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|
|**Lenguaje**|Inglés|
|**Mensaje**|QoS no pudo abrir la clave raíz de nivel de usuario para las directivas QoS. Código de error: "%2".|

|||
|-|-|
|**MessageId**|16704|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|
|**Lenguaje**|Inglés|
|**Mensaje**|Una directiva QoS de equipo supera la longitud de nombre máxima permitida. La Directiva infractora aparece en la clave raíz de la directiva QoS de nivel de equipo con el índice "%2".|

|||
|-|-|
|**MessageId**|16705|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|
|**Lenguaje**|Inglés|
|**Mensaje**|Una directiva QoS de usuario supera la longitud máxima permitida del nombre. La Directiva infractora aparece en la clave raíz de la directiva QoS de nivel de usuario, con el índice "%2".|

|||
|-|-|
|**MessageId**|16706|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|
|**Lenguaje**|Inglés|
|**Mensaje**|Una directiva QoS de equipo tiene un nombre de longitud cero. La Directiva infractora aparece en la clave raíz de la directiva QoS de nivel de equipo con el índice "%2".|

|||
|-|-|
|**MessageId**|16707|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|
|**Lenguaje**|Inglés|
|**Mensaje**|Una directiva QoS de usuario tiene un nombre de longitud cero. La Directiva infractora aparece en la clave raíz de la directiva QoS de nivel de usuario, con el índice "%2".|

|||
|-|-|
|**MessageId**|16708|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|
|**Lenguaje**|Inglés|
|**Mensaje**|QoS no pudo abrir la subclave del registro para una directiva QoS del equipo. La directiva aparece en la clave raíz de la directiva QoS de nivel de equipo con el índice "%2".|

|||
|-|-|
|**MessageId**|16709|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|
|**Lenguaje**|Inglés|
|**Mensaje**|QoS no pudo abrir la subclave del registro para una directiva QoS de usuario. La directiva aparece en la clave raíz de la directiva QoS de nivel de usuario, con el índice "%2".|

|||
|-|-|
|**MessageId**|16710|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|
|**Lenguaje**|Inglés|
|**Mensaje**|QoS no pudo leer o validar el campo "%2" para la directiva QoS de equipo "%3".|

|||
|-|-|
|**MessageId**|16711|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|
|**Lenguaje**|Inglés|
|**Mensaje**|QoS no pudo leer o validar el campo "%2" para la directiva QoS de usuario "%3".|

|||
|-|-|
|**MessageId**|16712|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|
|**Lenguaje**|Inglés|
|**Mensaje**|QoS no pudo leer o establecer el nivel de rendimiento de TCP de entrada, código de error: "%2".|

|||
|-|-|
|**MessageId**|16713|
|**Gravedad**|Error|
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|
|**Lenguaje**|Inglés|
|**Mensaje**|QoS no pudo leer o establecer la configuración de invalidación de marcado de DSCP, código de error: "%2".|

Para el siguiente tema de esta guía, consulte [preguntas más frecuentes sobre la Directiva de QoS](qos-policy-faq.md).

En el primer tema de esta guía, vea [Directiva de calidad de servicio (QoS)](qos-policy-top.md).
