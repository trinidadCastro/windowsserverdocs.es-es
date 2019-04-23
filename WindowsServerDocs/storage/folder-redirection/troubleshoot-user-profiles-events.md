---
title: Solucionar problemas de perfiles de usuario con eventos
description: Cómo solucionar problemas de carga y descarga de perfiles de usuario mediante el uso de eventos y registros de seguimiento.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827956"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Solucionar problemas de perfiles de usuario con eventos

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016.

Este tema describe cómo solucionar problemas de carga y descarga de los perfiles de usuario mediante el uso de eventos y registros de seguimiento. Las secciones siguientes describen cómo usar los tres registros de eventos que registran información de perfil de usuario.

## <a name="step-1-checking-events-in-the-application-log"></a>Paso 1: Comprobación de eventos en el registro de aplicación

El primer paso para solucionar problemas con la carga y descarga de perfiles (incluidos los perfiles de usuario móviles) consiste en usar el Visor de eventos para examinar los eventos de Error y advertencia que los registros de servicio de perfil de usuario en la aplicación de registro de usuario.

Aquí le mostramos cómo ver los eventos de servicios de perfiles de usuario en el registro de aplicación:

1. Inicie el Visor de eventos. Para ello, abra **Panel de Control**, seleccione **sistema y seguridad**y, a continuación, en el **herramientas administrativas** sección, seleccione **ver registros de eventos**. Se abre la ventana Visor de eventos.
2. En el árbol de consola, vaya primero a **Windows registros**, a continuación, **aplicación**.
3. En el panel Acciones, seleccione **filtrar registro actual**. Se abre el cuadro de diálogo Filtrar registro actual.
4. En el **orígenes de eventos** cuadro, seleccione el **servicio de perfiles de usuario** casilla de verificación y, a continuación, seleccione **Aceptar**.
5. Revise la lista de eventos, prestando especial atención a los eventos de Error.
6. Cuando encuentre los eventos de interés, seleccione el vínculo Ayuda en línea de registro de eventos para mostrar información adicional y procedimientos de solución de problemas.
7. Para llevar a cabo continuar solucionando el problema, tenga en cuenta la fecha y hora de eventos de interés y, a continuación, examinar el registro operativo (como se describe en el paso 2) para ver los detalles sobre lo que hacía el servicio de perfil de usuario en torno a la hora de los eventos de Error o advertencia.

>[!NOTE]
>Puede ignorar el servicio de perfiles de usuario eventos 1530 "Windows ha detectado que el archivo de registro todavía está en uso por otras aplicaciones o servicios."

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Paso 2: Ver el registro operativo para el servicio de perfil de usuario

Si no se puede resolver el problema mediante el registro de aplicación por sí solo, utilice el procedimiento siguiente para ver los eventos de servicio de perfil de usuario en el registro operativo. Este registro muestra algunos de los mecanismos internos del servicio y puede ayudar a identificar dónde en la carga del perfil o descargar el proceso que se está produciendo el problema.

El registro de aplicación de Windows y el registro operativo de servicio de perfil de usuario están habilitadas de forma predeterminada en todas las instalaciones de Windows.

Aquí le mostramos cómo ver el registro operativo para el servicio de perfil de usuario:

1. En el árbol de consola del Visor de eventos, vaya a **registros de aplicaciones y servicios**, a continuación, **Microsoft**, a continuación, **Windows**, a continuación, **deserviciodeperfilesdeusuario**y, a continuación, **operativa**.
2. Examine los eventos ocurridos aproximadamente a la hora de los eventos de Error o advertencia que anotó en el registro de aplicación.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Paso 3: Habilitar y ver analíticas y registros de depuración

Si necesita más detalles que proporciona el registro operativo, puede habilitar analítico y depurar los registros en el equipo afectado. Este nivel de registro es mucho más detallado y debe deshabilitarse, excepto cuando se intenta solucionar un problema.

Le mostramos cómo habilitar y ver el análisis y registros de depuración:

1. En el **acciones** panel del Visor de eventos, seleccione **vista**y, a continuación, seleccione **mostrar registros analíticos y depuración**.
2. Vaya a **registros de aplicaciones y servicios**, a continuación, **Microsoft**, a continuación, **Windows**, a continuación, **servicio de perfil de usuario**y, a continuación,  **Diagnóstico**.
3. Seleccione **Habilitar registro** y, a continuación, seleccione **Sí**. Esto permite el registro de diagnóstico, que se iniciará el registro.
4. Si necesita información más detallada, consulte [paso 4: Creación y descodificar una traza](#step-4:-creating-and-decoding-a-trace) para obtener más información sobre cómo crear un registro de seguimiento.
5. Cuando haya terminado de solucionar el problema, vaya a la **diagnóstico** registro, seleccione **Deshabilitar registro**, seleccione **vista** y, a continuación, desactive la **mostrar Depurar registros analíticos y** casilla para ocultar la analítica y el registro de depuración.

## <a name="step-4-creating-and-decoding-a-trace"></a>Paso 4: Creación y descodificar un seguimiento

Si no se puede resolver el problema mediante los eventos, puede crear un registro de seguimiento (un archivo ETL) mientras se reproduce el problema y, a continuación, descodificar mediante símbolos públicos desde el servidor de símbolos de Microsoft. Los registros de seguimiento proporcionan información muy específica sobre lo que hace el servicio de perfil de usuario y puede ayudarle a identificar dónde se produjo el error.

Es la mejor estrategia al utilizar el seguimiento de ETL capturar en primer lugar el registro más pequeño posible. Una vez que se descodifica el registro, busque el registro de errores.

Le mostramos cómo crear y descodificar un seguimiento para el servicio de perfil de usuario:

1. Inicie sesión en el equipo donde el usuario está experimentando problemas, con una cuenta que sea miembro del grupo Administradores local.
2. Desde un símbolo del sistema con privilegios elevados, escriba los comandos siguientes, donde *\<ruta\>* es la ruta de acceso a una carpeta local que ha creado anteriormente, por ejemplo C:\\registros:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. En la pantalla Inicio, seleccione el nombre de usuario y, a continuación, seleccione **cambiar cuenta**, teniendo cuidado de no cerrar el administrador. Si está utilizando Escritorio remoto, cierre la sesión de administrador para establecer la sesión de usuario.
4. Reproduzca el problema. El procedimiento para reproducir el problema es normalmente para iniciar sesión como el usuario que experimentan el problema, inicie sesión el usuario, o ambos.
5. Después de reproducir el problema, inicie sesión en como administrador local nuevo.
6. Desde un símbolo del sistema con privilegios elevados, ejecute el siguiente comando para guardar el registro en un archivo ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Escriba el siguiente comando para exportar el archivo ETL en un archivo de lenguaje natural en el directorio actual (es probable que la carpeta principal o la carpeta % WINDIR %\\carpeta System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Abra el **Summary.txt** archivo y **Dumpfile.xml** archivo (puede abrirlos en Microsoft Excel para ver más fácilmente los detalles completos del registro). Busque eventos que incluyen **producirá un error en** o **no se pudo**; puede pasar por alto las líneas que incluyen la **desconocido** nombre del evento.

## <a name="more-information"></a>Más información

* [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)