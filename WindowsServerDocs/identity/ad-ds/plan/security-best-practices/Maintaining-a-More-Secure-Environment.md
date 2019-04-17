---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: "Mantenimiento de un entorno más seguro"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d471ee2ba73ef7425464e1aa928c75318f7dd82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="maintaining-a-more-secure-environment"></a>Mantenimiento de un entorno más seguro

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Ley número diez: tecnología no es una panacea.* - [10 inmutables de administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Cuando hayas creado un entorno manejable y seguro para los activos críticos para el negocio, debería cambiar el foco para garantizar que se mantenga segura. Aunque se han proporcionado controles específicos de técnicos para aumentar la seguridad de las instalaciones de AD DS, tecnología por sí sola no lo protegerán un entorno donde no funciona en colaboración con la empresa para mantener una infraestructura segura utilizable. Las recomendaciones de alto niveles en esta sección están diseñadas para usarse como directrices que pueden utilizar para desarrollar una seguridad eficaz, pero no solo administración eficaz del ciclo de vida.  
  
En algunos casos, la organización de TI que ya tenga una relación de trabajo cerrar con unidades de negocio, que será más fácil implementar estas recomendaciones. En las organizaciones donde y unidades de negocio no están estrechamente relacionadas, es posible que debas obtener patrocinio ejecutivo esfuerzos falsificar una relación entre más de cerca de TI y empresas. La [resumen](../../../ad-ds/manage/component-updates/Executive-Summary.md) está pensado para ser útiles como un documento independiente para su revisión ejecutivo y pueda transmitirse a tomar decisiones en la organización.  
  
## <a name="creating-business-centric-security-practices-for-active-directory"></a>Creación de procedimientos de seguridad centrada en la empresa para Active Directory  
En el pasado, se ha visto la tecnología de información en muchas organizaciones como una estructura de soporte técnico y un centro de costo. Los departamentos de TI se separen a menudo en gran medida a los usuarios empresariales e interacciones limitadas a un modelo de solicitud y respuesta en las que la empresa solicitó recursos y TI respondido.  
  
La tecnología que ha evolucionado y proliferado, la visión de "un equipo en cada escritorio" tiene provienen de manera eficaz para pasar de gran parte del mundo e incluso ha eclipsed por la amplia gama de tecnologías fácilmente accesibles disponibles actualmente. Tecnología de información ya no es una función de soporte técnico, es una función empresarial. Si la organización no puede continuar funcionando si todos los servicios no estaban disponibles, la actividad de tu organización es, al menos en parte, tecnología de la información.  
  
Para crear planes de recuperación de compromiso eficaz, los servicios de TI deben trabajan estrechamente con empresas en la organización para identificar no solo los componentes más importantes del entorno de TI, pero las funciones fundamentales necesarias para la empresa. Al identificar qué aspectos son importantes para la organización como un todo, puedes centrarte en proteger los componentes que tienen el mayor valor. Esto no es una recomendación para shirk la seguridad de datos y sistemas de valor bajo. En su lugar, como definir los niveles de servicio para el rendimiento del sistema, puede definir los niveles de control de seguridad y supervisión en función de la importancia del elemento.  
  
Cuando se ha invertido en la creación de un entorno actual, seguro y administrable, puede cambie el foco a la administración de manera eficaz y garantizar que dispones de los procesos de administración eficaz del ciclo de vida que no son determinados solamente por TI, pero el negocio. Para lograrlo, necesitas no solo colaborar con la empresa, pero para invertir el negocio en "posesión" de datos y sistemas de Active Directory.  
  
Cuando los datos y sistemas se introducen en Active Directory sin propietarios designados, propietarios de empresas y los propietarios de TI, no hay ninguna cadena borrar de responsabilidad para el aprovisionamiento, la administración, supervisión, actualizar y finalmente la retirada del sistema. Esto da como resultado infraestructuras en el que sistemas exponen la organización a riesgo, pero no pueden darse de baja porque la propiedad no está clara. Para administrar eficazmente el ciclo de vida de los usuarios, datos, aplicaciones y sistemas que administrados por la instalación de Active Directory, debes seguir los principios descritos en esta sección.  
  
### <a name="assign-a-business-owner-to-active-directory-data"></a>Asignar a un propietario de la empresa a los datos de Active Directory  
Los datos en Active Directory deben tener un propietario empresarial identificado, es decir, un usuario que es el punto de contacto para tomar decisiones sobre el ciclo de vida del activo o departamento especificado. En algunos casos, el propietario de un componente de Active Directory será un usuario o el departamento de TI. Componentes de infraestructura como controladores de dominio, servidores DHCP y DNS y Active Directory serán más probable es que "propiedad" por TI. Para los datos que se agregan en AD DS para admitir la empresa (por ejemplo, nuevos empleados, nuevas aplicaciones y los nuevos repositorios de información), un negocio designado unidad o el usuario debe asociarse con los datos.  
  
Si usan Active Directory para la propiedad de los registros de datos en el directorio, o si implementas una base de datos independiente para realizar el seguimiento de los activos de TI, no debe crearse ninguna cuenta de usuario, no debe instalarse ningún servidor o estación de trabajo y ninguna aplicación que debe implementarse sin un propietario designado de registro. Intenta establecer la propiedad de sistemas una vez que hayan ha implementado en producción puede ser un desafío de mejor e imposible en algunos casos. Por lo tanto, la propiedad debe establecerse en el momento en que los datos se introducen en Active Directory.  
  
### <a name="implement-business-driven-lifecycle-management"></a>Implementar la administración del ciclo de vida impulsados por el negocio  
Administración del ciclo de vida debe implementarse para todos los datos en Active Directory. Por ejemplo, cuando se introduce una nueva aplicación en un dominio de Active Directory, propietario de la aplicación de la empresa debe, a intervalos regulares, se espera para atestar la utilización continuada de la aplicación. Cuando se libera una nueva versión de una aplicación, el propietario de la aplicación debe estar informado y debe decidir si se van a implementar la nueva versión.  
  
Si el propietario de una empresa decide no aprobar la implementación de una nueva versión de una aplicación, dicho propietario de la empresa también se debe notificar la fecha en la versión actual ya no se admitirán y debe ser el responsable de determinar si la aplicación se retiran o reemplazará. Mantener aplicaciones heredadas ejecutando y no compatible, no debe ser una opción.  
  
Cuando se crean cuentas de usuario en Active Directory, sus administradores de registro deben ser una notificación en la creación de objetos y necesarios para atestar la validez de la cuenta a intervalos regulares. Al implementar un negocio controlada por ciclo de vida y atestación normal de la validez de los datos, las personas que están mejor equipadas para identificar anomalías en los datos son las personas que revisión los datos.  
  
Por ejemplo, los atacantes pueden crear cuentas de usuario que parezcan estar cuentas válidas, sigue las convenciones de nomenclatura y la colocación de objetos de la organización. Para detectar las creaciones de estas cuentas, podría implementar una tarea diaria que devuelve todos los objetos de usuario sin propietario de una empresa designado para que puedas investigar las cuentas. Si los atacantes crean cuentas y asignar al propietario de una empresa, mediante la implementación de una tarea que notifica el objeto nuevo para el propietario designado, el propietario puede identificar rápidamente si la cuenta es legítima.  
  
Se deben implementar métodos similares a grupos de seguridad y la distribución. Aunque es posible que algunos grupos grupos funcionales creados por TI, al crear cada grupo con un propietario designado, puede recuperar todos los grupos que pertenecen a un usuario designado y requieren que el usuario atestar la validez de sus miembros. Al igual que el enfoque hecho con la creación de cuentas de usuario, puedes activar informes Modificaciones de grupo en el propietario de la empresa designado. La rutina más se convierte en propietario de un negocio atestar la validez o invalidez de datos en Active Directory, más capacitado que son identificar anomalías que pueden indicar los errores de proceso ni arreglo real.  
  
### <a name="classify-all-active-directory-data"></a>Clasificar todos los datos de Active Directory  
Además de registrar el propietario de una empresa para todos los datos de Active Directory en el momento en que se agrega al directorio, también deberías requerir que los propietarios de empresas proporcionar la clasificación de los datos. Por ejemplo, si una aplicación almacena datos fundamentales de la empresa, el propietario de la empresa debe etiquetar como tal, la aplicación con arreglo a la infraestructura de clasificación de la organización.  
  
Algunas organizaciones implementan directivas de clasificación de datos datos según el daño que incurriría exposición de los datos si se te lo roban o exponen etiqueta. Otras organizaciones implementan la clasificación de datos que los datos etiquetas importancia, requisitos de acceso y retención. Independientemente del modelo de clasificación de datos en uso en la organización, debes asegurarte de que se puede aplicar clasificación a los datos de Active Directory, no solo a los datos de "file". Si una cuenta de usuario es una cuenta de VIP, deben identificarse en la base de datos de clasificación activo (si esto se implementa mediante el uso de atributos en los objetos en AD DS, o si implementas bases de datos de clasificación activo independiente).  
  
Dentro de su modelo de clasificación de datos, debes incluir la clasificación para datos de AD DS, como los siguientes.  
  
### <a name="systems"></a>Sistemas  
Se debe no solo clasificar datos, pero también sus poblaciones del servidor. Para cada servidor, debes saber qué sistema operativo está instalado, qué roles generales que proporciona el servidor, qué aplicaciones se ejecutan en el servidor, el propietario de TI de registro y el propietario de registro, en su caso. Para todos los datos o aplicaciones que se ejecutan en el servidor, deberías requerir clasificación y el servidor se debe proteger según los requisitos de las cargas de trabajo admite y las clasificaciones que se aplica a los datos y el sistema. También puedes agrupar servidores por la clasificación de sus cargas de trabajo, que permite identificar rápidamente los servidores que deben ser la mejor supervisada y más estrictas configurado.  
  
### <a name="applications"></a>Aplicaciones  
Se deben clasificar las aplicaciones por funcionalidad (lo que hacen), la base de usuarios (que usa las aplicaciones) y el sistema operativo en el que se ejecutan. Deberás mantener registros que contienen información sobre la versión, el estado de revisión y cualquier otra información pertinente. También debes clasificar las aplicaciones de los tipos de datos controlan, como se describió anteriormente.  
  
### <a name="users"></a>Usuarios  
Si llamarlos usuarios de "Muy de importantes", cuentas críticas, o usar una etiqueta diferente, las cuentas en las instalaciones de Active Directory que están más probable que los atacantes destino deben ser etiquetadas y supervisar. En la mayoría de las organizaciones, no es factible supervisar todas las actividades de todos los usuarios. Sin embargo, si se puede identificar las cuentas críticas en la instalación de Active Directory, puedes supervisar esas cuentas para los cambios, como se describió anteriormente en este documento.  
  
También, se puede comenzar a crear una base de datos de "esperados comportamientos" de estas cuentas durante la auditoría de las cuentas. Por ejemplo, si ves que un determinado ejecutivo usa su estación de trabajo protegida para acceder a datos empresariales de su oficina y desde su hogar, pero rara vez desde otras ubicaciones, si ves que intenta acceder a datos mediante su cuenta de un equipo no autorizado o una ubicación hasta la mitad alrededor del planeta donde se sabe que no se encuentra actualmente el ejecutivo, más rápido puede identificar e investigar este comportamiento anómala.  
  
Mediante la integración de información de la empresa a tu infraestructura, puedes usar esa información empresarial para ayudarte a identificar falsos positivos. Por ejemplo, si ejecutivo viajes están registrados en un calendario que sea accesible para el personal de TI responsable de supervisar el entorno, puede correlacionar los intentos de conexión con ubicaciones conocidas de los ejecutivos.  
  
Supongamos que ejecutivo A normalmente se encuentra en Chicago y usa una estación de trabajo protegida para acceder a datos empresariales desde su despacho y un evento se desencadena mediante un intento fallido de acceder a los datos de una estación de trabajo no segura que se encuentra en Atlanta. Si se puede comprobar que el ejecutivo está actualmente en Atlanta, puedes resolver el evento en contacto con el ejecutivo o Asistente de ejecutivo para determinar si el error de acceso fue el resultado de la ejecutivo olvido de usar la estación de trabajo segura para acceder a los datos. Mediante la creación de un programa que usa los enfoques descritos en [planificación de comprometer](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), puedes empezar a crear una base de datos de comportamientos esperados para detectan y responden a los ataques de las cuentas más "importantes" en la instalación de Active Directory que potencialmente pueden ayudarte más rápidamente.  
  


