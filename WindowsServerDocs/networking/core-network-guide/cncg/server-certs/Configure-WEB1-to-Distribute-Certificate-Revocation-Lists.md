---
title: Configurar WEB1 para distribuir las listas de revocación de certificados (CRL)
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b96fad769638de9835c52137e5165fa32a6b9bd4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurar WEB1 para distribuir las listas de revocación de certificados (CRL)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para configurar el servidor web WEB1 para distribuir las CRL.  
  
En las extensiones de la CA de raíz, se ha dicho que la CRL de la CA de raíz sería disponible a través de http://pki.corp.contoso.com/pki. Actualmente, no hay un directorio virtual de infraestructura de clave pública en WEB1, por lo que se debe crear uno.  
  
Para realizar este procedimiento, debe ser miembro del **administradores de dominio**.  
  
> [!NOTE]  
> En el siguiente procedimiento, reemplaza el nombre de cuenta de usuario, el nombre del servidor Web, nombres de carpetas y ubicaciones y otros valores con los que son adecuados para la implementación.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Para configurar WEB1 para distribuir los certificados y las CRL  
  
1.  En WEB1, ejecutar Windows PowerShell como administrador, escribe `explorer c:\`, y, a continuación, presione ENTRAR. Abre el Explorador de Windows en la unidad C.   
  
2.  Crea una nueva carpeta denominada infraestructura de clave pública en la unidad C:. Para ello, haz clic en **Home**y, a continuación, haz clic en **nueva carpeta**. Se crea una nueva carpeta con el nombre temporal resaltado. Tipo **pki** y, a continuación, presione ENTRAR.  
  
3.  En el Explorador de Windows, haz clic en la carpeta recién creada, sitúa el cursor del mouse sobre **compartir con**y, a continuación, haz clic en **personas concretas**. La **uso compartido de archivos** abre el cuadro de diálogo.  
  
4.  En **uso compartido de archivos**, tipo **publicadores**y, a continuación, haz clic en **agregar**. Grupo Publicadores de certificados se agrega a la lista. En la lista, en **nivel de permiso**, haz clic en la flecha situada junto a **publicadores**y, a continuación, haz clic en **lectura y escritura**. Haz clic en **compartir**y, a continuación, haz clic en **listo**.  
  
5.  Cierra el Explorador de Windows.  
  
6.  Abre la consola IIS. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Internet Information Services (IIS) Manager**.  
  
7.  En el árbol de consola Internet Information Services (IIS) Manager, expande **WEB1**. Si ha sido invitado para comenzar con la plataforma Web de Microsoft, haz clic en **cancelar**.  
  
8.  Expande **sitios** y, a continuación, haz clic en el **sitio Web predeterminado** y, a continuación, haz clic en **Agregar directorio Virtual**.  
  
9. En **Alias**, tipo **pki**. En **ruta de acceso física** tipo **C:\pki**, a continuación, haz clic en **Aceptar**.  
  
10. Habilitar anónimo tener acceso al directorio virtual pki, para que cualquier cliente puede comprobar la validez de los certificados de entidad emisora de certificados y las CRL. Para ello:  
  
    1.  En la **conexiones** panel, asegúrate de que **pki** está seleccionado.  
  
    2.  En **principal de pki** haga clic en **autenticación**.  
  
    3.  En la **acciones** panel, haz clic en **Editar permisos**.  
  
    4.  En la **seguridad**, haga clic **editar**  
  
    5.  En la **permisos para que la pki** cuadro de diálogo, haz clic en **agregar**.  
  
    6.  En la **Seleccionar usuarios, equipos, cuentas de servicio o grupos**, tipo **ANONYMOUS LOGON; Todos los usuarios** y, a continuación, haz clic en **comprobar nombres**. Haz clic en **Aceptar**.  
  
    7.  Haz clic en **Aceptar** en la **Seleccionar usuarios, equipos, cuentas de servicio o grupos** cuadro de diálogo.  
  
    8.  Haz clic en **Aceptar** en la **permisos para que la pki** cuadro de diálogo.  
  
11. Haz clic en **Aceptar** en la **pki propiedades** cuadro de diálogo.  
  
12. En la **principal de pki** panel, haz doble clic en **solicitar filtrado**.  
  
13. La **extensiones de nombre de archivo** ficha está seleccionada de manera predeterminada en la **solicitar filtrado** panel. En la **acciones** panel, haz clic en **configuración de la característica Editar**.  
  
14. En **Editar configuración de filtrado solicitar**, selecciona **permitir el doble de escape** y, a continuación, haz clic en **Aceptar**.  
  
15. En el MMC del Administrador de Internet Information Services (IIS), haz clic en el nombre del servidor Web. Por ejemplo, si tu servidor Web se denomina WEB1, haz clic en **WEB1**.  
  
16. En **acciones**, haz clic en **reiniciar**. Servicios de Internet se detiene y, a continuación, reinicia.  
  

