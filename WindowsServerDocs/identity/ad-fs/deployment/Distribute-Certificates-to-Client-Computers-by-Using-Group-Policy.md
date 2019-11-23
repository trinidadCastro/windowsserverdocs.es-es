---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuir certificados a los equipos cliente mediante directiva de grupo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1d9692bc099174f15b77e792087f4c7055bf85d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359624"
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
