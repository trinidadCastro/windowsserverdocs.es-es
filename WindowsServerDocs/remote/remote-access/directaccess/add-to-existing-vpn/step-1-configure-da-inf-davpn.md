---
title: Paso 1 configurar la infraestructura de DirectAccess
description: Este tema forma parte de la guía agregar DirectAccess a una implementación de acceso remoto (VPN) existente para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5dc529f7-7bc3-48dd-b83d-92a09e4055c4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4437101c6cde25ebb370fe54a2f8ef821997f15d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388772"
---
# <a name="step-1-configure-the-directaccess-infrastructure"></a>Paso 1 configurar la infraestructura de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo configurar la infraestructura necesaria para habilitar DirectAccess en una implementación de VPN existente. Antes de comenzar con los pasos de implementación, asegúrese de haber completado los pasos de planeación descritos en [paso 1: planear la infraestructura de DirectAccess](Step-1-Plan-DirectAccess-Infrastructure.md).  
  
|Tarea|Descripción|  
|----|--------|  
|Configurar los valores de red del servidor|Configura los valores de red del servidor en el servidor de acceso remoto.|  
|Configurar el enrutamiento en la red corporativa|Configura el enrutamiento en la red corporativa para asegurarte de que el tráfico se enruta correctamente.|  
|Configurar los firewalls|Configure los firewalls adicionales, si es necesario.|  
|Configurar las entidades de certificación y los certificados|El asistente para habilitar DirectAccess configura un proxy Kerberos integrado que autentica utilizando nombres de usuario y contraseñas. También configura un certificado IP-HTTPS en el servidor de acceso remoto.|  
|Configurar el servidor DNS|Configura los valores de DNS para el servidor de acceso remoto.|  
|Configurar Active Directory|Une los equipos cliente al dominio de Active Directory.|  
|Configurar GPO|Configura los GPO para la implementación, si es necesario.|  
|Configurar grupos de seguridad|Configura los grupos de seguridad que contendrán equipos cliente de DirectAccess, y cualquier otro grupo de seguridad necesario en la implementación.|  
|Configurar el servidor de ubicación de red|El asistente para habilitar DirectAccess configura el servidor de ubicación de red en el servidor de DirectAccess.|  
  
## <a name="ConfigNetworkSettings"></a>Configurar las opciones de red del servidor  
Los siguientes valores de la interfaz de red son necesarios para una implementación de un solo servidor en un entorno con IPv4 e IPv6. Todas las direcciones IP se configuran mediante **Cambiar configuración del adaptador** en el **Centro de redes y recursos compartidos de Windows**.  
  
-   Topología perimetral  
  
    -   Una dirección IPv4 o IPv6 estática pública con acceso a Internet.  
  
    -   Una sola dirección IPv4 o IPv6 estática interna.  
  
-   Detrás de un dispositivo NAT (dos adaptadores de red)  
  
    -   Una sola dirección IPv4 o IPv6 estática interna con acceso a la red.  
  
-   Detrás de un dispositivo NAT (un adaptador de red)  
  
    -   Una sola dirección IPv4 o IPv6 estática.  
  
> [!NOTE]  
> En el caso de que el servidor de acceso remoto tenga dos adaptadores de red (uno clasificado en el perfil de dominio y el otro en un perfil público/privado) pero se vaya a utilizar una sola topología NIC, se aconseja lo siguiente:  
>   
> 1.  Asegúrate de que el segundo NIC también está clasificado en el perfil de dominio (recomendado).  
> 2.  Si el segundo NIC no puede configurarse para el perfil de dominio por alguna razón, el ámbito de la directiva IPsec de DirectAccess debe ampliarse manualmente a todos los perfiles mediante los siguientes comandos de Windows PowerShell:  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
  
## <a name="ConfigRouting"></a>Configurar el enrutamiento en la red corporativa  
Configura el enrutamiento en la red corporativa de la siguiente manera:  
  
-   Si se implementa IPv6 nativa en la organización, agrega una ruta para que los enrutadores de la red interna enruten el tráfico IPv6 a través del servidor de acceso remoto.  
  
-   Configura manualmente las rutas de IPv4 e IPv6 de la organización en los servidores de acceso remoto. Agrega una ruta publicada para que todo el tráfico con un prefijo IPv6 (/48) de organización se reenvíe a la red interna. Además, para el tráfico IPv4, agrega rutas explícitas para que el tráfico IPv4 se reenvíe a la red interna.  
  
## <a name="ConfigFirewalls"></a>Configuración de firewalls  
Si usa firewalls adicionales en la implementación, aplique las siguientes excepciones del firewall con conexión a Internet para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentra en Internet IPv4:  
  
-   tráfico 6to4: protocolo IP 41 entrante y saliente.  
  
-   IP-HTTPS-puerto de destino del Protocolo de control de transmisión (TCP) 443 y puerto de origen TCP 443 de salida. Si el servidor de acceso remoto tiene un único adaptador de red y el servidor de ubicación de red se encuentra en el servidor de acceso remoto, el puerto TCP 62000 también es obligatorio.  
  
Si usa firewalls adicionales, aplique las siguientes excepciones del firewall con conexión a Internet para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentra en Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
Si usa firewalls adicionales, aplique las siguientes excepciones de firewall de la red interna para el tráfico de acceso remoto:  
  
-   ISATAP-protocolo 41 entrante y saliente  
  
-   TCP/UDP para todo el tráfico IPv4/IPv6  
  
## <a name="ConfigCAs"></a>Configurar entidades de certificación y certificados  
El asistente para habilitar DirectAccess configura un proxy Kerberos integrado que autentica utilizando nombres de usuario y contraseñas. También configura un certificado IP-HTTPS en el servidor de acceso remoto.  
  
### <a name="ConfigCertTemp"></a>Configurar plantillas de certificado  
Cuando uses una CA interna para emitir certificados, debes configurar una plantilla de certificado para el certificado IP-HTTPS y el certificado del sitio web del servidor de ubicación de red.  
  
##### <a name="to-configure-a-certificate-template"></a>Para configurar una plantilla de certificado  
  
1.  En la CA interna, cree una plantilla de certificado del modo descrito en el tema sobre [creación de plantillas de certificado](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Implemente la plantilla de certificado según se indica en el tema sobre [implementación de plantillas de certificado](https://technet.microsoft.com/library/cc770794.aspx).  
  
### <a name="configure-the-ip-https-certificate"></a>Configurar el certificado IP-HTTPS  
El acceso remoto requiere que un certificado IP-HTTPS autentique las conexiones IP-HTTPS con el servidor de acceso remoto. Hay tres opciones de certificado para el certificado IP-HTTPS:  
  
-   **Público**: proporcionado por un tercero.  
  
    Un certificado utilizado para la autenticación IP-HTTPS. Si el nombre del firmante del certificado no es un carácter comodín, debe ser la dirección URL FQDN que pueda resolverse externamente utilizada solo para conexiones IP-HTTPS al servidor de acceso remoto.  
  
-   **Privado**: se necesita lo siguiente, si aún no existen:  
  
    -   Un certificado de sitio web utilizado para la autenticación IP-HTTPS. El firmante del certificado debe ser un nombre de dominio completo (FQDN) que pueda resolverse externamente y accesible desde Internet.  
  
    -   Un punto de distribución de lista de revocación de certificados (CRL) que sea accesible desde un FQDN que pueda resolverse públicamente.  
  
-   **Autofirmado**: se requiere lo siguiente, si aún no existen:  
  
    > [!NOTE]  
    > Los certificados autofirmados no pueden usarse en implementaciones multisitio.  
  
    -   Un certificado de sitio web utilizado para la autenticación IP-HTTPS. El firmante del certificado debe ser un FQDN que pueda resolverse externamente y accesible desde Internet.  
  
    -   Un punto de distribución de CRL accesible desde un nombre de dominio completo (FQDN) que pueda resolverse públicamente.  
  
Asegúrate de que el certificado de sitio web utilizado para la autenticación IP-HTTPS reúna estos requisitos:  
  
-   El nombre común del certificado debe coincidir con el nombre del sitio IP-HTTPS.  
  
-   En el campo Asunto, especifica una dirección IPv4 del adaptador orientado externamente del servidor de acceso remoto, o bien el FQDN de la dirección URL IP-HTTPS.  
  
-   En el campo Uso mejorado de claves, use el identificador de objeto (OID) Autenticación de servidor.  
  
-   Para el campo Puntos de distribución CRL, indica un punto de distribución CRL al que puedan obtener acceso los clientes de DirectAccess que estén conectados a Internet.  
  
-   El certificado IP-HTTPS debe tener una clave privada.  
  
-   El certificado IP-HTTPS se debe importar directamente al almacén personal.  
  
-   Los nombres de certificado IP-HTTPS pueden contener comodines.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Cómo instalar el certificado IP-HTTPS desde una CA interna  
  
1.  En el servidor de acceso remoto: en la pantalla **Inicio** , escriba**MMC. exe**y, a continuación, presione Entrar.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos**, haz clic en **Certificados**, **Agregar**, **Cuenta de equipo**, **Siguiente**, **Equipo local**, **Finalizar** y **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En la página **solicitar certificados** , active la casilla de la plantilla de certificado y, si es necesario, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
8.  En el cuadro de diálogo **Propiedades de certificado**, en la pestaña **Firmante**, en la zona **Nombre del firmante**, en **Tipo**, selecciona **Nombre común**.  
  
9. En **Valor**, especifica una dirección IPv4 del adaptador orientado externamente del servidor de acceso remoto, o bien el FQDN de la dirección URL IP-HTTPS, y haz clic en **Agregar**.  
  
10. En la zona **Nombre alternativo**, en **Tipo**, selecciona **DNS**.  
  
11. En **Valor**, especifica una dirección IPv4 del adaptador orientado externamente del servidor de acceso remoto, o bien el FQDN de la dirección URL IP-HTTPS, y haz clic en **Agregar**.  
  
12. En la pestaña **General**, en **Nombre descriptivo**, puedes escribir un nombre que te ayude a identificar el certificado.  
  
13. En la pestaña **Extensiones**, junto a **Uso mejorado de clave**, haz clic en la flecha y asegúrate de que Autenticación de servidor se encuentra en la lista de **Opciones seleccionadas**.  
  
14. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
15. En el panel de detalles del complemento Certificados, comprueba que se inscribió un nuevo certificado con el valor Propósitos planteados de autenticación del servidor.  
  
## <a name="ConfigDNS"></a>Configurar el servidor DNS  
Debes configurar manualmente una entrada DNS para el sitio web del servidor de ubicación de red para la red interna de tu implementación.  
  
### <a name="NLS_DNS"></a>Para crear el servidor de ubicación de red y los registros DNS de sondeo Web  
  
1.  En el servidor DNS de la red interna: en la pantalla **Inicio** , escriba * * DNSMgmt. msc * * y, a continuación, presione Entrar.  
  
2.  En el panel izquierdo de la consola del **Administrador del DNS**, expande la zona de búsqueda directa de tu dominio. Haz clic con el botón secundario en el dominio y haz clic en **Host nuevo (A o AAAA)** .  
  
3.  En el cuadro de diálogo **Host nuevo**, en el cuadro **Nombre (si se deja en blanco, se usa el nombre del dominio primario)** , escribe el nombre DNS del sitio web del servidor de ubicación de red (es el nombre que los clientes de DirectAccess usan para conectarse al servidor de ubicación de red). En el cuadro **Dirección IP**, escribe la dirección IPv4 del servidor de ubicación de red y haz clic en **Agregar host**. En el cuadro de diálogo **DNS**, haz clic en **Aceptar**.  
  
4.  En el cuadro de diálogo **Host nuevo**, en el cuadro **Nombre (si se deja en blanco, se usa el nombre del dominio primario)** , escribe el nombre DNS del sondeo web (el nombre del sondeo web predeterminado es directaccess-webprobehost). En el cuadro **Dirección IP**, escribe la dirección IPv4 del sondeo web y haz clic en **Agregar host**. Repite este proceso para directaccess-corpconnectivityhost y todos los comprobadores de conectividad creados manualmente. En el cuadro de diálogo **DNS**, haz clic en **Aceptar**.  
  
5.  Haz clic en **Listo**.  

![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
También debes configurar entradas DNS para los siguiente:  
  
-   **El servidor IP-https**: los clientes de DirectAccess deben ser capaces de resolver el nombre DNS del servidor de acceso remoto desde Internet.  
  
-   **Comprobación de revocación de CRL**: DirectAccess usa la comprobación de revocación de certificados para la conexión IP-https entre los clientes de DirectAccess y el servidor de acceso remoto, y para la conexión basada en https entre el cliente de DirectAccess y el servidor de ubicación de red. En ambos casos, los clientes de DirectAccess deben ser capaces de resolver y acceder a la ubicación del punto de distribución de CRL.  
  
## <a name="ConfigAD"></a>Configurar Active Directory  
El servidor de acceso remoto y todos los equipos cliente de DirectAccess deben estar unidos a un dominio de Active Directory. Los equipos cliente de DirectAccess deben pertenecer a uno de los siguientes tipos de dominio:  
  
-   Dominios que pertenecen al mismo bosque que el servidor de acceso remoto.  
  
-   Dominios que pertenecen a bosques con confianza bidireccional con el bosque del servidor de acceso remoto.  
  
-   Dominios con confianza de dominio bidireccional con el dominio del servidor de acceso remoto.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Cómo unir equipos cliente al dominio  
  
1.  En la pantalla **Inicio** , escriba **Explorer. exe**y, a continuación, presione Entrar.  
  
2.  Haz clic con el botón secundario en el icono del equipo y, a continuación, haz clic en **Propiedades**.  
  
3.  En la página **Sistema**, haz clic en **Configuración avanzada del sistema**.  
  
4.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
5.  En **Nombre de equipo**, escribe el nombre del equipo si también estás cambiando el nombre del equipo al unir el servidor al dominio. En **Miembro de**, haga clic en **Dominio** y, a continuación, escriba el nombre del dominio al que desea unir el servidor, por ejemplo corp.contoso.com, y haga clic en **Aceptar**.  
  
6.  Cuando se le solicite un nombre de usuario y una contraseña, escriba el nombre de usuario y la contraseña de un usuario con derechos para unir equipos al dominio y, a continuación, haga clic en **Aceptar**.  
  
7.  Cuando vea un cuadro de diálogo en el que se le da la bienvenida al dominio, haga clic en **Aceptar**.  
  
8.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
9. En el cuadro de diálogo **Propiedades del sistema**, haga clic en Cerrar. Haz clic en **Reiniciar ahora** cuando se te solicite.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Tenga en cuenta que debe proporcionar credenciales de dominio después de introducir el comando Add-Computer a continuación.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>Configurar GPO  
Para implementar el acceso remoto, se requiere un mínimo de dos objetos directiva de grupo: uno directiva de grupo objeto contiene la configuración para el servidor de acceso remoto y otro contiene la configuración de los equipos cliente de DirectAccess. Al configurar el acceso remoto, el asistente crea automáticamente los objetos directiva de grupo necesarios. Sin embargo, si su organización exige una Convención de nomenclatura, o si no tiene los permisos necesarios para crear o editar objetos directiva de grupo, se deben crear antes de configurar el acceso remoto.  
  
Para crear directiva de grupo objetos, vea [crear y editar un objeto Directiva de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> El administrador puede vincular manualmente los objetos de directiva de grupo de DirectAccess a una unidad organizativa siguiendo estos pasos:  
>   
> 1.  Antes de configurar DirectAccess, vincula los GPO creados a las unidades organizativas respectivas.  
> 2.  Configura DirectAccess, especificando un grupo de seguridad para los equipos cliente.  
> 3.  Es posible que el administrador de acceso remoto tenga permisos para vincular los objetos de directiva de grupo al dominio. En cualquier caso, los objetos de directiva de grupo se configurarán automáticamente. Si los GPO ya están vinculados a una unidad organizativa, los vínculos no se eliminarán y los GPO no se vincularán al dominio. Para un GPO de servidor, la unidad organizativa debe contener el objeto de equipo del servidor, o el GPO se vinculará a la raíz del dominio.  
> 4.  Si el vínculo a la unidad organizativa no se ha realizado antes de ejecutar el Asistente de DirectAccess, una vez completada la configuración, el administrador de dominio puede vincular los objetos de directiva de grupo de DirectAccess a las unidades organizativas necesarias. El vínculo al dominio puede eliminarse. [Aquí](https://technet.microsoft.com/library/cc732979.aspx)encontrará los pasos para vincular un objeto de directiva de grupo a una unidad organizativa.  
  
> [!NOTE]  
> Si un objeto de directiva de grupo se creó manualmente, es posible que durante la configuración de DirectAccess el objeto de directiva de grupo no esté disponible. Es posible que el objeto de directiva de grupo no se haya replicado en el controlador de dominio más próximo al equipo de administración. En este caso, el administrador puede esperar a que la replicación finalice, o bien forzarla.  
  
## <a name="ConfigSGs"></a>Configurar grupos de seguridad  
La configuración de DirectAccess contenida en el objeto de directiva de grupo equipo cliente solo se aplica a los equipos que son miembros de los grupos de seguridad que se especifican al configurar el acceso remoto. Además, si usas grupos de seguridad para administrar tus servidores de aplicación, debes crear un grupo de seguridad para dichos servidores.  
  
### <a name="Sec_Group"></a>Para crear un grupo de seguridad para clientes de DirectAccess  
  
1.  En la pantalla **Inicio** , escriba**DSA. msc**y, a continuación, presione Entrar. En la consola **Usuarios y equipos de Active Directory**, en el panel izquierdo, expande el dominio que contendrá el grupo de seguridad, haz clic con el botón secundario en **Usuarios**, elige **Nuevo** y haz clic en **Grupo**.  
  
2.  En el cuadro de diálogo **Nuevo objeto: Grupo**, en **Nombre de grupo**, escribe el nombre de grupo de seguridad.  
  
3.  En **Ámbito de grupo** haz clic en **Global**, en **Tipo de grupo** haz clic en **Seguridad** y, a continuación, haz clic en **Aceptar**.  
  
4.  Haz doble clic en el grupo de seguridad de equipos cliente de DirectAccess y, en el cuadro de diálogo de propiedades, haz clic en la pestaña **Miembros**.  
  
5.  En la pestaña **Miembros** , haga clic en **Agregar**.  
  
6.  En el cuadro de diálogo **Seleccionar Usuarios, Contactos, Equipos o Cuentas de servicio**, selecciona los equipos cliente que deseas habilitar para DirectAccess y haz clic en **Aceptar**.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)**comandos equivalentes** de Windows PowerShell Windows PowerShell  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="ConfigNLS"></a>Configurar el servidor de ubicación de red  
El servidor de ubicación de red debe encontrarse en un servidor con alta disponibilidad y un certificado SSL válido de confianza para los clientes de DirectAccess. Hay dos opciones de certificados para el certificado de servidor de ubicación de red:  
  
-   **Privado**: se necesita lo siguiente, si aún no existen:  
  
    -   Un certificado de sitio web usado para el servidor de ubicación de red. El firmante del certificado debe ser la dirección URL del servidor de ubicación de red.   
  
    -   Un punto de distribución de CRL altamente disponible desde la red interna.  
  
-   **Autofirmado**: se requiere lo siguiente, si aún no existen:  
  
    > [!NOTE]  
    > Los certificados autofirmados no pueden usarse en implementaciones multisitio.  
  
    -   Un certificado de sitio web usado para el servidor de ubicación de red. El firmante del certificado debe ser la dirección URL del servidor de ubicación de red.  
  
> [!NOTE]  
> Si el sitio web del servidor de ubicación de red está ubicado en el servidor de acceso remoto, se creará automáticamente un sitio web al configurar el acceso remoto enlazado al certificado de servidor que proporciones.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Cómo instalar el certificado de servidor de ubicación de red desde una CA interna  
  
1.  En el servidor que hospedará el sitio web del servidor de ubicación de red: en la pantalla **Inicio** , escriba**MMC. exe**y, a continuación, presione Entrar.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos**, haz clic en **Certificados**, **Agregar**, **Cuenta de equipo**, **Siguiente**, **Equipo local**, **Finalizar** y **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En la página **solicitar certificados** , active la casilla de la plantilla de certificado y, si es necesario, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
8.  En el cuadro de diálogo **Propiedades de certificado**, en la pestaña **Firmante**, en la zona **Nombre del firmante**, en **Tipo**, selecciona **Nombre común**.  
  
9. En **Valor**, escribe el FQDN del sitio web del servidor de ubicación de red y, a continuación, haz clic en **Agregar**.  
  
10. En la zona **Nombre alternativo**, en **Tipo**, selecciona **DNS**.  
  
11. En **Valor**, escribe el FQDN del sitio web del servidor de ubicación de red y, a continuación, haz clic en **Agregar**.  
  
12. En la pestaña **General**, en **Nombre descriptivo**, puedes escribir un nombre que te ayude a identificar el certificado.  
  
13. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
14. En el panel de detalles del complemento Certificados, comprueba que se inscribió un nuevo certificado con el valor Propósitos planteados de autenticación del servidor.  
  

  


