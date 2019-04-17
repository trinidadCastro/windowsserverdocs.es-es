---
title: Administrar el acceso Web remoto en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: de01b2fd2395377b6e7b3349b9862eb0e51a59b8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>Administrar el acceso Web remoto en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Acceso Web remoto en Windows Server Essentials o en Windows Server 2012 R2 con el rol de experiencia en Windows Server Essentials instalado, proporciona una experiencia de explorador optimizado para pantallas táctiles para acceder a las aplicaciones y datos desde prácticamente cualquier lugar que tienes una conexión a Internet y mediante el uso de casi cualquier dispositivo. Para usar la funcionalidad de acceso Web remoto, debes activar primero mediante la desde cualquier lugar acceso Asistente para configurar el y, a continuación, configurar el enrutador y el nombre de dominio.  
  
## <a name="in-this-topic"></a>En este tema  
  
-   [Activar y configurar el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurar el enrutador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurar el nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Personalizar el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Solucionar problemas de acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="BKMK_1"></a>Activar y configurar el acceso Web remoto  
 Los temas siguientes te ayudará a activar y configurar el acceso Web remoto:  
  
-   [Introducción al acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Activar el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [Cambiar la región](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [Administrar los permisos de acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [Proteger el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [Administrar los usuarios acceso Web remoto y VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="BKMK_Overview"></a>Introducción al acceso Web remoto  
 Cuando estás lejos de la oficina, puedes abre un explorador web y obtener acceso a acceso Web remoto desde cualquier lugar que tenga acceso a Internet. En el acceso Web remoto, puedes:  
  
-   El acceso compartir archivos y carpetas en el servidor.  
  
-   Obtener acceso al servidor y equipos de la red. Esto significa que acceder al escritorio de un equipo de la red como si estuviera sentado delante de la oficina.  
  
  Acceso Web remoto no está activado de manera predeterminada. Cuando se ejecuta, configurar el Asistente de acceso desde cualquier lugar, intenta el asistente Configurar el enrutador y conectividad a Internet. Después de acceso Web remoto está activado, puedes configurar un nombre de dominio de tu servidor y personalizar el acceso Web remoto. También pueden configurar el enrutador nuevo si cambias el enrutador.  
  
 Permiso para acceder a acceso Web remoto no se concede automáticamente al agregar una nueva cuenta de usuario. Cuando agregas una cuenta de usuario, puedes permitir el acceso a carpetas compartidas, la biblioteca multimedia, equipos, vínculos de la página principal y el servidor del panel. También puedes especificar que un usuario no se pueden usar acceso Web remoto.  
  
 Se muestra la configuración de acceso Web remoto para cada cuenta de usuario en el **usuarios** ficha del escritorio de Windows Server Essentials. Para cambiar la configuración de acceso Web remoto, haz clic en la cuenta de usuario y, a continuación, haz clic en **ver las propiedades de la cuenta**.  
  
###  <a name="BKMK_TurnOnRWA"></a>Activar el acceso Web remoto  
 Para activar acceso Web remoto mediante la ejecución de configurar el Asistente de acceso desde cualquier lugar del panel del servidor.  
  
##### <a name="to-turn-on-remote-web-access"></a>Para activar el acceso Web remoto  
  
1.  Abra el panel.  
  
2.  Haz clic en **configuración**y, a continuación, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
3.  Haz clic en **configurar**. Aparece el desde cualquier lugar acceso asistente Configurar.  
  
4.  En la **características elige acceso desde cualquier lugar para habilitar** página, seleccione la **acceso Web remoto** casilla de verificación.  
  
5.  Sigue las instrucciones para completar al asistente.  
  
###  <a name="BKMK_Region"></a>Cambiar la región  
 Debe ser un administrador de red para cambiar la configuración de región en Windows Server Essentials.  
  
##### <a name="to-change-the-region-setting"></a>Para cambiar la configuración de región  
  
1.  En un equipo que está conectado a Windows Server Essentials, abra el panel.  
  
2.  Haz clic en **configuración**.  
  
3.  En la **General** pestaña, haz clic en la lista desplegable en la **ubicación del país o región del servidor** sección.  
  
4.  En la lista desplegable, selecciona la nueva región y, a continuación, haz clic en **aplicar** para aceptar la nueva configuración de región.  
  
###  <a name="BKMK_ManagePerms"></a>Administrar los permisos de acceso Web remoto  
 Cuando agregas una cuenta de usuario en Windows Server Essentials, se permite el nuevo usuario de manera predeterminada para usar el acceso Web remoto. Si has elegido no permitir el acceso Web remoto para una cuenta de usuario y, a continuación, busca que el usuario necesita usar el acceso Web remoto, puede actualizar las propiedades de s de cuenta de usuario.  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>Para administrar los permisos de acceso Web remoto de una cuenta de usuario  
  
1.  Iniciar sesión en el panel y, a continuación, haz clic en **usuarios**.  
  
2.  Haz clic en la cuenta de usuario que quieras administrar y, a continuación, haz clic en **ver las propiedades de la cuenta** en la **tareas** panel.  
  
3.  En la **propiedades** cuadro de diálogo, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
4.  En la **acceso desde cualquier lugar**, selecciona el **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web** casilla para permitir que un usuario que se conecte al servidor mediante el acceso Web remoto.  
  
5.  Haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
 Para obtener más información, consulta [administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecureRWA"></a>Proteger el acceso Web remoto  
 Windows Server Essentials usa un certificado de seguridad para ayudar a proteger la información que se intercambia entre el software y un explorador web. Cuando instalas el software del conector en los equipos, el certificado de seguridad para Windows Server Essentials se agrega a la lista de certificados de confianza en los equipos. La mejor manera para que los usuarios tienen acceso a acceso Web remoto cuando se encuentran fuera de la oficina es usar un equipo portátil que tenga instalado el software del conector.  
  
> [!WARNING]
>  Los usuarios que usan el acceso Web remoto desde ubicaciones públicas o a otros equipos de confianza deben asegurarse de que inician sesión desactivar el sitio Web antes de dejar el equipo desatendido o cuando hayan terminado con la sesión.  
  
###  <a name="BKMK_ManageRWAVPN"></a>Administrar los usuarios acceso Web remoto y VPN  
 Puedes usar VPN para conectarte a Windows Server Essentials y obtener acceso a todos los recursos que se almacenan en el servidor. Esto es especialmente útil si tienes un equipo cliente que está configurado con cuentas de red que pueden usarse para conectarse a un servidor de Windows Server Essentials hospedado a través de una conexión VPN. Todas las cuentas de usuario recién creado en el servidor de Windows Server Essentials hospedado deben usar VPN para iniciar sesión en el equipo cliente por primera vez.  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>Para establecer permisos de acceso Web remoto y VPN para los usuarios de red  
  
1.  Abra el panel.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieras para conceder permisos para acceder al escritorio de forma remota.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **propiedades**.  
  
5.  En **< usuario Account\ > propiedades**, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
6.  En la **acceso desde cualquier lugar** pestaña, haz lo siguiente:  
  
    1.  Para permitir que un usuario que se conecte al servidor mediante VPN, selecciona el **permitir red privada Virtual (VPN)** casilla de verificación.  
  
    2.  Para permitir que un usuario que se conecte al servidor mediante el uso de acceso Web remoto, selecciona el **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web** casilla de verificación.  
  
7.  Haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
##  <a name="BKMK_2"></a>Configurar el enrutador  
 Al configurar el servidor de acceso Web remoto, el desde cualquier lugar acceso Asistente para configurar el intenta configurar el enrutador. Si cambias de enrutadores o cambiar la configuración del enrutador, debes volver a ejecutar al su enrutador asistente Configurar. Para obtener más información, consulta los siguientes temas:  
  
-   [Configurar el enrutador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [Reemplazar un enrutador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [Ubicación de red definida](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [Habilitar los controles ActiveX de servicios de escritorio remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="BKMK_SetUpRouter"></a>Configurar el enrutador  
 Durante este paso, Windows Server Essentials intenta configurar automáticamente el enrutador mediante comandos de UPnP. Para ello, debe ser compatible con el enrutador UPnP estándares y la configuración de UPnP debe estar habilitada en el enrutador.  
  
> [!NOTE]
>  La configuración de red debe seguir los requisitos de red compatibles para Windows Server Essentials. Debería haber solo un enrutador de la red.  
  
 Si el enrutador no se configura mediante la tu dominio nombre Asistente para configurar el, debe reenviar manualmente el puerto 443. Para obtener información acerca de cómo configurar el reenvío de puerto del enrutador, vea [configuración de enrutador](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx).  
  
###  <a name="BKMK_ReplaceRouter"></a>Reemplazar un enrutador  
 Reemplaza el enrutador de acuerdo con las instrucciones del fabricante s y, a continuación, ejecuta el su enrutador Asistente para configurar el para configurar el enrutador nuevo.  
  
##### <a name="to-set-up-your-new-router"></a>Para configurar el nuevo enrutador  
  
1.  En el panel de Windows Server Essentials, haz clic en **configuración**.  
  
2.  Haz clic en el **acceso desde cualquier lugar** pestaña y, a continuación, en la **enrutador** sección, haz clic en **configurar**. Inicia el su enrutador asistente Configurar.  
  
3.  Sigue las instrucciones del Asistente para terminar de configurar el nuevo enrutador.  
  
###  <a name="BKMK_NetworkLocation"></a>Ubicación de red definida  
 Una ubicación de red es una colección de opciones de configuración de red que Windows se aplica cuando te conectas a una red. La configuración varía y se puede personalizar en función del tipo de red que usas. La configuración de una ubicación de red determina si determinadas características (por ejemplo, compartir archivos e impresoras, detección de redes y compartir la carpeta pública) están activados o desactivado. Ubicaciones de red son útiles cuando se necesita para conectarse a redes diferentes.  
  
 Por ejemplo, es posible que disponga un equipo portátil que usas en casa y en el trabajo. Cuando se encuentra en la oficina, se conecta a la red. Sin embargo, cuando vuelvas casa, usas el equipo portátil para acceder a y reproducir vídeos y música que se almacena en el servidor principal. Cuando se conecta a una nueva red y especifica el tipo de ubicación, Windows asigna un perfil de red preestablecido para ese tipo de ubicación. La próxima vez que se conecte a esa red, Windows detecta la red y asigna automáticamente la configuración correcta. Esto agrega un nivel de seguridad para ayudar a proteger la información en el equipo y solo las características de red que necesitas para esa ubicación están activadas.  
  
 Existen cuatro tipos de ubicaciones de red:  
  
-   **Red doméstica** elegir esta red para redes domésticas o cuando conozca y confíe en los usuarios y dispositivos en la red. Equipos en una red doméstica pueden pertenecer a un grupo en el hogar. Detección de redes está activada para redes domésticas, que te permite ver otros equipos y dispositivos en la red y que otros usuarios de red ver su equipo.  
  
-   **Red de trabajo** elegir esta red de oficina pequeña u otras redes de área de trabajo. Detección de redes, que te permite ver otros equipos y dispositivos en una red y permite a otros usuarios de red ver el equipo, está activada de manera predeterminada, pero no puede crear o unirse a un grupo en el hogar.  
  
-   **Las redes públicas** elegir esta red de lugares públicos (como cafeterías o aeropuertos). Esta ubicación está diseñada para evitar que el equipo sea visible para otros equipos y para ayudar a proteger el equipo de software malintencionado de Internet. Grupo hogar no está disponible en redes públicas y detección de redes está desactivada. También debe elegir esta opción si estás conectado directamente a Internet sin usar un enrutador, o si tienes una conexión de banda ancha móvil.  
  
-   **Dominio** elegir esta red de dominios, como las de las empresas. Este tipo de ubicación de red se controla mediante el Administrador de red, y no se puede activar o cambiado.  
  
###  <a name="BKMK_ActiveX"></a>Habilitar los controles ActiveX de servicios de escritorio remoto  
 Los controles ActiveX de servicios de escritorio remoto le permite acceder a su equipo doméstica o de empresa, a través de Internet, desde otro equipo mediante el uso de acceso Web remoto.  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>Para habilitar los controles ActiveX de servicios de escritorio remoto  
  
1.  En Internet Explorer, haga clic en **herramientas**y, a continuación, haz clic en **opciones de Internet**.  
  
2.  En la **seguridad**, haga clic **nivel personalizado**.  
  
3.  En la **ActiveX controles y complementos** sección, haz lo siguiente:  
  
    1.  En **descargar controles ActiveX firmados**, haz clic en **pedir**.  
  
    2.  En **ejecutar controles ActiveX y complementos**, haz clic en **habilitar**.  
  
4.  Haz clic en **Aceptar** dos veces para aceptar los cambios y cerrar el cuadro de diálogo.  
  
##  <a name="BKMK_3"></a>Configurar el nombre de dominio  
 Después de acceso Web remoto está activado, puedes configurar un nombre de dominio para que el servidor que ejecuta Windows Server Essentials. Esto es un paso necesario si vas a usar el acceso Web remoto desde un equipo remoto. Para obtener más información, consulta los siguientes temas:  
  
-   [Introducción a los nombres de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [Comprender los nombres de dominio personalizado de Microsoft](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [Usar un nombre de dominio nueva o existente](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [Configurar un nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [Elegir un proveedor de servicio de nombres de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [Elige un nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [Elegir un prefijo de nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [Elige una extensión de nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [Actualizar o ampliar el servicio de nombres de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [Exportar o importar el certificado en el servidor](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [Configurar manualmente un nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [Encuentra tu proveedor de servicio de nombres de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="BKMK_DNOverview"></a>Introducción a los nombres de dominio  
 Un nombre de dominio identifica el servidor de Internet. Nombres de dominio constan de al menos dos partes: un nombre de dominio de nivel superior (TLD) y un segundo nombre de dominio de nivel. Por ejemplo, en contoso.com, com es el TLD y contoso es el segundo nombre de dominio de nivel.  
  
 Mientras estás fuera de la oficina, puedes usar el nombre de dominio para acceder a archivos compartidos en el servidor o equipos de la red. También puedes administrar el servidor cuando estás fuera. Por ejemplo, registrarían contoso.com de tu servidor. Cuando estás lejos de la oficina, puedes abrir un explorador web en su equipo portátil y escriba **contoso.com** en el cuadro de texto de dirección para conectarse a la instancia de acceso Web remoto que configuraste en Windows Server Essentials.  
  
###  <a name="BKMK_PersonalizedNames"></a>Comprender los nombres de dominio personalizado de Microsoft  
 Un nombre de dominio personalizado de Microsoft incluye las siguientes características:  
  
-   Un nombre de dominio personalizado para el acceso Web remoto (por ejemplo, *yourhostname*. remotewebaccess.com). El nombre del dominio está asociado con su dirección IP pública.  
  
-   Una dinámica DNS actualiza el servicio de protocolo para que no se interrumpirá acceso Web remoto con el nombre de dominio si cambia su dirección IP pública. Por lo general, los proveedores de servicios de Internet (ISP) para las conexiones de banda ancha de organización s proporcionar direcciones IP públicas dinámicas que pueden cambiar.  
  
-   Un certificado de confianza asociado con el nombre de dominio.  
  
 Para integrar un nombre de dominio personalizado de Microsoft con el servidor, necesitas una cuenta de Microsoft (anteriormente denominada Windows Live ID). Si no tienes una cuenta de Microsoft, puedes registrarte para uno en el [Microsoft Hotmail](https://login.live.com/) sitio Web.  
  
> [!IMPORTANT]
>  Windows Live, se permite caracteres especiales en la contraseña de tu cuenta de Microsoft que no es compatible con el servidor. Si usas un dominio personalizado de Microsoft, asegúrate de que la contraseña de tu cuenta de Microsoft contenga solo los caracteres que el servidor la admite. El servidor no admite el uso de caracteres $, /, ' y %.  
  
###  <a name="BKMK_UseNewName"></a>Usar un nombre de dominio nueva o existente  
 Para configurar automáticamente el nombre de dominio en un servidor que ejecuta Windows Server Essentials, debes usar un proveedor de servicios de nombre de dominio que aparece en la establece su nombre Asistente para el dominio. Puedes obtener un nuevo nombre de dominio o usar un nombre de dominio existente. Realiza una de las siguientes acciones:  
  
-   Si quieres obtener un nuevo nombre de dominio de uno del dominio de proveedores de servicios de nombre que se enumeran en el asistente, haz clic en **desea configurar un nuevo nombre de dominio**.  
  
-   Si tienes un nombre de dominio existente que haya adquirido de uno de los proveedores de servicio de nombre de dominio compatibles, puedes usar a la tu dominio nombre Asistente para configurar el para configurar el nombre de dominio de tu servidor. Haz clic en **desea usar un nombre de dominio ya compradas**y, a continuación, escribe el nombre de dominio en el **configurar su nombre de dominio** cuadro de texto. Debes proporcionar el nombre de usuario y contraseña que usaste para el nombre de dominio de la compra.  
  
-   Si tienes un nombre de dominio existente que haya adquirido de un proveedor de servicio de nombres de dominio que no es compatible con Windows Server Essentials y quieres usar a la tu dominio nombre Asistente para configurar el para configurar el nombre de dominio de tu servidor, puedes transferir el nombre de dominio a uno de los proveedores de servicios de nombre de dominio aparece en el asistente. Haz clic en **desea usar un nombre de dominio ya compradas**, escribe el nombre de dominio en el **nombre de dominio** texto cuadro y, a continuación, sigue las instrucciones en el sitio Web proveedor s de dominio nombre servicio para transferir el nombre de dominio.  
  
###  <a name="BKMK_SetUpName"></a>Configurar un nombre de dominio  
 Cuando activas acceso Web remoto, puedes configurar el nombre de dominio de Internet del servidor.  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>Para configurar o administrar un nombre de dominio de Internet  
  
1.  Abra el panel.  
  
2.  Haz clic en **configuración del servidor**y, a continuación, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
3.  En la **nombre de dominio** sección, haz clic en **configurar**.  
  
4.  Sigue las instrucciones para completar al asistente. Si aún no tiene un nombre de dominio y un certificado, el asistente ayuda a encontrar un proveedor de nombre de dominio para comprar un nombre de dominio y un certificado o puedes conseguir un nombre de dominio personalizado de Microsoft.  
  
###  <a name="BKMK_ChooseProvider"></a>Elegir un proveedor de servicio de nombres de dominio  
 Debes elegir un proveedor de servicio de nombres de dominio que admite la extensión de nombre de dominio que quieras usar. El tu dominio nombre asistente Configurar incluye una lista de proveedores completos que se pueden usar con un vínculo a cada sitio Web s de proveedor. Haz clic en el **información más** vínculo junto a cada nombre de proveedor de s para obtener información sobre los servicios y precios proporcionadas por el proveedor.  
  
> [!NOTE]
>  Algunos proveedores de servicios de nombre de dominio servir amplia regiones del mundo y otras sirven mercados más pequeños. Por este motivo, es podrán que algunos proveedores no ofrezca un sitio Web que se traduce en el idioma de preferencia.  
  
 Al adquirir el nombre de dominio, también puede adquirir el servicio de protocolo de actualización dinámica de sistema de nombres de dominio (DNS) de tu proveedor de servicio de nombres de dominio. Protocolo de actualización dinámica de DNS es un servicio que cualquiera en Internet permite obtener acceso a los recursos en una red local cuando cambia constantemente la dirección IP de esa red. O bien, puedes comprar una dirección IP estática de tu proveedor de servicios de Internet (ISP) para garantizar que no cambie su dirección IP.  
  
###  <a name="BKMK_ChooseDomainName"></a>Elige un nombre de dominio  
 Elige un nombre que identifica unívocamente tu servidor de empresa. Por ejemplo, si el nombre de tu empresa es Contoso Ltd., podrías elegir Contoso para identificar tu servidor doméstica o de empresa de Internet. Si el nombre de dominio no está disponible, prueba otra variación de ese nombre, o quizás algo completamente diferente.  
  
 El nombre que escriba puede incluir lo siguiente:  
  
-   63 caracteres como máximo  
  
-   Letras (inglés o sus caracteres localizadas), números o guiones (-). El nombre debe empezar y terminar con una letra o un número.  
  
    > [!NOTE]
    >  Nombres de dominio no distinguen mayúsculas de minúsculas.  
  
###  <a name="BKMK_Prefixes"></a>Elegir un prefijo de nombre de dominio  
 Un nombre de dominio se compone de etiquetas jerárquicas.  
  
 **La extensión de dominio de nivel superior** es la etiqueta del extremo derecho en el nombre de dominio. Por ejemplo, en www.contoso.com, com es la extensión de nombre de dominio de nivel superior.  
  
 **El nombre de dominio de segundo nivel** es la etiqueta junto a la extensión de nombre de dominio de nivel superior. El nombre de dominio de segundo nivel a menudo se crea basándose en el nombre de la compañía, productos o servicios. Por ejemplo, en www.contoso.com, contoso es el nombre de dominio de segundo nivel y se escogió para el nombre de la empresa Contoso farmacéuticas. El dominio de segundo nivel se denomina a veces, el nombre de host, que tiene una dirección IP asociada con él.  
  
 **El prefijo de nombre de dominio** identifica un subdominio. El nombre de un subdominio puede usarse para identificar los servicios, dispositivos o regiones. Por ejemplo, Contoso farmacéuticas quiere permitir a los usuarios remotos iniciar sesión en acceso Web remoto, pero no desea que el sitio Web a tu disposición del público, por lo que crean un subdominio que permita a sólo los usuarios con los permisos adecuados para acceder al sitio Web. Empresas farmacéuticas Contoso configura remote.contoso.com como el subdominio y remoto es el prefijo de nombre de dominio.  
  
> [!TIP]
>  Se recomienda que uses el valor predeterminado **remoto** como prefijo para el nombre de dominio.  
  
###  <a name="BKMK_Extension"></a>Elige una extensión de nombre de dominio  
 Cuando elijas un nombre de dominio de tu sitio Web de Internet, también debes especificar la extensión de nombre de dominio que quieras usar. La extensión se identifica por las letras que siguen el punto final de cualquier nombre de dominio. (El término formal para la extensión es el dominio de nivel superior o TLD).  
  
 Hay dos tipos principales de extensiones de dominio que puedes usar: código de país y genérica.  
  
#### <a name="generic-top-level-domains"></a>Dominios de nivel superior genéricos  
 Son las extensiones de dominio genérico tres o más letras de longitud, y normalmente se usan determinados tipos de las organizaciones.  
  
 **Ejemplos de dominios de nivel superior genéricas**  
  
|Extensión de dominio|Descripción|  
|----------------------|-----------------|  
|.com|Normalmente lo hacen las organizaciones comerciales, pero se puede usar cualquier persona.|  
|.NET|Diseñado para empresas que ofrecen servicios de infraestructura de red.|  
|.org|Usa originalmente sin ánimo de lucro agencias y otras empresas que no se encuentran a ninguna otra categoría genérico de dominio de nivel superior. Puede usarse por todo el mundo.|  
|.edu|Restringido para su uso por las organizaciones educativas.|  
  
#### <a name="country-code-top-level-domains"></a>Dominios de nivel superior de código de país  
 Estas extensiones de dominio tienen dos letras. Están diseñados para usarse en las organizaciones en el país o región que está asociada con ese código. Algunos dominios de nivel superior de código de país están restringidos para su uso por ciudadanos de ese país o región. Otras están disponibles para su uso por todo el mundo.  
  
 **Ejemplos de dominios de nivel superior del código de país**  
  
|Extensión de dominio|Descripción|  
|----------------------|-----------------|  
|. CA|Para su uso por los sitios Web en Canadá|  
|.CN|Para su uso por los sitios Web en China|  
|.de|Para su uso por los sitios Web en Alemania|  
|. co.uk|Para su uso por los sitios Web en el Reino Unido|  
  
 Para ver la lista completa de dominios de nivel superior, consulta el [sitio Web de Internet Assigned números Authority](https://go.microsoft.com/fwlink/?LinkId=117438).  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>Si una extensión de dominio no está disponible para seleccionar en el asistente Configurar nombre de dominio  
 Cuando ejecutas el asistente Configurar nombre de dominio, el asistente examina la información del sistema para determinar tu país o región. El asistente muestra a continuación, solo las extensiones de dominio que admiten los proveedores participantes en tu zona. Si la extensión del dominio que quieres no aparece en la lista, debes elegir una extensión de diferentes dominios para continuar. Selecciona una extensión de la lista que devuelve el asistente.  
  
###  <a name="BKMK_UpdateService"></a>Actualizar o ampliar el servicio de nombres de dominio  
 Necesitas actualizar o ampliar el servicio de nombres de dominio si adquirió un nombre de dominio, pero ha comprado un certificado. Debes tener un certificado para su nombre de dominio de tu proveedor de servicio de nombres de dominio.  
  
> [!NOTE]
>  Trabajar con tu proveedor de servicio de nombres de dominio para determinar el tipo de certificado que necesitas. El certificado puede ser uno de los certificados de bajo costosos que se ofrecen. Sin embargo, debes revisar la documentación y las características de certificados de seguridad de nivel superior para determinar si cumplen mejor tus necesidades empresariales.  
  
###  <a name="BKMK_ExportCert"></a>Exportar o importar el certificado en el servidor  
 Si quieres crear una copia de seguridad de un certificado o usarlo en otro servidor, debe exportar el certificado. Para obtener información sobre cómo exportar certificados, consulta [exportar un certificado](https://go.microsoft.com/fwlink/p/?LinkId=214362).  
  
###  <a name="BKMK_SetNameManually"></a>Configurar manualmente un nombre de dominio  
 Si elige esta opción, el servidor no supervisar o mantener tu nombre de dominio y no le avisan si hay un problema de configuración. También puedes considerar esta opción si se cumple alguna de las siguientes acciones:  
  
-   No hay proveedores de nombre de dominio de partners se enumeran de tu país o región.  
  
-   Los proveedores de dominio de partner enumerados son compatibles con la extensión de nombre de dominio.  
  
-   Tiene un nombre de dominio existente desde un proveedor de nombre de dominio que no es actualmente un asociado, y no va a transferir ese nombre de dominio a un proveedor de nombre de dominio compatibles de Windows Server Essentials.  
  
-   El asistente no incluye la extensión de nombre de dominio que quieras usar, pero la extensión está disponible desde un proveedor de nombre de dominio que no es actualmente un asociado.  
  
 Si eliges que configurar manualmente el nombre de dominio, funcionan con tu proveedor de servicio de nombres de dominio para crear un registro de tu dominio.  
  
##### <a name="to-create-an-a-record"></a>Para crear un registro  
  
1.  Decide en un nombre de host, como el control remoto. Este es el prefijo de nombre de dominio. El prefijo de nombre de dominio y el nombre de dominio vas a definir la dirección URL para abrir la página de inicio de sesión de acceso Web remoto; Por ejemplo, **http://remote.contoso.com**.  
  
2.  En su dominio nombre servicio proveedores configuración escritorio (normalmente en su página Web), crea el registro para el nombre de host que haya decidido en el paso 1. Asegúrate de que la dirección IP que especifiques en el registro es la dirección IP en el lado WAN del enrutador (mira al lado Internet). Consulta la documentación del enrutador para buscar la dirección IP WAN.  
  
3.  Se recomienda que ponerte en contacto con tu proveedor de servicios de Internet (ISP) para adquirir una dirección IP estática de la red. Esto garantiza que no cambia la dirección IP y que la entrada DNS no pierde obsoleta.  
  
     Si no tienes la opción para obtener una dirección IP estática del ISP, también puede adquirir el servicio de protocolo de actualización dinámica de sistema de nombres de dominio (DNS) de tu proveedor de servicio de nombres de dominio o de otro proveedor de servicios. Protocolo de actualización dinámica de DNS es un servicio que mantiene la dirección IP WAN para la red actualizado para que la dirección IP se puede resolver el nombre de dominio incluso si cambia la dirección IP.  
  
4.  Importar un certificado de confianza cuando el asistente te los pedirá. Si no tienes un certificado de confianza, puede obtener uno en uno de los proveedores de nombre de dominio compatibles que aparecen en el asistente o adquirir uno del proveedor de confianza de tu elección. Para obtener más información acerca de un certificado de confianza, ponte en contacto con tu proveedor de nombre de dominio.  
  
###  <a name="BKMK_Find"></a>Encuentra tu proveedor de servicio de nombres de dominio  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>Para encontrar el proveedor de servicio de nombres de dominio para el nombre de dominio  
  
1.  Abre un explorador web y, a continuación, escribe **www.internic.com** en la barra de direcciones para ir a la página principal Internic.  
  
2.  En la página principal Internic, haz clic en **Whois**.  
  
3.  En la **Whois**, escriba el nombre de dominio (por ejemplo, contoso.com).  
  
4.  Haz clic en el **dominio** opción y, a continuación, haz clic en **enviar**.  
  
5.  En los resultados de búsqueda, aparece bajo el nombre de tu proveedor de servicio de nombres de dominio **registrador**.  
  
##  <a name="BKMK_4"></a>Personalizar el acceso Web remoto  
 Puedes personalizar tu sitio de acceso Web remoto mediante la adición de una imagen de logotipo o en segundo plano personal. También puedes agregar vínculos en la página principal para que esta información está disponible para todos los usuarios. Para obtener más información, consulta los siguientes temas:  
  
-   [Personalizar el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [Personalizar las imágenes de fondos y los logotipos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [Reparar acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="BKMK_CustomizeRWA"></a>Personalizar el acceso Web remoto  
 Puedes personalizar acceso Web remoto, cambia el título del sitio Web, cambiar la imagen de fondo y el logotipo y agregar vínculos a otros sitios Web en la página principal.  
  
##### <a name="to-customize-remote-web-access"></a>Para personalizar el acceso Web remoto  
  
1.  Abra el panel.  
  
2.  Haz clic en **configuración**y, a continuación, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
3.  En la **configuración del sitio Web** sección, haz clic en **personalizar**.  
  
4.  Cuando termines de personalizar acceso Web remoto, haga clic en **Aceptar**. Probar los cambios en acceso Web remoto.  
  
###  <a name="BKMK_CustomizeImages"></a>Personalizar las imágenes de fondos y los logotipos  
 Esta sección proporciona información sobre las imágenes que puedes usar para personalizar el acceso Web remoto.  
  
#### <a name="image-size"></a>Tamaño de la imagen  
 **Imágenes de logotipo**  
  
 Se recomienda que uses imágenes de logotipo que son 32 x 32 píxeles. Imágenes más grandes se reducen a 32 x 32 e imágenes más pequeñas se estiran a 32 x 32, que puede distorsionar la imagen.  
  
 **Imágenes de fondo**  
  
 Si bien no hay ningún límite de tamaño para las imágenes de fondo, para obtener mejores resultados, se recomienda que uses imágenes que están aproximadamente 800 x 500 píxeles. La imagen de fondo se coloca en el centro (horizontal y vertical) de la página de inicio de sesión. Para ayudar a hacer que el texto en la página de inicio de sesión fáciles de leer, el centro de la imagen de fondo debe ser un color claro.  
  
#### <a name="image-file-types"></a>Tipos de archivo de imagen  
 Los siguientes tipos de archivo de imagen pueden usarse para reemplazar el logotipo en segundo plano y el sitio Web de forma predeterminada:  
  
-   Mapa de bits (*.bmp, \*.dib, \*.rle)  
  
-   GIF (*.gif)  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="BKMK_RepairRWA"></a>Reparar acceso Web remoto  
 El Asistente para reparación te ayuda a detectar y resolver problemas con el enrutador o el nombre de dominio. Existen dos formas para detectar problemas con acceso Web remoto:  
  
-   En la configuración del servidor en el panel, en la pestaña acceso desde cualquier lugar, se muestra un icono con una X roja junto con una descripción del problema.  
  
-   Un aviso en el Visor de alertas.  
  
> [!NOTE]
>  El Asistente para reparación no está disponible hasta que desactives en acceso Web remoto. Para obtener información acerca de cómo activar el acceso Web remoto, vea [Activar acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
##### <a name="to-repair-remote-web-access"></a>Para reparar el acceso Web remoto  
  
1.  Iniciar sesión en el panel.  
  
2.  Haz clic en **configuración**y, a continuación, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
3.  Haz clic en **reparación**. La **acceso Web remoto de reparación** inicia el Asistente para.  
  
4.  Haz clic en **siguiente**. El asistente analiza acceso Web remoto, identifica el problema y, a continuación, intenta reparar el problema.  
  
5.  Si recibes una alerta cuando finalice el asistente, haz clic en **Reintentar** para intentar reparar el problema de nuevo. Si sigues recibiendo una alerta, comprueba la alerta para obtener más información sobre el problema y pasos para solucionar problemas.  
  
##  <a name="BKMK_5"></a>Solucionar problemas de acceso Web remoto  
  
-   [Solucionar problemas de conectividad de acceso Web remoto](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [Solucionar problemas del firewall](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [Solucionar problemas de acceso, en cualquier lugar](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>Consulta también  
  
-   [Opciones de escritorio remotos](Remote-desktop-options.md)  
  
-   [Usar el acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar el acceso desde cualquier lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
