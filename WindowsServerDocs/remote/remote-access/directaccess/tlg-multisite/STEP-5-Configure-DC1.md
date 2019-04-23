---
title: 'PASO 5: configurar DC1'
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7f907c3bf463e3a90d413e5b167a70051057f06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876776"
---
# <a name="step-5-configure-dc1"></a>PASO 5: configurar DC1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

DC1 actúa como un controlador de dominio, servidor DNS y servidor DHCP para el dominio corp.contoso.com.  
  
Para configurar el acceso remoto para usar una topología de multisitio, es necesario para agregar un sitio adicional de los servicios de dominio de Active Directory (AD DS) para el segundo 2 controlador de dominio-DC1 y para configurar el enrutamiento entre subredes.  
  
1. Para configurar la puerta de enlace predeterminada en el controlador de dominio. Configurar la puerta de enlace predeterminada en DC1.  
  
2. Crear grupos de seguridad de Windows 7 para clientes de DirectAccess en DC1. Cuando se configura DirectAccess, crea automáticamente objetos de directiva de grupo (GPO) y la configuración de GPO que se aplica a los clientes de DirectAccess y servidores. Se aplica el GPO de cliente de DirectAccess a grupos de seguridad de Active Directory específicos.  
  
3. Para agregar un nuevo sitio de AD DS. Cree un segundo sitio AD DS.  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>Para configurar la puerta de enlace predeterminada en el controlador de dominio  
  
1.  En la consola de administrador del servidor, haga clic en **servidor Local**y, a continuación, en el **propiedades** área, junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En la ventana Conexiones de red, haga clic en **conexión cableada Ethernet**y, a continuación, haga clic en **propiedades**.  
  
3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
4.  En **puerta de enlace predeterminada**, tipo **10.0.0.254**y en **servidor DNS alternativo**, tipo **10.2.0.1**y, a continuación, haga clic en **Aceptar** .  
  
5.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
6.  En **puerta de enlace predeterminada**, tipo **2001:db8:1::fe**y en **servidor DNS alternativo**, tipo **2001:db8:2::1**y, a continuación, haga clic en **Aceptar**.  
  
7.  En el **propiedades de conexión cableada Ethernet** cuadro de diálogo, haga clic en **cerrar**.  
  
8.  Cierre la ventana **Conexiones de red**.  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>Crear grupos de seguridad de Windows 7 para clientes de DirectAccess en DC1  
Cree los grupos de seguridad de DirectAccess para Windows 7 con el procedimiento siguiente.  
  
 Los equipos cliente de Windows 7 deben ser miembros de grupos de seguridad independientes porque son capaz de conectarse a los recursos internos a través de un único punto de entrada solo. Al habilitar la compatibilidad con multisitio, o agregando la entrada puntos, si se solicita el soporte técnico para Windows 7, a continuación, un GPO independiente se creará automáticamente los clientes de DirectAccess para Windows 7 para cada punto de entrada.  
  
### <a name="create-security-groups"></a>Crear grupos de seguridad  
  
1.  En el **iniciar** , escriba**dsa.msc**, y, a continuación, presione ENTRAR.  
  
2.  En el panel izquierdo, expanda **corp.contoso.com**, haga clic en **usuarios**, a continuación, haga clic en **usuarios**, apunte a **New**y, a continuación, haga clic en **Grupo**.  
  
3.  En el **nuevo objeto - grupo** cuadro de diálogo **conmutaciónporerror**, escriba **Win7_Clients_Site1**.  
  
4.  En **Ámbito de grupo** haz clic en **Global**, en **Tipo de grupo** haz clic en **Seguridad** y, a continuación, haz clic en **Aceptar**.  
  
5.  Haga doble clic en el **Win7_Clients_Site1** grupo de seguridad y en el **Win7_Clients_Site1 propiedades** cuadro de diálogo, haga clic en el **miembros** ficha.  
  
6.  En la pestaña **Miembros** , haga clic en **Agregar**.  
  
7.  En el **Seleccionar usuarios, contactos, equipos o cuentas de servicio** cuadro de diálogo, haga clic en **tipos de objeto**. En el **tipos de objeto** cuadro de diálogo, seleccione **equipos**y, a continuación, haga clic en **Aceptar**.  
  
8.  En **escriba los nombres de objeto para seleccionar**, tipo **client2**y, a continuación, haga clic en **Aceptar**y, a continuación, en el **Win7_Clients_Site1 propiedades** cuadro de diálogo clic en el cuadro **Aceptar**.  
  
9. En el **equipos y usuarios de Active Directory** consola, en el panel izquierdo, haga clic en **usuarios**, apunte a **New**y, a continuación, haga clic en **grupo** .  
  
10. En el **nuevo objeto - grupo** cuadro de diálogo **conmutaciónporerror**, escriba **Win7_Clients_Site2**.  
  
11. En **Ámbito de grupo** haz clic en **Global**, en **Tipo de grupo** haz clic en **Seguridad** y, a continuación, haz clic en **Aceptar**.  
  
12. Cierre la consola de **Usuarios y equipos de Active Directory**.  
  
## <a name="to-add-a-new-ad-ds-site"></a>Para agregar un nuevo sitio de AD DS  
  
1.  En el **iniciar** , escriba**dssite.msc**, y, a continuación, presione ENTRAR.  
  
2.  En la consola de servicios y sitios de Active Directory, en el árbol de consola, haga clic en **sitios**y, a continuación, haga clic en **nuevo sitio**.  
  
3.  En el **nuevo objeto - sitio** cuadro de diálogo el **nombre** , escriba **segundo sitio**.  
  
4.  En el cuadro de lista, haga clic en **DEFAULTIPSITELINK**y, a continuación, haga clic en **Aceptar** dos veces.  
  
5.  En el árbol de consola, expanda **sitios**, haga clic en **subredes**y, a continuación, haga clic en **nueva subred**.  
  
6.  En el **nuevo objeto - subred** cuadro de diálogo **prefijo**, tipo **10.0.0.0/24**, en el **seleccionar un objeto de sitio para este prefijo** lista, haga clic en **Default-First-Site-Name**y, a continuación, haga clic en **Aceptar**.  
  
7.  En el árbol de consola, haga clic en **subredes**y, a continuación, haga clic en **nueva subred**.  
  
8.  En el **nuevo objeto - subred** cuadro de diálogo **prefijo**, tipo **2001:db8:1:: / 64**, en el **seleccionar un objeto de sitio para este prefijo** lista, Haga clic en **Default-First-Site-Name**y, a continuación, haga clic en **Aceptar**.  
  
9. En el árbol de consola, haga clic en **subredes**y, a continuación, haga clic en **nueva subred**.  
  
10. En el **nuevo objeto - subred** cuadro de diálogo **prefijo**, tipo **10.2.0.0/24**, en el **seleccionar un objeto de sitio para este prefijo** lista, haga clic en **Segundo sitio**y, a continuación, haga clic en **Aceptar**.  
  
11. En el árbol de consola, haga clic en **subredes**y, a continuación, haga clic en **nueva subred**.  
  
12. En el **nuevo objeto - subred** cuadro de diálogo **prefijo**, tipo **2001:db8:2:: / 64**, en el **seleccionar un objeto de sitio para este prefijo** lista, Haga clic en **segundo sitio**y, a continuación, haga clic en **Aceptar**.  
  
13. Cierre los servicios y sitios de Active Directory.  
  


