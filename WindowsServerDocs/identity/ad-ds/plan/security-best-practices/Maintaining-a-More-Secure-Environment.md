---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: Mantenimiento de un entorno más seguro
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b058fb084b0c46010ba03a11a45e840aa902c7b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367685"
---
# <a name="maintaining-a-more-secure-environment"></a>Mantenimiento de un entorno más seguro

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Law número diez: La tecnología no es una panacea.* - [10 leyes inmutables de administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Cuando haya creado un entorno seguro y administrable para sus activos empresariales críticos, el enfoque debe cambiar para asegurarse de que se mantiene de forma segura. Aunque se han dado controles técnicos específicos para aumentar la seguridad de las instalaciones de AD DS, la tecnología por sí sola no protegerá un entorno en el que no trabaje en colaboración con la empresa para mantener una infraestructura segura y utilizable. Las recomendaciones de alto nivel de esta sección están pensadas para usarse como directrices que puede usar para desarrollar no solo una seguridad eficaz, sino una administración de ciclo de vida eficaz.  
  
En algunos casos, es posible que la organización de ti ya tenga una estrecha relación de trabajo con las unidades de negocio, lo que facilitará la implementación de estas recomendaciones. En las organizaciones en las que los departamentos de ti y las unidades de negocio no están estrechamente vinculados, es posible que tenga que obtener primero el patrocinio ejecutivo de esfuerzos para falsificar una relación más estrecha entre ti y las unidades de negocio. El [Resumen Ejecutivo](../../../ad-ds/manage/component-updates/Executive-Summary.md) está pensado para ser útil como documento independiente para la revisión Ejecutiva y se puede difundir a los responsables de toma de decisiones de su organización.  
  
## <a name="creating-business-centric-security-practices-for-active-directory"></a>Creación de prácticas de seguridad centradas en el negocio para Active Directory  
En el pasado, la tecnología de la información dentro de muchas organizaciones se veía como una estructura de soporte técnico y un centro de costos. Los departamentos de ti a menudo se segregan de usuarios empresariales y las interacciones están limitadas a un modelo de solicitud-respuesta en el que el negocio solicitó recursos y respondió.  
  
A medida que la tecnología ha evolucionado y proliferado, la visión de "un equipo en cada escritorio" ha ido pasando de forma eficaz para gran parte del mundo e incluso ha sido Eclipse en la amplia gama de tecnologías de fácil acceso disponibles hoy en día. La tecnología de la información ya no es una función de soporte técnico, sino que es una función empresarial principal. Si su organización no puede continuar funcionando si todos los servicios de ti no estuvieran disponibles, el negocio de su organización es, al menos, en la tecnología de la información.  
  
Para crear planes de recuperación en peligro eficaces, los servicios de TI deben trabajar en estrecha colaboración con las unidades de negocio de su organización para identificar no solo los componentes más importantes del panorama de ti, sino también las funciones críticas que requiere la empresa. Mediante la identificación de lo que es importante para su organización en su conjunto, puede centrarse en proteger los componentes que tienen más valor. No se trata de una recomendación para shirkr la seguridad de sistemas y datos de bajo valor. En su lugar, al igual que define los niveles de servicio para el tiempo de actividad del sistema, debe considerar la posibilidad de definir niveles de control de seguridad y supervisión en función de la importancia del activo.  
  
Cuando haya invertido en la creación de un entorno actual, seguro y administrable, puede cambiar el enfoque a administrarlo de forma eficaz y asegurarse de que tiene procesos de administración del ciclo de vida efectivos que no están determinados solo por este, sino por la empresa. Para lograrlo, no solo tiene que asociarse con la empresa, sino también para invertir el negocio en "propiedad" de datos y sistemas en Active Directory.  
  
Cuando los datos y los sistemas se introducen en Active Directory sin propietarios designados, propietarios de empresas y propietarios de ti, no hay ninguna cadena clara de responsabilidad para el aprovisionamiento, la administración, la supervisión, la actualización y la retirada definitiva del sistema. Esto da como resultado infraestructuras en las que los sistemas exponen a la organización riesgo pero no se pueden retirar porque la propiedad no está clara. Para administrar de forma eficaz el ciclo de vida de los usuarios, los datos, las aplicaciones y los sistemas administrados por la instalación de Active Directory, debe seguir los principios descritos en esta sección.  
  
### <a name="assign-a-business-owner-to-active-directory-data"></a>Asignar un propietario empresarial a Active Directory datos  
Los datos de Active Directory deben tener un propietario empresarial identificado, es decir, un departamento o usuario específico que es el punto de contacto para tomar decisiones sobre el ciclo de vida del activo. En algunos casos, el propietario comercial de un componente de Active Directory será un departamento de ti o un usuario. Los componentes de la infraestructura, como los controladores de dominio, los servidores DHCP y DNS, y Active Directory probablemente serán "propiedad". En el caso de los datos que se agregan a AD DS para admitir el negocio (por ejemplo, nuevos empleados, nuevas aplicaciones y nuevos repositorios de información), se debe asociar a los datos una unidad de negocio o un usuario designados.  
  
Si usa Active Directory para registrar la propiedad de los datos en el directorio o si implementa una base de datos independiente para el seguimiento de los recursos de ti, no se debe crear ninguna cuenta de usuario, no se debe instalar ningún servidor o estación de trabajo y no se debe implementar ninguna aplicación. sin un propietario designado de registro. El intento de establecer la propiedad de los sistemas una vez que se han implementado en producción puede ser un desafío lo mejor posible e imposible en algunos casos. Por lo tanto, se debe establecer la propiedad en el momento en que se introducen los datos en Active Directory.  
  
### <a name="implement-business-driven-lifecycle-management"></a>Implementación de la administración del ciclo de vida basado en negocios  
La administración del ciclo de vida debe implementarse para todos los datos de Active Directory. Por ejemplo, cuando se introduce una nueva aplicación en un dominio de Active Directory, se espera que el propietario empresarial de la aplicación dé fe del uso continuado de la aplicación. Cuando se publica una nueva versión de una aplicación, se debe informar al propietario del negocio de la aplicación y decidir si se implementará la nueva versión.  
  
Si el propietario de una empresa decide no aprobar la implementación de una nueva versión de una aplicación, también debe recibir una notificación de la fecha en la que la versión actual dejará de ser compatible y debe ser responsable de determinar si la aplicación se debe retirar o reemplazar. Mantener las aplicaciones heredadas en ejecución y no compatibles no debe ser una opción.  
  
Cuando las cuentas de usuario se crean en Active Directory, se debe notificar a sus administradores de registros en la creación de objetos y es necesario que certifiquen la validez de la cuenta a intervalos regulares. Mediante la implementación de un ciclo de vida controlado por la empresa y la atestación regular de la validez de los datos, las personas que están mejor equipadas para identificar anomalías en los datos son las personas que revisan los datos.  
  
Por ejemplo, los atacantes pueden crear cuentas de usuario que parezcan cuentas válidas, siguiendo las convenciones de nomenclatura y la colocación de objetos de su organización. Para detectar estas creaciones de cuentas, podría implementar una tarea diaria que devuelve todos los objetos de usuario sin un propietario empresarial designado para que pueda investigar las cuentas. Si los atacantes crean cuentas y asignan el propietario de una empresa mediante la implementación de una tarea que informa de la creación de nuevos objetos al propietario empresarial designado, el propietario de la empresa puede identificar rápidamente si la cuenta es legítima.  
  
Debe implementar enfoques similares a los grupos de seguridad y distribución. Aunque algunos grupos pueden ser grupos funcionales creados por él, al crear cada grupo con un propietario designado, puede recuperar todos los grupos propiedad de un usuario designado y requerir al usuario que dé fe de la validez de sus pertenencias. De forma similar al enfoque que se lleva a cabo con la creación de cuentas de usuario, puede desencadenar modificaciones en el grupo de informes para el propietario de la empresa designada. Cuanto más rutinaria se convierta en el propietario de la empresa en atestiguarse de la validez o la invalidez de los datos en Active Directory, más equiparemos es identificar anomalías que pueden indicar errores en los procesos o los riesgos reales.  
  
### <a name="classify-all-active-directory-data"></a>Clasificar todos los datos de Active Directory  
Además de registrar un propietario empresarial para todos los datos de Active Directory en el momento en que se agregan al directorio, también debe requerir a los propietarios de empresas que proporcionen la clasificación de los datos. Por ejemplo, si una aplicación almacena datos críticos para la empresa, el propietario de la empresa debe etiquetar la aplicación como tal, de acuerdo con la infraestructura de clasificación de su organización.  
  
Algunas organizaciones implementan directivas de clasificación de datos que etiquetan los datos de acuerdo con los daños causados por la exposición de los datos en caso de robo o exposición. Otras organizaciones implementan la clasificación de datos que etiqueta los datos por importancia, por los requisitos de acceso y por retención. Independientemente del modelo de clasificación de datos que se utilice en su organización, debe asegurarse de que puede aplicar la clasificación a Active Directory datos, no solo a los datos de "archivo". Si la cuenta de un usuario es una cuenta de VIP, debe identificarse en la base de datos de clasificación de recursos (si se implementa mediante el uso de atributos en los objetos de AD DS o si se implementan bases de datos de clasificación de recursos independientes).  
  
Dentro del modelo de clasificación de datos, debe incluir la clasificación de AD DS datos como los siguientes.  
  
### <a name="systems"></a>certificados  
No solo se deben clasificar los datos, sino también sus rellenados de servidor. Para cada servidor, debe saber qué sistema operativo está instalado, qué roles generales proporciona el servidor, qué aplicaciones se están ejecutando en el servidor, el propietario de TI del registro y el propietario empresarial del registro, si procede. En el caso de todos los datos o aplicaciones que se ejecutan en el servidor, debe requerir la clasificación y el servidor debe protegerse según los requisitos de las cargas de trabajo que admite y las clasificaciones aplicadas al sistema y a los datos. También puede agrupar los servidores según la clasificación de sus cargas de trabajo, lo que le permite identificar rápidamente los servidores que deben ser los más supervisados y configurados de forma más rigurosa.  
  
### <a name="applications"></a>Aplicaciones  
Debe clasificar las aplicaciones por funcionalidad (lo que hacen), usuario base (que usa las aplicaciones) y el sistema operativo en el que se ejecutan. Debe mantener registros que contengan información de versión, estado de revisión y cualquier otra información pertinente. También debe clasificar las aplicaciones por los tipos de datos que administran, como se ha descrito anteriormente.  
  
### <a name="users"></a>Usuarios  
Tanto si se le llama usuarios "VIP", cuentas críticas, como si usa una etiqueta diferente, las cuentas de las instalaciones de Active Directory que tengan más probabilidades de ser destinadas a los atacantes se deben etiquetar y supervisar. En la mayoría de las organizaciones, simplemente no es factible supervisar todas las actividades de todos los usuarios. Sin embargo, si es capaz de identificar las cuentas críticas en la instalación de Active Directory, puede supervisar esas cuentas en busca de cambios como se describió anteriormente en este documento.  
  
También puede empezar a crear una base de datos de "comportamientos esperados" para estas cuentas a medida que audita las cuentas. Por ejemplo, si observa que un ejecutivo determinado usa su estación de trabajo protegida para tener acceso a los datos críticos para la empresa desde su oficina y desde su hogar, pero rara vez desde otras ubicaciones, si ve intentos de obtener acceso a los datos mediante su cuenta desde un equipo no autorizado o un la ubicación intermedia en torno al planeta en el que sabe que el Ejecutivo no está ubicada actualmente, puede identificar e investigar este comportamiento anómalo con mayor rapidez.  
  
Al integrar la información empresarial en la infraestructura de, puede usar esa información empresarial para ayudarle a identificar falsos positivos. Por ejemplo, si el viaje Ejecutivo se registra en un calendario que es accesible para el personal de TI responsable de la supervisión del entorno, puede correlacionar los intentos de conexión con las ubicaciones conocidas de los ejecutivos.  
  
Supongamos que el Ejecutivo A se encuentra normalmente en Chicago y utiliza una estación de trabajo protegida para acceder a los datos críticos para la empresa de su escritorio, y un evento se desencadena por un intento fallido de acceder a los datos desde una estación de trabajo no segura ubicada en Atlanta. Si es capaz de comprobar que el Ejecutivo está actualmente en Atlanta, puede resolver el evento poniéndose en contacto con el Ejecutivo o con el asistente del Ejecutivo para determinar si el error de acceso fue el resultado de que el Ejecutivo olvidase de usar la estación de trabajo protegida para acceder a los datos. Mediante la creación de un programa que usa los enfoques descritos en [planeación del riesgo](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), puede empezar a crear una base de datos de comportamientos previstos para las cuentas más "importantes" en la instalación de Active Directory que pueden ayudarle más detecte y responda rápidamente a ataques.  
  


