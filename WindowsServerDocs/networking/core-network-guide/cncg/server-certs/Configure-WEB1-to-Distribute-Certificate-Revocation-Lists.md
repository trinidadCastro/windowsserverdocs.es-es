---
title: Configurar WEB1 para distribuir listas de revocación de certificados (CRL)
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 756dcdc046387fc11774fe751bd8901498a19b0d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950211"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurar WEB1 para distribuir listas de revocación de certificados (CRL)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar el servidor Web WEB1 para distribuir las CRL.

En las extensiones de la CA raíz, se indicó que la CRL de la entidad de certificación raíz estaba disponible a través de https://pki.corp.contoso.com/pki . Actualmente, no hay un directorio virtual de PKI en WEB1, por lo que se debe crear uno.

Para llevar a cabo este procedimiento, debe ser miembro del **grupo Admins**. del dominio.

> [!NOTE]
> En el procedimiento siguiente, reemplace el nombre de la cuenta de usuario, el nombre del servidor Web, los nombres y las ubicaciones de las carpetas y otros valores por los que sean adecuados para su implementación.

#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Para configurar WEB1 para distribuir certificados y CRL

1.  En WEB1, ejecute Windows PowerShell como administrador, escriba `explorer c:\` y, a continuación, presione Entrar. El explorador de Windows se abre para la unidad C.

2.  Cree una nueva carpeta denominada PKI en la unidad C:. Para ello, haga clic en **Inicio** y, a continuación, haga clic en **nueva carpeta**. Se crea una nueva carpeta con el nombre temporal resaltado. Escriba **PKI** y, a continuación, presione Entrar.

3.  En el explorador de Windows, haga clic con el botón secundario en la carpeta que acaba de crear, mantenga el cursor del mouse sobre **compartir con** y, a continuación, haga clic en **personas específicas**. Se abre el cuadro de diálogo **Archivos compartidos**.

4.  En **uso compartido de archivos**, escriba **publicadores de certificados** y, a continuación, haga clic en **Agregar**. El grupo Publicadores de certificados se agrega a la lista. En el **nivel de permisos** de la lista, haga clic en la flecha situada junto a **publicadores de certificados** y, a continuación, haga clic en **lectura/escritura**. Haga clic en **compartir** y, a continuación, en **listo**.

5.  Cierre el Explorador de Windows.

6.  Abra la consola de IIS. En Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Administrador de Internet Information Services (IIS)**.

7.  En el árbol de la consola del administrador de Internet Information Services (IIS), expanda **web1**. Si se le invita a ver una introducción a la Plataforma web de Microsoft, haga clic en **Cancelar**.

8.  Expanda **Sitios** y haga clic con el botón secundario en **Default Web Site**; a continuación, haga clic en **Agregar directorio virtual**.

9. En **alias**, escriba **PKI**. En **ruta de acceso física** , escriba **C:\pki** y haga clic en **Aceptar**.

10. Habilite el acceso anónimo al directorio virtual de PKI para que cualquier cliente pueda comprobar la validez de los certificados y las CRL de la entidad de certificación. Para ello:

    1.  En el panel **Conexiones**, asegúrese de que la opción **pki** esté activada.

    2.  En **Página principal de pki**, haga clic en **Autenticación**.

    3.  En el panel **Acciones**, haga clic en **Editar permisos**.

    4.  En la pestaña **Seguridad**, haga clic en **Editar**.

    5.  En el cuadro de diálogo **Permisos de pki**, haga clic en **Agregar**.

    6.  En **Seleccionar usuarios, equipos, cuentas de servicio o grupos**, escriba **Inicio de sesión anónimo; Todos** y, a continuación, haga clic en **Comprobar nombres**. Haga clic en **Aceptar**.

    7.  Haga clic en **Aceptar** en el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** .

    8.  Haga clic en **Aceptar** en el cuadro de diálogo **permisos de PKI** .

11. Haga clic en **Aceptar** en el cuadro de diálogo **propiedades de PKI** .

12. En el panel **Página principal de pki**, haga doble clic en **Filtrado de solicitudes**.

13. La pestaña **Extensiones de nombre de archivo** se selecciona de manera predeterminada en el panel **Filtrado de solicitudes**. En el panel **Acciones**, haga clic en **Modificar configuración de característica**.

14. En **Modificar configuración del filtrado de solicitudes**, active **Permitir doble escape** y, a continuación, haga clic en **Aceptar**.

15. En el MMC del administrador de Internet Information Services (IIS), haga clic en el nombre del servidor Web. Por ejemplo, si el servidor Web se denomina WEB1, haga clic en **web1**.

16. En **acciones**, haga clic en **reiniciar**. Los servicios de Internet se detienen y se reinician.


