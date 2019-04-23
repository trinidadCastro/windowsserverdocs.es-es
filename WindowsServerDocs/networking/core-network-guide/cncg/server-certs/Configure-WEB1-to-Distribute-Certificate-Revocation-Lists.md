---
title: Configurar WEB1 para distribuir listas de revocación de certificados (CRL)
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 57fa45eff87a1f0cdaae8b780d7f605e54ff6871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839196"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurar WEB1 para distribuir listas de revocación de certificados (CRL)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar el servidor web WEB1 para distribuir las CRL.  
  
En las extensiones de la entidad emisora raíz, se ha mencionado que la CRL de la entidad de certificación raíz estaría disponible a través de https://pki.corp.contoso.com/pki. Actualmente, no hay un directorio virtual PKI en WEB1, por lo que debe crearse uno.  
  
Para llevar a cabo este procedimiento, debe ser miembro de **Admins. del dominio**.  
  
> [!NOTE]  
> En el procedimiento siguiente, reemplace el nombre de cuenta de usuario, el nombre del servidor Web, los nombres de carpetas y ubicaciones y otros valores con aquéllos que sean adecuados para su implementación.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Para configurar WEB1 para distribuir certificados y CRL  
  
1.  En WEB1, ejecute Windows PowerShell como administrador, escriba `explorer c:\`, y, a continuación, presione ENTRAR. Se abre el Explorador de Windows en la unidad C.   
  
2.  Cree una carpeta nueva denominada PKI en la unidad C:. Para ello, haga clic en **inicio**y, a continuación, haga clic en **nueva carpeta**. Se crea una nueva carpeta con el nombre temporal resaltado. Tipo **pki** y, a continuación, presione ENTRAR.  
  
3.  En el Explorador de Windows, haga clic en la carpeta que acaba de crear, sitúe el cursor del mouse sobre **compartir con**y, a continuación, haga clic en **personas específicas**. Se abre el cuadro de diálogo **Archivos compartidos**.  
  
4.  En **uso compartido de archivos**, tipo **publicadores de certificados**y, a continuación, haga clic en **agregar**. El grupo de publicadores de certificados se agrega a la lista. En la lista, en **nivel de permiso**, haga clic en la flecha situada junto a **publicadores de certificados**y, a continuación, haga clic en **lectura/escritura**. Haga clic en **Share**y, a continuación, haga clic en **realiza**.  
  
5.  Cierre el Explorador de Windows.  
  
6.  Abra la consola de IIS. En Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Administrador de Internet Information Services (IIS)**.  
  
7.  En el árbol de consola de administrador de Internet Information Services (IIS), expanda **WEB1**. Si se le invita a ver una introducción a la Plataforma web de Microsoft, haga clic en **Cancelar**.  
  
8.  Expanda **Sitios** y haga clic con el botón secundario en **Sitio web predeterminado**; a continuación, haga clic en **Agregar directorio virtual**.  
  
9. En **Alias**, tipo **pki**. En **ruta de acceso física** tipo **C:\pki**, a continuación, haga clic en **Aceptar**.  
  
10. Habilitar Anonymous tener acceso al directorio virtual pki, para que cualquier cliente pueda comprobar la validez de los certificados de CA y CRL. Para ello:  
  
    1.  En el panel **Conexiones**, asegúrese de que la opción **pki** esté activada.  
  
    2.  En **Página principal de pki**, haga clic en **Autenticación**.  
  
    3.  En el panel **Acciones**, haga clic en **Editar permisos**.  
  
    4.  En la pestaña **Seguridad**, haga clic en **Editar**.  
  
    5.  En el cuadro de diálogo **Permisos de pki** , haga clic en **Agregar**.  
  
    6.  En el **Seleccionar usuarios, equipos, cuentas de servicio o grupos**, tipo **inicio de sesión anónimo; Todo el mundo** y, a continuación, haga clic en **comprobar nombres**. Haga clic en **Aceptar**.  
  
    7.  Haga clic en **Aceptar** en el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** cuadro de diálogo.  
  
    8.  Haga clic en **Aceptar** en el **permisos de pki** cuadro de diálogo.  
  
11. Haga clic en **Aceptar** en el **pki propiedades** cuadro de diálogo.  
  
12. En el panel **Página principal de pki** , haga doble clic en **Filtrado de solicitudes**.  
  
13. La pestaña **Extensiones de nombre de archivo** se selecciona de manera predeterminada en el panel **Filtrado de solicitudes**. En el panel **Acciones** , haga clic en **Modificar configuración de característica**.  
  
14. En **Modificar configuración del filtrado de solicitudes**, active **Permitir doble escape** y, a continuación, haga clic en **Aceptar**.  
  
15. En MMC del Administrador de Internet Information Services (IIS), haga clic en el nombre del servidor Web. Por ejemplo, si el servidor Web se denomina WEB1, haga clic en **WEB1**.  
  
16. En **acciones**, haga clic en **reiniciar**. Servicios de Internet se detienen y se reinicia a continuación.  
  

