---
title: Migración de las licencias de acceso de cliente para Servicios de Escritorio remoto (CAL de RDS)
description: En este artículo se describe cómo migrar las licencias de acceso de cliente de Servicios de Escritorio remoto a los nuevos servidores de Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: 5d95c2bc3a92a8cdcba4b308c88d94cb9af6d2a5
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855998"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Migración de las licencias de acceso de cliente para Servicios de Escritorio remoto (CAL de RDS)

Tienes tres opciones para migrar las CAL de RDS:
1. Método de conexión automática: Este método recomendado se comunica a través de Internet directamente a la salida del Centro de activación de Microsoft (Clearinghouse) por el puerto TCP 443.  
2. Mediante un explorador web: Este método permite la migración cuando el servidor que ejecuta la herramienta Administrador de licencias de Escritorio remoto no tiene conexión a Internet, pero el administrador tiene conectividad a Internet en un dispositivo independiente. La dirección URL para el método de migración de Web se muestra en el Asistente para administrar las CAL de RDS. 
3. Mediante un teléfono: Este método permite al administrador completar el proceso de migración por teléfono con un representante de Microsoft. El número de teléfono apropiado viene determinado por el país o región que hayas elegido en el Asistente de activación del servidor y se muestra en el Asistente para administrar CAL de RDS.

En este artículo, la sección [Establecimiento del método de migración de las CAL de RDS](#establish-rds-cal-migration-method) resalta los pasos generales comunes en cualquier método de migración de CAL de RDS, mientras que [Migración de las CAL de RDS](#migrate-rds-cals) resalta los pasos específicos para cada método de migración.

Con independencia del método de migración, debes, como mínimo, ser miembro del grupo de administradores locales para realizar los pasos de migración.

## <a name="establish-rds-cal-migration-method"></a>Establecimiento del método de migración de las CAL de RDS

1. En el servidor de licencias, abra **Administrador de licencias de Escritorio remoto**. Haz clic en **Inicio > Herramientas administrativas**. Especifica el directorio de **Servicios de Escritorio remoto** e inicia **Administrador de licencias de Escritorio remoto**).
2. Comprueba el método de conexión para el servidor de licencias de Escritorio remoto: haz clic con el botón derecho en el servidor de licencias al que quieres migrar las CAL de RDS y, después, haz clic en **Propiedades**. En la pestaña **Método de conexión**, comprueba el **método de conexión**. También puedes cambiarlo en el menú desplegable. Haga clic en **Aceptar**.
3. Haz clic con el botón derecho en el servidor de licencias al que quieras migrar las CAL de RDS y, después, haz clic en **Manage RDS CALs** (Administrar CAL de RDS).
4. Sigue los pasos del Asistente a la página **Selección de acción**. Haz clic en **Migrate RDS CALs from another license server to this license server** (Migrar las CAL de RDS desde otro servidor de licencias a este servidor de licencias).
6. Elige el motivo para migrar las CAL de RDS y, después, haz clic en **Siguiente**. Dispone de las siguientes opciones:
    - El servidor de licencias de origen se va a reemplazar por este servidor de licencias.
    - El servidor de licencias de origen ha dejado de funcionar.
7. La página siguiente del Asistente depende del motivo de migración que hayas elegido.
    - Si selecciona **El servidor de licencias de origen se va a reemplazar por este servidor de licencias** como motivo para migrar las CAL de RDS, se mostrará la página **Información del servidor de licencias de origen**.
    
       En la página Información del servidor de licencias de origen, introduzca el nombre o la dirección IP del servidor de licencias de origen.

       Si el servidor de licencias de origen está disponible en la red, haz clic en **Siguiente**. El asistente se pone en contacto con el servidor de licencias de origen. Si el servidor de licencias de origen ejecuta un sistema operativo anterior a Windows Server 2008 R2 o el servidor de licencias de origen está desactivado, se te recuerda que debes quitar las CAL de RDS manualmente del servidor de licencias de origen después de que el asistente haya finalizado. Después de confirmar que entiende este requisito, aparece la página **Obtener el paquete de claves de licencia de cliente**.

       Si el servidor de licencias de origen no está disponible en la red, selecciona **El servidor de licencias de origen especificado no está disponible en la red**. Especifica el sistema operativo en el que se está ejecutando el servidor de licencias de origen y, a continuación, proporciona la identificación del servidor de licencias de origen. Después de hacer clic en **Siguiente**, se te recordará que debes eliminar las CAL de RDS manualmente del servidor de licencias de origen una vez que el asistente haya finalizado. Después de confirmar que entiende este requisito, aparece la página **Obtener el paquete de claves de licencia de cliente**.

    - Si eliges **El servidor de licencias de origen ha dejado de funcionar** como motivo para migrar las CAL de RDS, se te recuerda que debes eliminar las CAL de RDS manualmente del servidor de licencias de origen una vez que el asistente haya finalizado. Después de confirmar que entiende este requisito, aparece la página **Obtener el paquete de claves de licencia de cliente**.

El siguiente paso es migrar las CAL: utiliza la información siguiente para completar al asistente. Ten en cuenta que lo que ves en el asistente depende del método de conexión que hayas identificado en el paso 2 anterior.

## <a name="migrate-rds-cals"></a>Migración de las CAL de RDS

Existen tres mecanismos para migrar licencias al servidor de licencias de destino; continúa con los pasos correspondientes al **Método de conexión** comprobado en el paso 2:
  - [Método de conexión automática](#automatic-connection-method)
  - [Uso de un explorador web](#using-a-web-browser)
  - [Uso de un teléfono](#using-a-telephone)

### <a name="automatic-connection-method"></a>Método de conexión automática

1. En la página **Programa de licencias**, selecciona el programa correspondiente mediante el cual has adquirido las CAL de RDS y haz clic en **Siguiente**.
2. Especifica la información necesaria (normalmente un código de licencia o un número de contrato, en función del **programa de licencia**) y, a continuación, haz clic en **Siguiente**. Consulta la documentación suministrada al comprar las CAL de RDS.
4. Selecciona la versión del producto, el tipo de licencia y la cantidad de CAL de RDS adecuados para tu entorno según el acuerdo de compra de CAL de RDS y haz clic en **Siguiente**.
5. El Centro de activación de Microsoft (Clearinghouse), con el que se establece contacto automáticamente, se encarga de procesar su solicitud . Las CAL de RDS migrarán después al servidor de licencias.
6. Haz clic en **Finalizar** para completar el proceso de migración de CAL de RDS.

### <a name="using-a-web-browser"></a>Uso de un explorador web
1. En la página **Obtener un paquete de claves de licencia de cliente**, haz clic en el hipervínculo para conectarse al sitio web de Administración de licencias de Servicios de Escritorio remoto.
   Si estás ejecutando el administrador de licencias de Escritorio remoto en un equipo que no tiene conexión a Internet, anota la dirección del sitio web de administración de licencias de Servicios de Escritorio remoto y, a continuación, conéctate al sitio web desde un equipo que tenga conexión a Internet. 
2. En la página web de Administración de licencias de Servicios de Escritorio remoto, en **Select Option** (Seleccionar opciones), selecciona **Manage CALs** (Administrar CAL) y, después, haz clic en **Siguiente**.
3. Proporciona la siguiente información necesaria y, después, haz clic en **Siguiente**:
    - **Identificación del servidor de licencias de destino**: Un número de 35 dígitos, en grupos de cinco números, que se muestra en la página **Obtener un paquete de claves de licencia de cliente** en el Asistente para administrar CAL de RDS.
    - **Motivo para la recuperación**: elige el motivo para migrar las CAL de RDS.
    - **Programa de licencias**: elige el programa a través del cual has adquirido las CAL de RDS.
4. Proporciona la siguiente información necesaria y, después, haz clic en **Siguiente**:
   - Apellidos
   - Nombre propio
   - Nombre de la empresa
   - País o región

     También puedes proporcionar la información opcional solicitada, como la dirección de la empresa, la dirección de correo electrónico y el número de teléfono. En el campo de la unidad organizativa, puedes describir la unidad dentro de la organización a la que sirve este servidor de licencias.

5. El programa de licencias que has seleccionado en la página anterior determina la información que deberás proporcionar en la página siguiente. En la mayoría de los casos, deberás proporcionar un código de licencia o un número del acuerdo. Consulta la documentación suministrada al comprar las CAL de RDS. Además, deberás especificar qué tipo de CAL de RDS y la cantidad que deseas migrar al servidor de licencias.
6. Después de haber proporcionado la información necesaria, haga clic en **Siguiente**.
7. Comprueba que toda la información que has indicado es correcta y, después, haz clic en **Siguiente** para enviar tu solicitud al Centro de activación de Microsoft (Clearinghouse). La página web muestra una identificación del paquete de claves de licencia generado por el Centro de activación de Microsoft (Clearinghouse).

   > [!IMPORTANT] 
   > Conserva una copia de la identificación del paquete de claves de licencia. Si necesitas ayuda para recuperar las CAL de RDS, esta información te permitirá agilizar la comunicación con el Centro de activación de Microsoft (Clearinghouse).

8. En la misma página **Obtener un paquete de claves de licencia de cliente**, introduce la identificación del paquete de claves de licencia y, después, haz clic en **Siguiente** para migrar las CAL de RDS a tu servidor de licencias.
9. Haz clic en **Finalizar** para completar el proceso de migración de CAL de RDS.

### <a name="using-a-telephone"></a>Uso de un teléfono
1. En la página **Obtener un paquete de claves de licencia de cliente**, utiliza el número de teléfono que se muestra para llamar al Centro de activación de Microsoft (Clearinghouse). Proporciona al representante la identificación del servidor de licencias de Escritorio remoto y la información necesaria para el programa de licencias mediante el cual has adquirido tus CAL de RDS. Después, el representante procesa tu solicitud para instalar las CAL de RDS y te asigna una identificación única para ellas. Esta identificación única se conoce como la **identificación del paquete de clave de licencia**.

   > [!IMPORTANT]
   > Conserva una copia de la identificación del paquete de claves de licencia. Si necesitas ayuda para recuperar las CAL de RDS, esta información te permitirá agilizar la comunicación con el Centro de activación de Microsoft (Clearinghouse).

2. En la misma página **Obtener un paquete de claves de licencia de cliente**, introduce la identificación del paquete de claves de licencia y, después, haz clic en **Siguiente** para migrar las CAL de RDS a tu servidor de licencias.
3. Haz clic en **Finalizar** para completar el proceso de migración de CAL de RDS.
