---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuir certificados a los equipos cliente mediante directiva de grupo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c0e1ab21faa42789bee20c7b8945284aba068310
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855448"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuir certificados a los equipos cliente mediante directiva de grupo


Puede usar el siguiente procedimiento para desplazar el Capa de sockets seguros adecuado \(certificados\) SSL \(o certificados equivalentes que encadenan a un\) raíz de confianza para los servidores de Federación de cuenta, los servidores de Federación de recursos y los servidores web a cada equipo cliente del bosque del asociado de cuenta mediante directiva de grupo.  
  
Para completar este procedimiento, lo mínimo que se necesita es pertenecer al grupo Admins. del **dominio** o **administradores de organización**, o equivalente, en Active Directory Domain Services \(AD DS\).  Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir certificados a los equipos cliente mediante directiva de grupo  
  
1.  En un controlador de dominio del bosque de la organización del asociado de cuenta, inicie el\-**Directiva de grupo de administración** en.  
  
2.  Busque un objeto de directiva de grupo existente \(GPO\) o cree un nuevo GPO para que contenga la configuración del certificado. Asegúrese de que el GPO está asociado con el dominio, sitio o unidad organizativa \(unidad organizativa\) donde residen las cuentas de usuario y equipo adecuadas.  
  
3.  \-haga clic en el GPO y, a continuación, haga clic en **Editar**.  
  
4.  En el árbol de consola, Abra **configuración del equipo\\directivas\\configuración de Windows\\configuración de seguridad\\directivas de clave pública**, haga clic con el botón secundario\-en **entidades de certificación raíz de confianza**y, a continuación, haga clic en **importar**.  
  
5.  En la página **Éste es el Asistente para importación de certificados**, haga clic en **Siguiente**.  
  
6.  En la página **archivo para importar** , escriba la ruta de acceso a los archivos de certificado adecuados \(por ejemplo, \\\\FS1\\c $\\FS1. cer\)y, a continuación, haga clic en **siguiente**.  
  
7.  En la página **almacén de certificados** , haga clic en **colocar todos los certificados en el siguiente almacén**y, a continuación, haga clic en **siguiente**.  
  
8.  En la página **finalización del Asistente para importación de certificados** , compruebe que la información proporcionada sea correcta y, a continuación, haga clic en **Finalizar**.  
  
9. Repita los pasos del 2 al 6 para agregar certificados adicionales para cada uno de los servidores de Federación de la granja.  
