---
description: Más información acerca de cómo configurar el DNS corporativo para el Servicio de federación y el DRS
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurar el DNS corporativo para los servicios de federación y DRS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: fab24c7abfc589ce68e8989f3ea4f4b601a173e5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050293"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar el DNS corporativo para los servicios de federación y DRS

## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Paso 6: agregar un \( registro de \) recursos CNAME y alias de host \( \) a DNS corporativo para el servicio de Federación y DRS
Debe agregar los siguientes registros de recursos al DNS del sistema de nombres de dominio corporativo \( \) para el servicio de Federación y el servicio de registro de dispositivos que configuró en los pasos anteriores.

|Entrada|Tipo|Dirección|
|---------|--------|-----------|
|\_nombre del servicio de Federación \_|Hospedar \( un\)|Dirección IP del servidor de AD FS o la dirección IP del equilibrador de carga que se configura delante de la granja de servidores de AD FS|
|enterpriseregistration|Alias \( CNAME\)|servidor de Federación \_ \_ Name.contoso.com|

Puede usar el siguiente procedimiento para agregar un host a \( \) y los registros de recursos CNAME de alias \( \) al DNS corporativo para el servidor de Federación y el servicio de registro de dispositivos.

La pertenencia al grupo **administradores**, o equivalente, es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para agregar un host \( a \) y alias \( CNAME \) registros de recursos en DNS para el servidor de Federación

1.  En el controlador de dominio, en Administrador del servidor, en el menú **herramientas** , haga clic en **DNS** para abrir el complemento DNS \- .

2.  En el árbol de consola, expanda el nodo **\_ \_ nombre del controlador de dominio** , expanda zonas de **búsqueda directa**, haga clic con el botón secundario \- en **\_ nombre de dominio** y, a continuación, haga clic en **nuevo host \( a o AAAA \)**.

3.  En el cuadro **nombre** , escriba el nombre que se usará para la granja de AD FS.

4.  En el cuadro **dirección IP** , escriba la dirección IP del servidor de Federación. Haz clic en **Agregar host**.

5.  \-Haga clic con el botón secundario en el nodo **\_ nombre de dominio** y, a continuación, haga clic en **nuevo alias \( CNAME \)**.

6.  En el cuadro de diálogo **Nuevo registro de recursos**, escribe **enterpriseregistration** en el cuadro **Nombre de alias**.

7.  En el cuadro de texto nombre \( de dominio completo FQDN \) del host de destino, escriba nombre de la **granja de servidores de federación \_ \_ \_ . dominio \_ Name.com** y, a continuación, haga clic en **Aceptar**.

    > [!IMPORTANT]
    > En una implementación real, si su empresa tiene varios sufijos UPN de nombre principal de usuario \( \) , debe crear varios registros CNAME para cada uno de esos sufijos UPN en DNS.

## <a name="see-also"></a>Consulte también

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guía de implementación de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)


