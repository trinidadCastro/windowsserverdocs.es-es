---
title: Solucionar problemas en la instalación de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862026"
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Solucionar problemas en la instalación de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En este tema proporciona una solución de problemas que se producen al instalar Windows Server Essentials. Se proporciona orientación en las áreas siguientes:  
  

-   [Pasos de solución de problemas generales](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de controladores](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Pasos de solución de problemas generales](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de controladores](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Para la información de solución de problemas más reciente de la Comunidad de Windows Server Essentials, le recomendamos que visite el [foro de Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). El foro de Windows Server Essentials es un buen lugar para buscar ayuda o realizar una pregunta.  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a> Pasos de solución de problemas generales  
 Si se produce un error en la instalación de Windows Server Essentials, siga estos pasos para ayudar a identificar el problema que provocó el error.  
  
> [!IMPORTANT]
>  Es importante que no reinicie manualmente el servidor durante la instalación de Windows Server Essentials. El servidor se reiniciará automáticamente varias veces durante la instalación y la configuración inicial. Si reinicia el servidor de forma manual antes de ver el mensaje **Instalación correcta del servidor**, es posible que la instalación se interrumpiese y esto provocase el error.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Para identificar problemas en una instalación con errores de Windows Server Essentials  
  
1.  Compruebe que el hardware del servidor cumple los requisitos mínimos. Para obtener información acerca de los requisitos de hardware, consulte [requisitos del sistema para Windows Server Essentials](../get-started/system-requirements.md).  
  
2.  Si ha recibido el DVD de instalación de Windows Server Essentials desde MSDN, compruebe que el DVD es válido comprobando la suma de SHA1. Para obtener más información, consulte [disponibilidad y descripción de la herramienta File Checksum Integrity Verifier](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Compruebe que el adaptador de red del servidor está conectado a un enrutador mediante un cable de red.  
  
4.  Si el servidor tiene más de un adaptador de red, compruebe que solo está habilitado un adaptador de red.  
  
    > [!IMPORTANT]
    >  No desconecte el cable de red ni reinicie el enrutador al instalar Windows Server Essentials.  
  
5.  Consulte "instalación del servidor e implementación" en [Release Documentation for Windows Server Essentials](../get-started/release-notes.md)  
  
6.  Si recibe el mensaje de error se produjo un error al configurar el servidor durante la instalación, use el DVD de recuperación del servidor y las instrucciones proporcionadas por el fabricante del hardware para restaurar el servidor a la configuración predeterminada de fábrica.  
  
##  <a name="BKMK_TroubleshootDrivers"></a> Solucionar problemas de controladores  
 El problema más común cuando se instala Windows Server Essentials es controladores de almacenamiento que necesitan tener controladores instalados manualmente. Windows incluye controladores para muchos controladores de almacenamiento, pero podría no incluir controladores para su hardware en concreto.  
  
 También podría tener que instalar manualmente los controladores de tarjeta de red de su hardware en concreto.  
  
###  <a name="BKMK_StorageDrivers"></a> Agregar controladores para los controladores de almacenamiento  
 Si su hardware requiere controladores de almacenamiento que no se incluyen con Windows Server Essentials, use la información siguiente para completar la instalación.  
  
 Si ve el mensaje siguiente durante la instalación, deberá agregar manualmente controladores para el controlador de almacenamiento:  
  
 **Error de instalación de Windows Server**  
  
 No se encontró la unidad de disco duro capaces de alojar Windows Server Essentials. ¿Desea cargar controladores de almacenamiento adicionales?  
  
 Use el siguiente procedimiento para instalar un controlador del controlador de almacenamiento.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Para instalar manualmente un controlador del controlador de almacenamiento  
  
1.  Busque los controladores del controlador de almacenamiento. Los proporciona el fabricante de hardware, pero también pueden estar disponibles en el sitio web del fabricante.  
  
2.  Cree una carpeta denominada CONTROLADORES en un disquete o una unidad flash USB y copie los controladores en la carpeta.  
  
3.  Conecte al equipo la unidad de disquete o la unidad flash USB con los controladores.  
  
4.  Arranque el equipo desde el DVD de Windows Server Essentials.  
  
     Si faltan los controladores del controlador de almacenamiento, se muestra el cuadro de diálogo de Error de instalación de Windows Server Essentials.  
  
5.  En el cuadro de diálogo de Error de instalación de Windows Server Essentials, haga clic en **Sí** para cargar los controladores de almacenamiento adicional.  
  
6.  En el símbolo del sistema **Seleccione el archivo inf de su controlador** , vaya al archivo .inf de la carpeta CONTROLADORES en el disquete o la unidad flash USB, seleccione el archivo, haga clic con el botón derecho en el nombre del archivo y, a continuación, haga clic en **Abrir**. De este modo, se carga el controlador.  
  
    > [!NOTE]
    >  Antes de intentar cargar el archivo, compruebe que la extensión del nombre de archivo (.inf) está en minúsculas. Esta operación distingue mayúsculas de minúsculas, y un archivo de controlador no se cargará si la extensión del nombre de archivo tiene letras mayúsculas.  
  
7.  En el símbolo del sistema, haga clic en **Sí** para que el controlador de almacenamiento esté disponible durante la fase de modo de texto del programa de instalación.  
  
 La instalación debería proseguir con normalidad.  
  
###  <a name="BKMK_AddingNICdrivers"></a> Agregar controladores de adaptadores de red  
 Si un adaptador de red en el equipo no es compatible con Windows Server Essentials, el servidor no tendrá conectividad de red una vez completada la instalación, y no podrá conectar equipos al servidor.  
  
 Al final de la instalación de Windows Server Essentials, se le informa si un controlador de adaptador de red no se instaló automáticamente. También puede usar **Conexiones de red** en el Panel de control para comprobar si falta un controlador de adaptador de red. Si no ve una conexión de red asociada al adaptador de red en el servidor, deberá instalar un controlador.  
  
 Si en el equipo falta un controlador compatible para cualquier adaptador de red, deberá instalar manualmente el controlador de adaptador de red apropiado y, a continuación, reiniciar el servidor. Use el siguiente procedimiento.  
  
##### <a name="to-install-a-network-adapter-driver"></a>Para instalar un controlador de adaptador de red  
  
1.  Recurra al fabricante del adaptador de red para obtener el controlador que falta.  
  
2.  Siga las instrucciones de instalación del fabricante para instalar el controlador.  
  
3.  Reinicie el equipo.  
  
    > [!IMPORTANT]
    >  Si no reinicia el servidor después de instalar al controlador de adaptador de red que faltan, se puede producir un error en la instalación del software del conector de Windows Server Essentials en los equipos cliente.
