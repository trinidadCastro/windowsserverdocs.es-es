---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: "Apéndice I - crear cuentas de administración de cuentas protegidas y los grupos de Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 666180adca691d6c9783a43063df76877115fc40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Apéndice I: crear administración de cuentas para cuentas protegidas y los grupos de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Apéndice I: crear administración de cuentas para cuentas protegidas y los grupos de Active Directory  

Uno de los retos en la implementación de un modelo de Active Directory que no se basa en permanente pertenencia a grupos con privilegios elevados es que debe haber un mecanismo para rellenar estos grupos cuando se requiere la pertenencia temporal a los grupos. Algunas soluciones de administración de identidad con privilegios requieren que las cuentas de servicio del software se conceden permanente pertenencia a grupos como DA o los administradores en cada dominio del bosque. Sin embargo, técnicamente no es necesario para las soluciones de administración de identidades con privilegios (PIM) ejecutar sus servicios en estos contextos con privilegios elevados.  
  
Este apéndice proporciona información que puedes usar soluciones de PIM de forma nativa implementado o de terceros para crear cuentas que tienen privilegios limitados y pueden controlarse estrictas, pero pueden usarse para rellenar grupos con privilegios en Active Directory cuando se requiere elevación temporal. Si estás implementando PIM como una solución nativa, estas cuentas pueden usarse por personal administrativo para llevar a cabo la población de grupo temporal y, si estás implementando PIM a través del software de terceros, es posible que puede adaptar a estas cuentas para que funcionen como cuentas de servicio.  
  
> [!NOTE]  
> Los procedimientos descritos en este apéndice proporcionan un enfoque para la administración de grupos con privilegios elevados en Active Directory. Puedes adaptar estos procedimientos según tus necesidades, agregar restricciones adicionales u omitir algunas de las restricciones que se describen aquí.  
  
### <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Crear la administración de cuentas para cuentas protegidas y los grupos de Active Directory  
Creación de cuentas que puede usarse para administrar la pertenencia a grupos con privilegios sin necesidad de las cuentas de administración para que se garantice excesivos derechos y permisos consta de cuatro actividades generales que se describen en las instrucciones paso a paso que siguen:  
  
1.  En primer lugar, debes crear un grupo que se va a administrar las cuentas, porque estas cuentas deberían ser administradas por un conjunto limitado de usuarios de confianza. Si todavía no tienes una estructura de unidad organizativa que aloja separan cuentas con privilegios y protegidas y sistemas de la población general en el dominio, debes crear uno. Aunque no se proporcionan instrucciones específicas en este apéndice, capturas de pantalla muestran un ejemplo de una jerarquía de unidad organizativa de estos.  
  
2.  Crear las cuentas de administración. Estas cuentas deben crearse como cuentas de usuario "normal" y no concede ningún derecho de usuario más allá de los que ya se otorgan a los usuarios de manera predeterminada.  
  
3.  Implementar restricciones en las cuentas de administración que puedan usarse solo para el propósito especializado para el que se crearon, además de controlar quién puede habilitar y usar las cuentas (es decir, el grupo que creaste en el primer paso).  
  
4.  Configurar los permisos en el objeto AdminSDHolder en cada dominio para permitir que las cuentas de administración cambiar la pertenencia de los grupos en el dominio con privilegios.  
  
Debes completamente todos estos procedimientos de prueba y modificarlas según sea necesario para el entorno antes de implementarlas en un entorno de producción. También debes comprobar que todas las configuraciones funcionan según lo esperado (algunos procedimientos de prueba se proporcionan en este apéndice), y debes probar un escenario de recuperación ante desastres en los que las cuentas de administración no están disponibles para usarse para rellenar grupos protegidos para fines de recuperación. Para obtener más información sobre cómo hacer copias de seguridad y restauración de Active Directory, consulte la [copia de seguridad de AD DS y Guía paso a paso de recuperación](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Al implementar los pasos descritos en este apéndice, crearás cuentas que podrán administrar la pertenencia de todos los grupos protegidos en cada dominio, no solo los grupos de Active Directory privilegio más alto como EAs, DAs y BAs. Para obtener más información acerca de los grupos protegidos en Active Directory, consulte [Apéndice C: protegido cuentas y los grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
#### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Instrucciones paso a paso para crear cuentas de administración de grupos protegidos  
  
##### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Crear un grupo para habilitar y deshabilitar las cuentas de administración  
Cuentas de administración deben tener sus contraseñas restablecer en cada uso y deben deshabilitarse cuando se completan la necesidad de actividades. Aunque también puede optar por implementar los requisitos de inicio de sesión de tarjeta inteligente de estas cuentas, es una configuración opcional y estas instrucciones se supone que las cuentas de administración se configurarán con un nombre de usuario y contraseñas largas y complejas como el número mínimo de controles. En este paso, vas a crear un grupo que tenga permisos para restablecer la contraseña de cuentas de administración y para habilitar y deshabilitar las cuentas.  
  
Para crear un grupo para habilitar y deshabilitar las cuentas de administración, realiza los siguientes pasos:  
  
1.  En la estructura de unidad organizativa donde se aloja las cuentas de administración, haz clic en la unidad organizativa donde quieras para crear el grupo, haz clic en **nueva** y haz clic en **grupo**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  En la **nuevo objeto - grupo** diálogo cuadro, escribe un nombre para el grupo. Si tienes previsto usar este grupo "activar" todas las cuentas de administración en el bosque, facilitan un grupo de seguridad universal. Si tienes un bosque de dominio único, o si vas a crear un grupo en cada dominio, puedes crear un grupo de seguridad global. Haz clic en **Aceptar** para crear el grupo.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  En el grupo que acabas de crear, haz clic **propiedades**y haz clic en el **objeto** pestaña. En el grupo **propiedad del objeto** cuadro de diálogo, selecciona **objeto proteger contra eliminación accidental**, que no solo impedirá que los usuarios autorizados lo contrario de eliminar el grupo, pero también de moverlo a otra unidad organizativa a menos que la primero se anula la selección de atributo.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  

    > Si ya has configurado permisos en unidades organizativas para restringir la administración de un conjunto limitado de usuarios elemento primario del grupo, no necesitarás para llevar a cabo los siguientes pasos. Se proporcionan aquí para que incluso si aún no ha implementado limitado control administrativo sobre la estructura de unidad organizativa en el que has creado este grupo, se puede proteger contra la modificación el grupo de usuarios no autorizados.  
  
4.  Haz clic en el **miembros** y agregar las cuentas para los miembros de tu equipo quién serán responsables de habilitar cuentas de administración o rellenar protegido grupos cuando sea necesario.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Si aún no has hecho por lo tanto, en la **equipos y usuarios de Active Directory** de consola, haz clic en **vista** y selecciona **características avanzadas**. En el grupo que acabas de crear, haz clic **propiedades**y haz clic en el **seguridad** pestaña. En la **seguridad**, haga clic **avanzadas**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  En la **la configuración de seguridad avanzada para [grupo]** cuadro de diálogo, haz clic en **deshabilitar herencia**. Cuando se te solicita, haz clic en **convertir los permisos en permisos explícitos de este objeto heredados**y haz clic en **Aceptar** para devolver al grupo **seguridad** cuadro de diálogo.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  En la **seguridad** pestaña, quitar los grupos que no deberían poder tener acceso a este grupo. Por ejemplo, si no desea que los usuarios autenticados puedan leer propiedades generales y el nombre del grupo, puedes quitar esa ACE. También puedes quitar ACE, como los de la cuenta los operadores y las versiones anteriores de Windows 2000 Server acceso compatible. Sin embargo, debes, dejar un conjunto mínimo de permisos de objeto en su lugar. Dejar que las ACE siguientes intacto:  
  
    -   SELF  
  
    -   SISTEMA  
  
    -   Administradores de dominio  
  
    -   Administradores de empresa  
  
    -   Administradores  
  
    -   Grupo de acceso de autorización de Windows (si procede)  
  
    -   ENTERPRISE DOMAIN CONTROLLERS  
  
    Aunque puede parecer poco intuitiva para permitir que a los grupos de privilegios más altos en Active Directory para administrar este grupo, tu objetivo en la implementación de estas opciones de configuración no es impedir que los miembros de los grupos se realicen cambios autorizados. En su lugar, el objetivo es asegurarse de que, cuando tengas ocasión que requieren un alto grado de privilegios, se realizará cambios autorizados. Es por este motivo que cambiar de forma predeterminada con privilegios anidamiento de grupos, derechos y permisos se desaconseja realizar a lo largo de este documento. Dejando estructuras predeterminado intacto y vaciar la pertenencia de los grupos de privilegios más altos en el directorio, puedes crear un entorno más seguro que aún funciona según lo esperado.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Si no has configurado ya las directivas de auditoría para los objetos en la estructura de unidad organizativa donde creaste este grupo, debes configurar la auditoría para registrar los cambios de este grupo.  
  
8.  Se ha completado la configuración del grupo que se usará "echa un vistazo a" administración de cuentas cuando son necesarios y los "registros" las cuentas cuando se han completado sus actividades.  
  
##### <a name="creating-the-management-accounts"></a>Crear las cuentas de administración  
Debes crear al menos una cuenta que se usará para administrar la pertenencia a grupos con privilegios en la instalación de Active Directory y preferiblemente una segunda cuenta para que actúe como una copia de seguridad. Si decides crear las cuentas de administración en un solo dominio en el bosque y concederle capacidades de administración para todos los dominios protegido grupos o, si decides implementar las cuentas de administración en cada dominio del bosque, los procedimientos son efectivamente el mismo.  
  
> [!NOTE]  
> En este documento se supone que aún no ha implementado controles de acceso basadas en roles y administración de identidades con privilegios de Active Directory. Por lo tanto, deben realizarse algunos procedimientos de un usuario cuya cuenta es miembro del grupo Administradores de dominio del dominio en cuestión.  
>   
> Cuando usas una cuenta con privilegios de DA, puede iniciar sesión en un controlador de dominio para realizar las actividades de configuración. Pueden realizar los pasos que no requieren privilegios DA cuentas con menos privilegios que se registran estaciones de trabajo administrativas. Capturas de pantalla que muestran cuadros de diálogo con borde en el color celeste representan las actividades que se pueden realizar en un controlador de dominio. Capturas de pantalla que muestran cuadros de diálogo en el color azul más oscuro representan las actividades que se pueden realizar en estaciones de trabajo administrativas con cuentas que tienen privilegios limitados.  
  
Para crear las cuentas de administración, realiza los siguientes pasos:  
  
1.  Iniciar sesión en un controlador de dominio con una cuenta que sea miembro del grupo de DA del dominio.  
  
2.  Iniciar **equipos y usuarios de Active Directory** y navegar a la unidad organizativa que va a crear la cuenta de administración.  
  
3.  Haz clic en la unidad organizativa y haz clic en **nueva** y haz clic en **usuario**.  
  
4.  En la **nuevo objeto - usuario** diálogo cuadro, escribe tu información de nomenclatura deseada de la cuenta y haz clic en **siguiente**.  
  
5.  Iniciar sesión en un controlador de dominio con una cuenta que sea miembro del grupo de DA del dominio.  
  
6.  Iniciar **equipos y usuarios de Active Directory** y navegar a la unidad organizativa que va a crear la cuenta de administración.  
  
7.  Haz clic en la unidad organizativa y haz clic en **nueva** y haz clic en **usuario**.  
  
8.  En la **nuevo objeto - usuario** diálogo cuadro, escribe tu información de nomenclatura deseada de la cuenta y haz clic en **siguiente**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
9. Proporcionar una contraseña inicial para la cuenta de usuario, desactive **usuario debe cambiar la contraseña en el siguiente inicio de sesión**, selecciona **usuario no puede cambiar la contraseña** y **cuenta está deshabilitada**y haz clic en **siguiente**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  
  
10. Comprueba que los detalles de la cuenta sean correctos y haz clic en **finalizar**.  
  
11. Haz clic en el objeto de usuario que acabas de crear y haz clic en **propiedades**.  
  
12. Haz clic en el **cuenta** pestaña.  
  
13. En la **opciones de la cuenta** campo, seleccione la **cuenta es confidencial y no se puede delegar** marca, selecciona el **esta cuenta es compatible con el cifrado Kerberos AES de 128 bits** o la **esta cuenta admite el cifrado de Kerberos AES 256** marcar y haz clic en **Aceptar**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  
  
    > [!NOTE]  
    > Debido a que esta cuenta, al igual que otras cuentas, tiene una función limitada, pero eficaz, la cuenta solo debe usarse en hosts administrativos seguros. Para todos los hosts administrativos seguros en tu entorno, es recomendable implementar la configuración de directiva de grupo **seguridad de red: tipos de configurar el cifrado permiten para Kerberos** para permitir que solo los tipos de cifrado más seguros puede implementar para hosts seguros.  
    >   
    > Aunque la implementación de los tipos de cifrado más seguros para los hosts no mitiga los ataques de robo de credenciales, el uso adecuado y la configuración de los hosts seguros hace. Establecer los tipos de cifrado más sólidos para los hosts que se usan solo por las cuentas con privilegios, simplemente reduce la superficie de ataque general de los equipos.  
    >   
    > Para obtener más información acerca de cómo configurar los tipos de cifrado en los sistemas y cuentas, consulta [configuraciones de Windows para el tipo de cifrado Kerberos admite](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
    >   
    > **Ten en cuenta** estas opciones de configuración son compatibles solo en equipos que ejecutan Windows Server 2012, Windows Server 2008 R2, Windows 8 o Windows 7.  
  
14. En la **objeto** , selecciona **objeto proteger contra eliminación accidental**. Esto no solo evitará que el objeto eliminando (incluso los usuarios autorizados), pero impedirá se mueve a otra unidad organizativa de la jerarquía de AD DS, a menos que primero se desactiva la casilla de verificación por un usuario sin permiso para cambiar el atributo.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  
  
15. Haz clic en el **control remoto** pestaña.  
  
16. Borrar la **habilitar el control remoto** marca. Nunca debería ser necesario para el personal de soporte técnico para conectarse a las sesiones de esta cuenta para implementar correcciones.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  
  
    > [!NOTE]  
    > Todos los objetos de Active Directory deben tener un propietario designado de TI y el propietario de una empresa designado, como se describe en [planificación de comprometer](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Si se realizan un seguimiento de la propiedad de objetos de AD DS en Active Directory (en lugar de una base de datos externa), debe especificar la información de propiedad adecuados en Propiedades de este objeto.  
    >   
    > En este caso, el propietario de la empresa más probable es que es una división de TI, andthere no es prohibición de los propietarios de empresas también está propietarios de TI. Es el punto de establecer la propiedad de objetos para que pueda identificar contactos cuando cambios deben realizarse para los objetos, quizás años desde su creación inicial.  
  
17. Haz clic en el **organización** pestaña.  
  
18. Introduce la información que se necesita en los estándares de objeto de AD DS.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  
  
19. Haz clic en el **marcado** pestaña.  
  
20. En la **permiso de acceso de red** campo, seleccione **denegar el acceso**. Esta cuenta nunca deben conectarse a través de una conexión remota.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  
  
    > [!NOTE]  
    > No es probable que esta cuenta se usará para iniciar sesión en los controladores de dominio de solo lectura (RODC) en su entorno. Sin embargo, debe circunstancia nunca requieren la cuenta para iniciar sesión en un RODC, debes agregar esta cuenta para el grupo de replicación denegado RODC contraseña para que no se almacena en caché su contraseña en el RODC.  
    >   
    > Aunque se debe restablecer la contraseña de la cuenta después de cada uso y se debe deshabilitar la cuenta, implementar esta configuración no tiene un efecto perjudicial en la cuenta y puede ser útil en situaciones en que un administrador olvida de restablecer la contraseña de la cuenta y deshabilitarlo.  
  
21. Haz clic en el **miembro de** pestaña.  
  
22. Haz clic en **agregar**.  
  
23. Tipo **denegado RODC contraseña Replication Group** en la **Seleccionar usuarios, contactos, equipos** cuadro de diálogo y haz clic en **comprobar nombres**. Cuando se subraya el nombre del grupo en el selector de objetos, haz clic en **Aceptar** y comprueba que la cuenta ahora es un miembro de los dos grupos que se muestra en la siguiente captura de pantalla. No agregues la cuenta a cualquier grupo protegido.  
  
24. Haz clic en **Aceptar**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  
  
25. Haz clic en el **seguridad** pestaña y haz clic en **avanzadas**.  
  
26. En la **configuración de seguridad avanzada** cuadro de diálogo, haz clic en **deshabilitar la herencia** y copiar los permisos heredados como permisos explícitos y haz clic en **agregar**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  
  
27. En la **entrada de permiso para [cuenta]** cuadro de diálogo, haz clic en **seleccionar una entidad de seguridad** y agrega el grupo que creaste en el procedimiento anterior. Desplázate a la parte inferior del cuadro de diálogo y haz clic en **Borrar todo** para quitar todos los permisos predeterminados.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  
  
28. Desplázate hasta la parte superior de la **entrada de permiso** cuadro de diálogo. Asegúrate de que la **tipo** lista desplegable y se establece en **permitir**y en la **se aplica a** lista desplegable, selecciona **solo este objeto**.  
  
29. En la **permisos** campo, seleccione **leer todas las propiedades**, **permisos de lectura**, y **restablecer la contraseña**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  
  
30. En la **propiedades** campo, seleccione **leer userAccountControl** y **escribir userAccountControl**.  
  
31. Haz clic en **Aceptar**, **Aceptar** nuevamente en la **configuración de seguridad avanzada** cuadro de diálogo.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  
  
    > [!NOTE]  
    > La **userAccountControl** atributo controla varias opciones de configuración de cuenta. No puede conceder permiso para cambiar solo algunas de las opciones de configuración cuando se concede permiso de escritura en el atributo.  
  
32. En la **nombres de grupo o usuario** campo de la **seguridad** pestaña, quitar todos los grupos que no deberían poder tener acceso o administrar la cuenta. No quitar todos los grupos que se han configurado con ACE de denegación, como el grupo todos y la SELF calculado cuenta (esa ACE se estableció cuando el **usuario no puede cambiar la contraseña** marca se habilitó durante la creación de la cuenta. También quita el grupo que acabas de agregar, la cuenta del sistema o los grupos como EA, DA, BA o el grupo de acceso de autorización de Windows.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  
  
33. Haz clic en **avanzadas** y comprueba que el cuadro de diálogo de configuración de seguridad avanzada tenga una apariencia similar a la siguiente captura de pantalla.  
  
34. Haz clic en **Aceptar**, y **Aceptar** nuevamente para cerrar el cuadro de diálogo de propiedades de la cuenta.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  
  
35. Configuración de la primera cuenta de administración ahora está completa. Probará la cuenta en un procedimiento más adelante.  
  
###### <a name="creating-additional-management-accounts"></a>Creación de cuentas de administración adicional  
Puedes crear cuentas de administración adicional, repita los pasos anteriores, mediante la copia de la cuenta que acabas de crear o mediante la creación de un script para crear cuentas con las opciones de configuración deseada. Sin embargo, ten en cuenta que si copias la cuenta que acabas de crear, muchas de las opciones de configuración personalizadas y ACL no se copiarán a la nueva cuenta y será necesario repetir la mayoría de los pasos de configuración.  
  
En su lugar, puede crear un grupo a la que delegan derechos para rellenar y unpopulate grupos protegidos, pero debes proteger el grupo y las cuentas que colocan en ella. Dado que debería haber muy pocos cuentas en el directorio que tienen la capacidad para administrar la pertenencia a grupos protegidos, crear cuentas individuales podría ser el enfoque más simple.  
  
Independientemente de cómo elegir crear un grupo en el que ubicar las cuentas de administración, debes asegurarte de que se protege cada cuenta como se describió anteriormente. También debería implementar restricciones de GPO similares a los que se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
###### <a name="auditing-management-accounts"></a>Auditoría de administración de cuentas  
Debes configurar la auditoría en la cuenta para iniciar sesión, como mínimo, todas las escrituras a la cuenta. Esto permitirá que no sólo identifica la habilitación de la cuenta y el restablecimiento de su contraseña durante los usos autorizados, pero también identificar los intentos de usuarios no autorizados para manipular la cuenta correcta. Error escrituras en la cuenta se deben capturar en el sistema de supervisión de eventos (SIEM) e información de seguridad (si procede) y deberían desencadenar los avisos que proporcionan notificación para el personal responsable de investigar posibles pone en peligro.  
  
Soluciones SIEM toman la información de evento de orígenes de seguridad implicados (por ejemplo, registros de eventos, datos de la aplicación, las secuencias de red, productos antimalware y orígenes de detección de intrusiones), mostrar los datos e intentan realizar acciones proactivas y vistas inteligentes. Hay muchas soluciones SIEM comerciales y muchas empresas crear implementaciones privadas. Un SIEM bien diseñado e implementado correctamente puede mejorar considerablemente la supervisión de seguridad y las funcionalidades de la respuesta a incidentes. No obstante, capacidades y precisión varían enormemente entre soluciones. SIEMs quedan fuera del ámbito de este documento, pero deben tener en cuenta las recomendaciones de evento específico que contiene cualquier implementador SIEM.  
  
Para obtener más información sobre las opciones de configuración de auditoría recomendada para controladores de dominio, consulta el tema [supervisión de Active Directory de signos de comprometer](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Opciones de configuración específicas de controlador de dominio se proporcionan en [supervisión de Active Directory de signos de comprometer](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
##### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Habilitar la administración de cuentas modificar la pertenencia a grupos protegidos  
En este procedimiento, va a configurar permisos en el objeto de AdminSDHolder del dominio para permitir que las cuentas de administración recién creado modificar la pertenencia a grupos protegidos en el dominio. Este procedimiento no se puede realizar a través de una interfaz gráfica de usuario (GUI).  
  
Como se explica en [Apéndice C: protegido cuentas y los grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), la ACL en AdminSDHolder de un dominio objeto eficazmente "Copiar" para los objetos protegidos cuando se ejecuta la tarea SDProp. Cuentas y grupos protegidos no heredan sus permisos del objeto AdminSDHolder; los permisos se establecen explícitamente para que coincida con los que estén en el objeto AdminSDHolder. Por lo tanto, cuando se modifican permisos en el objeto AdminSDHolder, debes modificar para ver los atributos apropiados para el tipo del objeto protegido de que destino.  
  
En este caso, se puede conceder las cuentas de administración recién creado para permitir que leer y escribir los miembros de atributo en objetos de grupo. Sin embargo, el objeto AdminSDHolder no es un objeto de grupo y atributos de grupo no se exponen en el editor gráfico de ACL. Es por este motivo que implementas los cambios de permisos a través de la utilidad de línea de comandos de Dsacls. Para conceder permisos de cuentas para modificar la pertenencia a grupos protegidos de la administración (deshabilitada), realiza los siguientes pasos:  
  
1.  Iniciar sesión en un controlador de dominio, preferiblemente el controlador de dominio que contiene la función de emulador PDC (PDCE), con las credenciales de cuenta de usuario que se ha realizado un miembro del grupo DA en el dominio.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2.  Abre un símbolo del sistema con privilegios elevados pulsando **símbolo** y haz clic en **ejecutar como administrador**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3.  Cuando se te solicite para aprobar la elevación, haz clic en **Sí**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
    > [!NOTE]  
    > Para obtener más información sobre la elevación y usuario control de cuentas (UAC) en Windows, consulta [UAC procesos e interacciones](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) en el sitio Web de TechNet.  
  
4.  En el símbolo del sistema, escriba (sustituir la información específica de dominio) **Dsacls [nombre completo del objeto AdminSDHolder en tu dominio] / g [cuenta Administración UPN]: RPWP; miembro**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
    El comando anterior (que no distingue mayúsculas de minúsculas) funciona del siguiente modo:  
  
    -   DSACLS establece o muestra ACE de objetos de directorio  
  
    -   CN = AdminSDHolder, CN = sistema, DC = TailSpinToys, DC = msft identifica el objeto que quieres modificar  
  
    -   / G indica que se configura una ACE de concesión  
  
    -   PIM001@tailspintoys.msftes el nombre Principal de usuario (UPN) de la entidad de seguridad a la que se va a conceder las ACE  
  
    -   Concesiones RPWP propiedad permisos lectura y escritura propiedad  
  
    -   Miembro es el nombre de la propiedad (atributo) en la que se establecen los permisos  
  
    Para obtener más información acerca del uso de **Dsacls**, escriba Dsacls sin parámetros en un símbolo del sistema.  
  
    Si ha creado varias cuentas de administración del dominio, debes ejecuta el comando de Dsacls para cada cuenta. Cuando se han completado la configuración de ACL en el objeto AdminSDHolder, debe obligar SDProp para ejecutar o esperar hasta que termine de su ejecución programada. Para obtener información acerca de cómo forzar SDProp para ejecutar, consulta "Ejecuta manualmente SDProp" en [Apéndice C: protegido cuentas y los grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    Cuando se haya ejecutado SDProp, puedes comprobar que los cambios realizados en el objeto AdminSDHolder se han aplicado a grupos protegidos en el dominio. No puede comprobar esto mediante la visualización de la ACL en el objeto AdminSDHolder por los motivos descritos anteriormente, pero puedes comprobar que se han aplicado los permisos mediante la visualización de las ACL en grupos protegidos.  
  
5.  En **equipos y usuarios de Active Directory**, comprueba que has habilitado **características avanzadas**. Para ello, haz clic en **vista**, busque la **administradores de dominio** de grupo, haz clic en el grupo y haga clic en **propiedades**.  
  
6.  Haz clic en el **seguridad** pestaña y haz clic en **avanzadas** para abrir el **la configuración de seguridad avanzada para administradores de dominio** cuadro de diálogo.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7.  Selecciona **permitir ACE para la cuenta de administración** y haz clic en **editar**. Comprueba que la cuenta se ha concedido solo **miembros de lectura** y **escribir miembros** permisos en el grupo de DA y haz clic en **Aceptar**.  
  
8.  Haz clic en **Aceptar** en la **configuración de seguridad avanzada** cuadro de diálogo y haz clic en **Aceptar** nuevamente para cerrar el cuadro de diálogo de propiedad para el grupo DA.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Puedes repetir los pasos anteriores para otros grupos protegidos en el dominio; los permisos deben ser el mismo para todos los grupos protegidos. Se han completado la creación y la configuración de las cuentas de administración de los grupos protegidos en este dominio.  
  
    > [!NOTE]  
    > Cualquier cuenta que tenga permiso para escribir la pertenencia a un grupo en Active Directory también puede agregar propio al grupo. Este comportamiento es por diseño y no se puede deshabilitar. Por este motivo, siempre debes tener cuentas administración deshabilitadas cuando no está en uso y debería supervisar detenidamente las cuentas cuando están deshabilitadas y cuando estén en uso.  
  
##### <a name="verifying-group-and-account-configuration-settings"></a>Comprobación de grupo y opciones de configuración de cuenta  
Ahora que ha creado y configurado cuentas de administración que se pueden modificar la pertenencia a grupos protegidos en el dominio (que incluye los grupos EA, DA y BA con privilegios más elevados), debes comprobar que las cuentas y su grupo de administración se han creado correctamente. Verificación consiste en estas tareas generales:  
  
1.  Prueba el grupo al que se puede habilitar y deshabilitar las cuentas de administración para comprobar que los miembros del grupo puede habilitan y deshabilitar las cuentas y restablezcan las contraseñas, pero no pueden realizar otras actividades administrativas en las cuentas de administración.  
  
2.  Prueba las cuentas de administración para comprobar que se pueden agregar y quitar miembros a protegidas grupos en el dominio, pero no pueden cambiar otras propiedades de grupos y cuentas protegidas.  
  
###### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Probar el grupo que habilitar y deshabilitar las cuentas de administración  
  
1.  Para probar una cuenta de administración de activación y restablecer su contraseña, iniciar sesión en una estación de trabajo seguro con una cuenta que es un miembro del grupo que creaste en [apéndice I: creación de administración de cuentas para las cuentas protegidas y los grupos de Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Abre **equipos y usuarios de Active Directory**, haz clic en la cuenta de administración y haz clic en **habilitar cuenta**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Debe mostrar un cuadro de diálogo, confirmar que se ha habilitado la cuenta.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  A continuación, restablecer la contraseña de la cuenta de administración. Para ello, haz clic en la cuenta otra vez y haz clic en **restablecer contraseña**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Escribe una contraseña nueva para la cuenta en la **nueva contraseña** y **Confirmar contraseña** campos y haz clic en **Aceptar**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Debe aparecer un cuadro de diálogo, confirmar que se ha restablecido la contraseña de la cuenta.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Ahora intenta modificar las propiedades adicionales de la cuenta de administración. Haz clic en la cuenta y haz clic en **propiedades**y haz clic en el **control remoto** pestaña.  
  
8.  Selecciona **habilitar el control remoto** y haz clic en **aplicar**. Debería generar un error en la operación y una **acceso denegado** debe mostrar el mensaje de error.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Haz clic en el **cuenta** ficha de la cuenta e intentan cambiar nombre de la cuenta, las horas de inicio de sesión o estaciones de trabajo de inicio de sesión. Todos deben producirá un error y opciones que no se controlan con la cuenta del **userAccountControl** atributo debe ser gris out y no está disponible para su modificación.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Intenta agregar el grupo de administración a un grupo protegido como el grupo DA. Al hacer clic en **Aceptar**, debe aparecer un mensaje que le informa de que no tiene permisos para modificar el grupo.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Realizar pruebas adicionales según sea necesario para comprobar que no se puede configurar nada en la cuenta de administración excepto **userAccountControl** configuración y restablecimiento de la contraseña.  
  
    > [!NOTE]  
    > La **userAccountControl** atributo controla varias opciones de configuración de cuenta. No puede conceder permiso para cambiar solo algunas de las opciones de configuración cuando se concede permiso de escritura en el atributo.  
  
###### <a name="test-the-management-accounts"></a>La administración de cuentas de prueba  
Ahora que has habilitado uno o más cuentas que pueden cambiar la pertenencia a grupos protegidos, puedes probar las cuentas para garantizar que puedes modificar la pertenencia al grupo protegido, pero no puede realizar otras modificaciones en grupos y cuentas protegidas.  
  
1.  Inicie sesión en un host administrativo seguro como la primera cuenta de administración.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Iniciar **equipos y usuarios de Active Directory** y busca el **grupo Domain Admins**.  
  
3.  Haz clic en el **administradores de dominio** de grupo y haga clic en **propiedades**.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  En la **propiedades de Admins**, haz clic en el **miembros** pestaña y **haga clic en** agregar. Escribe el nombre de una cuenta que le dará temporales privilegios de administrador de dominio y haz clic en **comprobar nombres**. Cuando se subraya el nombre de la cuenta, haz clic en **Aceptar** para volver a la **miembros** pestaña.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  En la **miembros** pestaña para la **propiedades de Admins** cuadro de diálogo, haz clic en **aplicar**. Después de hacer clic **aplicar**, la cuenta debe permanecer un miembro del grupo DA y no recibirás ningún mensaje de error.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Haz clic en el **administrado por** pestaña en la **propiedades de Admins** diálogo cuadro y comprueba que no puede escribir texto en los campos y todos los botones se atenúan.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Haz clic en el **General** pestaña en la **propiedades de Admins** diálogo cuadro y comprueba que no se puede modificar la información acerca de la pestaña.  
  
    ![creación de administración de cuentas](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Repite estos pasos para grupos protegidos adicionales según sea necesario. Cuando hayas terminado, inicia sesión en un host administrativo seguro con una cuenta que es un miembro del grupo que creaste para habilitar y deshabilitar las cuentas de administración. A continuación, restablecer la contraseña de la cuenta de administración que acabas de probar y deshabilitar la cuenta. Se ha completado la configuración de las cuentas de administración y el grupo que será responsable de habilitar y deshabilitar las cuentas.  
  


