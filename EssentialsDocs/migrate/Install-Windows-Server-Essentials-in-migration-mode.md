---
title: "Instalar Windows Server Essentials en el modo de migración 1"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 808a4b1e120fa559d603b34ad006b18de6b94378
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>Instalar Windows Server Essentials en el modo de migración 1

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede tener solo un servidor de la red que ejecute Windows Server Essentials y el servidor debe ser un controlador de dominio de la red.  
  
 Cuando se instalación Windows Server Essentials en modo de migración, el Asistente para la instalación realiza las siguientes tareas:  
  
1.  Instala y configura el software de servidor de Windows Server Essentials en el servidor de destino.  
  
2.  Actualiza el esquema de dominio a la versión más reciente.  
  
3.  El servidor de destino se une al dominio existente. El servidor de origen y el servidor de destino pueden estar miembros del mismo dominio hasta que finalice el proceso de migración. Después de finalizar la migración, debes quitar el servidor de origen de la red en 21 días.  
  
    > [!WARNING]
    >  Un mensaje de error se agrega al registro de eventos cada día durante el período de gracia de 21 días hasta que se elimina el servidor de origen de la red. El texto de las lecturas de mensaje, "la comprobación de la función FSMO detecta una condición en tu entorno que está fuera del cumplimiento de la directiva de licencia. El servidor de administración debe mantener el controlador de dominio principal y los roles de maestro de Active Directory de nombres de dominio. Mueva los roles de Active Directory al servidor de administración ahora. Este servidor se automáticamente apagará si no se ha corregido el problema en 21 días desde el momento en que esta condición se detectó por primera vez." Después del período de gracia 21 días, se cerrará el servidor de origen.  
  
4.  Transfiere los roles de maestro (también denominado operaciones de maestro único flexibles o FSMO) de las operaciones del servidor de origen al servidor de destino. Roles de maestro de operaciones son tareas especializado de controlador de dominio, que se usan cuando los métodos de transferencia de datos y actualización estándar no son suficientes. Cuando el servidor de destino se convierte en un controlador de dominio, deben contener las operaciones de roles de maestro.  
  
5.  Configura el servidor de destino como un servidor de catálogo global. El servidor de catálogo global es un controlador de dominio que administra un repositorio de datos distribuidos. Contiene una representación de búsqueda, parcial de todos los objetos en cada dominio en el bosque de Active Directory.  
  
6.  Configura el servidor de destino como el servidor de licencias de sitios.  
  
##  <a name="BKMK_Install"></a>Instalar Windows Server Essentials en el servidor de destino  
 Para instalar y configurar Windows Server Essentials en el servidor de destino en modo de migración, realiza el siguiente procedimiento.  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>Para instalar Windows Server Essentials en el servidor de destino  
  
1.  Activa el servidor de destino e inserte la unidad de DVD en Windows Server Essentials DVD1. Si ves un mensaje que pregunta si quieres arrancar desde un CD o DVD, presiona cualquier tecla para hacerlo.  
  
    > [!NOTE]
    >  Si el servidor de destino admite el arranque desde una unidad flash USB, puedes usar la **herramienta de descarga de USB o DVD de Windows 7** para crear una unidad Flash USB de arranque desde el archivo ISO de Windows Server Essentials. Usar una unidad flash USB significativamente acelerar el proceso de instalación ya unidades flash leer los datos mucho más rápidos de las unidades de DVD-ROM. Después de crear una unidad flash USB de arranque, puedes agregar un archivo de respuesta en la unidad flash. Puedes [descargar la herramienta de descarga de USB o DVD de Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=248282) gratuito en el sitio Web de Microsoft Store.  
  
    > [!NOTE]
    >  Si el servidor de destino no arranca desde el DVD, reinicie el equipo y comprobar la configuración del BIOS para garantizar que **DVD-ROM** aparece en primer lugar en la secuencia de arranque. Para obtener más información sobre cómo cambiar la secuencia de arranque de la configuración del BIOS, consulta la documentación del fabricante de tu hardware.  
  
2.  Haz clic en **nueva instalación**.  
  
3.  Si tienes un disco duro interno que no se muestra en la lista, haz clic en **cargar controladores** e instalar el controlador necesario antes de continuar.  
  
4.  Selecciona la casilla de verificación que comprueba todos los archivos y carpetas en el disco duro principal se eliminarán y, a continuación, haz clic en **instalar**.  
  
5.  En la **elegir el modo de instalación de servidor** página, haz clic en **migración de servidor**y, a continuación, proporcionar la información necesaria de migración.  
  
6.  Cuando la **el servidor se ha migrado correctamente** aparece el mensaje, haz clic en **cerrar**.  
  
 Una vez finalizada la instalación, automáticamente sesión con la cuenta de usuario de administrador y la contraseña que proporcionaste en el archivo de respuesta de la migración.  
  
> [!NOTE]
>  Para desbloquear el escritorio mientras se instala Windows Server Essentials, usa la cuenta predefinida de administrador y deja la contraseña en blanco.  
  
##  <a name="BKMK_VerifyTheHealthOfDC"></a>Comprobar el estado del controlador de dominio  
 Antes de continuar con la migración, debes asegurarte de que el controlador de dominio y la red de Windows Server Essentials están en buenas condiciones.  
  
 La siguiente tabla enumera las herramientas que puedes usar para diagnosticar problemas en el servidor de destino y la red y, en el dominio:  
  
|Herramienta|Descripción|  
|----------|-----------------|  
|Netdiag|Ayuda a aislar los problemas de conectividad y redes. Para obtener más información y para descargar, consulta [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388).|  
|Dcdiag.exe|Analiza el estado de controladores de dominio de un bosque o empresa y la notifica problemas para ayudarle a solucionar problemas. Para obtener más información y para descargar, consulta [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389).|  
|Repadmin.exe|Le ayuda a diagnosticar problemas de replicación entre controladores de dominio. Esta herramienta requiere parámetros de línea de comandos para ejecutar. Para obtener más información y para descargar, consulta [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387).|  
  
 Se deben corregir todos los problemas que estos informe herramientas antes de continuar con la migración.  
  
> [!NOTE]
>  Si vas a migrar el correo electrónico a otro servidor de Exchange local, consulta [integrar un servidor de Exchange local con Windows Server Essentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) para obtener información acerca de cómo configurar el servidor de Exchange local.
