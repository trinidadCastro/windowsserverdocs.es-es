---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: Mantenimiento de un entorno más seguro
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 554f841473d79edbac19ce4c53e99f5a43fa2b23
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821706"
---
# <a name="maintaining-a-more-secure-environment"></a>Mantenimiento de un entorno más seguro

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Ley número diez: Tecnología no es una panacea.* - [10 leyes inmutables de la administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Cuando haya creado un entorno manejable y seguro para sus recursos críticos del negocio, debe cambiar su enfoque para asegurarse de que se mantiene segura. Aunque se le haya concedido controles técnicos específicos para aumentar la seguridad de sus instalaciones de AD DS, tecnología por sí sola no protegerá un entorno donde no funciona en colaboración con la empresa para mantener una infraestructura segura y utilizable. Las recomendaciones de alto niveles en esta sección están diseñadas para usarse como instrucciones que puede usar para desarrollar una seguridad eficaz, pero no solo administración efectiva del ciclo de vida.  
  
En algunos casos, su organización de TI podría tener ya una estrecha relación laboral con unidades de negocio, lo que facilitará la implementación de estas recomendaciones. En organizaciones donde y unidades de negocio no están estrechamente relacionadas, deberá primero obtener el patrocinio ejecutivo para los esfuerzos para falsificar una relación más cercana entre TI y las unidades de negocio. El [resumen ejecutivo](../../../ad-ds/manage/component-updates/Executive-Summary.md) está pensado para ser útil como un documento independiente para su revisión ejecutivo y pueda transmitirse a la toma de decisiones en su organización.  
  
## <a name="creating-business-centric-security-practices-for-active-directory"></a>Creación de procedimientos recomendados de seguridad centrada en la empresa para Active Directory  
En el pasado, la tecnología de la información en muchas organizaciones se vio como una estructura de soporte técnico y un centro de costo. Los departamentos de TI a menudo en gran medida estaban separados a los usuarios empresariales y las interacciones limitadas a un modelo de solicitud-respuesta en el que el negocio recursos solicitados y TI respondido.  
  
Como la tecnología ha evolucionado y proliferado, la visión de "un equipo en todos los escritorios" tiene eficazmente vienen a pasar durante gran parte del mundo e incluso ha eclipsada por la amplia gama de tecnologías accesibles fácilmente disponibles hoy en día. Tecnología de la información ya no es una función de soporte técnico, es una función empresarial central. Si su organización no ha podido continuar funcionando si todos los servicios de TI no estaban disponibles, la actividad de su organización es, al menos en parte, tecnología de la información.  
  
Para crear los planes de recuperación de equilibrio eficaz, los servicios de TI deben trabajar estrechamente con las unidades de negocio de su organización para identificar no solo los componentes más importantes de la infraestructura de TI, pero las funciones críticas necesarias para la empresa. Mediante la identificación de qué es importante para su organización como un todo, puede centrarse en proteger los componentes que tienen el mayor valor. Esto no es una recomendación para shirk la seguridad de datos y sistemas de valor bajo. En su lugar, como definir los niveles de servicio para el tiempo de actividad del sistema, debe considerar definir los niveles de control de seguridad y la supervisión según la importancia del activo.  
  
Cuando se han invertido en la creación de un entorno actual, seguro y administrable, puede cambiar el foco a la administración de forma eficaz y asegurarnos de que tiene los procesos de administración del ciclo de vida efectivo que no son sólo determinados por TI, pero la empresa. Para lograr esto, necesita no solo para ser asociado de la empresa, sino también para invertir el negocio en "propiedad" de los datos y sistemas de Active Directory.  
  
Cuando sistemas y datos se presentan en Active Directory sin propietarios designados, los propietarios de empresas y propietarios de TI, no hay ninguna cadena de responsabilidad para el aprovisionamiento, administración, supervisión, actualización y retirada finalmente el sistema clara. Esto da como resultado las infraestructuras en el que exponen la organización en riesgo los sistemas pero no se pueden retirar porque la propiedad está clara. Para administrar eficazmente el ciclo de vida de los usuarios, datos, aplicaciones y sistemas administrados por la instalación de Active Directory, debe seguir los principios descritos en esta sección.  
  
### <a name="assign-a-business-owner-to-active-directory-data"></a>Asignar a un propietario de la empresa a los datos de Active Directory  
Datos de Active Directory deben tener un propietario identificado empresariales, es decir, un departamento especificado o el usuario que es el punto de contacto para las decisiones sobre el ciclo de vida del recurso. En algunos casos, el propietario de un componente de Active Directory será un usuario o el departamento de TI. Componentes de infraestructura, como controladores de dominio, servidores DHCP y DNS y Active Directory será más probable es que "propiedad" por TI. Para los datos que se agregan a AD DS para admitir la empresa (por ejemplo, nuevos empleados, nuevas aplicaciones y los nuevos repositorios de información), una empresa unidad o el usuario se debe asociar con los datos.  
  
Si usa Active Directory para la propiedad de los registros de datos en el directorio, o si implementar una base de datos independiente para el seguimiento de activos de TI, no debe crearse ninguna cuenta de usuario, ningún servidor o estación de trabajo debe instalarse y no debe implementarse ninguna aplicación sin un propietario designado del registro. Al intentar establecer la propiedad de los sistemas después de que implementó en producción puede ser un desafío en el mejor e imposible en algunos casos. Por lo tanto, se debe establecer la propiedad en el momento en que los datos se introdujeron en Active Directory.  
  
### <a name="implement-business-driven-lifecycle-management"></a>Implementar la administración del ciclo de vida de orientación comercial  
Administración del ciclo de vida debe implementarse para todos los datos en Active Directory. Por ejemplo, cuando se introduce una nueva aplicación en un dominio de Active Directory, propietario de la empresa de la aplicación debe, a intervalos regulares, se espera dar fe de que el uso continuado de la aplicación. Cuando se lanza una nueva versión de una aplicación, propietario de la empresa de la aplicación debe estar informado y debe decidir si y cuándo se implementará la nueva versión.  
  
Si el propietario de una empresa elige no aprobar la implementación de una nueva versión de una aplicación, también se debe notificar ese propietario empresarial de la fecha en la versión actual dejará de ser compatible y debe ser responsable de determinar si la aplicación se iniciará se retiran o reemplazan. Mantener las aplicaciones heredadas ejecutando y no admitidos no debe ser una opción.  
  
Cuando se crean las cuentas de usuario en Active Directory, sus administradores de registro deben enviar una notificación en la creación de objetos y se deben dar fe de la validez de la cuenta a intervalos regulares. Mediante la implementación de una empresa controlado por ciclo de vida y atestación regular de la validez de los datos, las personas que están mejor equipadas para identificar anomalías en los datos son las personas que revisión los datos.  
  
Por ejemplo, los atacantes pueden crear cuentas de usuario que parecen ser cuentas válidas, sigue las convenciones de nomenclatura y la posición de los objetos de su organización. Para detectar las creaciones de estas cuentas, podría implementar una tarea diaria que devuelve todos los objetos de usuario sin un propietario de la empresa designada para que pueda investigar las cuentas. Si los atacantes crean cuentas y asignación a un propietario de la empresa, mediante la implementación de una tarea que notifica la creación de nuevos objetos al propietario de la empresa designada, el propietario de la empresa puede identificar rápidamente si la cuenta es legítima.  
  
Debe implementar enfoques similares a los grupos de seguridad y distribución. Aunque algunos de los grupos pueden ser grupos funcionales creados por TI, mediante la creación de cada grupo con un propietario designado, puede recuperar todos los grupos que pertenecen a un usuario designado y exigir al usuario que dar fe de la validez de sus pertenencias. Al igual que el enfoque adoptado con creación de cuentas de usuario, puede desencadenar las modificaciones del grupo de informes para el propietario de la empresa designada. La rutina más se convierte en propietario de un negocio dar fe de la validez o invalidez de datos en Active Directory, más capacitado que son identificar anomalías que pueden indicar errores en el proceso o riesgo real.  
  
### <a name="classify-all-active-directory-data"></a>Clasificar todos los datos de Active Directory  
Además de registrar el propietario de una empresa para todos los datos de Active Directory en el momento en que se agrega al directorio, también se deben exigir los propietarios de empresas proporcionar la clasificación de los datos. Por ejemplo, si una aplicación almacena datos críticos, propietario de la empresa debe etiquetar como tal, la aplicación con arreglo a la infraestructura de clasificación de su organización.  
  
Algunas organizaciones implementan directivas de clasificación de datos de esos datos de etiqueta según el daño que podría incurrir la exposición de los datos si se lo roban o se expone. Otras organizaciones implementan esos datos de etiquetas de clasificación de datos según su importancia, los requisitos de acceso y retención. Independientemente del modelo de clasificación de datos en uso en su organización, debe asegurarse de que es posible aplicar la clasificación de datos de Active Directory, no sólo para datos de "archivo". Si una cuenta de usuario es una cuenta de VIP, deben identificarse en la base de datos de clasificación de activos (si esto se implementa mediante el uso de atributos en los objetos de AD DS, o si implementar bases de datos de clasificación de recurso independiente).  
  
En el modelo de clasificación de datos, debe incluir la clasificación de datos de AD DS como la siguiente.  
  
### <a name="systems"></a>certificados  
Debería no solo clasificar datos, sino también sus poblaciones de servidor. Para cada servidor, debe saber qué sistema operativo está instalado, los roles generales que proporciona el servidor, las aplicaciones que se ejecutan en el servidor, el propietario de la TI de registro y el propietario de la empresa del registro, si procede. Para todos los datos o aplicaciones que se ejecutan en el servidor, debe exigir clasificación y se debe proteger el servidor según los requisitos de las cargas de trabajo que admite y las clasificaciones que se aplica a del sistema y los datos. También puede agrupar los servidores mediante la clasificación de sus cargas de trabajo, lo que permite identificar rápidamente los servidores que deben ser el mejor supervisado y configurado de forma más rigurosa.  
  
### <a name="applications"></a>Aplicaciones  
Se deben clasificar aplicaciones por funcionalidad (lo que hacen), base de usuarios (que se emplean las aplicaciones) y el sistema operativo en el que se ejecutan. Debe mantener registros que contienen información de versión, estado de la revisión y cualquier otra información pertinente. También debe clasificar las aplicaciones en los tipos de datos, controlar, como se describió anteriormente.  
  
### <a name="users"></a>Usuarios  
Si llamarlos usuarios "dirección VIP", cuentas críticas, o usar una etiqueta diferente, deben ser etiquetadas y supervisar las cuentas en las instalaciones de Active Directory que suelen ser objetivo de los atacantes. En la mayoría de las organizaciones, no es factible para supervisar todas las actividades de todos los usuarios. Sin embargo, si es capaz de identificar las cuentas críticas en la instalación de Active Directory, puede supervisar las cuentas para los cambios como se describe anteriormente en este documento.  
  
También puede empezar a crear una base de datos de "comportamientos esperados" para estas cuentas durante la auditoría de las cuentas. Por ejemplo, si encuentra que un determinado ejecutivo usa su estación de trabajo protegida para acceder a los datos empresariales desde su oficina y desde su casa, pero rara vez desde otras ubicaciones, si ve que intenta obtener acceso a datos con su cuenta desde un equipo no autorizado o un no se encuentra actualmente la ubicación a medio camino todo el planeta donde sabe que el ejecutivo, puede identificar más rápidamente e investigar este comportamiento anómalo.  
  
Mediante la integración de información empresarial con la infraestructura, puede usar esa información empresarial que le ayudarán a identificar los falsos positivos. Por ejemplo, si viaje executive se registra en un calendario que sea accesible para el personal de TI responsable de supervisar el entorno, puede correlacionar los intentos de conexión con ubicaciones conocidas de los ejecutivos.  
  
Supongamos que un ejecutivo normalmente se encuentra en Chicago y utiliza una estación de trabajo protegida para tener acceso a los datos empresariales críticos de su mesa de trabajo y un evento es desencadenado por un error al intentar tener acceso a los datos desde una estación de trabajo no segura en Atlanta. Si es capaz de comprobar que el ejecutivo está actualmente en Atlanta, puede resolver el evento ponerse en contacto con el ejecutivo o un asistente del ejecutivo para determinar si el error de acceso fue el resultado del ejecutivo si se olvida de utilizar la estación de trabajo protegida acceso a los datos. Mediante la creación de un programa que usa los métodos descritos en [planeación de compromiso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), puede empezar a crear una base de datos de los comportamientos esperados para cuentas más "important" en la instalación de Active Directory puede podría contribuir más rápidamente, detectar y responder a los ataques.  
  


