---
title: Paso 5 configurar DC1
description: Obtenga información sobre cómo configurar la puerta de enlace predeterminada en el controlador de dominio, crear grupos de seguridad para clientes de DirectAccess de Windows 7 en DC1 y agregar un nuevo sitio AD DS.
manager: brianlic
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 9939cb397fa7c5386e4058a834a205afaa99cd9f
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040485"
---
# <a name="step-5-configure-dc1"></a>Paso 5 configurar DC1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

DC1 actúa como controlador de dominio, servidor DNS y servidor DHCP para el dominio corp.contoso.com.

Para configurar el acceso remoto para que use una topología multisitio, es necesario agregar un sitio Active Directory Domain Services (AD DS) adicional para el segundo controlador de dominio 2-DC1 y para configurar el enrutamiento entre las subredes.

1. Para configurar la puerta de enlace predeterminada en el controlador de dominio. Configurar la puerta de enlace predeterminada en DC1.

2. Cree grupos de seguridad para clientes de DirectAccess de Windows 7 en DC1. Cuando se configura DirectAccess, crea automáticamente objetos de directiva de grupo (GPO) y valores de GPO que se aplican a los clientes y servidores de DirectAccess. El GPO de cliente de DirectAccess se aplica a grupos de seguridad de Active Directory específicos.

3. Para agregar un nuevo sitio AD DS. Cree un segundo sitio AD DS.

## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>Para configurar la puerta de enlace predeterminada en el controlador de dominio

1.  En la consola de Administrador del servidor, haga clic en **servidor local** y, a continuación, en el área **propiedades** , junto a **conexión cableada Ethernet**, haga clic en el vínculo.

2.  En la ventana conexiones de red, haga clic con el botón secundario en **conexión cableada Ethernet** y, a continuación, haga clic en **propiedades**.

3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.

4.  En **puerta de enlace predeterminada**, escriba **10.0.0.254** y, en **servidor DNS alternativo**, escriba **10.2.0.1** y, a continuación, haga clic en **Aceptar**.

5.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.

6.  En **puerta de enlace predeterminada**, escriba **2001: db8:1:: fe** y, en **servidor DNS alternativo**, escriba **2001: db8:2:: 1** y, a continuación, haga clic en **Aceptar**.

7.  En el cuadro de diálogo **propiedades de conexión cableada Ethernet** , haga clic en **cerrar**.

8.  Cierre la ventana **Conexiones de red**.

## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>Crear grupos de seguridad para clientes de DirectAccess de Windows 7 en DC1
Cree los grupos de seguridad de DirectAccess para Windows 7 con el procedimiento siguiente.

 Los equipos cliente de Windows 7 deben ser miembros de grupos de seguridad independientes, ya que pueden conectarse a recursos internos a través de un solo punto de entrada. Al habilitar la compatibilidad con varios sitios o agregar puntos de entrada, si se solicita compatibilidad con Windows 7, DirectAccess creará automáticamente un GPO independiente para los clientes de Windows 7 para cada punto de entrada.

### <a name="create-security-groups"></a>Crear grupos de seguridad

1.  En la pantalla **Inicio** , escriba **DSA. msc** y, a continuación, presione Entrar.

2.  En el panel izquierdo, expanda **Corp.contoso.com**, haga clic en **usuarios**, haga clic con el botón secundario en **usuarios**, seleccione **nuevo** y, a continuación, haga clic en **Grupo**.

3.  En el cuadro de diálogo **nuevo objeto-grupo** , en **nombre de grupo**, escriba **Win7_Clients_Site1**.

4.  En **Ámbito de grupo** haz clic en **Global**, en **Tipo de grupo** haz clic en **Seguridad** y, a continuación, haz clic en **Aceptar**.

5.  Haga doble clic en el grupo de seguridad **Win7_Clients_Site1** y, en el cuadro de diálogo **propiedades de Win7_Clients_Site1** , haga clic en la pestaña **miembros** .

6.  En la pestaña **Miembros**, haga clic en **Agregar**.

7.  En el cuadro de diálogo **Seleccionar usuarios, contactos, equipos o cuentas de servicio** , haga clic en **tipos de objeto**. En el cuadro de diálogo **tipos de objeto** , seleccione **equipos** y, a continuación, haga clic en **Aceptar**.

8.  En **Escriba los nombres de objeto que desea seleccionar**, escriba **cliente2** y, a continuación, haga clic en **Aceptar** y, a continuación, en el cuadro de diálogo **propiedades de Win7_Clients_Site1** , haga clic en **Aceptar**.

9. En la consola de **Active Directory usuarios y equipos** , en el panel izquierdo, haga clic con el botón secundario en **usuarios**, seleccione **nuevo** y, a continuación, haga clic en **Grupo**.

10. En el cuadro de diálogo **nuevo objeto-grupo** , en **nombre de grupo**, escriba **Win7_Clients_Site2**.

11. En **Ámbito de grupo** haz clic en **Global**, en **Tipo de grupo** haz clic en **Seguridad** y, a continuación, haz clic en **Aceptar**.

12. Cierre la consola de **Usuarios y equipos de Active Directory**.

## <a name="to-add-a-new-ad-ds-site"></a>Para agregar un nuevo sitio AD DS

1.  En la pantalla **Inicio** , escriba **Dssite. msc** y, a continuación, presione Entrar.

2.  En la consola sitios y servicios de Active Directory, en el árbol de consola, haga clic con el botón secundario en **sitios** y, a continuación, haga clic en **nuevo sitio**.

3.  En el cuadro de diálogo **nuevo objeto-sitio** , en el cuadro **nombre** , escriba **segundo-sitio**.

4.  En el cuadro de lista, haga clic en **DEFAULTIPSITELINK** y, a continuación, haga clic en **Aceptar** dos veces.

5.  En el árbol de consola, expanda **sitios**, haga clic con el botón secundario en **subredes** y, a continuación, haga clic en **nueva subred**.

6.  En el cuadro de diálogo **nuevo objeto-subred** , en **prefijo**, escriba **10.0.0.0/24**, en la lista **Seleccione un objeto de sitio para este prefijo** , haga clic en **Default-First-Site-Name** y, a continuación, haga clic en **Aceptar**.

7.  En el árbol de consola, haga clic con el botón secundario en **subredes** y, a continuación, haga clic en **nueva subred**.

8.  En el cuadro de diálogo **nuevo objeto-subred** , en **prefijo**, escriba **2001: db8:1::/64**, en la lista **Seleccione un objeto de sitio para este prefijo** , haga clic en **Default-First-Site-Name** y, a continuación, haga clic en **Aceptar**.

9. En el árbol de consola, haga clic con el botón secundario en **subredes** y, a continuación, haga clic en **nueva subred**.

10. En el cuadro de diálogo **nuevo objeto-subred** , en **prefijo**, escriba **10.2.0.0/24**, en la lista **Seleccione un objeto de sitio para este prefijo** , haga clic en **segundo sitio** y, a continuación, haga clic en **Aceptar**.

11. En el árbol de consola, haga clic con el botón secundario en **subredes** y, a continuación, haga clic en **nueva subred**.

12. En el cuadro de diálogo **nuevo objeto-subred** , en **prefijo**, escriba **2001: db8:2::/64**, en la lista **Seleccione un objeto de sitio para este prefijo** , haga clic en **segundo sitio** y, a continuación, haga clic en **Aceptar**.

13. Cierre Sitios y servicios de Active Directory.



