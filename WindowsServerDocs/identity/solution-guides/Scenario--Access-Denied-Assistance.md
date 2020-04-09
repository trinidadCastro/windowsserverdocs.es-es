---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Escenario de asistencia para acceso denegado
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a6418f7ac317f060adb72f32e231e1577a5f8b92
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861148"
---
# <a name="scenario-access-denied-assistance"></a>Escenario: Asistencia de acceso denegado

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los usuarios obtendrán un mensaje de acceso denegado cuando intenten acceder a los archivos y carpetas compartidos en un servidor de archivos para el que no tienen permisos. Con frecuencia, los administradores no tienen el contexto apropiado para solucionar los problemas de acceso, lo que hace más difícil la resolución del problema.  
  
## <a name="scenario-description"></a>Descripción del escenario  
La asistencia para acceso denegado es una nueva característica de Windows Server 2012, que proporciona las siguientes formas de solucionar problemas relacionados con el acceso a archivos y carpetas:  
  
-   **Autoasistencia.** Si un usuario puede determinar el problema y solucionarlo para obtener el acceso solicitado, el impacto para la empresa es bajo y no se necesitan excepciones especiales en la directiva de acceso central. La asistencia para acceso denegado proporciona un mensaje de acceso denegado que los administradores del servidor de archivos pueden personalizar con información específica de sus organizaciones. Por ejemplo, un administrador puede definir el mensaje para que los usuarios puedan solicitar acceso al propietario de los datos sin implicar al administrador del servidor de archivos.  
  
-   **Asistencia del propietario de datos.** Puedes definir una lista de distribución para carpetas compartidas y configurarla para que el propietario de la carpeta reciba una notificación por correo electrónico cuando un usuario necesite acceso. Si el propietario de los datos no sabe cómo ayudar al usuario a obtener acceso, puede reenviar esta información al administrador del servidor de archivos.  
  
-   **Asistencia del administrador del servidor de archivos.** Este tipo de asistencia está disponible cuando el usuario no puede corregir un problema y el propietario de los datos no puede ayudar.  Windows Server 2012 proporciona una interfaz de usuario en la que los administradores del servidor de archivos pueden ver los permisos vigentes de un usuario en un archivo o carpeta para que resulte más fácil solucionar problemas de acceso.  
  
La asistencia para acceso denegado en Windows Server 2012 proporciona a los administradores del servidor de archivos los detalles de acceso pertinentes para que puedan determinar el problema y las herramientas apropiadas para que puedan realizar cambios en la configuración para satisfacer la solicitud de acceso. Por ejemplo, un usuario podría seguir este proceso para acceder a un archivo al que actualmente no tiene acceso:  
  
-   El usuario intenta leer un archivo en la carpeta compartida \\\financeshares, pero el servidor muestra un mensaje de acceso denegado.  
  
-    Windows Server 2012 muestra al usuario la información de asistencia para acceso denegado con una opción para solicitar asistencia.  
  
-   Si el usuario solicita acceso al recurso, el servidor envía al propietario de la carpeta un correo electrónico con información sobre la solicitud de acceso.  
  
Encontrarás información sobre planeación para configurar la asistencia para acceso denegado en [Planificar la asistencia para acceso denegado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Puede encontrar pasos sobre la configuración de la asistencia para acceso denegado en [pasos &#40;&#41;de demostración de la asistencia para acceso denegado](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>En este escenario  
Este escenario forma parte del escenario de control de acceso dinámico. Para obtener más información sobre el control de acceso dinámico, consulta:  
  
-   [Access Control dinámico: información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
La asistencia para acceso denegado en Windows Server 2012 contribuye a la Access Control dinámica, ya que permite a los usuarios solicitar acceso a archivos y carpetas compartidos directamente desde un mensaje de acceso denegado.  
  
## <a name="features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Características incluidas en este escenario  
En la tabla siguiente, se enumeran las características que forman parte de este escenario y se describe la manera en que son compatibles con él.  
  
|Característica|Compatibilidad con este escenario|  
|-----------|---------------------------------|  
|[Información general de Administrador de recursos de servidor de archivos](https://technet.microsoft.com/library/hh831701.aspx)|La asistencia para acceso denegado se puede configurar en la consola del Administrador de recursos del servidor de archivos, en el servidor de archivos.|  
|[Introducción a los servicios de archivos y almacenamiento](https://technet.microsoft.com/library/hh831487.aspx)|El Administrador de recursos del servidor de archivos es un servicio de rol de Servicios de archivos y almacenamiento, y está compuesto por un conjunto de características que se pueden usar para administrar los servidores de archivos de tu red.|  
  


