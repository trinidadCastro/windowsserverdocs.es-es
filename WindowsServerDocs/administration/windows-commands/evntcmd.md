---
title: Evntcmd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2a4a7f9e6bdf6f200b86e028617cae924571455
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439454"
---
# <a name="evntcmd"></a>Evntcmd

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura la traducción de eventos a interrupciones, destinos de captura o ambos según la información en un archivo de configuración.   
## <a name="syntax"></a>Sintaxis  
```  
evntcmd [/s <computerName>] [/v <verbosityLevel>] [/n] <FileName>  
```  
### <a name="parameters"></a>Parámetros  

|      Parámetro      |                                                                                                                                                            Descripción                                                                                                                                                             |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /s <computerName>  |                                                         Especifica el nombre, el equipo en el que desea configurar la traducción de eventos a interrupciones, destinos de captura o ambos. Si no especifica un equipo, la configuración se produce en el equipo local.                                                          |
| /v <verbosityLevel> | Especifica qué tipos de estado aparecen como capturas de mensajes y destinos de captura están configurados. Este parámetro debe ser un entero entre 0 y 10. Si especifica 10, aparecen todos los tipos de mensajes, incluido el seguimiento de mensajes y las advertencias sobre si la configuración de captura fue correcta. Si especifica 0, no aparece ningún mensaje. |
|         /n          |                                                                                                           Especifica que no se debe reiniciar el servicio SNMP si este equipo recibe los cambios de configuración de captura.                                                                                                            |
|     <FileName>      |                                                                                     Especifica el nombre, el archivo de configuración que contiene información sobre la traducción de sucesos a capturas y los destinos de captura que desea configurar.                                                                                     |
|         /?          |                                                                                                                                                Muestra la ayuda en el símbolo del sistema.                                                                                                                                                |

## <a name="remarks"></a>Comentarios  
- Si desea configurar capturas, pero no los destinos de captura, puede crear un archivo de configuración válido mediante eventos al traductor de captura, que es una utilidad gráfica. Si tiene instalado el servicio SNMP, puede iniciar la captura de traductor de suceso a escribiendo **evntwin** en un símbolo del sistema. Después de haber definido las capturas que desee, haga clic en Exportar para crear un archivo adecuado para su uso con **evntcmd**. Puede usar para crear fácilmente un archivo de configuración y, a continuación, use el archivo de configuración con captura de traductor de suceso a **evntcmd** en el símbolo del sistema para configurar rápidamente las capturas en varios equipos.  
- La sintaxis para configurar una captura es como sigue:  
  **#pragma add**<em><EventLogFile> <EventSource> <EventID> [<Count> [<Period>]]</em>  
  -   El texto **#pragma** debe aparecer al principio de cada entrada en el archivo.  
  -   El parámetro **agregar** especifica que desea agregar un evento para capturar la configuración.  
  -   Los parámetros *archivoRegistroSuceso*, *EventSource*, y *EventID* son necesarios. El parámetro *archivoRegistroSuceso* especifica el archivo en el que se registra el evento. El parámetro *EventSource* especifica la aplicación que genera el evento. El *EventID* parámetro especifica el número único que identifica cada evento. Para averiguar qué valores corresponden a eventos concretos, inicie la captura de traductor de suceso a escribiendo **evntwin** en un símbolo del sistema. Haga clic en **personalizado**y, a continuación, haga clic en **editar**. En **orígenes de eventos**, examinar las carpetas hasta que encuentre el evento que desea configurar, haga clic en él y, a continuación, haga clic en **agregar**. Información sobre el origen del evento, el archivo de registro de eventos y el identificador de evento aparecen bajo **de origen, inicie sesión**, y **interceptar el Id. específico**, respectivamente.  
  -   El *recuento* parámetro es opcional y especifica cuántas veces se debe producir el evento antes de enviar un mensaje de captura. Si no usa el *recuento* parámetro, se envía el mensaje de captura después de que el evento tiene lugar una vez.  
  -   El *período* parámetro es opcional, pero requiere que use el *recuento* parámetro. El *período* parámetro especifica un período de tiempo (en segundos) durante el cual el evento debe producir el número de veces especificado con el parámetro de recuento antes de enviar un mensaje de captura. Si no usa el *período* parámetro, se envía un mensaje de captura después de que el evento tiene lugar el número de veces especificado con el *recuento* parámetro, independientemente de cuánto tiempo transcurre entre las repeticiones.  
- La sintaxis para eliminar una captura es como sigue:  
  **#pragma delete**<em><EventLogFile> <EventSource> <EventID></em>  
  -   El texto **#pragma** debe aparecer al principio de cada entrada en el archivo.  
  -   El parámetro *eliminar* especifica que desea quitar un evento para capturar la configuración.  
  -   Los parámetros *archivoRegistroSuceso*, *EventSource*, y *EventID* son necesarios. El parámetro *archivoRegistroSuceso* especifica el registro en el que se registró el evento. El parámetro *EventSource* especifica la aplicación que genera el evento. El *EventID* parámetro especifica el número único que identifica cada evento.  
- La sintaxis para configurar un destino de captura es como sigue:  
  **#pragma add_TRAP_DEST**<em><CommunityName> <HostID></em>  
  -   El texto **#pragma** debe aparecer al principio de cada entrada en el archivo.  
  -   El parámetro **add_TRAP_DEST** especifica que desea que los mensajes se envíen a un host específico de una comunidad de captura.  
  -   El parámetro *nombreComunidad* especifica el nombre, la Comunidad en qué interrupción se envían los mensajes.  
  -   El parámetro *HostID* especifica por nombre o la dirección IP del host al que desea que se envíen los mensajes de captura.  
- La sintaxis para quitar un destino de captura es como sigue:  
  **#pragma delete_TRAP_DEST**<em><CommunityName> <HostID></em>  
  - El texto **#pragma** debe aparecer al principio de cada entrada en el archivo.  
  - El parámetro *delete_TRAP_DEST* especifica que no desea que los mensajes se envíen a un host específico de una comunidad de captura.  
  - El parámetro *nombreComunidad* especifica el nombre, la Comunidad en qué interrupción se envían los mensajes.  
  - El parámetro *HostID* especifica por nombre o la dirección IP del host al que no desea que se envíen los mensajes de captura.  
    ## <a name="BKMK_Examples"></a>Ejemplos  
    Los ejemplos siguientes ilustran las entradas en el archivo de configuración para el **evntcmd** comando. No están diseñados para escribirse en un símbolo del sistema.  
    Para enviar un mensaje de captura si se reinicia el servicio de registro de eventos, escriba:  
    ```  
    #pragma add System "Eventlog" 2147489653  
    ```  
    Para enviar un mensaje de captura si se reinicia el servicio de registro de eventos dos veces en tres minutos, escriba:  
    ```  
    #pragma add System "Eventlog" 2147489653 2 180  
    ```  
    Para dejar de enviar un mensaje de captura siempre que se reinicie el servicio de registro de eventos, escriba:  
    ```  
    #pragma delete System "Eventlog" 2147489653  
    ```  
    Para enviar mensajes de captura dentro de la Comunidad denominado Public en el host con la dirección IP 192.168.100.100, escriba:  
    ```  
    #pragma add_TRAP_DEST public 192.168.100.100  
    ```  
    Para enviar mensajes de captura dentro de la Comunidad denominada a privada para el host denominado Host1, escriba:  
    ```  
    #pragma add_TRAP_DEST private Host1  
    ```  
    Para detener el envío de mensajes de captura dentro de la Comunidad denominada a privada en el mismo equipo en el que va a configurar los destinos de captura, escriba:  
    ```  
    #pragma delete_TRAP_DEST private localhost  
    ```  
    ## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
