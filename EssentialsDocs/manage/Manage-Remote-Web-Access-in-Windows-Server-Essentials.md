---
title: Administrar Acceso web remoto en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3457eb28c05bd79f0de3a982da77ca01ffe9b52d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852768"
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>Administrar Acceso web remoto en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Acceso Web remoto en Windows Server Essentials, o en Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado, proporciona una experiencia de explorador optimizada y táctil para el acceso a aplicaciones y datos desde prácticamente cualquier lugar donde tenga una conexión a Internet y con casi cualquier dispositivo. Para usar la funcionalidad Acceso web remoto, primero debe activarla usando el asistente para la configuración de Acceso desde cualquier lugar y, después, configurar el enrutador y el nombre de dominio.  
  
## <a name="in-this-topic"></a>En este tema  
  
-   [Activar y configurar acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurar el enrutador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurar el nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Personalización del acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Solucionar problemas de acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="turn-on-and-configure-remote-web-access"></a><a name="BKMK_1"></a>Activar y configurar acceso Web remoto  
 Los siguientes temas le ayudarán a activar y configurar Acceso web remoto:  
  
-   [Introducción al acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Activar el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [Cambiar la región](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [Administrar permisos de acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [Acceso Web remoto seguro](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [Administrar acceso Web remoto y usuarios de VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="remote-web-access-overview"></a><a name="BKMK_Overview"></a>Introducción al acceso Web remoto  
 Cuando esté fuera de la oficina, puede abrir un explorador Web y acceder al acceso Web remoto desde cualquier lugar que tenga acceso a Internet. En acceso Web remoto, puede:  
  
- Acceder a carpetas y archivos compartidos en el servidor.  
  
- Acceder al servidor y a sus equipos en la red. Esto significa que puede acceder al escritorio de un equipo en red como si estuviera sentado frente a él en la oficina.  
  
  El acceso Web remoto no está activado de forma predeterminada. Al ejecutar el Asistente para configurar el Acceso desde cualquier lugar, este intenta configurar el enrutador y la conectividad a Internet. Una vez activado el acceso Web remoto, puede configurar un nombre de dominio para el servidor y personalizar el acceso Web remoto. Si cambia de enrutador, también puede volver a configurarlo.  
  
  El permiso para tener acceso al acceso Web remoto no se concede automáticamente cuando se agrega una nueva cuenta de usuario. Cuando se realiza esta operación, puede elegir si desea permitir el acceso a las carpetas compartidas, la biblioteca multimedia, los equipos, los vínculos de página principal y el panel del servidor. También puede especificar que un usuario no tenga permiso para usar el acceso Web remoto.  
  
  La configuración de acceso Web remoto se muestra para cada cuenta de usuario en la pestaña **usuarios** del panel de Windows Server Essentials. Para cambiar la configuración de acceso Web remoto, haga clic con el botón secundario en la cuenta de usuario y, a continuación, haga clic en **ver las propiedades de la cuenta**.  
  
###  <a name="turn-on-remote-web-access"></a><a name="BKMK_TurnOnRWA"></a>Activar el acceso Web remoto  
 Para activar Acceso web remoto, ejecute el asistente para configuración de Acceso desde cualquier lugar desde el panel del servidor.  
  
##### <a name="to-turn-on-remote-web-access"></a>Para activar Acceso web remoto  
  
1.  Abra el Panel.  
  
2.  Haga clic en **Configuración** y, a continuación, en la pestaña **Acceso desde cualquier lugar**.  
  
3.  Haga clic en **Configurar**. Se muestra el asistente para configuración de Acceso desde cualquier lugar.  
  
4.  En la página **Elegir las características de Acceso desde cualquier lugar que desea habilitar**, active la casilla **Acceso Web remoto**.  
  
5.  Siga las instrucciones para completar el asistente.  
  
###  <a name="change-your-region"></a><a name="BKMK_Region"></a>Cambiar la región  
 Para cambiar la configuración de región en Windows Server Essentials, debe ser un administrador de red.  
  
##### <a name="to-change-the-region-setting"></a>Para cambiar la configuración de región:  
  
1.  Abra el panel en un equipo conectado a Windows Server Essentials.  
  
2.  Haga clic en **Configuración**.  
  
3.  En la pestaña **General**, haga clic en la lista desplegable de la sección **Ubicación del país o región del servidor**.  
  
4.  En la lista desplegable, seleccione la nueva región y haga clic en **Aplicar** para aceptar la nueva configuración de región.  
  
###  <a name="manage-remote-web-access-permissions"></a><a name="BKMK_ManagePerms"></a>Administrar permisos de acceso Web remoto  
 Cuando se agrega una cuenta de usuario en Windows Server Essentials, se permite al nuevo usuario usar el acceso Web remoto de forma predeterminada. Si decide no permitir el acceso Web remoto para una cuenta de usuario y, a continuación, encuentra que el usuario debe usar el acceso Web remoto, puede actualizar las propiedades de la cuenta de usuario.  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>Para administrar los permisos de acceso Web remoto para una cuenta de usuario:  
  
1. Inicie sesión en el panel y haga clic en **Usuarios**.  
  
2. Haga clic en la cuenta de usuario que desea administrar y en **Ver las propiedades de la cuenta** en el **Panel de tareas**.  
  
3. En el cuadro de diálogo **Propiedades**, haga clic en la pestaña **Acceso desde cualquier lugar**.  
  
4. En la pestaña **Acceso desde cualquier lugar**, active la casilla **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web** para permitir que un usuario se conecte al servidor con acceso Web remoto.  
  
5. Haga clic en **Aplicar** y en **Aceptar**.  
  
   Para obtener más información, consulte [administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
###  <a name="secure-remote-web-access"></a><a name="BKMK_SecureRWA"></a>Acceso Web remoto seguro  
 Windows Server Essentials usa un certificado de seguridad para proteger la información que se intercambia entre el software y un explorador web. Al instalar el software del Conector en los equipos, se agrega el certificado de seguridad para Windows Server Essentials a la lista de certificados de confianza de los equipos. La mejor manera para los usuarios de poder utilizar el acceso Web remoto cuando se encuentran fuera de la oficina consiste en usar un equipo portátil donde se haya instalado el software del Conector.  
  
> [!WARNING]
>  Los usuarios que utilizan acceso Web remoto desde ubicaciones públicas u otros equipos que no son de confianza deben asegurarse de cerrar sesión en el sitio web antes de dejar el equipo desatendido o cuando ya no lo vayan a usar.  
  
###  <a name="manage-remote-web-access-and-vpn-users"></a><a name="BKMK_ManageRWAVPN"></a>Administrar acceso Web remoto y usuarios de VPN  
 Puede conectarse a Windows Server Essentials mediante VPN y acceder a todos los recursos que se almacenan en el servidor. Esto resulta especialmente útil si un equipo cliente está configurado con las cuentas de red que pueden usarse para conectar con un servidor de Windows Server Essentials hospedado, a través de una conexión VPN. Todas las cuentas de usuario nuevas que se creen en el servidor de Windows Server Essentials hospedado deben usar VPN cuando inicien sesión en el equipo cliente por primera vez.  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>Para establecer permisos de acceso Web remoto y VPN para los usuarios de red:  
  
1.  Abra el Panel.  
  
2.  En la barra de navegación, haga clic en **USUARIOS**.  
  
3.  En la lista de cuentas de usuario, seleccione aquella a la que desea conceder permisos de acceso remoto al escritorio.  
  
4.  En el panel tareas de la **cuenta de usuario de <\>** , haga clic en **propiedades**.  
  
5.  En **< cuenta de usuario\> propiedades**, haga clic en la pestaña **acceso desde cualquier lugar** .  
  
6.  En la pestaña **Acceso desde cualquier lugar**, haga lo siguiente:  
  
    1.  Para permitir que un usuario se conecte al servidor mediante VPN, active la casilla **Permitir red privada virtual (VPN)** .  
  
    2.  Para permitir que un usuario se conecte al servidor mediante acceso Web remoto, active la casilla **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.  
  
7.  Haga clic en **Aplicar** y en **Aceptar**.  
  
##  <a name="set-up-your-router"></a><a name="BKMK_2"></a>Configurar el enrutador  
 Cuando intente configurar el servidor para el acceso Web remoto, el Asistente para configurar el Acceso desde cualquier lugar intentará configurar el enrutador. Si cambia el enrutador o su configuración, debe volver a ejecutar el Asistente para configurar el enrutador. Para obtener más información, vea los temas siguientes:  
  
-   [Configurar el enrutador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [Reemplazar un enrutador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [Ubicación de red definida](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [Habilitar Servicios de Escritorio remoto controles ActiveX](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="set-up-your-router"></a><a name="BKMK_SetUpRouter"></a>Configurar el enrutador  
 En este paso, Windows Server Essentials intenta configurar automáticamente el enrutador con comandos UPnP. Para ello, el enrutador debe admitir las normas UPnP y la opción de configuración UPnP debe estar habilitada en el enrutador.  
  
> [!NOTE]
>  La configuración de red debe cumplir los requisitos de red compatibles con Windows Server Essentials. Solo debe haber un enrutador en la red.  
  
 Si el enrutador no se configura con el Asistente para configurar el nombre de dominio, debe desviar manualmente el puerto 443. Para obtener más información sobre cómo configurar el enrutamiento de puerto en su enrutador, consulte [Configuración del enrutador](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx).  
  
###  <a name="replace-a-router"></a><a name="BKMK_ReplaceRouter"></a>Reemplazar un enrutador  
 Reemplace el enrutador según las instrucciones del fabricante y, a continuación, ejecute el Asistente para configurar el enrutador para configurar el nuevo enrutador.  
  
##### <a name="to-set-up-your-new-router"></a>Para configurar el enrutador nuevo:  
  
1.  En el panel de Windows Server Essentials, haga clic en **Configuración**.  
  
2.  Haga clic en la pestaña **Acceso desde cualquier lugar** y, en la sección **Enrutador**, haga clic en **Configurar**. Se iniciará el Asistente para configurar el enrutador.  
  
3.  Siga las instrucciones del asistente para terminar de configurar el enrutador nuevo.  
  
###  <a name="network-location-defined"></a><a name="BKMK_NetworkLocation"></a>Ubicación de red definida  
 Una ubicación de red es una colección de valores de configuración de red que Windows aplica cuando el usuario se conecta a una red. La configuración varía y puede personalizarse conforme al tipo de red. La configuración de una ubicación de red determina si deben activarse o no determinadas características (como compartir archivos e impresoras, la detección de redes y el uso compartido de carpetas públicas). Las ubicaciones de red son útiles cuando es necesario conectarse a distintas redes.  
  
 Por ejemplo, imaginemos que tiene un equipo portátil que usa en casa y en el trabajo. Cuando está en la oficina, se conecta a la red de esta. Y, cuando llega a casa, con el portátil puede acceder a la música y los vídeos que se encuentran almacenados en el servidor principal, y reproducirlos. Cuando se conecta a una nueva red y especifica el tipo de ubicación, Windows asigna un perfil de red preestablecido para este tipo. La próxima vez que se conecte a la red, Windows la reconocerá y asignará automáticamente la configuración correcta. Así se agrega un nivel de seguridad para proteger la información del equipo y solo se activan las características de red necesarias para la ubicación.  
  
 Hay cuatro tipos de ubicaciones de red:  
  
-   **Red doméstica** Elija esta red para las redes domésticas, o si conoce a los usuarios y dispositivos de la red y confía en ellos. Los equipos de una red doméstica pueden pertenecer a un grupo en el hogar. La detección de redes se encuentra activada para las redes domésticas, por lo que usted puede ver otros equipos y dispositivos de la red, y los demás usuarios de red pueden ver su equipo.  
  
-   **Red de trabajo** Elija esta red para redes de oficina pequeña o de otros tipos de área de trabajo. La detección de redes, gracias a la cual usted puede ver otros equipos y dispositivos de la red y otros usuarios de red pueden ver su equipo, se encuentra activada de forma predeterminada. No obstante, no se permite crear un grupo en el hogar ni unirse a él.  
  
-   **Red pública** Elija esta red para lugares públicos (como cafeterías o aeropuertos). Esta ubicación se ha diseñado para que su equipo no resulte visible a otros equipos y protegerlo así del software malintencionado de Internet. En redes públicas no se pueden usar grupos en el hogar y la detección de redes está desactivada. Elija también esta opción si está conectado directamente a Internet (sin enrutador) o dispone de una conexión de banda ancha móvil.  
  
-   **Dominio** Elija esta red para dominios como los de las áreas de trabajo empresariales. El administrador de red controla este tipo de ubicación de red, que no se puede seleccionar ni cambiar.  
  
###  <a name="enable-remote-desktop-services-activex-controls"></a><a name="BKMK_ActiveX"></a>Habilitar Servicios de Escritorio remoto controles ActiveX  
 El Servicios de Escritorio remoto controles ActiveX permite tener acceso a su equipo doméstico o profesional, a través de Internet, desde otro equipo mediante el acceso Web remoto.  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>Para habilitar los controles ActiveX de Servicios de Escritorio remoto:  
  
1.  En Internet Explorer, haga clic en **Herramientas** y **Opciones de Internet**.  
  
2.  En la pestaña **Seguridad**, haga clic en **Nivel personalizado**.  
  
3.  En la sección **Controles y complementos de ActiveX**, haga lo siguiente:  
  
    1.  En **Descargar controles ActiveX firmados**, haga clic en **Pedir datos**.  
  
    2.  En **Ejecutar controles y complementos de ActiveX**, haga clic en **Habilitar**.  
  
4.  Haga clic en **Aceptar** dos veces para aceptar los cambios y cerrar el cuadro de diálogo.  
  
##  <a name="set-up-your-domain-name"></a><a name="BKMK_3"></a>Configurar el nombre de dominio  
 Tras activar el acceso Web remoto, puede configurar el nombre de dominio para el servidor que ejecuta Windows Server Essentials. Este es un paso necesario si planea usar Acceso web remoto desde un equipo remoto. Para obtener más información, vea los temas siguientes:  
  
-   [Información general sobre nombres de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [Descripción de los nombres de dominio personalizados de Microsoft](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [Usar un nombre de dominio nuevo o existente](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [Configurar un nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [Elegir un proveedor de servicios de nombres de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [Elegir un nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [Elegir un prefijo de nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [Elegir una extensión de nombre de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [Actualizar el servicio de nombres de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [Exportar o importar el certificado en el servidor](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [Configurar un nombre de dominio manualmente](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [Busque el proveedor de servicios de nombres de dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="domain-names-overview"></a><a name="BKMK_DNOverview"></a>Información general sobre nombres de dominio  
 Un nombre de dominio identifica de forma única el servidor en Internet. Los nombres de dominio constan al menos de dos partes: un nombre de dominio de primer nivel (TLD) y otro de segundo nivel. Por ejemplo, en contoso.com, com es el TLD y contoso es el nombre de dominio de segundo nivel.  
  
 Mientras está fuera de la oficina, puede usar el nombre de dominio para acceder a archivos compartidos en el servidor o equipos de la red. También puede administrar el servidor en la distancia. Por ejemplo, supongamos que registra contoso.com para el servidor. Si está fuera de la oficina, puede abrir un explorador web en el portátil y escribir **contoso.com** en el cuadro de texto de dirección para conectarse a la instancia de acceso Web remoto que se ha configurado en Windows Server Essentials.  
  
###  <a name="understand-microsoft-personalized-domain-names"></a><a name="BKMK_PersonalizedNames"></a>Descripción de los nombres de dominio personalizados de Microsoft  
 Un nombre de dominio personalizado de Microsoft incluye las siguientes características:  
  
- Un nombre de dominio personalizado para el acceso Web remoto (por ejemplo, *sunombredehost*. remotewebaccess.com). El nombre de dominio está asociado con la dirección IP pública.  
  
- Un servicio de protocolo de actualización dinámica de DNS para que el acceso Web remoto con el nombre de dominio no se interrumpa si cambia la dirección IP pública. Normalmente, los proveedores de servicios Internet (ISP) de las conexiones de banda ancha de su organización proporcionan direcciones IP públicas dinámicas que pueden cambiar.  
  
- Un certificado de confianza asociado con el nombre de dominio.  
  
  Para integrar un nombre de dominio personalizado de Microsoft con el servidor, necesita una cuenta de Microsoft (anteriormente conocida como Windows Live ID). Si no tiene ninguna, puede suscribirse en el sitio web de [Microsoft Hotmail](https://login.live.com/) .  
  
> [!IMPORTANT]
>  Windows Live admite caracteres especiales en la contraseña de cuenta de Microsoft que el servidor no acepta. Si usa un dominio personalizado de Microsoft, asegúrese de que la contraseña de cuenta de Microsoft contiene solo caracteres compatibles con el servidor. El servidor no admite el uso de los caracteres $, /, ' y %.  
  
###  <a name="use-a-new-or-existing-domain-name"></a><a name="BKMK_UseNewName"></a>Usar un nombre de dominio nuevo o existente  
 Para configurar automáticamente el nombre de dominio en un servidor que ejecuta Windows Server Essentials, debe usar uno de los proveedores de servicios de nombres de dominio que aparecen en el Asistente para configurar el nombre de dominio. Puede obtener un nuevo nombre de dominio o usar uno existente. Realice una de las siguientes acciones:  
  
-   Si desea obtener un nuevo nombre de dominio de uno de los proveedores de servicios de nombres de dominio que aparecen en el asistente, haga clic en **Deseo configurar un nombre de dominio nuevo**.  
  
-   Si dispone de un nombre de dominio existente que adquirió de uno de los proveedores de servicios de nombres de dominio compatibles, puede configurar el nombre de dominio del servidor con el Asistente para configurar el nombre de dominio. Haga clic en **Deseo usar un nombre de dominio del que ya soy propietario** y escriba el nombre de dominio en el cuadro de texto **Configurar un nombre de dominio**. Debe proporcionar el nombre de usuario y la contraseña que usó para comprar el nombre de dominio.  
  
-   Si dispone de un nombre de dominio existente que adquirió de un proveedor de servicios de nombres de dominio no compatible con Windows Server Essentials, y desea configurar el nombre de dominio para el servidor con el Asistente para configurar el nombre de dominio, puede transferir el nombre de dominio a uno de los proveedores que aparecen en el asistente. Haga clic en **deseo usar un nombre de dominio del que ya soy propietario**, escriba el nombre de dominio en el cuadro de texto **nombre de dominio** y, a continuación, siga las instrucciones del sitio web del proveedor de servicios de nombres de dominio para transferir el nombre de dominio.  
  
###  <a name="set-up-a-domain-name"></a><a name="BKMK_SetUpName"></a>Configurar un nombre de dominio  
 Si activa el acceso Web remoto, puede configurar el nombre de dominio de Internet del servidor.  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>Para configurar o administrar un nombre de dominio de Internet:  
  
1.  Abra el Panel.  
  
2.  Haga clic en **Configuración del servidor** y en **Acceso desde cualquier lugar**.  
  
3.  En la sección **Nombre de dominio**, haga clic en **Configurar**.  
  
4.  Siga las instrucciones para completar el asistente. Si aún no tiene un certificado y un nombre de dominio propios, el asistente le ayudará a buscar un proveedor de nombres de dominio para adquirirlos. O también puede obtener un nombre de dominio de Microsoft personalizado.  
  
###  <a name="choose-a-domain-name-service-provider"></a><a name="BKMK_ChooseProvider"></a>Elegir un proveedor de servicios de nombres de dominio  
 Debe elegir un proveedor de servicios de nombres de dominio que admita la extensión de nombre de dominio que desea usar. El Asistente para configurar el nombre de dominio incluye una lista de proveedores calificados que puede usar con un vínculo al sitio web de cada proveedor. Haga clic en el vínculo **más información** situado junto al nombre de cada proveedor para obtener información sobre los servicios y precios que ofrece el proveedor.  
  
> [!NOTE]
>  Algunos proveedores de servicios de nombres de dominio operan en amplias regiones internacionales y otros en mercados más reducidos. Por este motivo, con algunos proveedores no podrá usar un sitio web y traducirlo a su idioma de preferencia.  
  
 Si compra un nombre de dominio, considere la posibilidad de adquirir también el servicio de protocolo de actualización dinámica de Sistema de nombres de dominio (DNS) del proveedor de servicios de nombres de dominio. El protocolo de actualización dinámica de DNS es un servicio que permite a todos los usuarios de Internet acceder a recursos en una red local cuando la dirección IP de esa red cambia constantemente. O puede comprar una dirección IP estática de su proveedor de acceso a Internet (ISP) para garantizar que la dirección IP no cambie.  
  
###  <a name="choose-a-domain-name"></a><a name="BKMK_ChooseDomainName"></a>Elegir un nombre de dominio  
 Elija un nombre que identifique al servidor de empresa de forma única. Por ejemplo, si el nombre de su empresa es Contoso Ltd, puede elegir Contoso para identificar al servidor principal o de empresa en Internet. Si el nombre de dominio no está disponible, intente otra variación de ese nombre, o quizás algo completamente diferente.  
  
 El nombre que escriba puede contener los elementos siguientes:  
  
-   63 caracteres como máximo.  
  
-   Letras (en inglés o con los caracteres localizados), números o guiones (-). El nombre debe comenzar y terminar con una letra o un número.  
  
    > [!NOTE]
    >  Los nombres de dominio no diferencian mayúsculas de minúsculas.  
  
###  <a name="choose-a-domain-name-prefix"></a><a name="BKMK_Prefixes"></a>Elegir un prefijo de nombre de dominio  
 Un nombre de dominio se compone de etiquetas jerárquicas.  
  
 **La extensión de dominio de primer nivel** es la etiqueta que aparece más a la derecha en el nombre de dominio. Por ejemplo, en www\.contoso.com, com es la extensión de nombre de dominio de nivel superior.  
  
 **El nombre de dominio de segundo nivel** es la etiqueta que aparece junto a la extensión de primer nivel. El nombre de dominio de segundo nivel a menudo se crea a partir del nombre de la compañía, productos o servicios. Por ejemplo, en www\.contoso.com, contoso es el nombre de dominio de segundo nivel y se ha elegido para el nombre de la compañía contoso Pharmaceuticals. El dominio de segundo nivel a veces se conoce como nombre de host, que cuenta con una dirección IP asociada.  
  
 **El prefijo de nombre de dominio** identifica un subdominio. El nombre del subdominio puede usarse para identificar regiones, dispositivos o servicios. Por ejemplo, Contoso Pharmaceuticals desea permitir que los usuarios remotos inicien sesión en el acceso Web remoto, pero no desea que el sitio web esté disponible para el público general. Por ello, se crea un subdominio que solo permite acceder al sitio web a los usuarios con los permisos adecuados. Contoso Pharmaceuticals configura remoto.contoso.com como subdominio y remoto como prefijo de nombre de dominio.  
  
> [!TIP]
>  Se recomienda usar el valor predeterminado de **Remoto** como prefijo del nombre de dominio.  
  
###  <a name="choose-a-domain-name-extension"></a><a name="BKMK_Extension"></a>Elegir una extensión de nombre de dominio  
 Cuando elija un nombre de dominio para el sitio web de Internet, también deberá especificar la extensión de nombre de dominio que desea usar. La extensión se identifica con las letras que siguen al punto final del nombre de dominio. (El término formal para la extensión es el dominio de nivel superior o el TLD).  
  
 Puede usar dos tipos principales de extensiones de dominio: las genéricas y de código de país.  
  
#### <a name="generic-top-level-domains"></a>Dominios genéricos de primer nivel  
 Las extensiones de dominio genéricas se componen de tres o más letras y generalmente se usan en tipos de organizaciones determinados.  
  
 **Ejemplos de dominios genéricos de nivel superior**  
  
|Extensión de dominio|Descripción|  
|----------------------|-----------------|  
|.com|Normalmente la usan organizaciones comerciales, pero la puede utilizar cualquier usuario.|  
|.net|Diseñada para empresas que ofrecen servicios de infraestructura de red.|  
|.org|En un primer momento, la usaban agencias sin ánimo de lucro y otras empresas que no correspondían a las demás categorías de dominios genéricos de primer nivel. La puede usar cualquier usuario.|  
|.edu|Restringida para organizaciones educativas.|  
  
#### <a name="country-code-top-level-domains"></a>Dominios de primer nivel de código de país  
 Estas extensiones de dominio se componen de dos letras. Están diseñadas para organizaciones de una región o un país asociado con el código correspondiente. Algunos dominios de primer nivel de código de país están restringidos a los ciudadanos de dicho país o región. Otros están disponibles para cualquier usuario.  
  
 **Ejemplos de dominios de primer nivel de código de país**  
  
|Extensión de dominio|Descripción|  
|----------------------|-----------------|  
|.ca|Para sitios web de Canadá.|  
|.cn|Para sitios web de China.|  
|.de|Para sitios web de Alemania.|  
|.co.uk|Para sitios web del Reino Unido.|  
  
 Para ver la lista completa de dominios de primer nivel, consulte el [sitio web de la autoridad de asignación de números de Internet](https://go.microsoft.com/fwlink/?LinkId=117438).  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>Si una extensión de dominio no se puede seleccionar en el Asistente para configurar el nombre de dominio:  
 Cuando se ejecuta el Asistente para configurar el nombre de dominio, examina la información del sistema para determinar el país o la región. Entonces muestra solo las extensiones de dominio que admiten los proveedores participantes en su área. Si la extensión de dominio que desea no aparece en la lista, debe elegir otra para continuar. Seleccione una extensión de la lista que ofrece el asistente.  
  
###  <a name="update-or-upgrade-your-domain-name-service"></a><a name="BKMK_UpdateService"></a>Actualizar el servicio de nombres de dominio  
 Deberá actualizar el servicio de nombres de dominio si adquirió un nombre de dominio, pero no un certificado. Debe obtener un certificado para el nombre de dominio de su proveedor de servicios de nombres de dominio.  
  
> [!NOTE]
>  Determine junto con su proveedor el tipo de certificado que necesita. El certificado puede ser uno de precio reducido de los que le han ofertado. Sin embargo, es conveniente que revise la documentación y las características de otros certificados de seguridad de niveles superiores para averiguar si van a satisfacer mejor o no sus necesidades empresariales.  
  
###  <a name="export-or-import-your-certificate-on-your-server"></a><a name="BKMK_ExportCert"></a>Exportar o importar el certificado en el servidor  
 Si desea crear una copia de seguridad de un certificado o usarlo en otro servidor, debe exportar el certificado. Para obtener información acerca de la exportación de certificados, consulte [Exportar un certificado](https://go.microsoft.com/fwlink/p/?LinkId=214362).  
  
###  <a name="set-up-a-domain-name-manually"></a><a name="BKMK_SetNameManually"></a>Configurar un nombre de dominio manualmente  
 Si elige esta opción, el servidor no supervisa ni mantiene el nombre de dominio, y no le alerta si hay un problema de configuración. También puede considerar esta opción si se da alguno de los siguientes casos:  
  
- No se encuentran proveedores de nombres de dominio asociados para su país o región.  
  
- Los proveedores de dominio asociados enumerados no admiten su extensión de nombre de dominio.  
  
- Ya tiene un nombre de dominio de un proveedor de nombres de dominio que actualmente no es un asociado, y no desea transferir ese nombre de dominio a uno de los proveedores de nombres de dominio que admite Windows Server Essentials.  
  
- El asistente no incluye la extensión de nombre de dominio que quiere usar, pero la extensión está disponible en un proveedor de nombres de dominio que actualmente no es un asociado.  
  
  Si decide configurar el nombre de dominio manualmente, trabaje con su proveedor de servicios de nombres de dominio para crear un registro A para su dominio.  
  
##### <a name="to-create-an-a-record"></a>Para crear un registro d  
  
1.  Decida un nombre de host, como Remote. Este es el prefijo de nombre de dominio. El prefijo de nombre de dominio más el nombre de dominio definirá la dirección URL para abrir la página de inicio de sesión de acceso Web remoto. por ejemplo, **http://remote.contoso.com** .  
  
2.  En el panel de configuración de proveedores de servicios de nombres de dominio (normalmente en su página web), cree un registro A para el nombre de host que decidió en el paso 1. Asegúrese de que la dirección IP que especifique en el registro A es la dirección IP en el lado de la WAN del enrutador (el lado accesible desde Internet). Consulte la documentación del enrutador para obtener la dirección IP WAN.  
  
3.  Es recomendable que se ponga en contacto con su proveedor de servicios de Internet (ISP) para comprar una IP estática para su red. De esta manera se asegura de que la dirección IP no cambia y de que su entrada DNS no se queda obsoleta.  
  
     Si no puede comprar una dirección IP estática a su ISP, considere la opción de adquirir un servicio de protocolo de actualización dinámica de Sistema de nombres de dominio (DNS), bien de su proveedor de servicios de nombres de dominio, o bien de otro proveedor de servicios. El protocolo de actualización dinámica de DNS es un servicio que mantiene actualizada la dirección IP de WAN de la red, de manera que la dirección IP puede resolverse en un nombre de dominio aunque cambie.  
  
4.  Importe un certificado de confianza cuando el asistente se lo pida. Si no tiene un certificado de confianza, puede obtener uno de alguno de los proveedores de nombres de dominio admitidos que se enumeran en el asistente, o bien comprar uno al proveedor de confianza que elija. Para obtener más información sobre los certificados de confianza, póngase en contacto con su proveedor de nombres de dominio.  
  
###  <a name="find-your-domain-name-service-provider"></a><a name="BKMK_Find"></a>Busque el proveedor de servicios de nombres de dominio  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>Para buscar el proveedor de servicios de nombres de dominio para un nombre de dominio:  
  
1. Abra un explorador web y escriba <strong>www.internic.com</strong> en la barra de direcciones para ir a la página principal de Internic.  
  
2. En la página principal de Internic, haga clic en **Whois**.  
  
3. En el cuadro **Whois Search**, escriba el nombre de dominio (por ejemplo, contoso.com).  
  
4. Haga clic en la opción **Domain** y en **Submit**.  
  
5. En los resultados de la búsqueda, aparece el nombre de su proveedor de servicios de nombres de dominio en **Registrar**.  
  
##  <a name="customize-remote-web-access"></a><a name="BKMK_4"></a>Personalización del acceso Web remoto  
 Puede personalizar el sitio de Acceso web remoto agregando un logotipo o una imagen de fondo personalizados. También puede agregar vínculos a la página Inicio para que esta información esté disponible para todos sus usuarios. Para obtener más información, vea los temas siguientes:  
  
-   [Personalización del acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [Personalización de imágenes de fondo y logotipos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [Reparar acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="customize-remote-web-access"></a><a name="BKMK_CustomizeRWA"></a>Personalización del acceso Web remoto  
 Para personalizar el acceso Web remoto, puede cambiar el título del sitio web, el logotipo y la imagen de fondo, y agregar vínculos a otros sitios web en la página principal.  
  
##### <a name="to-customize-remote-web-access"></a>Para personalizar el acceso Web remoto:  
  
1.  Abra el Panel.  
  
2.  Haga clic en **Configuración** y, a continuación, en la pestaña **Acceso desde cualquier lugar**.  
  
3.  En la sección **Configuración del sitio web**, haga clic en **Personalizar**.  
  
4.  Cuando haya terminado de personalizar el acceso Web remoto, haga clic en **Aceptar**. Pruebe los cambios en el acceso Web remoto.  
  
###  <a name="customize-images-for-backgrounds-and-logos"></a><a name="BKMK_CustomizeImages"></a>Personalización de imágenes de fondo y logotipos  
 Esta sección proporciona información sobre las imágenes que puede usar para personalizar el acceso Web remoto.  
  
#### <a name="image-size"></a>Tamaño de la imagen  
 **Imágenes de logotipo**  
  
 Se recomiendan las imágenes de logotipo de 32 x 32 píxeles. Las imágenes que superan o no alcanzan los 32 x 32 píxeles se adaptan a este parámetro, y pueden distorsionarse.  
  
 **Imágenes de fondo**  
  
 Aunque no hay límite de tamaño para las imágenes de fondo, para obtener los mejores resultados, se recomiendan imágenes de 800 x 500 píxeles aproximadamente. La imagen de fondo se coloca en el centro (vertical y horizontal) de la página de inicio de sesión. Para facilitar la lectura del texto de la página de inicio de sesión, conviene usar un color claro en el centro de la imagen de fondo.  
  
#### <a name="image-file-types"></a>Tipos de archivo de imagen  
 Puede reemplazar la imagen de fondo y el logotipo del sitio web predeterminados por los tipos de archivo de imagen siguientes:  
  
-   Mapa de bits (*. bmp, \*. dib, \*. RLE)  
  
-   GIF (*.gif)  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="repair-remote-web-access"></a><a name="BKMK_RepairRWA"></a>Reparar acceso Web remoto  
 El asistente para reparar le ayudará a detectar y resolver los problemas del enrutador o el nombre de dominio. Puede detectar los problemas con el acceso Web remoto de dos formas:  
  
-   En Configuración del servidor (en el panel), vaya a la pestaña Acceso desde cualquier lugar. Allí verá un icono con una X roja y una descripción del problema.  
  
-   Una alerta en el Visor de alertas.  
  
> [!NOTE]
>  Si desea que el asistente para reparar esté disponible, active el acceso Web remoto. Para más información, consulte [Activar Acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
##### <a name="to-repair-remote-web-access"></a>Para reparar el acceso Web remoto:  
  
1.  Inicie sesión en el panel.  
  
2.  Haga clic en **Configuración** y, a continuación, en la pestaña **Acceso desde cualquier lugar**.  
  
3.  Haga clic en **Reparar**. Se iniciará el **Asistente para reparación del acceso Web remoto**.  
  
4.  Haga clic en **Siguiente**. El asistente analiza el acceso Web remoto, identifica el problema e intenta repararlo.  
  
5.  Si recibe una alerta cuando finalice el asistente, haga clic en **Reintentar** para volver a intentar la reparación. Si recibe otra alerta, compruébela para obtener información adicional sobre el problema y los pasos necesarios para solucionarlo.  
  
##  <a name="troubleshoot-remote-web-access"></a><a name="BKMK_5"></a>Solucionar problemas de acceso Web remoto  
  
-   [Solucionar problemas de conectividad de acceso Web remoto](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [Solucionar problemas del firewall](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [Solucionar problemas de acceso desde cualquier lugar](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>Vea también  
  
-   [Opciones de escritorio remoto](Remote-desktop-options.md)  
  
-   [Usar acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar el acceso desde cualquier lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
