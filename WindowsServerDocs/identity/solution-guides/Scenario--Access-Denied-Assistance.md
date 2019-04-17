---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Asistencia deniega el acceso de escenario
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-access-denied-assistance"></a>Escenario: Acceso denegado asistencia

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los usuarios recibirán un mensaje de acceso denegado cuando intentan acceder a archivos y carpetas en un servidor de archivos para que no tienen permisos compartidos. Los administradores a menudo no tienen el contexto adecuado para solucionar el problema de acceso, que hace más difícil resolver el problema.  
  
## <a name="scenario-description"></a>Descripción del escenario  
Acceso denegado asistencia es una nueva característica de Windows Server 2012, que proporciona los siguientes métodos para solucionar problemas relacionados con acceso a archivos y carpetas:  
  
-   **Asistencia automática.** Si un usuario puede determinar el problema y corregir el problema para que pueda obtener el acceso solicitado, el impacto en el negocio es bajo y ninguna excepción especial es necesarios en la directiva de acceso central. Acceso denegado asistencia proporciona un mensaje de acceso denegado que los administradores de servidor de archivos pueden personalizar con información específica para sus organizaciones. Por ejemplo, un administrador puede establecer el mensaje para que los usuarios pueden solicitar acceso desde un propietario de datos sin relacionado con el administrador del servidor de archivos.  
  
-   **Asistencia por el propietario de datos.** Puedes definir una lista de distribución de carpetas compartidas y configurarlo para que el propietario de la carpeta recibe una notificación por correo electrónico cuando un usuario necesita acceder. Si el propietario de los datos no sabe cómo ayudar al usuario obtener acceso, el propietario puede reenviar esta información al administrador del servidor de archivos.  
  
-   **Asistencia por el administrador del servidor de archivos.** Este tipo de asistencia está disponible cuando el usuario no puede resolver un problema y no puede ayudar al propietario de datos.  Windows Server 2012 proporciona una interfaz de usuario donde los administradores de servidor de archivos pueden ver los permisos eficaces para un usuario en un archivo o carpeta para que sea más fácil solucionar problemas de acceso.  
  
Acceso denegado asistencia en Windows Server 2012 proporciona los detalles relevantes de acceso de administradores de servidor de archivos para que pueden determinar el problema y las herramientas adecuadas para que puedan realizar cambios en la configuración para satisfacer la solicitud de acceso. Por ejemplo, un usuario puede seguir este proceso para tener acceso a un archivo que actualmente no tienen acceso a:  
  
-   El usuario intenta leer un archivo en la carpeta compartida de \\\financeshares, pero el servidor muestra un mensaje de acceso denegado.  
  
-    Windows Server 2012, se muestra la información de Ayuda de acceso denegado al usuario una opción para solicitar asistencia.  
  
-   Si el usuario solicita acceso al recurso, el servidor envía un correo electrónico con la información de solicitud de acceso al propietario de la carpeta.  
  
Puedes encontrar información de planificación de la configuración de acceso denegado asistencia en [planear Access-Denied asistencia](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Puedes encontrar pasos sobre la configuración de acceso denegado asistencia en [Deploy Access-Denied asistencia & #40; pasos de demostración & #41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>En este escenario  
Este escenario es parte del escenario de Control de acceso dinámico. Para obtener más información sobre el Control de acceso dinámico, consulta:  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
Acceso denegado asistencia en Windows Server 2012 contribuye al Control de acceso dinámico ofreciéndoles a los usuarios la capacidad de solicitar acceso a archivos y carpetas compartidos directamente desde un mensaje de acceso denegado.  
  
## <a name="BKMK_NEW"></a>Características incluidas en este escenario  
La siguiente tabla enumera las características que forman parte de este escenario y describe cómo apoyan.  
  
|Característica|¿Cómo admite este escenario|  
|-----------|---------------------------------|  
|[Información general del Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/hh831701.aspx)|Acceso denegado asistencia puede configurarse mediante la consola de administrador de recursos del servidor de archivos en el servidor de archivos.|  
|[Información general de servicios de almacenamiento y de archivo](https://technet.microsoft.com/library/hh831487.aspx)|Administrador de recursos del servidor de archivos es un servicio de rol de servicios de archivos y almacenamiento y consta de un conjunto de características que pueden usarse para administrar los servidores de archivos de la red.|  
  


