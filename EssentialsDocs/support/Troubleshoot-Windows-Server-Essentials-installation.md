---
title: "Solucionar problemas de instalación de Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 293b392203269a65efffcefb3744bedc659f71c9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Solucionar problemas de instalación de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tema proporciona la solución de problemas para los problemas que se producen durante la instalación de Windows Server Essentials. Guía se proporciona en las siguientes áreas:  
  

-   [Pasos de solución de problemas generales](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de controladores](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Pasos de solución de problemas generales](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de controladores](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Para obtener información sobre solución de problemas más reciente de la Comunidad de Windows Server Essentials, te sugerimos que visite el [foro de Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). El foro de Windows Server Essentials es un lugar excelente para buscar ayuda, o hacer una pregunta.  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a>Pasos de solución de problemas generales  
 Si se produce un error en la instalación de Windows Server Essentials, siga estos pasos para ayudar a identificar el problema que causó el error.  
  
> [!IMPORTANT]
>  Es importante que no reinicie manualmente el servidor durante la instalación de Windows Server Essentials. El servidor se reiniciará automáticamente varias veces durante la instalación y configuración inicial. Si ha reiniciado el servidor manualmente antes que vio el **instalación correcta del servidor** mensaje, que es posible que se interrumpió la instalación y provocó un error.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Para identificar problemas de un error de instalación de Windows Server Essentials  
  
1.  Comprueba que el hardware de servidor cumple los requisitos mínimos. Para obtener información sobre los requisitos de hardware, consulta [requisitos del sistema para Windows Server Essentials](../get-started/system-requirements.md).  
  
2.  Si recibe el DVD de instalación de Windows Server Essentials de MSDN, comprueba que el DVD es válido comprobando la suma SHA1. Para obtener más información, consulta [disponibilidad y descripción de la herramienta Comprobador de integridad de suma de comprobación de archivo](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Comprueba que el adaptador de red en el servidor está conectado a un enrutador mediante un cable de red.  
  
4.  Si el servidor tiene más de un adaptador de red, compruebe que el adaptador de solo red está habilitado.  
  
    > [!IMPORTANT]
    >  No desconecte el cable de red o reinicia el enrutador durante la instalación de Windows Server Essentials.  
  
5.  Revisar "instalación del servidor e implementación" en [documentación de versión para Windows Server Essentials](../get-started/release-notes.md)  
  
6.  Si recibes el mensaje de error Error al configurar el servidor durante la instalación, usa el DVD de recuperación del servidor y las instrucciones proporcionadas por el fabricante del hardware para restaurar el servidor para la configuración predeterminada de fábrica.  
  
##  <a name="BKMK_TroubleshootDrivers"></a>Solucionar problemas de controladores  
 El problema más común cuando se instala Windows Server Essentials es controladores de almacenamiento que se deben tener controladores que se instalan manualmente. Windows incluye controladores para muchos de los controladores de almacenamiento, pero no puede incluir controladores para tu hardware específico.  
  
 También es posible que debas instalar manualmente los controladores de tarjeta de red para tu hardware específico.  
  
###  <a name="BKMK_StorageDrivers"></a>Agregar controladores para los controladores de almacenamiento  
 Si tu hardware requiere controladores de almacenamiento que no se incluyen con Windows Server Essentials, usa la siguiente información para completar la instalación.  
  
 Si aparece el mensaje siguiente durante la instalación, debes agregar manualmente los controladores para el controlador de almacenamiento:  
  
 **Error de instalación de Windows Server**  
  
 No se encontró la unidad de disco duro pueden hospedar Windows Server Essentials. ¿Quieres cargar los controladores de almacenamiento adicional?  
  
 Usa el siguiente procedimiento para instalar a un controlador de controladora de almacenamiento.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Instalar manualmente un controlador de controladora de almacenamiento  
  
1.  Encuentra los controladores para el controlador de almacenamiento. Estos proporcionados por el fabricante del hardware, y también podrían estar disponibles en el sitio Web del fabricante.  
  
2.  Crea una carpeta denominada de controladores en un disco o unidad flash USB y, a continuación, copia los controladores en la carpeta.  
  
3.  Conecte la unidad de disco o unidad flash USB con los controladores en el equipo  
  
4.  Arranca el equipo desde el DVD de Windows Server Essentials.  
  
     Si faltan los controladores de controladora de almacenamiento, se muestra el cuadro de diálogo de Error de instalación de Windows Server Essentials.  
  
5.  En el cuadro de diálogo de Error de instalación de Windows Server Essentials, haz clic en **Sí** para cargar los controladores de almacenamiento adicional.  
  
6.  En la **selecciona archivo inf de controlador de tu** pedir, navega al archivo .inf en la carpeta de controladores en el disco o unidad flash USB, selecciona el archivo, haz clic en el nombre de archivo y, a continuación, haz clic en **abrir**. Esto carga el controlador.  
  
    > [!NOTE]
    >  Antes de intentar cargar el archivo, comprueba que la extensión de nombre de archivo (.inf) en minúsculas. Esta operación distingue mayúsculas de minúsculas, y no se cargará un archivo de controlador si la extensión de nombre de archivo tiene letras en mayúsculas.  
  
7.  En el símbolo del sistema, haga clic en **Sí** para hacer que el controlador de almacenamiento disponibles durante la fase de configuración modo de texto.  
  
 El programa de instalación debería continuar normalmente.  
  
###  <a name="BKMK_AddingNICdrivers"></a>Agregar controladores para adaptadores de red  
 Si un adaptador de red en el equipo no es compatible con Windows Server Essentials, el servidor no tendrá conectividad de red después de finalizar la instalación y no podrás conectar equipos a tu servidor.  
  
 Al final de la instalación de Windows Server Essentials, se le informa de si un controlador del adaptador de red no se instala automáticamente. También puedes usar **conexiones de red** en el Panel de Control para comprobar si un controlador del adaptador de red que faltan. Si no ves una conexión de red asociada con el adaptador de red en el servidor, debes instalar un controlador.  
  
 Si el equipo no tiene un controlador compatible para cualquier adaptador de red, debes instalar manualmente el controlador del adaptador de red correcta y, a continuación, reiniciar el servidor. Usa el siguiente procedimiento.  
  
##### <a name="to-install-a-network-adapter-driver"></a>Para instalar a un controlador del adaptador de red  
  
1.  Obtener el controlador que falta del fabricante del adaptador de red.  
  
2.  Sigue las instrucciones del fabricante instalación para instalar al controlador.  
  
3.  Reinicia el equipo.  
  
    > [!IMPORTANT]
    >  Si no reinicia el servidor después de instalar al controlador del adaptador de red que faltan, puede producir un error de instalación del software del conector de Windows Server Essentials en los equipos cliente.
