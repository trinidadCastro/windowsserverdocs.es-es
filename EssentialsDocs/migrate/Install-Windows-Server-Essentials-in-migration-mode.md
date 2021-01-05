---
title: Instalación de Windows Server Essentials en modo de migración
description: Obtenga información acerca de cómo instalar Windows Server Essentials en modo de migración.
ms.date: 04/29/2020
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.custom:
- CI ID 117135
- CSSTroubleshoot
ms.openlocfilehash: 288a3a3c6680da8f595cb55d77637b6d64678667
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810952"
---
# <a name="install-windows-server-essentials-in-migration-mode"></a>Instalación de Windows Server Essentials en modo de migración

> Se aplica a: Windows Server 2012 Essentials

Solo puede tener un servidor en la red que ejecute Windows Server Essentials y ese servidor debe ser un controlador de dominio para la red.

 Al instalar Windows Server Essentials en modo de migración, el Asistente para la instalación realiza las siguientes tareas:

1.  Instala y configura el software de servidor de Windows Server Essentials en el servidor de destino.

2.  Actualiza el esquema de dominio a la versión más reciente.

3.  Une el servidor de destino al dominio existente. Tanto el servidor de origen como el servidor de destino pueden ser miembros del mismo dominio hasta que finalice el proceso de migración. Cuando haya finalizado la migración, debe quitar el servidor de origen de la red en un período de 21 días.

    > [!WARNING]
    >  Se agrega un mensaje de error al registro de eventos cada día durante el período de gracia de 21 días hasta que quite el servidor de origen de la red. El texto del mensaje dice: "La comprobación del rol FSMO detectó un estado en el entorno no compatible con la directiva de licencias. El Servidor de administración debe asumir los roles de Active Directory de controlador del dominio principal y de maestro de nomenclatura de dominios. Por lo tanto, debe mover las funciones de Active Directory al servidor de administración ahora. Este servidor se apagará automáticamente si no se corrige el problema en 21 días desde el momento en que se detectó el estado." Tras el período de gracia 21 días, el servidor de origen se apagará.

4.  Transfiera el maestro de operaciones, también conocidas como funciones FSMO (Flexible Single Master Operations) del servidor de origen al servidor de destino. Las funciones de maestro de operaciones son tareas de controlador de dominio especializadas que se usan cuando los métodos de actualización y transferencia de datos estándar no son adecuados. Cuando el servidor de destino se convierte en un controlador de dominio, debe conservar las funciones de maestro de operaciones.

5.  Configura el servidor de destino como servidor de catálogo global. El servidor de catálogo global es un controlador de dominio que administra un repositorio de datos distribuido. Contiene una representación parcial que permite búsquedas de cada objeto en todos los dominios del bosque de Active Directory.

6.  Configura el servidor de destino como el servidor de licencias de sitios.

##  <a name="install-windows-server-essentials-on-the-destination-server"></a><a name="BKMK_Install"></a> Instalar Windows Server Essentials en el servidor de destino
 Para instalar y configurar Windows Server Essentials en el servidor de destino en modo de migración, realice el procedimiento siguiente.

#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>Para instalar Windows Server Essentials en el servidor de destino

1. Active el servidor de destino e inserte Windows Server Essentials DVD1 en la unidad de DVD. Si ve un mensaje que le pregunta si quiere arrancar desde un CD o DVD, presione cualquier tecla para hacerlo.

   > [!NOTE]
   >  Si el servidor de destino admite el arranque desde una unidad flash USB, puede usar la **herramienta de descarga USB/DVD de Windows 7** para crear una unidad flash USB de arranque desde el archivo ISO de Windows Server Essentials. El uso de una unidad flash USB puede acelerar considerablemente el proceso de instalación porque las unidades flash leen datos mucho más rápidos que las unidades de DVD-ROM. Después de crear una unidad flash USB de arranque, puede agregar un archivo de respuesta a la unidad flash. Puede [descargar la herramienta de descarga USB/DVD de Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=248282) gratis en Microsoft Store sitio Web.

   > [!NOTE]
   >  Si el servidor de destino no se arranca desde el DVD, reinicie el equipo y compruebe la configuración del BIOS para asegurarse de que **DVD-ROM** aparece en primer lugar en la secuencia de arranque. Para obtener más información acerca de cómo cambiar la secuencia de arranque de la configuración del BIOS, vea la documentación del fabricante del hardware.

2. Haga clic en **Nueva instalación**.

3. Si tiene un disco duro interno que no aparece en la lista, haga clic en **Cargar controladores** e instale los controladores necesarios antes de continuar.

4. Seleccione la casilla que verifica que se eliminarán todos los archivos y carpetas del disco duro principal y, a continuación, haga clic en **Instalar**.

5. En la página **Elegir modo de instalación del servidor**, haga clic en **Migración del servidor** y, a continuación, introduzca la información de migración necesaria.

6. Cuando aparezca el mensaje **Se ha migrado correctamente el servidor**, haga clic en **Cerrar**.

   Una vez finalizada la instalación, iniciará automáticamente la sesión con la cuenta de usuario administrador y la contraseña que proporcionó en el archivo de respuesta de la migración.

> [!NOTE]
>  Para desbloquear el escritorio mientras se instala Windows Server Essentials, use la cuenta predefinida Administrador y deje la contraseña en blanco.

##  <a name="verify-the-health-of-the-domain-controller"></a><a name="BKMK_VerifyTheHealthOfDC"></a> Comprobar el estado del controlador de dominio
 Antes de continuar con la migración, debe asegurarse de que el controlador de dominio y la red de Windows Server Essentials son correctos.

 La siguiente tabla enumera las herramientas que puede usar para diagnosticar problemas en el servidor de destino, en la red y en el dominio:

|Herramienta|Descripción|
|----------|-----------------|
|Netdiag|Ayuda a aislar problemas de red y conectividad. Para obtener más información y para descargar, consulte [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388).|
|Dcdiag.exe|Analiza el estado de los controladores de dominio en un bosque o empresa, y notifica los errores para ayudarle a solucionarlos. Para obtener más información y para descargar, consulte [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389).|
|Repadmin.exe|Ayuda a diagnosticar problemas de replicación entre controladores de dominio. Para ejecutarse, esta herramienta requiere parámetros de línea de comandos. Para obtener más información y para descargar, consulte [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387).|

 Debe corregir todos los problemas notificados por estas herramientas antes de continuar con la migración.

> [!NOTE]
>  Si piensa migrar el correo electrónico a otro servidor local de Exchange Server, consulte [integrar un servidor local de Exchange Server con Windows Server Essentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) para obtener información acerca de cómo configurar el servidor local de Exchange Server.
