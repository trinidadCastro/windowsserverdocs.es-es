---
title: Solucionar problemas de perfiles de usuario con eventos
description: Cómo solucionar problemas al cargar y descargar perfiles de usuario mediante eventos y registros de seguimiento.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e927a77627e786015a928d798aafee13a2cc34b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394382"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Solucionar problemas de perfiles de usuario con eventos

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server (canal semianual).

En este tema se describe cómo solucionar problemas de carga y descarga de perfiles de usuario mediante eventos y registros de seguimiento. En las secciones siguientes se describe cómo usar los tres registros de eventos que registran información de Perfil de usuario.

## <a name="step-1-checking-events-in-the-application-log"></a>Paso 1: Comprobar eventos en el registro de aplicaciones

El primer paso para la solución de problemas con la carga y descarga de perfiles de usuario (incluidos los perfiles de usuario móviles) es usar Visor de eventos para examinar los eventos de advertencia y error que registra el servicio de perfiles de usuario en el registro de aplicaciones.

Aquí se muestra cómo ver eventos de servicios de perfiles de usuario en el registro de aplicaciones:

1. Inicie Visor de eventos. Para ello, abra el **Panel de control**, **Seleccione sistema y seguridad**y, a continuación, en la sección **herramientas administrativas** , seleccione **Ver registros de eventos**. Se abre la ventana Visor de eventos.
2. En el árbol de consola, vaya primero a **registros de Windows**y luego a **aplicación**.
3. En el panel acciones, seleccione **filtrar registro actual**. Se abre el cuadro de diálogo filtrar registro actual.
4. En el cuadro **orígenes de eventos** , active la casilla servicio de perfiles de **usuario** y, después, haga clic en **Aceptar**.
5. Revise la lista de eventos y preste especial atención a los eventos de error.
6. Cuando encuentre eventos destacados, seleccione el vínculo ayuda en línea de registro de eventos para mostrar información adicional y procedimientos de solución de problemas.
7. Para realizar más tareas de solución de problemas, tenga en cuenta la fecha y la hora de los eventos destacados y examine el registro operativo (como se describe en el paso 2) para ver los detalles acerca de lo que el servicio de perfiles de usuario estaba haciendo aproximadamente a la hora de los eventos de error o de advertencia.

>[!NOTE]
>Puede omitir con seguridad el evento 1530 de servicio de Perfil de usuario "Windows detectó que el archivo de registro todavía está siendo utilizado por otras aplicaciones o servicios".

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Paso 2: Ver el registro operativo del servicio de perfiles de usuario

Si no puede resolver el problema solo con el registro de aplicación, utilice el procedimiento siguiente para ver los eventos del servicio de perfiles de usuario en el registro operativo. Este registro muestra algunos de los trabajos internos del servicio y puede ayudar a identificar dónde se está produciendo el problema en el proceso de descarga o carga de perfiles.

El registro de aplicación de Windows y el registro operativo de servicio de Perfil de usuario están habilitados de forma predeterminada en todas las instalaciones de Windows.

Aquí se muestra cómo ver el registro operativo del servicio de Perfil de usuario:

1. En el árbol de consola de Visor de eventos, vaya a **registros de aplicaciones y servicios**, **Microsoft**, después **Windows**, servicio de **perfiles de usuario**y, a continuación, **operativo**.
2. Examine los eventos que se produjeron aproximadamente a la hora de los eventos de error o de advertencia que anotó en el registro de aplicaciones.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Paso 3: Habilitación y visualización de registros analíticos y de depuración

Si necesita más detalles de los proporcionados por el registro operativo, puede habilitar los registros analíticos y de depuración en el equipo afectado. Este nivel de registro es mucho más detallado y debe deshabilitarse, excepto cuando se intenta solucionar un problema.

Aquí se muestra cómo habilitar y ver los registros analíticos y de depuración:

1. En el panel **acciones** de visor de eventos, seleccione **Ver**y, a continuación, seleccione **Mostrar registros analíticos y de depuración**.
2. Vaya a **registros de aplicaciones y servicios**, **Microsoft**, **Windows**, servicio de **perfiles de usuario**y **diagnóstico**.
3. Seleccione **Habilitar registro** y, a continuación, seleccione **sí**. Esto habilita el registro de diagnóstico, que iniciará el registro.
4. Si necesita información aún más detallada, consulte @no__t 0Step 4: Crear y descodificar un seguimiento @ no__t-0 para obtener más información acerca de cómo crear un registro de seguimiento.
5. Cuando haya terminado de solucionar el problema, vaya al registro de **diagnóstico** , seleccione **deshabilitar registro**, seleccione **Ver** y, a continuación, desactive la casilla **Mostrar registros analíticos y de depuración** para ocultar el registro analítico y de depuración.

## <a name="step-4-creating-and-decoding-a-trace"></a>Paso 4: Crear y descodificar un seguimiento

Si no puede resolver el problema mediante eventos, puede crear un registro de seguimiento (un archivo ETL) mientras se reproduce el problema y, a continuación, descodificarlo mediante símbolos públicos del servidor de símbolos de Microsoft. Los registros de seguimiento proporcionan información muy específica sobre lo que hace el servicio de perfiles de usuario y pueden ayudar a identificar dónde se ha producido el error.

La mejor estrategia al usar el seguimiento ETL es capturar primero el registro más pequeño posible. Una vez descodificado el registro, busque errores en el registro.

Aquí se muestra cómo crear y descodificar un seguimiento para el servicio de Perfil de usuario:

1. Inicie sesión en el equipo en el que el usuario está experimentando problemas con una cuenta que sea miembro del grupo local Administradores.
2. En un símbolo del sistema con privilegios elevados, escriba los siguientes comandos, donde *\<Path @ no__t-2* es la ruta de acceso a una carpeta local que ha creado anteriormente, por ejemplo, C: \\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. En la pantalla Inicio, seleccione el nombre de usuario y, a continuación, seleccione **cambiar de cuenta**, con cuidado de no cerrar la sesión del administrador. Si está utilizando Escritorio remoto, cierre la sesión de administrador para establecer la sesión de usuario.
4. Reproduzca el problema. El procedimiento para reproducir el problema suele ser iniciar sesión como el usuario que experimenta el problema, cerrar la sesión del usuario o ambas cosas.
5. Después de reproducir el problema, inicie sesión como administrador local de nuevo.
6. Desde un símbolo del sistema con privilegios elevados, ejecute el comando siguiente para guardar el registro en un archivo ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Escriba el siguiente comando para exportar el archivo ETL en un archivo legible en el directorio actual (probablemente la carpeta particular o la carpeta% WINDIR% \\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Abra el archivo **Summary. txt** y el archivo **Dumpfile. XML** (puede abrirlos en Microsoft Excel para ver más fácilmente los detalles completos del registro). Busque eventos que incluyan **failed** o **failed**. puede omitir con seguridad las líneas que incluyen el nombre de evento **desconocido** .

## <a name="more-information"></a>Más información

* [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)