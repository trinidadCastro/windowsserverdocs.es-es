---
title: Migración de las licencias de acceso de cliente para Servicios de Escritorio remoto (CAL de RDS)
description: En este artículo se describe cómo migrar sus licencias de acceso de cliente de servicios de escritorio remoto a los nuevos servidores de licencias de Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: 3b809d4d624f21eaeae5a66277db683c95c79bdd
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034446"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Migración de las licencias de acceso de cliente para Servicios de Escritorio remoto (CAL de RDS)

Tiene tres opciones para migrar las CAL de RDS:
1. Método de conexión automática: Este método recomendado se comunica a través de internet directamente a Microsoft Clearinghouse saliente a través del puerto TCP 443.  
2. Mediante un explorador web: Este método permite la migración cuando el servidor que ejecuta la herramienta Administrador de licencias de escritorio remoto no tiene conectividad a internet, pero el administrador tiene conectividad a internet en un dispositivo independiente. La dirección URL para el método de migración de Web se muestra en el Asistente para administrar CAL de RDS. 
3. Uso de un teléfono: Este método permite al administrador completar el proceso de migración por teléfono con un representante de Microsoft. El número de teléfono viene determinada por el país o región que eligió en el Asistente para activar servidor y se muestra en el Asistente para administrar CAL de RDS.

En este artículo, el [método de migración de CAL de RDS establecer](#establish-rds-cal-migration-method) resalta los pasos generales comunes a través de cualquier método de migración de CAL de RDS, mientras que [migrar las CAL de RDS](#migrate-rds-cals) resalta los pasos específicos para cada migración método.

Independientemente del método de migración, como mínimo, debe un miembro del grupo Administradores local para realizar los pasos de migración.

## <a name="establish-rds-cal-migration-method"></a>Establecer método de migración de CAL de RDS

1. En el servidor de licencias, abra **el Administrador de licencias de escritorio remoto**. (Haga clic en **Inicio > Herramientas administrativas**. Escriba el **servicios de escritorio remoto** directorio e inicie **el Administrador de licencias de escritorio remoto**.)
2. Compruebe que el método de conexión para el servidor de licencias de escritorio remoto: derecho a la que desea migrar las CAL de RDS y, a continuación, haga clic en el servidor de licencias **propiedades**. En el **método de conexión** , compruebe la **método de conexión** -puede cambiarlo en el menú desplegable. Haga clic en **Aceptar**.
3. Haga clic en el servidor de licencias al que desea migrar las CAL de RDS y, a continuación, haga clic en **administrar CAL de RDS**.
4. Siga los pasos del Asistente para la **selección de acción** página. Haga clic en **migrar las CAL de RDS desde otro servidor de licencias para este servidor de licencias**.
6. Elegir el motivo para migrar las CAL de RDS y, a continuación, haga clic en **siguiente**. Tiene las siguientes opciones:
    - El servidor de licencias de origen se está reemplazando por este servidor de licencias.
    - El servidor de licencias de origen ya no está funcionando.
7. La siguiente página del Asistente depende de la razón de migración que eligió.
    - Si eligió **el servidor de licencias de origen se está reemplazando por este servidor de licencias** como motivo para migrar las CAL de RDS, el **información del servidor de licencias de origen** se muestra la página.
    
       En la página de información del servidor de licencias de origen, escriba el nombre o dirección IP del servidor de licencias de origen.

       Si el servidor de licencias de origen está disponible en la red, haga clic en **siguiente**. El asistente pone en contacto con el servidor de licencias de origen. Si el servidor de licencias de origen se está ejecutando un sistema operativo anterior a Windows Server 2008 R2 o se desactiva el servidor de licencias de origen, se recuerda que quite manualmente las CAL de RDS desde el servidor de licencias de origen después de completar el asistente. Después de confirmar que comprende este requisito, la **obtener un paquete de clave de licencia de cliente** aparece la página.

       Si el servidor de licencias de origen no está disponible en la red, seleccione **el servidor de licencias de origen especificado no está disponible en la red**. Especifique el sistema operativo que se está ejecutando el servidor de licencias de origen y, a continuación, proporcione el identificador del servidor de licencias para el servidor de licencias de origen. Tras hacer clic en **siguiente**, se recuerda que quite manualmente las CAL de RDS desde el servidor de licencias de origen después de completar el asistente. Después de confirmar que comprende este requisito, la **obtener un paquete de clave de licencia de cliente** aparece la página.

    - Si eligió **deja de funcionar el servidor de licencias de origen** como motivo para migrar las CAL de RDS, se recuerda que quite manualmente las CAL de RDS desde el servidor de licencias de origen después de completar el asistente. Después de confirmar que comprende este requisito, la **obtener un paquete de clave de licencia de cliente** aparece la página.

El siguiente paso es migrar las CAL: use la información siguiente para completar al asistente. Tenga en cuenta que lo que ve en el Asistente depende del método de conexión que identificó en el paso 2 anterior.

## <a name="migrate-rds-cals"></a>Migrar las CAL de RDS

Existen tres mecanismos para migrar las licencias para el servidor de licencias de destino; Siga los pasos correspondientes a la **método de conexión** comprobado en el paso 2:
  - [Método de conexión automática](#automatic-connection-method)
  - [Mediante un explorador web](#using-a-web-browser)
  - [Uso de un teléfono](#using-a-telephone)

### <a name="automatic-connection-method"></a>Método de conexión automática

1. En el **programa de licencias** página, seleccione el programa a través del cual adquirió las CAL de RDS y, a continuación, haga clic en **siguiente**.
2. Escriba la información necesaria (normalmente un código de licencia o un número de contrato, según la **programa de licencias**) y, a continuación, haga clic en **siguiente**. Consulte la documentación suministrada al comprar las CAL de RDS.
4. Seleccione la versión del producto, el tipo de licencia y la cantidad de CAL de RDS para su entorno en función de su contrato de adquisición de CAL de RDS y, a continuación, haga clic en **siguiente**.
5. El Centro de activación de Microsoft (Clearinghouse), con el que se establece contacto automáticamente, se encarga de procesar su solicitud . A continuación, se migran las CAL de RDS en el servidor de licencias.
6. Haga clic en **finalizar** para completar el proceso de migración de CAL de RDS.

### <a name="using-a-web-browser"></a>Mediante un explorador web
1. En el **obtener un paquete de clave de licencia de cliente** página, haga clic en el hipervínculo para conectar al sitio Web de licencias de servicios de escritorio remoto.
Si está ejecutando el Administrador de licencias de escritorio remoto en un equipo que no tiene conectividad a Internet, anote la dirección del sitio Web de licencias de servicios de escritorio remoto y, a continuación, conectarse al sitio Web desde un equipo que tenga conectividad a Internet. 
2. En la página Web de licencias de servicios de escritorio remoto, en **seleccione opción**, seleccione **administrar CAL**y, a continuación, haga clic en **siguiente**.
3. Proporcione la siguiente información necesaria, a continuación, haga clic en **siguiente**:
    - **Identificador del servidor de licencias de destino**: Un número de 35 dígitos, en grupos de 5 números, que se muestra en el **obtener un paquete de clave de licencia de cliente** página del Asistente para administrar CAL de RDS.
    - **Motivo para la recuperación**: Elegir el motivo para migrar las CAL de RDS.
    - **Programa de licencias**: Elija el programa a través del cual adquirió las CAL de RDS.
4. Proporcione la siguiente información necesaria, a continuación, haga clic en **siguiente**:
    - Apellidos
    - Nombre o un nombre dado
    - Nombre de la empresa
    - País o región

    También puede proporcionar la información opcional solicitada, como la dirección de la compañía, dirección de correo electrónico y número de teléfono. En el campo de unidad organizativa, puede describir la unidad dentro de su organización que actúa este servidor de licencias.

5. El programa de licencias que ha seleccionado en la página anterior determinará la información que debe proporcionar en la página siguiente. En la mayoría de los casos, debe proporcionar un código de licencia o un número de contrato. Consulte la documentación suministrada al comprar las CAL de RDS. Además, deberá especificar qué tipo de CAL de RDS y la cantidad que desea migrar al servidor de licencias.
6. Después de haber proporcionado la información necesaria, haga clic en **Siguiente**.
7. Compruebe que toda la información que ha escrito es correcta, a continuación, haga clic en **siguiente** para enviar la solicitud a Microsoft Clearinghouse. La página web, a continuación, muestra un identificador de paquete de claves de licencia generado por Microsoft Clearinghouse.

   > [!IMPORTANT] 
   > Mantenga una copia del identificador del paquete de claves de licencia. Esta información le facilita las comunicaciones con Microsoft Clearinghouse, si necesita ayuda para recuperar CAL de RDS.

8. En el mismo **obtener un paquete de clave de licencia de cliente** página, escriba el identificador de paquete de claves de licencia y, a continuación, haga clic en **siguiente** para migrar las CAL de RDS a su servidor de licencias.
9. Haga clic en **finalizar** para completar el proceso de migración de CAL de RDS.

### <a name="using-a-telephone"></a>Uso de un teléfono
1. En el **obtener un paquete de clave de licencia de cliente** , utilice el número de teléfono de muestra para llamar a Microsoft Clearinghouse. Proporcione al representante el identificador de servidor de licencias de escritorio remoto y la información necesaria para el programa de licencias a través del cual adquirió las CAL de RDS. El representante le proporciona un identificador único para las CAL de RDS y, a continuación, procesa su solicitud para migrar las CAL de RDS. Este identificador único se conoce como el **Id. de paquete de claves de licencia**.

   > [!IMPORTANT]
   > Mantenga una copia del identificador del paquete de claves de licencia. Esta información le facilita las comunicaciones con Microsoft Clearinghouse si necesita ayuda para recuperar CAL de RDS.

2. En el mismo **obtener un paquete de clave de licencia de cliente** página, escriba el identificador de paquete de claves de licencia y, a continuación, haga clic en **siguiente** para migrar las CAL de RDS a su servidor de licencias.
3. Haga clic en **finalizar** para completar el proceso de migración de CAL de RDS.
