---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: Apéndice I - creación de cuentas de administración de cuentas protegidas y grupos en Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e90c2c075ba2dc2b63e9a18c9eba192116265b90
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443526"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Apéndice I: Creación de la administración de cuentas para cuentas protegidas y grupos en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uno de los desafíos en la implementación de un modelo de Active Directory que no se basa en la pertenencia permanente a grupos con privilegios elevados es que debe haber un mecanismo para rellenar estos grupos cuando se requiere la pertenencia temporal a los grupos. Algunas soluciones de administración de identidades con privilegios requieren que las cuentas de servicio del software se conceden pertenencia permanente a grupos como administradores en cada dominio del bosque o DA. Sin embargo, técnicamente no es necesario para las soluciones de Privileged Identity Management (PIM) ejecutar sus servicios en estos contextos con privilegios elevados.  
  
Este apéndice proporciona información que puede usar para las soluciones PIM de forma nativa implementadas o de terceros para crear las cuentas con privilegios limitados y pueden controlarse rigurosamente, pero pueden usarse para rellenar los grupos con privilegios en Active Directory cuando se requiere la elevación temporal. Si está implementando PIM como una solución nativa, estas cuentas se utilizan para realizar el relleno de grupo temporal por personal administrativo y, si va a implementar PIM a través de software de terceros, es posible que pueda adaptar estas cuentas para que funcione como servicio cuentas.  
  
> [!NOTE]  
> Los procedimientos descritos en este apéndice proporcionan un enfoque para la administración de grupos con privilegios elevados en Active Directory. Puede adaptar estos procedimientos para que se adapte a sus necesidades, agregar restricciones adicionales, u omitir algunas de las restricciones que se describen aquí.  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Creación de la administración de cuentas para cuentas protegidas y grupos en Active Directory

Creación de cuentas que puede usarse para administrar la pertenencia a grupos con privilegios, sin necesidad de las cuentas de administración que se deben conceder permisos y derechos excesivos consta de cuatro actividades generales que se describen en las instrucciones paso a paso que seguimiento:  
  
1.  En primer lugar, debe crear un grupo que va a administrar las cuentas, ya que estas cuentas deben administrarse mediante un conjunto limitado de usuarios de confianza. Si no dispone de una estructura de unidades Organizativas que dé cabida a cuentas con privilegios y protegidas separadas y sistemas de la población general en el dominio, debe crear uno. Aunque no se proporcionan instrucciones específicas de este apéndice, capturas de pantalla muestran un ejemplo de una jerarquía de unidades Organizativas de este tipo.  
  
2.  Cree las cuentas de administración. Estas cuentas deben crearse como cuentas de usuario "regular" y no concede ningún derecho de usuario más allá de los que ya se conceden a los usuarios de forma predeterminada.  
  
3.  Implementar restricciones en las cuentas de administración que puedan utilizarse solo con el fin especializado para la que se crearon, además de controlar quién puede habilitar y usar las cuentas (el grupo que creó en el primer paso).  
  
4.  Configurar permisos en el objeto AdminSDHolder en cada dominio para permitir que las cuentas de administración cambiar la pertenencia de los grupos con privilegios en el dominio.  
  
Debe probar todos estos procedimientos exhaustivamente y modifíquelas según corresponda para su entorno antes de implementarlos en un entorno de producción. También debe comprobar que todas las configuraciones funcionan según lo esperado (algunos procedimientos de prueba se proporcionan en este apéndice), y debe probar un escenario de recuperación ante desastres en el que las cuentas de administración no están disponibles para usarse para rellenar los grupos protegidos para la recuperación con fines. Para obtener más información acerca de la copia de seguridad y restauración de Active Directory, consulte el [copia de seguridad de AD DS y Guía paso a paso de recuperación](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Mediante la implementación de los pasos descritos en este apéndice, creará las cuentas que podrán administrar la pertenencia de todos los grupos protegidos en cada dominio, no solo los grupos de Active Directory de privilegio más alto como EAs, DAs y BAs. Para obtener más información sobre grupos protegidos en Active Directory, consulte [Apéndice C: Grupos de Active Directory y cuentas protegidas](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Instrucciones paso a paso para crear cuentas de administración de grupos protegidos  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Creación de un grupo para habilitar y deshabilitar cuentas de administración

Las cuentas de administración deben tener sus contraseñas en cada uso restablecer y deben estar deshabilitadas cuando finalizan las actividades que sea necesaria otra. Aunque también es posible que considere la posibilidad de implementar los requisitos de inicio de sesión de tarjeta inteligente para estas cuentas, es una configuración opcional y estas instrucciones se supone que las cuentas de administración se configurará con un nombre de usuario y una contraseña larga y compleja como mínimo controles. En este paso, creará un grupo que tenga permisos para restablecer la contraseña en las cuentas de administración y para habilitar y deshabilitar las cuentas.  
  
Para crear un grupo para habilitar y deshabilitar cuentas de administración, realice los pasos siguientes:  
  
1.  En la estructura de unidad organizativa donde se aloja las cuentas de administración, haga clic en la unidad organizativa donde desea crear el grupo, haga clic en **New** y haga clic en **grupo**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  En el **nuevo objeto - grupo** diálogo cuadro, escriba un nombre para el grupo. Si va a usar este grupo "activar" todas las cuentas de administración en el bosque, conviértalo en un grupo de seguridad universal. Si tiene un bosque de dominio único o si va a crear un grupo en cada dominio, puede crear un grupo de seguridad global. Haga clic en **Aceptar** para crear el grupo.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Haga clic con el botón secundario en el grupo que acaba de crear y luego haga clic en **Propiedades**y en la pestaña **Objeto** . En el grupo **propiedad de objeto** cuadro de diálogo, seleccione **proteger objeto contra eliminación accidental**, que no solo impedirá estarían autorizados eliminen el grupo, sino también desde lo mueve al otra UO a menos que el atributo es el primero anule la selección.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > Si ya ha configurado los permisos en el primario del grupo unidades organizativas para restringir la administración a un conjunto limitado de usuarios, es posible que no deberá realizar los pasos siguientes. Proporciona aquí para que incluso si aún no ha implementado un control administrativo limitado sobre la estructura de unidad organizativa en la que ha creado este grupo, puede proteger el grupo de la modificación de los usuarios no autorizados.  
  
4.  Haga clic en el **miembros** y agregar las cuentas para los miembros del equipo que serán responsables de la habilitación de las cuentas de administración o rellenar protegidas grupos cuando sea necesario.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Si se aún no lo ha hecho, en el **equipos y usuarios de Active Directory** de consola, haga clic en **vista** y seleccione **características avanzadas**. Haga clic en el grupo que acaba de crear, haga clic en **propiedades**y haga clic en el **seguridad** ficha. En la pestaña **Seguridad** , haga clic en **Opciones avanzadas**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  En el **Advanced Security Settings for [grupo]** cuadro de diálogo, haga clic en **deshabilitar herencia**. Cuando se le solicite, haga clic en **convertir permisos heredados en permisos explícitos para este objeto**y haga clic en **Aceptar** para devolver al grupo **seguridad** cuadro de diálogo.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  En el **seguridad** pestaña, quite los grupos que no deberían poder tener acceso a este grupo. Por ejemplo, si no desea que los usuarios autenticados puedan leer las propiedades generales y el nombre del grupo, puede quitar esa ACE. También puede quitar las ACE, tales como los de la cuenta los operadores y las versiones anteriores de Windows 2000 Server acceso compatible. Sin embargo, debería, deje un conjunto mínimo de permisos de objetos en su lugar. Dejar intactas las siguientes ACE:  
  
    -   SELF  
  
    -   SISTEMA  
  
    -   Admins. del dominio  
  
    -   Administradores de empresas  
  
    -   Administradores  
  
    -   Grupo de acceso de autorización de Windows (si procede)  
  
    -   CONTROLADORES DE DOMINIO EMPRESARIALES  
  
    Aunque pueda parecer contradictorio permitir a los grupos con privilegios más altos en Active Directory para administrar este grupo, su objetivo en la implementación de esta configuración no es evitar que a los miembros de esos grupos realicen cambios autorizados. En su lugar, el objetivo es garantizar que cuando tenga la ocasión que requieren un alto grado de privilegios, los cambios autorizados se realizará correctamente. Es por esta razón que cambiar de forma predeterminada con privilegios de anidamiento de grupos, los derechos, y no se recomienda usar los permisos a lo largo de este documento. Al conservar intactos estructuras predeterminadas y vaciando la pertenencia de los grupos de privilegio más altos en el directorio, puede crear un entorno más seguro que aún funciona según lo previsto.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Si no ha configurado ya las directivas de auditoría para los objetos de la estructura de unidad organizativa en la que creó este grupo, debe configurar la auditoría para registrar cambios en este grupo.  
  
8.  Ha completado la configuración del grupo que se usará para "desproteger" administración de cuentas cuando sean necesarios y "proteger" las cuentas cuando se hayan completado sus actividades.  
  
#### <a name="creating-the-management-accounts"></a>Creación de las cuentas de administración

Debe crear al menos una cuenta que se usará para administrar la pertenencia a grupos con privilegios en la instalación de Active Directory y, preferiblemente, una segunda cuenta para que actúe como una copia de seguridad. Si opta por crear las cuentas de administración en un único dominio en el bosque y concederles capacidades de administración de dominios de todos los grupos protegidos, o si decide implementar cuentas de administración en cada dominio del bosque, los procedimientos son efectivamente el mismo.  
  
> [!NOTE]  
> Los pasos descritos en este documento se suponen que aún no ha implementado los controles de acceso basado en roles y privileged identity management para Active Directory. Por lo tanto, algunos procedimientos deben realizarla un usuario cuya cuenta se encuentra un miembro del grupo Admins. del dominio para el dominio en cuestión.  
>   
> Cuando se usa una cuenta con privilegios DA, puede iniciar sesión en un controlador de dominio para realizar las actividades de configuración. Los pasos que no requieren privilegios DA pueden realizarse mediante cuentas con menos privilegios que han iniciado sesión en estaciones de trabajo administrativas. Capturas de pantalla que muestran cuadros de diálogo con borde de color azul más claro representan las actividades que se pueden realizar en un controlador de dominio. Capturas de pantalla que muestran cuadros de diálogo en el color azul más oscuro representan las actividades que se pueden realizar en estaciones de trabajo administrativas con cuentas que tienen privilegios limitados.  
  
Para crear las cuentas de administración, realice los pasos siguientes:  
  
1. Inicie sesión en un controlador de dominio con una cuenta que sea miembro del grupo de DA del dominio.  

2. Iniciar **equipos y usuarios de Active Directory** y vaya a la unidad organizativa donde va a crear la cuenta de administración.  

3. Haga clic en la unidad organizativa y haga clic en **New** y haga clic en **usuario**.  

4. En el **nuevo objeto - usuario** diálogo cuadro, escriba la información de nombres deseada para la cuenta y haga clic en **siguiente**.  

   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. Proporcione una contraseña inicial para la cuenta de usuario, desactive **usuario debe cambiar la contraseña en el siguiente inicio de sesión**, seleccione **usuario no puede cambiar la contraseña** y **cuenta está deshabilitada**, y Haga clic en **siguiente**.  

   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. Compruebe que la información de cuenta es correcta y haga clic en **finalizar**.  

7. Haga clic en el objeto de usuario que acaba de crear y haga clic en **propiedades**.  

8. Haga clic en el **cuenta** ficha.  

9. En el **opciones de la cuenta** campos, seleccione el **cuenta es importante y no se puede delegar** marca, seleccione el **esta cuenta admite cifrado AES de Kerberos, 128 bits** o el **esta cuenta admite cifrado AES de Kerberos 256** marca y haga clic en **Aceptar**.  

   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > Dado que esta cuenta, al igual que otras cuentas, tendrá una función limitada, pero eficaz, la cuenta debe usarse solo en hosts administrativos seguros. Para todos los hosts administrativos seguros en su entorno, debe considerar la implementación de la configuración de directiva de grupo **seguridad de red: Configurar tipos de cifrado permitidos para Kerberos** para permitir que solo los tipos de cifrado más seguros, puede implementar para hosts seguros.  
   >
   > Aunque implementa tipos de cifrado más seguros para los hosts no mitiga los ataques de robo de credenciales, se admite el uso adecuado y la configuración de los hosts seguros. Establecer tipos de cifrado más seguros para los hosts que utilizan sólo las cuentas con privilegios, simplemente reduce la superficie de ataque general de los equipos.  
   >
   > Para obtener más información sobre cómo configurar tipos de cifrado en los sistemas y las cuentas, vea [las configuraciones de Windows para el tipo de cifrado de Kerberos admite](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
   >
   > Esta configuración solo se admite en los equipos que ejecutan Windows Server 2012, Windows Server 2008 R2, Windows 8 o Windows 7.  
  
10. En el **objeto** ficha, seleccione **proteger objeto contra eliminación accidental**. Esto no solo impedirá el objeto que se va a eliminar (incluso por usuarios no autorizados), pero impedirá se mueve a una unidad organizativa diferente en la jerarquía de AD DS, a menos que la casilla de verificación está desactivada en primer lugar por un usuario con permiso para cambiar el atributo.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. Haga clic en el **control remoto** ficha.  

12. Desactive el **Habilitar control remoto** marca. Nunca debería ser necesario para el personal de soporte técnico para conectarse a las sesiones de esta cuenta para implementar las correcciones.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > Todos los objetos de Active Directory deben tener un propietario designado de TI y un propietario de la empresa, como se describe en [planeación de compromiso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Si está realizando el seguimiento de la propiedad de objetos de AD DS en Active Directory (en lugar de una base de datos externo), debe escribir la información de la propiedad adecuada en las propiedades de este objeto.  
    >
    > En este caso, el propietario de la empresa es muy probable que una división de TI, andthere no es prohibición de propietarios de empresas también que se va a los propietarios de TI. Es el punto de establecer la propiedad de objetos para que pueda identificar contactos cuando los cambios deben realizarse en los objetos, quizás años desde su creación inicial.  

13. Haga clic en el **organización** ficha.  

14. Escriba cualquier información necesaria en los estándares de objeto de AD DS.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. Haga clic en el **acceso telefónico** ficha.  

16. En el **permiso de acceso de red** campos, seleccione **denegar el acceso**. Esta cuenta nunca deben conectarse a través de una conexión remota.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > No es probable que esta cuenta se utilizará para iniciar sesión en controladores de dominio de solo lectura (RODC) en su entorno. Sin embargo, debería circunstancia nunca requieren que la cuenta para iniciar sesión en un RODC, debe agregar esta cuenta para el grupo de replicación de contraseñas de RODC denegado para que su contraseña no se almacena en caché en el RODC.  
    >
    > Aunque se debe restablecer la contraseña de la cuenta después de cada uso y la cuenta debe estar deshabilitada, implementar esta configuración no tiene un efecto negativo sobre la cuenta, y puede ser útil en situaciones en que un administrador se olvida de restablecer la cuenta contraseña y deshabilitarla.  

17. Haga clic en la ficha **Miembro de**.  

18. Haz clic en **Agregar**.  

19. Tipo **denegado grupo contraseñas en RODC** en el **Seleccionar usuarios, contactos, equipos** cuadro de diálogo y haga clic en **comprobar nombres**. Cuando el nombre del grupo está subrayado en el selector de objetos, haga clic en **Aceptar** y compruebe que la cuenta ahora es un miembro de los dos grupos que se muestra en la captura de pantalla siguiente. No agregue la cuenta a los grupos protegidos.  

20. Haga clic en **Aceptar**.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. Haga clic en el **seguridad** ficha y haga clic en **avanzadas**.  

22. En el **configuración de seguridad avanzada** cuadro de diálogo, haga clic en **deshabilitar herencia** y copiar los permisos heredados como permisos explícitos y haga clic en **agregar**.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. En el **entrada de permiso para [cuenta]** cuadro de diálogo, haga clic en **seleccionar una entidad de seguridad** y agregue el grupo que creó en el procedimiento anterior. Desplácese hasta la parte inferior del cuadro de diálogo y haga clic en **Borrar todo** para quitar todos los permisos predeterminados.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. Desplácese hasta la parte superior de la **entrada de permiso** cuadro de diálogo. Asegúrese de que el **tipo** lista desplegable se establece en **permitir**y en el **se aplica a** lista desplegable, seleccione **solo este objeto**.  

25. En el **permisos** campos, seleccione **leer todas las propiedades**, **permisos de lectura**, y **restablecer contraseña**.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. En el **propiedades** campos, seleccione **leer userAccountControl** y **escribir userAccountControl**.  

27. Haga clic en **Aceptar**, **Aceptar** nuevo en el **configuración de seguridad avanzada** cuadro de diálogo.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > El **userAccountControl** atributo controla varias opciones de configuración de cuenta. No se puede conceder permiso para cambiar solo algunas de las opciones de configuración cuando usted otorga permiso de escritura para el atributo.  

28. En el **los nombres de usuario o grupo** campo de la **seguridad** pestaña, quite los grupos que no deberían poder tener acceso o administrar la cuenta. No quite los grupos que se han configurado con las ACE de denegación, tales como el grupo todos y SELF calculan cuenta (esa ACE se estableció cuando la **usuario no puede cambiar la contraseña** se habilitó la marca durante la creación de la cuenta. También no quite el grupo que recién agregado, la cuenta del sistema o grupos como EA, DA, BA o el grupo de acceso de autorización de Windows.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. Haga clic en **avanzadas** y compruebe que el cuadro de diálogo Configuración de seguridad avanzada, tenga un aspecto similar a la captura de pantalla siguiente.  

30. Haga clic en **Aceptar**, y **Aceptar** otra vez para cerrar el cuadro de diálogo de propiedades de la cuenta.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. El programa de instalación de la primera cuenta de administración está completa ahora. Probará la cuenta en un procedimiento posterior.  

##### <a name="creating-additional-management-accounts"></a>Creación de cuentas de administración adicional

Puede crear cuentas de administración adicionales, repita los pasos anteriores, mediante la copia de la cuenta que recién creada o mediante la creación de una secuencia de comandos para crear cuentas con las opciones de configuración deseada. Sin embargo, tenga en cuenta que si copia la cuenta que recién creada, muchas de las configuraciones personalizadas y las ACL no se copiará a la nueva cuenta y tendrá que repetir la mayoría de los pasos de configuración.  
  
En su lugar, puede crear un grupo al que delegar derechos para rellenar y unpopulate grupos protegidos, pero necesita proteger el grupo y las cuentas que coloque en él. Ya debería haber muy pocas cuentas en el directorio que se conceden a la capacidad para administrar la pertenencia a grupos protegidos, la creación de cuentas individuales podría ser el enfoque más sencillo.  
  
Independientemente de cómo crear un grupo en los que coloca las cuentas de administración, debe asegurarse de que cada cuenta se protege como se describió anteriormente. También debe considerar implementar restricciones de GPO similares a las descritas en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
##### <a name="auditing-management-accounts"></a>Control de cuentas de administración

Debe configurar la auditoría en la cuenta para iniciar sesión, como mínimo, todas las escrituras a la cuenta. Esto permitirá que no sólo identifica correcta de habilitación de la cuenta y restablecer su contraseña durante los usos autorizados, pero además de identificar intentos de manipular la cuenta de usuarios no autorizados. Escrituras con error en la cuenta se deben capturar en el sistema de supervisión de eventos (SIEM) e información de seguridad (si procede) y deben desencadenar alertas que proporcionan notificaciones al personal responsable a investigar los posibles peligros.  
  
Las soluciones SIEM toman la información de eventos de orígenes de seguridad (por ejemplo, los registros de eventos, datos de aplicaciones, secuencias de red, productos contra código malintencionado y orígenes de detección de intrusiones), intercalan los datos e intentan realizar acciones proactivas y vistas inteligentes . Hay muchas soluciones SIEM comerciales, y muchas empresas creación implementaciones privadas. Una solución SIEM bien diseñado e implementado correctamente puede mejorar considerablemente las capacidades de respuesta a incidentes y supervisión de seguridad. Sin embargo, las capacidades y la precisión varían enormemente entre las soluciones. SIEM quedan fuera del ámbito de este artículo, pero se deben considerar las recomendaciones de evento específica contenidas por cualquier implementador SIEM.  
  
Para obtener más información acerca de los valores de configuración de auditoría recomendada para los controladores de dominio, consulte [supervisión de Active Directory para signos de peligro](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Se proporcionan valores de configuración específicos de controlador de dominio en [supervisión de Active Directory para signos de peligro](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Habilitación de las cuentas de administración modificar la pertenencia a grupos protegidos

En este procedimiento, configurará los permisos en el objeto AdminSDHolder del dominio para permitir que las cuentas de administración recién creado para modificar la pertenencia a grupos protegidos en el dominio. Este procedimiento no puede realizarse a través de una interfaz gráfica de usuario (GUI).  
  
Como se describe en [Apéndice C: Grupos y cuentas protegidas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), la ACL en AdminSDHolder del dominio en un objeto eficazmente "copia" en los objetos protegidos cuando se ejecuta la tarea SDProp. Cuentas y grupos protegidos no heredan sus permisos desde el objeto AdminSDHolder; sus permisos se establecen explícitamente para que coincida con los que están en el objeto AdminSDHolder. Por lo tanto, cuando se modifican los permisos en el objeto AdminSDHolder, debe modificar para los atributos que son adecuados para el tipo del objeto protegido tiene como destino.  
  
En este caso, se concederán las cuentas de administración recién creado para permitir que puedan leer y escribir los miembros de atributo en objetos de grupo. Sin embargo, el objeto AdminSDHolder no es un objeto de grupo y los atributos de grupo no se exponen en el editor gráfico de ACL. Es por esta razón que implementará los cambios de permisos a través de la utilidad de línea de comandos de Dsacls. Para conceder permisos de cuentas para modificar la pertenencia a grupos protegidos de la administración (deshabilitada), realice los pasos siguientes:  
  
1. Inicie sesión en un controlador de dominio, preferiblemente el controlador de dominio que contiene el rol de emulador de PDC (PDCE) con las credenciales de una cuenta de usuario que se ha realizado un miembro del grupo DA en el dominio.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. Abra un símbolo del sistema con privilegios elevados haciendo clic **símbolo** y haga clic en **ejecutar como administrador**.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. Cuando se le solicite que aprobar la elevación, haga clic en **Sí**.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > Para obtener más información acerca de elevación y cuenta control de usuario (UAC) en Windows, consulte [UAC procesos e interacciones](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) en el sitio Web de TechNet.  
  
4. En el símbolo del sistema, escriba (sustituyendo su información específica de dominio) **Dsacls [nombre distintivo del objeto AdminSDHolder en el dominio] /G [cuenta de administración UPN]: RPWP; miembro**.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   El comando anterior (que no distingue mayúsculas de minúsculas) funciona del siguiente modo:  
  
   - DSACLS establece o muestra las ACE en objetos de directorio  
  
   - CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifica el objeto que se puede modificar  
  
   - /G indica que se está configurando una ACE de concesión  
  
   - PIM001@tailspintoys.msft es el nombre Principal de usuario (UPN) de la entidad de seguridad a la que se otorgará las ACE  
  
   - Concede RPWP lee la propiedad y permisos de propiedad de escritura  
  
   - Miembro es el nombre de la propiedad (atributo) en que se establecerá los permisos  
  
   Para obtener más información acerca del uso de **Dsacls**, escriba Dsacls sin parámetros en un símbolo del sistema.  
  
   Si ha creado varias cuentas de administración para el dominio, debe ejecutar el comando Dsacls para cada cuenta. Cuando haya completado la configuración de ACL en el objeto AdminSDHolder, debe forzar SDProp para ejecutar o espere a que finalice su ejecución programada. Para obtener información acerca de cómo forzar SDProp para ejecutar, consulte "Ejecución SDProp manualmente" en [Apéndice C: Grupos de Active Directory y cuentas protegidas](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
   Cuando se haya ejecutado SDProp, puede comprobar que se aplicaron los cambios realizados en el objeto AdminSDHolder en grupos protegidos en el dominio. No se puede comprobar esto mediante la visualización de la ACL en el objeto AdminSDHolder por las razones descritas anteriormente, pero puede comprobar que se han aplicado los permisos mediante la visualización de las ACL en grupos protegidos.  
  
5. En **equipos y usuarios de Active Directory**, compruebe que ha habilitado **características avanzadas**. Para ello, haga clic en **vista**, busque el **Admins. del dominio** agrupar, haga clic en el grupo y haga clic en **propiedades**.  
  
6. Haga clic en el **seguridad** ficha y haga clic en **avanzadas** para abrir el **Advanced Security Settings for Admins. del dominio** cuadro de diálogo.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. Seleccione **permitir ACE para la cuenta de administración** y haga clic en **editar**. Compruebe que la cuenta tiene concedida sólo **leer miembros** y **escribir miembros** permisos en el grupo DA y haga clic en **Aceptar**.  
  
8. Haga clic en **Aceptar** en el **configuración de seguridad avanzada** cuadro de diálogo y haga clic en **Aceptar** otra vez para cerrar el cuadro de diálogo de propiedades para el grupo de DA.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Puede repetir los pasos anteriores para otros grupos protegidos en el dominio; los permisos deben ser los mismos para todos los grupos protegidos. Ahora ha completado la creación y configuración de las cuentas de administración para los grupos protegidos en este dominio.  
  
    > [!NOTE]  
    > Cualquier cuenta que tenga permiso para escribir la pertenencia de un grupo en Active Directory también puede agregar propio al grupo. Este comportamiento es así por diseño y no se puede deshabilitar. Por este motivo, siempre debe tener cuentas de administración deshabilitadas cuando no esté en uso y debe supervisar de cerca las cuentas cuando se está deshabilitado y cuando estén en uso.  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>Comprobación de grupo y la configuración de cuenta

Ahora que ha creado y configurado las cuentas de administración que pueden modificar la pertenencia a grupos protegidos en el dominio (que incluye los grupos con privilegios más elevados de EA, DA y BA), debe comprobar que han sido las cuentas y su grupo de administración ha creado correctamente. Comprobación consta de estas tareas generales:  
  
1.  El grupo que puede habilitar y deshabilitar cuentas de administración para comprobar que los miembros de grupo puede habilitan y deshabilitar las cuentas y restablecen sus contraseñas, pero no pueden realizar otras actividades administrativas en las cuentas de administración de la prueba.  
  
2.  Pruebe las cuentas de administración para comprobar que puede agregar y quitar miembros a grupos en el dominio protegidos, pero no pueden cambiar otras propiedades de cuentas protegidas y grupos.  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>El grupo que habilitar y deshabilitar cuentas de administración de prueba
  
1.  Para probar la habilitación de una cuenta de administración y restablecer su contraseña, inicie sesión en un seguro estaciones de administración con una cuenta que sea miembro del grupo que creó en [apéndice I: Creación de cuentas de administración para proteger cuentas y grupos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Abra **equipos y usuarios de Active Directory**, haga clic en la cuenta de administración y haga clic en **habilitar cuenta**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Debe mostrar un cuadro de diálogo, que confirma que se ha habilitado la cuenta.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  A continuación, restablecer la contraseña en la cuenta de administración. Para ello, haga clic en la cuenta de nuevo y haga clic en **restablecer contraseña**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Escriba una nueva contraseña para la cuenta en el **nueva contraseña** y **Confirmar contraseña** campos y haga clic en **Aceptar**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Aparecerá un cuadro de diálogo que confirma que se ha restablecido la contraseña de la cuenta.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Ahora intente modificar las propiedades adicionales de la cuenta de administración. Haga clic en la cuenta y haga clic en **propiedades**y haga clic en el **control remoto** ficha.  
  
8.  Seleccione **Habilitar control remoto** y haga clic en **aplicar**. Debe generar un error en la operación y un **acceso denegado** debe mostrar el mensaje de error.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Haga clic en el **cuenta** pestaña de la cuenta e intente cambiar el nombre de la cuenta, las horas de inicio de sesión o estaciones de trabajo de inicio de sesión. Todos deben producirá un error y opciones que no se controlan mediante la cuenta el **userAccountControl** atributo debe ser sombreado y disponibles para su modificación.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Intento de agregar el grupo de administración a un grupo protegido, como el grupo de DA. Al hacer clic en **Aceptar**, aparecerá un mensaje que le informa de que no tiene permisos para modificar el grupo.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Realizar pruebas adicionales según sea necesario para comprobar que no se puede configurar nada en la cuenta de administración, excepto **userAccountControl** configuración y los restablecimientos de contraseña.  
  
    > [!NOTE]  
    > El **userAccountControl** atributo controla varias opciones de configuración de cuenta. No se puede conceder permiso para cambiar solo algunas de las opciones de configuración cuando usted otorga permiso de escritura para el atributo.  
  
##### <a name="test-the-management-accounts"></a>La administración de cuentas de prueba

Ahora que ha habilitado una o varias cuentas que pueden cambiar la pertenencia a grupos protegidos, puede probar las cuentas para asegurarse de que puede modificar la pertenencia al grupo protegido, pero no se puede realizar otras modificaciones en cuentas protegidas y grupos.  
  
1.  Inicie sesión en un host administrativo seguro como la primera cuenta de administración.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Iniciar **equipos y usuarios de Active Directory** y busque el **grupo Admins. del dominio**.  
  
3.  Haga clic en el **Admins. del dominio** de grupo y haga clic en **propiedades**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  En el **propiedades de administradores de dominio**, haga clic en el **miembros** pestaña y **haga clic en** agregar. Escriba el nombre de una cuenta que tendrá privilegios de administradores de dominio temporales y haga clic en **comprobar nombres**. Cuando el nombre de la cuenta está subrayado, haga clic en **Aceptar** para volver a la **miembros** ficha.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  En el **miembros** la pestaña para el **propiedades de administradores de dominio** cuadro de diálogo, haga clic en **aplicar**. Después de hacer clic **aplicar**, la cuenta debe permanecer un miembro del grupo DA y no debe recibir ningún mensaje de error.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Haga clic en el **administrado por** pestaña en el **propiedades de administradores de dominio** diálogo cuadro y compruebe que no se puede escribir texto en todos los campos y todos los botones aparecen atenuados.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Haga clic en el **General** pestaña en el **propiedades de administradores de dominio** diálogo cuadro y compruebe que no se puede modificar la información acerca de esa pestaña.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Repita estos pasos para los grupos protegidos adicionales según sea necesario. Cuando haya terminado, inicie sesión en un host administrativo seguro con una cuenta que sea miembro del grupo que ha creado para habilitar y deshabilitar las cuentas de administración. A continuación, restablecer la contraseña en la cuenta de administración que acaba de probar y deshabilita la cuenta. Ha completado el programa de instalación de las cuentas de administración y el grupo que será responsable de la habilitación y deshabilitación de las cuentas.  
