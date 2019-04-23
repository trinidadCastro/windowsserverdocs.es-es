---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Asistencia de acceso denegado de escenario
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829096"
---
# <a name="scenario-access-denied-assistance"></a>Escenario: Asistencia de acceso denegado

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los usuarios obtendrán un mensaje de acceso denegado cuando intenten acceder a los archivos y carpetas compartidos en un servidor de archivos para el que no tienen permisos. Con frecuencia, los administradores no tienen el contexto apropiado para solucionar los problemas de acceso, lo que hace más difícil la resolución del problema.  
  
## <a name="scenario-description"></a>Descripción del escenario  
Acceso denegado ayuda es una característica nueva en Windows Server 2012, que proporciona los siguientes métodos para solucionar problemas relacionados con el acceso a archivos y carpetas:  
  
-   **Autoasistencia.** Si un usuario puede determinar el problema y solucionarlo para obtener el acceso solicitado, el impacto para la empresa es bajo y no se necesitan excepciones especiales en la directiva de acceso central. La asistencia para acceso denegado proporciona un mensaje de acceso denegado que los administradores del servidor de archivos pueden personalizar con información específica de sus organizaciones. Por ejemplo, un administrador puede definir el mensaje para que los usuarios puedan solicitar acceso al propietario de los datos sin implicar al administrador del servidor de archivos.  
  
-   **Asistencia del propietario de datos.** Puedes definir una lista de distribución para carpetas compartidas y configurarla para que el propietario de la carpeta reciba una notificación por correo electrónico cuando un usuario necesite acceso. Si el propietario de los datos no sabe cómo ayudar al usuario a obtener acceso, puede reenviar esta información al administrador del servidor de archivos.  
  
-   **Asistencia del administrador del servidor de archivos.** Este tipo de asistencia está disponible cuando el usuario no puede corregir un problema y el propietario de los datos no puede ayudar.  Windows Server 2012 proporciona una interfaz de usuario donde los administradores del servidor de archivos pueden ver los permisos efectivos para un usuario en un archivo o carpeta para que resulte más fácil solucionar problemas de acceso.  
  
Asistencia de acceso denegado en Windows Server 2012 proporciona los detalles de acceso pertinente de los administradores del servidor de archivos para que pueden determinar el problema y las herramientas apropiadas para que se pueden realizar cambios de configuración para satisfacer la solicitud de acceso. Por ejemplo, un usuario podría seguir este proceso para acceder a un archivo al que actualmente no tiene acceso:  
  
-   El usuario intenta leer un archivo en el \\\financeshares carpeta compartida, pero el servidor muestra un mensaje acceso denegado.  
  
-    Windows Server 2012, se muestra la información de asistencia para acceso denegado al usuario con una opción para solicitar asistencia.  
  
-   Si el usuario solicita acceso al recurso, el servidor envía al propietario de la carpeta un correo electrónico con información sobre la solicitud de acceso.  
  
Encontrará información sobre planeación para configurar la asistencia para acceso denegado en [Plan for Access-Denied Assistance](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Puede encontrar pasos sobre cómo configurar la asistencia de acceso denegado en [asistencia para acceso denegado &#40;pasos de demostración&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>En este escenario  
Este escenario forma parte del escenario de control de acceso dinámico. Para obtener más información sobre el control de acceso dinámico, consulta:  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
Asistencia de acceso denegado en Windows Server 2012 contribuye al Control de acceso dinámico proporcionando a los usuarios la capacidad de solicitar acceso a archivos y carpetas compartidos directamente desde un mensaje acceso denegado.  
  
## <a name="BKMK_NEW"></a>Características incluidas en este escenario  
En la tabla siguiente, se enumeran las características que forman parte de este escenario y se describe la manera en que son compatibles con él.  
  
|Característica|Compatibilidad con este escenario|  
|-----------|---------------------------------|  
|[Información general del Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/hh831701.aspx)|La asistencia para acceso denegado se puede configurar en la consola del Administrador de recursos del servidor de archivos, en el servidor de archivos.|  
|[Información general de servicios de almacenamiento y de archivo](https://technet.microsoft.com/library/hh831487.aspx)|El Administrador de recursos del servidor de archivos es un servicio de rol de Servicios de archivos y almacenamiento, y está compuesto por un conjunto de características que se pueden usar para administrar los servidores de archivos de tu red.|  
  


