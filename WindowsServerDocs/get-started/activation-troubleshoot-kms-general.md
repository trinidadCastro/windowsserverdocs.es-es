---
title: Instrucciones para solucionar problemas de KMS
description: Proporciona información sobre el servicio KMS y sugiere herramientas y enfoques para solucionar problemas de activación.
ms.topic: troubleshooting
ms.date: 9/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: a089e0efb54af86f97595d8863926525a8416fea
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960467"
---
# <a name="guidelines-for-troubleshooting-the-key-management-service-kms"></a>Instrucciones para solucionar problemas del Servicio de administración de claves (KMS)

Como parte de su proceso de implementación, muchos clientes empresariales configuran el Servicio de administración de claves (KMS) para habilitar la activación de Windows en su entorno. Configurar el host de KMS es un proceso sencillo, después de lo cual los clientes de KMS detectan el host e intentan activarlo por sí solos. Pero ¿qué ocurre si el proceso no funciona? ¿Qué hacer a continuación? En este artículo, encontrarás una guía por los recursos que necesitas para solucionar el problema. Para obtener más información sobre las entradas del registro de eventos y el script Slmgr.vbs, consulta [Volume Activation Technical Reference](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502529(v=ws.11)) (Referencia técnica de activación de un volumen).

## <a name="kms-overview"></a>Introducción a KMS

Comencemos con una puesta a punto rápida sobre la activación de KMS. KMS es un modelo cliente-servidor. Conceptualmente, es similar a DHCP. En lugar de entregar direcciones IP a los clientes en su solicitud, KMS habilita la activación del producto. KMS también es un modelo de renovación, en el que los clientes intentan reactivarse de forma periódica. Hay dos roles: el *host de KMS* y el *cliente de KMS*.

- El **host de KMS** ejecuta el servicio de activación y habilita la activación en el entorno. Para configurar un host de KMS, tienes que instalar una clave de KMS desde el Centro de servicios de licencias por volumen (VLSC) y, luego, activar el servicio.
- El **cliente de KMS** es el sistema operativo Windows que se implementa en el entorno y tiene que activarse. Los clientes de KMS pueden ejecutar cualquier edición de Windows que use la activación por volumen. Los clientes de KMS se suministran con una clave preinstalada, denominada clave de licencia por volumen genérica (GVLK) o clave de configuración de cliente de KMS. La presencia de la GVLK es lo que hace que un sistema sea un cliente de KMS. Los clientes de KMS usan registros SRV DNS (_vlmcs._tcp) para identificar el host de KMS. Después, los clientes intentan detectar y usar este servicio automáticamente para activarse. Durante el período de gracia de 30 días inicial, intentarán activarse cada dos horas. Tras la activación, los clientes de KMS intentan renovar la activación cada siete días.

Desde la perspectiva de la solución de problemas, es posible que tengas que mirar ambos lados (host y cliente) para determinar lo que está ocurriendo.

## <a name="kms-host"></a>Host de KMS

Hay dos áreas para examinar en el host de KMS. En primer lugar, comprueba el estado del servicio de licencias de software del host. En segundo lugar, comprueba el Visor de eventos para los eventos relacionados con las licencias o la activación.

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs y el servicio de licencias de software

Para ver la salida detallada del servicio de licencias de software, abre una ventana del símbolo del sistema con privilegios elevados y escribe **slmgr.vbs /dlv** en el símbolo del sistema. En la captura de pantalla siguiente se muestran los resultados de este comando en uno de nuestros hosts de KMS dentro de Microsoft.

![Salida de slmgr en el host de KMS](./media/ee939272.kms_slmgr_output(en-us,technet.10).png)

Los campos más importantes para la solución de problemas son los siguientes. Lo que busques puede ser diferente en función del problema que tengas que resolver.

- **Información de versión**. En la parte superior de la salida de **slmgr.vbs /dlv**, se encuentra la versión del Servicio de licencias de software. Esto puede ser útil para determinar si está instalada la versión actual del servicio. Por ejemplo, las actualizaciones del servicio KMS en Windows Server 2003 admiten diferentes claves de host de KMS. Estos datos se pueden usar para evaluar si la versión está actualizada o no y si admite la clave de host de KMS que estás intentando instalar. Para obtener más información acerca de estas actualizaciones, consulta [An update is available for Windows Vista and for Windows Server 2008 to extend KMS activation support for Windows 7 and for Windows Server 2008 R2](https://support.microsoft.com/help/968912/an-update-is-available-for-windows-vista-and-for-windows-server-2008-t) (Hay disponible una actualización para Windows Vista y para Windows Server 2008 para ampliar la compatibilidad de activación de KMS para Windows 7 y Windows Server 2008 R2).
- **Nombre**. Indica la edición de Windows que está instalada en el sistema host de KMS. Esto puede ser importante para solucionar problemas si tienes problemas para agregar o cambiar la clave de host de KMS (por ejemplo, para comprobar que la clave es compatible con esa edición del SO).
- **Descripción**. Aquí es donde verás la clave que está instalada. Usa este campo para comprobar qué clave se usó para activar el servicio y si es la correcta o no para los clientes de KMS que has implementado.
- **Estado de licencia**. Este es el estado del sistema host de KMS. El valor debe ser **Con licencia**. Cualquier otro valor significa que hay algún error y puede que tenga que volver a activar el host.
- **Recuento actual**. El número mostrado estará entre **0** y **50**. El recuento es acumulativo (entre sistemas operativos) e indica el número de sistemas válidos que se han intentado activar en un período de 30 días.

  Si el recuento es **0**, significa que el servicio se ha activado recientemente o que no hay clientes válidos conectados al host de KMS.

  El recuento no aumentará por encima de **50**, independientemente del número de sistemas válidos que existan en el entorno. Esto se debe a que el recuento se establece para almacenarse en caché solo el doble que la directiva de licencias máxima devuelta por un cliente de KMS. Actualmente, el sistema operativo del cliente Windows establece la directiva máxima, que requiere un recuento de **25** o superior desde el host de KMS para activarse. Por lo tanto, el número más alto en el host de KMS es 2×25, o 50. Ten en cuenta que en entornos que contienen solo clientes de KMS de Windows Server, el recuento máximo en el host de KMS será de **10**. Esto se debe a que el umbral para las ediciones de Windows Server es de **5** (2×5, o 10).

  Un problema común relacionado con el recuento es si el entorno tiene un host de KMS activado y suficientes clientes, pero el recuento no aumenta más allá de uno. El problema principal es que la imagen del cliente implementada no se haya configurado correctamente (**sysprep /generalize**) y los sistemas no tengan id. de máquina de cliente (CMID) exclusivos. Para obtener más información, consulta [Cliente KMS](#kms-client) y [The KMS current count does not increase when you add new Windows Vista or Windows 7-based client computers to the network](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista) (El recuento actual de KMS no aumenta al agregar a la red nuevos equipos cliente basados en Windows Vista o Windows 7). Uno de nuestros ingenieros de asignación de nivel de soporte también ha escrito sobre este problema, en [KMS Host Client Count not Increasing Due to Duplicate CMID’S](/archive/blogs/askcore/kms-host-client-count-not-increasing-due-to-duplicate-cmids) (El recuento de clientes en el host KMS no aumenta debido a CMID duplicados).

  Otro motivo por el que puede que el recuento no aumente es que hay demasiados hosts de KMS en el entorno y el recuento se distribuya en todos ellos.
- **Escuchar en el puerto**. La comunicación con KMS usa RPC anónima. De forma predeterminada, los clientes usan el puerto TCP 1688 para conectarse al host de KMS. Asegúrate de que este puerto esté abierto entre los clientes de KMS y el host de KMS. Puedes cambiar o configurar el puerto en el host de KMS. Durante su comunicación, el host de KMS envía la designación de puerto a los clientes de KMS. Si cambias el puerto en un cliente de KMS, la designación de puerto se sobrescribe cuando el cliente se pone en contacto con el host.

A menudo nos preguntan sobre la sección "solicitudes acumulativas" de la salida **slmgr.vbs /dlv**. Normalmente, estos datos no son útiles para la solución de problemas. El host de KMS mantiene un registro constante del estado de cada cliente de KMS que intenta activar o reactivar. Las solicitudes incorrectas indican los clientes de KMS que el host de KMS no admite. Por ejemplo, si un cliente de KMS de Windows 7 intenta activar en un host de KMS que se ha activado mediante una clave de KMS de Windows Vista, la activación no se realiza. En las líneas "Solicitudes con Estado de la licencia" se describen todos los posibles estados de la licencia, pasados y presentes. Desde la perspectiva de la solución de problemas, estos datos solo son pertinentes solo si el recuento no aumenta según lo esperado. En ese caso, deberías ver aumentar el número de solicitudes incorrectas. Esto indica que debes comprobar la clave del producto que se usó para activar el sistema host de KMS. Además, ten en cuenta que los valores de solicitudes acumulativas solo se restablecen si reinstalas el sistema host de KMS.

### <a name="useful-kms-host-events"></a>Eventos útiles de host de KMS

#### <a name="event-id-12290"></a>Id. de evento 12290

El host de KMS registra el Id. de evento 12290 cuando un cliente de KMS se pone en contacto con el host para activarlo. El Id. de evento 12290 brinda una cantidad significativa de información que puedes usar para averiguar qué tipo de cliente se ha puesto en contacto con el host y por qué se ha producido un error. El siguiente segmento de una entrada de Id. de evento 12290 procede del registro de eventos del Servicio de administración de claves del host de KMS.

![Evento 12290 de KMS](./media/ee939272.kms_12290_event(en-us,technet.10).png)

Los detalles del evento incluyen la información siguiente:

- **Minimum count needed to activate** (Recuento mínimo necesario para activar). El cliente de KMS informa que el recuento del host de KMS debe ser **5** para activarse. Esto significa que se trata de un sistema operativo Windows Server, aunque no indica una edición específica. Si los clientes no se activan, asegúrate de que el recuento sea suficiente en el host.
- **Id. del equipo cliente (CMID)** . Este es un valor exclusivo en cada sistema. Si este valor no es exclusivo, se debe a que una imagen no se preparó correctamente para la distribución (**sysprep /generalize**). Este problema se manifiesta en el host de KMS como un recuento que no aumenta, aunque haya suficientes clientes en el entorno. Para obtener más información, consulta [The KMS current count does not increase when you add new Windows Vista or Windows 7-based client computers to the network](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista) (El recuento actual de KMS no aumenta al agregar a la red nuevos equipos cliente basados en Windows Vista o Windows 7).
- **License State and Time to State Expiration** (Estado de la licencia y tiempo hasta la expiración del estado). Este es el estado actual de la licencia del cliente. Puede ayudarte a diferenciar un cliente que está intentando activarse por primera vez de uno que está intentando reactivarse. La entrada de tiempo indica cuánto tiempo permanecerá el cliente en ese estado, si no cambia nada.

Si está solucionando problemas con un cliente y no encuentra un Id. de evento 12290 correspondiente en el host de KMS, ese cliente no se está conectando al host de KMS. Algunos de los motivos por los que puede que no exista una entrada con el Id. de evento 12290 son los siguientes:

- Se ha producido una interrupción en la red.
- El host no se resuelve o no está registrado en DNS.
- El firewall bloquea TCP 1688.
   El puerto podría bloquearse en muchos lugares del entorno, incluido en el propio sistema host de KMS. De forma predeterminada, el host de KMS tiene una excepción de firewall para KMS, pero no está habilitada automáticamente. Tienes que activar la excepción.
- El registro de eventos está lleno.

Los clientes de KMS registran dos eventos correspondientes, el Id. de evento 12288 y el Id. de evento 12289. Para obtener información acerca de estos eventos, consulta la sección [Cliente KMS](#kms-client).

#### <a name="event-id-12293"></a>Id. de evento 12293

Otro evento pertinente para buscar en el host de KMS es el Id. de evento 12293. Este evento indica que el host no ha publicado los registros necesarios en DNS. Se sabe que esta situación causa errores y es algo que debes verificar *después* de haber configurado el host y *antes* de implementar los clientes. Para obtener más información acerca de los problemas de DNS, consulta [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="kms-client"></a>Cliente KMS

En los clientes, se usan las mismas herramientas (slmgr y Visor de eventos) para solucionar problemas de activación.

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs y el servicio de licencias de software

Para ver la salida detallada del servicio de licencias de software, abre una ventana del símbolo del sistema con privilegios elevados y escribe **slmgr.vbs /dlv** en el símbolo del sistema. En la captura de pantalla siguiente se muestran los resultados de este comando en uno de nuestros hosts de KMS dentro de Microsoft.

![Salida de slmgr del cliente de KMS](./media/ee939272.kms_client_slmgr_output(en-us,technet.10).png)

La lista siguiente incluye los campos más importantes para la solución de problemas. Lo que busques puede ser diferente en función del problema que tengas que resolver.

- **Nombre**. Este valor es la edición de Windows que está instalada en el sistema cliente de KMS. Úsalo para verificar que la versión de Windows que intentas activar pueda usar KMS. Por ejemplo, el Departamento de soporte técnico ha detectado incidentes en los que los clientes intentan instalar la clave de configuración de cliente de KMS en una edición de Windows que no usa la activación por volumen, como Windows Vista Ultimate.
- **Descripción**. Este valor muestra la clave que está instalada. VOLUME_KMSCLIENT indica que la clave de configuración del cliente de KMS (o GVLK) está instalada (la configuración predeterminada para los medios de licencias por volumen) y que este sistema intenta activarse automáticamente mediante un host de KMS. Si ves algo más aquí, como MAK, tendrás que volver a instalar la GVLK para configurar este sistema como cliente de KMS. Puedes instalar manualmente la clave mediante **slmgr.vbs /ipk&lt;*GVLK*&gt;** (como se describe en [Claves de configuración de cliente KMS](kmsclientkeys.md)) o usar la Herramienta de administración de activación por volumen (VAMT). Para más información sobre cómo obtener y usar VAMT, consulta la [Referencia técnica de la Herramienta de administración de activación por volumen (VAMT)](/windows/deployment/volume-activation/volume-activation-management-tool).
- **Clave de producto parcial**. Como en el campo **Nombre**, puedes usar esta información para establecer si está instalada en este equipo la Clave de configuración de cliente KMS correcta (es decir, la clave coincide con el sistema operativo instalado en el cliente de KMS). De forma predeterminada, la clave correcta está presente en los sistemas que se han compilado mediante medios del portal del Centro de servicios de licencias por volumen (VLSC). En algunos casos, los clientes pueden usar la activación de la Clave de activación múltiple (MAK) hasta que haya suficientes sistemas en el entorno para admitir la activación de KMS. La Clave de configuración de cliente KMS debe instalarse en estos sistemas para realizar la transición de MAK a KMS. Usa VAMT para instalar esta clave y asegurarte de que se aplique la clave correcta.
- **Estado de licencia**. Este valor muestra el estado del sistema cliente de KMS. Para un sistema que se ha activado mediante KMS, este valor debe ser **Con licencia**. Cualquier otro valor puede indicar que hay un problema. Por ejemplo, si el host de KMS funciona correctamente y el cliente de KMS no se activa (por ejemplo, permanece en un estado **Gracia**), es posible que esté impidiendo que el cliente alcance el sistema host (por ejemplo, un problema de firewall, una interrupción de la red o algo similar).
- **Id. del equipo cliente (CMID)** . Cada cliente de KMS debe tener un CMID exclusivo. Como se ha mencionado en la sección [Host de KMS](#kms-host), un problema común relacionado con el recuento es si el entorno tiene un host de KMS activado y suficientes clientes, pero el recuento no aumenta más allá de **1**. Para obtener más información, consulta [The KMS current count does not increase when you add new Windows Vista or Windows 7-based client computers to the network](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista) (El recuento actual de KMS no aumenta al agregar a la red nuevos equipos cliente basados en Windows Vista o Windows 7).
- **Nombre del equipo KMS proveniente de DNS**. Este valor muestra el FQDN del host de KMS que el cliente usó correctamente para la activación, y el puerto TCP usado para la comunicación.
- **El almacenamiento en caché de host de KMS**. El valor final muestra si el almacenamiento en caché está habilitado o no. De forma predeterminada, está habilitado. Esto significa que el cliente de KMS almacena en caché el nombre del host de KMS que usó para la activación, y que se comunica directamente con este host (en lugar de consultar el DNS) cuando es hora de reactivar. Si el cliente no puede ponerse en contacto con el host de KMS almacenado en caché, consulta al DNS para detectar un nuevo host de KMS.

### <a name="useful-kms-client-events"></a>Eventos útiles del cliente de KMS

#### <a name="event-id-12288-and-event-id-12289"></a>Id. de evento 12288 e Id. de evento 12289

Cuando un cliente de KMS se activa o se reactiva correctamente, el cliente registra dos eventos: el Id. de evento 12288 y el Id. de evento 12289. El siguiente segmento de una entrada de Id. de evento 12288 procede del registro de eventos del Servicio de administración de claves del cliente de KMS.

![Id. de evento 12288 del cliente de KMS](./media/ee939272.client_12288(en-us,technet.10).png)

Si solo ves el Id. de evento 12288 (sin el Id. de evento 12289 correspondiente), significa que el cliente de KMS no ha podido comunicarse con el host de KMS, que el host de KMS no ha respondido o que el cliente no ha recibido la respuesta. En este caso, verifica si el host de KMS es reconocible y si los clientes de KMS pueden ponerse en contacto con él.

La información más importante en el Id. de evento 12288 son los datos de la sección de información. Por ejemplo, en esta sección se muestra el estado actual del cliente más el FQDN y el puerto TCP que el cliente usó cuando intentó activarse. Puedes usar el FQDN para solucionar problemas de casos en los que el recuento de un host de KMS no aumente. Por ejemplo, si hay demasiados hosts de KMS disponibles para los clientes (ya sean sistemas legítimos o no autorizados), el recuento se puede distribuir entre todos ellos.

Una activación incorrecta no siempre significa que el cliente tiene 12288 y no 12289. Una activación o reactivación incorrecta también puede tener ambos eventos. En este caso, tienes que examinar el segundo evento para comprobar el motivo del error.

![Id. de evento 12289 del cliente de KMS](./media/ee939272.client_12289(en-us,technet.10).png)

La sección de información del Id. de evento 12289 proporciona la siguiente información:

- **Marca de activación**. Este valor indica si la activación se realizó correctamente (**1**) o no (**0**).
- **Recuento actual en el host de KMS**. Este valor refleja el valor de recuento en el host de KMS cuando el cliente intenta activarse. Si no se realiza la activación, puede deberse a que el recuento no es suficiente para este sistema operativo de cliente o que no hay sistemas suficientes en el entorno para compilar el recuento.

## <a name="what-does-support-ask-for"></a>¿Qué pide Soporte técnico?

Si tienes que llamar a Soporte técnico para solucionar problemas de activación, el ingeniero de soporte técnico normalmente pedirá la siguiente información:

- La salida de **slmgr.vbs /dlv** del host de KMS y los sistemas cliente de KMS. Tanto si usas wscript como cscript para ejecutar el comando, puede usar CTRL+C para copiar el resultado y, luego, pegarlo en el Bloc de notas para enviarlo al contacto de Soporte técnico.
- Registros de eventos del host de KMS (registro del Servicio de administración de claves) y de los sistemas cliente de KMS (registro de aplicación)

## <a name="additional-references"></a>Referencias adicionales
- [Ask the Core Team: #Activation](/archive/blogs/askcore/kms-host-client-count-not-increasing-due-to-duplicate-cmids) (Pregunta al equipo principal: #Activación)
