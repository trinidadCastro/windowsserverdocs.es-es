---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: 'Apéndice I: crear cuentas de administración para cuentas y grupos protegidos en Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 834aa2611ff2b965c9184524fa6782fb4477a4cd
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949134"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Anexo I: creación de cuentas de administración para cuentas protegidas y grupos en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uno de los desafíos de la implementación de un modelo de Active Directory que no se basa en la pertenencia permanente a grupos con privilegios elevados es que debe haber un mecanismo para rellenar estos grupos cuando se requiere la pertenencia temporal a los grupos. Algunas soluciones privileged Identity Management requieren la pertenencia permanente a las cuentas de servicio del software en grupos como DA o administradores en cada dominio del bosque. Sin embargo, técnicamente no es necesario que las soluciones Privileged Identity Management (PIM) ejecuten sus servicios en contextos con privilegios elevados.  
  
Este apéndice proporciona información que puede usar para soluciones PIM de terceros o implementadas de forma nativa para crear cuentas con privilegios limitados y que se pueden controlar rigurosamente, pero que se pueden usar para rellenar grupos con privilegios en Active Directory cuando se requiere una elevación temporal. Si está implementando PIM como una solución nativa, el personal administrativo puede usar estas cuentas para realizar el rellenado temporal del grupo y, si está implementando PIM mediante software de terceros, es posible que pueda adaptar estas cuentas para que funcionen como servicio. contabilidad.  
  
> [!NOTE]  
> Los procedimientos descritos en este apéndice proporcionan un enfoque para la administración de grupos con privilegios elevados en Active Directory. Puede adaptar estos procedimientos para que se adapten a sus necesidades, agregar restricciones adicionales u omitir algunas de las restricciones que se describen aquí.  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Creación de cuentas de administración para cuentas y grupos protegidos en Active Directory

La creación de cuentas que se pueden usar para administrar la pertenencia de grupos con privilegios sin necesidad de que se concedan derechos y permisos excesivos a las cuentas de administración consta de cuatro actividades generales que se describen en las instrucciones paso a paso que cumplir  
  
1.  En primer lugar, debe crear un grupo que administre las cuentas, ya que estas cuentas deben administrarse mediante un conjunto limitado de usuarios de confianza. Si aún no tiene una estructura de unidad organizativa que admita cuentas y sistemas protegidos con privilegios y protegidos de la población general del dominio, debe crear uno. Aunque en este Apéndice no se proporcionan instrucciones específicas, las capturas de pantallas muestran un ejemplo de este tipo de jerarquía de unidades organizativas.  
  
2.  Cree las cuentas de administración. Estas cuentas deben crearse como cuentas de usuario "normales" y no tienen derechos de usuario más allá de las que ya se les ha concedido a los usuarios de forma predeterminada.  
  
3.  Implemente restricciones en las cuentas de administración que les permitan usarse solo para el propósito especializado para el que se crearon, además de controlar quién puede habilitar y usar las cuentas (el grupo que creó en el primer paso).  
  
4.  Configure permisos en el objeto AdminSDHolder de cada dominio para permitir que las cuentas de administración cambien la pertenencia de los grupos con privilegios del dominio.  
  
Debe probar exhaustivamente todos estos procedimientos y modificarlos según sea necesario para su entorno antes de implementarlos en un entorno de producción. También debe comprobar que todas las configuraciones funcionan según lo previsto (en este apéndice se proporcionan algunos procedimientos de prueba) y debe probar un escenario de recuperación ante desastres en el que las cuentas de administración no estén disponibles para rellenar los grupos protegidos para la recuperación. Fiere. Para obtener más información sobre la copia de seguridad y la restauración de Active Directory, consulte la [Guía paso a paso de AD DS de copia de seguridad y recuperación](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Mediante la implementación de los pasos descritos en este apéndice, creará cuentas que podrán administrar la pertenencia a todos los grupos protegidos de cada dominio, no solo los grupos de Active Directory con privilegios más altos, como EAs, DAs y BAs. Para obtener más información acerca de los grupos protegidos en Active Directory, vea [Apéndice C: cuentas y grupos protegidos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Instrucciones paso a paso para crear cuentas de administración para grupos protegidos  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Creación de un grupo para habilitar y deshabilitar cuentas de administración

Las cuentas de administración deben restablecer sus contraseñas en cada uso y deben deshabilitarse cuando se completen las actividades que lo requieren. Aunque también puede considerar la posibilidad de implementar los requisitos de inicio de sesión de tarjeta inteligente para estas cuentas, es una configuración opcional y estas instrucciones suponen que las cuentas de administración se configurarán con un nombre de usuario y una contraseña larga y larga como mínimo. permite. En este paso, creará un grupo con permisos para restablecer la contraseña en las cuentas de administración y para habilitar y deshabilitar las cuentas.  
  
Para crear un grupo con el fin de habilitar y deshabilitar cuentas de administración, realice los pasos siguientes:  
  
1.  En la estructura de la unidad organizativa en la que va a alojar las cuentas de administración, haga clic con el botón secundario en la unidad organizativa en la que desea crear el grupo, haga clic en **nuevo** y en **Grupo**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  En el cuadro de diálogo **nuevo objeto-grupo** , escriba un nombre para el grupo. Si tiene previsto usar este grupo para "activar" todas las cuentas de administración del bosque, conviértalo en un grupo de seguridad universal. Si tiene un bosque de un solo dominio o si tiene previsto crear un grupo en cada dominio, puede crear un grupo de seguridad global. Haga clic en **Aceptar** para crear el grupo.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Haga clic con el botón secundario en el grupo que acaba de crear, haga clic en **propiedades**y, a continuación, haga clic en la pestaña **objeto** . En el cuadro de diálogo **propiedad de objeto** del grupo, seleccione **proteger objeto contra eliminación accidental**, que no solo impedirá que los usuarios autorizados en caso contrario eliminen el grupo, sino que también lo muevan a otra unidad organizativa, a menos que se anule la selección del atributo por primera vez.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > Si ya ha configurado permisos en las unidades organizativas principales del grupo para restringir la administración a un conjunto limitado de usuarios, es posible que no tenga que realizar los pasos siguientes. Se proporcionan aquí para que incluso si aún no ha implementado un control administrativo limitado sobre la estructura de la unidad organizativa en la que ha creado este grupo, puede proteger el grupo frente a modificaciones por parte de usuarios no autorizados.  
  
4.  Haga clic en la pestaña **miembros** y agregue las cuentas de los miembros del equipo que serán responsables de habilitar las cuentas de administración o de rellenar los grupos protegidos cuando sea necesario.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Si todavía no lo ha hecho, en la consola de **Active Directory usuarios y equipos** , haga clic en **Ver** y seleccione **características avanzadas**. Haga clic con el botón secundario en el grupo que acaba de crear, haga clic en **propiedades**y, a continuación, haga clic en la pestaña **seguridad** . En la pestaña **seguridad** , haga clic en **Opciones avanzadas**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  En el cuadro de diálogo **configuración de seguridad avanzada para [Grupo]** , haga clic en **deshabilitar herencia**. Cuando se le solicite, haga clic en **convertir permisos heredados en permisos explícitos en este objeto**y haga clic en **Aceptar** para volver al cuadro de diálogo **seguridad** del grupo.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  En la pestaña **seguridad** , quite los grupos a los que no se les permite el acceso a este grupo. Por ejemplo, si no desea que los usuarios autenticados puedan leer el nombre y las propiedades generales del grupo, puede quitar esa ACE. También puede quitar las ACE, como las de los operadores de cuentas y el acceso compatible con versiones anteriores a Windows 2000 Server. Sin embargo, debe dejar un conjunto mínimo de permisos de objeto en su lugar. Deje intactas las siguientes ACE:  
  
    -   SELF  
  
    -   SISTEMA  
  
    -   Admins. del dominio  
  
    -   Administradores de empresas  
  
    -   Administradores  
  
    -   Grupo de acceso de autorización de Windows (si procede)  
  
    -   CONTROLADORES DE DOMINIO DE EMPRESA  
  
    Aunque puede parecer muy intuitivo permitir que los grupos con más privilegios de Active Directory administren este grupo, el objetivo de implementar esta configuración no es impedir que los miembros de esos grupos realicen cambios autorizados. En su lugar, el objetivo es asegurarse de que, cuando tenga la ocasión de requerir niveles de privilegio muy elevados, los cambios autorizados se realizarán correctamente. Por esta razón, no se recomienda cambiar el anidamiento, los derechos y los permisos de los grupos con privilegios predeterminados en este documento. Al dejar intactas las estructuras predeterminadas y vaciar la pertenencia de los grupos de privilegios más altos en el directorio, puede crear un entorno más seguro que siga funcionando según lo previsto.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Si aún no ha configurado directivas de auditoría para los objetos de la estructura de la unidad organizativa en la que creó este grupo, debe configurar la auditoría para registrar los cambios en este grupo.  
  
8.  Ha completado la configuración del grupo que se usará para "extraer del repositorio" las cuentas de administración cuando sean necesarias y "proteger" las cuentas cuando se hayan completado sus actividades.  
  
#### <a name="creating-the-management-accounts"></a>Crear las cuentas de administración

Debe crear al menos una cuenta que se usará para administrar la pertenencia de grupos con privilegios en la instalación de Active Directory y, preferiblemente, una segunda cuenta para que sirva como copia de seguridad. Si decide crear las cuentas de administración en un único dominio del bosque y concederles capacidades de administración para todos los grupos protegidos de los dominios, o si decide implementar cuentas de administración en cada dominio del bosque, los procedimientos son es realmente el mismo.  
  
> [!NOTE]  
> En los pasos de este documento se supone que todavía no ha implementado los controles de acceso basado en roles y Privileged Identity Management para Active Directory. Por lo tanto, algunos procedimientos los debe realizar un usuario cuya cuenta sea miembro del grupo Admins. del dominio para el dominio en cuestión.  
>   
> Cuando se usa una cuenta con privilegios de DA, puede iniciar sesión en un controlador de dominio para realizar las actividades de configuración. Los pasos que no requieren privilegios de DA se pueden realizar mediante cuentas con menos privilegios que hayan iniciado sesión en estaciones de trabajo administrativas. Las capturas de pantalla que muestran cuadros de diálogo con borde en el color azul más claro representan las actividades que se pueden realizar en un controlador de dominio. Las capturas de pantalla que muestran cuadros de diálogo en el color azul más oscuro representan las actividades que se pueden realizar en las estaciones de trabajo administrativas con cuentas con privilegios limitados.  
  
Para crear las cuentas de administración, realice los pasos siguientes:  
  
1. Inicie sesión en un controlador de dominio con una cuenta que sea miembro del grupo de das del dominio.  

2. Inicie **Active Directory usuarios y equipos** y navegue hasta la unidad organizativa donde va a crear la cuenta de administración.  

3. Haga clic con el botón secundario en la unidad organizativa y haga clic en **nuevo** y en **usuario**.  

4. En el cuadro de diálogo **nuevo objeto-usuario** , escriba la información de nomenclatura deseada para la cuenta y haga clic en **siguiente**.  

   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. Proporcione una contraseña inicial para la cuenta de usuario, desactive el **usuario debe cambiar la contraseña en el siguiente inicio de sesión**, seleccione el **usuario no puede cambiar la contraseña** y la **cuenta está deshabilitada**y haga clic en **siguiente**.  

   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. Compruebe que los detalles de la cuenta sean correctos y haga clic en **Finalizar**.  

7. Haga clic con el botón secundario en el objeto de usuario que acaba de crear y haga clic en **propiedades**.  

8. Haga clic en la pestaña **Account** (Cuenta).  

9. En el campo **Opciones de cuenta** , seleccione la marca la **cuenta es importante y no se puede delegar** , seleccione la marca de cifrado la **cuenta compatible con Kerberos AES 128 bits** y/o **esta cuenta es compatible con Kerberos AES 256** y haga clic en **Aceptar**.  

   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > Dado que esta cuenta, al igual que otras cuentas, tendrá una función limitada pero eficaz, la cuenta solo debe usarse en hosts administrativos seguros. En el caso de todos los hosts administrativos seguros de su entorno, considere la posibilidad de implementar la directiva de grupo configuración de **seguridad de red: configurar tipos de cifrado permitidos para Kerberos** para permitir solo los tipos de cifrado más seguros que se pueden implementar para hosts seguros.  
   >
   > Aunque la implementación de tipos de cifrado más seguros para los hosts no mitiga los ataques de robo de credenciales, es el uso y la configuración adecuados de los hosts seguros. El establecimiento de tipos de cifrado más seguros para los hosts que solo usan las cuentas con privilegios reduce la superficie de ataque general de los equipos.  
   >
   > Para obtener más información acerca de la configuración de tipos de cifrado en sistemas y cuentas, consulte [configuraciones de Windows para el tipo de cifrado compatible con Kerberos](https://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
   >
   > Esta configuración solo se admite en equipos que ejecutan Windows Server 2012, Windows Server 2008 R2, Windows 8 o Windows 7.  
  
10. En la pestaña **objeto** , seleccione **proteger objeto contra eliminación accidental**. Esto no solo impedirá que se elimine el objeto (incluso los usuarios autorizados), pero impedirá que se mueva a otra unidad organizativa de la jerarquía de AD DS, a menos que un usuario con permiso para cambiar el atributo desactive primero la casilla.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. Haga clic en la pestaña **control remoto** .  

12. Desactive la marca **Habilitar control remoto** . Nunca debe ser necesario para que el personal de soporte técnico se conecte a las sesiones de esta cuenta para implementar correcciones.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > Todos los objetos de Active Directory deben tener un propietario de ti designado y un propietario empresarial designado, como se describe en [planeación de riesgos](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Si va a realizar un seguimiento de la propiedad de AD DS objetos en Active Directory (en oposición a una base de datos externa), debe escribir la información de propiedad adecuada en las propiedades de este objeto.  
    >
    > En este caso, lo más probable es que el propietario de la empresa sea una división de ti, andthere no es una prohibición para los propietarios de la empresa. El punto en el que se establece la propiedad de los objetos es permitirle identificar contactos cuando sea necesario realizar cambios en los objetos, quizás años desde su creación inicial.  

13. Haga clic en la pestaña **organización** .  

14. Escriba la información necesaria en los estándares del objeto AD DS.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. Haga clic en la pestaña **marcado** .  

16. En el campo **permiso de acceso a la red** , seleccione **denegar acceso**. Esta cuenta nunca debe tener que conectarse a través de una conexión remota.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > No es probable que se use esta cuenta para iniciar sesión en controladores de dominio de solo lectura (RODC) en su entorno. Sin embargo, si alguna vez necesita la cuenta para iniciar sesión en un RODC, debe agregar esta cuenta al grupo de replicación de contraseñas de RODC denegado para que su contraseña no se almacene en caché en el RODC.  
    >
    > Aunque la contraseña de la cuenta debe restablecerse después de cada uso y la cuenta debe estar deshabilitada, la implementación de esta configuración no tiene un efecto perjudicial en la cuenta y puede ayudar en situaciones en las que un administrador olvida restablecer la contraseña y deshabilitarla.  

17. Haga clic en la ficha **Miembro de**.  

18. Haz clic en **Agregar**.  

19. Escriba **denegado grupo de replicación de contraseña RODC** en el cuadro de diálogo **Seleccionar usuarios, contactos, equipos** y haga clic en **Comprobar nombres**. Cuando el nombre del grupo esté subrayado en el selector de objetos, haga clic en **Aceptar** y compruebe que la cuenta es ahora miembro de los dos grupos mostrados en la captura de pantalla siguiente. No agregue la cuenta a ninguno de los grupos protegidos.  

20. Haz clic en **Aceptar**.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. Haga clic en la pestaña **seguridad** y haga clic en **avanzadas**.  

22. En el cuadro de diálogo **configuración de seguridad avanzada** , haga clic en **deshabilitar herencia** y copie los permisos heredados como permisos explícitos y haga clic en **Agregar**.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. En el cuadro de diálogo **entrada de permiso para [cuenta]** , haga clic en **seleccionar una entidad** de seguridad y agregue el grupo que creó en el procedimiento anterior. Desplácese a la parte inferior del cuadro de diálogo y haga clic en **Borrar todo** para quitar todos los permisos predeterminados.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. Desplácese a la parte superior del cuadro de diálogo **entrada de permiso** . Asegúrese de que la lista desplegable **tipo** esté establecida en **permitir**y, en la lista desplegable **se aplica a** , seleccione **solo este objeto**.  

25. En el campo **permisos** , seleccione **leer todas las propiedades**, **permisos de lectura**y **Restablecer contraseña**.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. En el campo **propiedades** , seleccione **lectura** y **escritura de UserAccountControl**.  

27. Haga clic en **Aceptar**y en **Aceptar** de nuevo en el cuadro de diálogo **configuración de seguridad avanzada** .  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > El atributo **UserAccountControl** controla varias opciones de configuración de cuenta. No se puede conceder permiso para cambiar solo algunas de las opciones de configuración al conceder permiso de escritura al atributo.  

28. En el campo **nombres de grupos o usuarios** de la pestaña **seguridad** , quite todos los grupos a los que no se les permita el acceso o la administración de la cuenta. No quite los grupos configurados con ACE de denegación, como el grupo todos y la cuenta autocalculada (esa ACE se estableció cuando el **usuario no puede cambiar** la marca de contraseña se habilitó durante la creación de la cuenta. Tampoco Quite el grupo que acaba de agregar, la cuenta del sistema o grupos como EA, DA, BA o el grupo de acceso de autorización de Windows.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. Haga clic en **avanzadas** y compruebe que el cuadro de diálogo Configuración de seguridad avanzada tiene un aspecto similar al de la captura de pantalla siguiente.  

30. Haga clic en **Aceptar**y en **Aceptar** de nuevo para cerrar el cuadro de diálogo de propiedades de la cuenta.  

    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. La configuración de la primera cuenta de administración ya está completa. Probará la cuenta en un procedimiento posterior.  

##### <a name="creating-additional-management-accounts"></a>Creación de cuentas de administración adicionales

Puede crear cuentas de administración adicionales repitiendo los pasos anteriores, copiando la cuenta que acaba de crear o creando un script para crear cuentas con las opciones de configuración que desee. Tenga en cuenta, sin embargo, que si copia la cuenta que acaba de crear, muchas de las ACL y la configuración personalizada no se copiarán en la nueva cuenta y tendrá que repetir la mayoría de los pasos de configuración.  
  
En su lugar, puede crear un grupo al que delegue los derechos para rellenar y quitar los grupos protegidos, pero tendrá que proteger el grupo y las cuentas que coloque en él. Dado que debe haber muy pocas cuentas en el directorio que tengan la capacidad de administrar la pertenencia de los grupos protegidos, la creación de cuentas individuales podría ser el enfoque más sencillo.  
  
Independientemente de cómo decida crear un grupo en el que se colocan las cuentas de administración, debe asegurarse de que todas las cuentas estén protegidas tal y como se ha descrito anteriormente. También debe considerar la posibilidad de implementar restricciones de GPO similares a las descritas en el [Apéndice D: proteger cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
##### <a name="auditing-management-accounts"></a>Auditoría de cuentas de administración

Debe configurar la auditoría en la cuenta para registrar, como mínimo, todas las escrituras en la cuenta. Esto le permitirá no solo identificar la habilitación correcta de la cuenta y restablecer su contraseña durante los usos autorizados, sino también identificar los intentos de usuarios no autorizados de manipular la cuenta. Las escrituras erróneas en la cuenta deben capturarse en el sistema de supervisión de eventos y de información de seguridad (SIEM) (si es aplicable) y deben desencadenar alertas que proporcionen notificación al personal responsable de investigar posibles peligros.  
  
Las soluciones SIEM toman información de eventos de orígenes de seguridad implicados (por ejemplo, registros de eventos, datos de aplicaciones, secuencias de red, productos antimalware y orígenes de detección de intrusiones), intercalan los datos e intentan realizar vistas inteligentes y acciones proactivas. . Hay muchas soluciones de SIEM comerciales y muchas empresas crean implementaciones privadas. Un SIEM bien diseñado y correctamente implementado puede mejorar considerablemente las capacidades de supervisión de la seguridad y respuesta a los incidentes. Sin embargo, las capacidades y la precisión varían enormemente entre las soluciones. Los SIEM están fuera del ámbito de este documento, pero las recomendaciones de eventos específicos que contiene deben tener en cuenta cualquier implementador de SIEM.  
  
Para obtener más información acerca de las opciones de configuración de auditoría recomendadas para los controladores de dominio, consulte [supervisión Active Directory para ver los signos de riesgo](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Las opciones de configuración específicas del controlador de dominio se proporcionan en la [supervisión Active Directory los síntomas de riesgo](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Habilitación de cuentas de administración para modificar la pertenencia de grupos protegidos

En este procedimiento, configurará permisos en el objeto AdminSDHolder del dominio para permitir que las cuentas de administración recién creadas modifiquen la pertenencia de los grupos protegidos en el dominio. Este procedimiento no se puede realizar a través de una interfaz gráfica de usuario (GUI).  
  
Como se describe en el [Apéndice C: cuentas y grupos protegidos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), la ACL del objeto AdminSDHolder de un dominio se "copia" en objetos protegidos cuando se ejecuta la tarea SDProp. Los grupos y cuentas protegidos no heredan sus permisos del objeto AdminSDHolder; sus permisos se establecen explícitamente para que coincidan con los del objeto AdminSDHolder. Por lo tanto, cuando modifique permisos en el objeto AdminSDHolder, deberá modificarlos para los atributos que sean adecuados para el tipo del objeto protegido al que está destinada.  
  
En este caso, va a conceder las cuentas de administración que acaba de crear para permitirles leer y escribir el atributo Members en objetos de grupo. Sin embargo, el objeto AdminSDHolder no es un objeto de grupo y los atributos de grupo no se exponen en el editor gráfico de ACL. Por esta razón, se implementarán los cambios de permisos mediante la utilidad de línea de comandos DSACLS. Para conceder permisos a las cuentas de administración (deshabilitadas) para modificar la pertenencia de grupos protegidos, realice los pasos siguientes:  
  
1. Inicie sesión en un controlador de dominio, preferiblemente el controlador de dominio que contiene el rol de emulador de PDC (PDCE), con las credenciales de una cuenta de usuario que se ha convertido en miembro del grupo de DA en el dominio.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. Abra un símbolo del sistema con privilegios elevados haciendo clic con el botón secundario en **símbolo del sistema** y haga clic en **Ejecutar como administrador**.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. Cuando se le pida que apruebe la elevación, haga clic en **sí**.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > Para obtener más información acerca de la elevación y el control de cuentas de usuario (UAC) en Windows, consulte [procesos e interacciones de UAC](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) en el sitio web de TechNet.  
  
4. En el símbolo del sistema, escriba (sustituyendo la información específica del dominio) **DSACLS [nombre distintivo del objeto AdminSDHolder en el dominio]/g [nombre de la cuenta de administración UPN]: RPWP; miembro**.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   El comando anterior (que no distingue entre mayúsculas y minúsculas) funciona de la siguiente manera:  
  
   - DSACLS establece o muestra ACE en objetos de directorio  
  
   - CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifica el objeto que se va a modificar.  
  
   - /G indica que se está configurando una ACE Grant  
  
   - PIM001@tailspintoys.msft es el nombre principal de usuario (UPN) de la entidad de seguridad a la que se concederán las ACE  
  
   - RPWP concede permisos de propiedad de lectura y de escritura  
  
   - Miembro es el nombre de la propiedad (atributo) en la que se establecerán los permisos.  
  
   Para obtener más información acerca del uso de **DSACLS**, escriba DSACLS sin ningún parámetro en el símbolo del sistema.  
  
   Si ha creado varias cuentas de administración para el dominio, debe ejecutar el comando DSACLS para cada cuenta. Cuando haya completado la configuración de ACL en el objeto AdminSDHolder, debe forzar la ejecución de SDProp o esperar a que se complete la ejecución programada. Para obtener información sobre cómo forzar la ejecución de SDProp, vea "ejecutar SDProp manualmente" en el [Apéndice C: cuentas y grupos protegidos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
   Cuando se ejecuta SDProp, puede comprobar que los cambios realizados en el objeto AdminSDHolder se han aplicado a los grupos protegidos del dominio. No puede comprobarlo mediante la visualización de la ACL en el objeto AdminSDHolder por las razones descritas anteriormente, pero puede comprobar que los permisos se han aplicado mediante la visualización de las ACL en los grupos protegidos.  
  
5. En **Active Directory usuarios y equipos**, compruebe que ha habilitado **las características avanzadas**. Para ello, haga clic en **Ver**, busque el grupo **Admins** . del dominio, haga clic con el botón secundario en el grupo y haga clic en **propiedades**.  
  
6. Haga clic en la pestaña **seguridad** y haga clic en **avanzadas** para abrir el cuadro **de diálogo Configuración de seguridad avanzada para administradores del dominio** .  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. Seleccione **permitir ACE para la cuenta de administración** y haga clic en **Editar**. Compruebe que la cuenta solo tiene permisos de **lectura de miembros** y de **escritura** en el grupo de da y haga clic en **Aceptar**.  
  
8. Haga clic en **Aceptar** en el cuadro de diálogo **configuración de seguridad avanzada** y haga clic en **Aceptar** de nuevo para cerrar el cuadro de diálogo de propiedades del grupo de da.  
  
   ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Puede repetir los pasos anteriores para otros grupos protegidos en el dominio; los permisos deben ser los mismos para todos los grupos protegidos. Ahora ha completado la creación y configuración de las cuentas de administración de los grupos protegidos en este dominio.  
  
    > [!NOTE]  
    > Cualquier cuenta que tenga permiso para escribir la pertenencia de un grupo en Active Directory también puede agregarse a sí mismo al grupo. Este comportamiento es así por diseño y no se puede deshabilitar. Por esta razón, siempre debe mantener las cuentas de administración deshabilitadas cuando no estén en uso y debe supervisar atentamente las cuentas cuando están deshabilitadas y cuando están en uso.  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>Comprobar la configuración de grupos y cuentas

Ahora que ha creado y configurado cuentas de administración que pueden modificar la pertenencia de grupos protegidos en el dominio (que incluye los grupos EA, DA y BA con mayor privilegios), debe comprobar que las cuentas y su grupo de administración se han creado correctamente. La comprobación consta de estas tareas generales:  
  
1.  Probar el grupo que puede habilitar y deshabilitar las cuentas de administración para comprobar que los miembros del grupo pueden habilitar y deshabilitar las cuentas y restablecer sus contraseñas, pero no pueden realizar otras actividades administrativas en las cuentas de administración.  
  
2.  Pruebe las cuentas de administración para comprobar que pueden agregar y quitar miembros de grupos protegidos en el dominio, pero no puede cambiar otras propiedades de cuentas y grupos protegidos.  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Probar el grupo que habilitará y deshabilitará las cuentas de administración
  
1.  Para probar la habilitación de una cuenta de administración y el restablecimiento de su contraseña, inicie sesión en una estación de trabajo administrativa segura con una cuenta que sea miembro del grupo que creó en el [Apéndice I: creación de cuentas de administración para cuentas y grupos protegidos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Abra **Active Directory usuarios y equipos**, haga clic con el botón secundario en la cuenta de administración y haga clic en **Habilitar cuenta**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Se debe mostrar un cuadro de diálogo que confirma que se ha habilitado la cuenta.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Después, restablezca la contraseña en la cuenta de administración. Para ello, vuelva a hacer clic con el botón secundario en la cuenta y haga clic en **Restablecer contraseña**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Escriba una nueva contraseña para la cuenta en los campos **nueva contraseña** y **Confirmar contraseña** y haga clic en **Aceptar**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Debe aparecer un cuadro de diálogo para confirmar que se ha restablecido la contraseña de la cuenta.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Ahora intente modificar las propiedades adicionales de la cuenta de administración. Haga clic con el botón secundario en la cuenta y haga clic en **propiedades**y en la pestaña **control remoto** .  
  
8.  Seleccione **Habilitar control remoto** y haga clic en **aplicar**. Se debe producir un error en la operación y se debe mostrar un mensaje de error de **acceso denegado** .  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Haga clic en la pestaña **cuenta** de la cuenta e intente cambiar el nombre de la cuenta, las horas de inicio de sesión o las estaciones de trabajo de inicio de sesión. All debe producir un error, y las opciones de cuenta que no están controladas por el atributo **UserAccountControl** deberían estar atenuadas y no estar disponibles para su modificación.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Intente agregar el grupo de administración a un grupo protegido, como el grupo de DA. Al hacer clic en **Aceptar**, debe aparecer un mensaje que le informa de que no tiene permisos para modificar el grupo.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Realice pruebas adicionales según sea necesario para comprobar que no se puede configurar nada en la cuenta de administración, excepto en los valores de **UserAccountControl** y los restablecimientos de contraseña.  
  
    > [!NOTE]  
    > El atributo **UserAccountControl** controla varias opciones de configuración de cuenta. No se puede conceder permiso para cambiar solo algunas de las opciones de configuración al conceder permiso de escritura al atributo.  
  
##### <a name="test-the-management-accounts"></a>Prueba de las cuentas de administración

Ahora que ha habilitado una o varias cuentas que pueden cambiar la pertenencia de los grupos protegidos, puede probar las cuentas para asegurarse de que pueden modificar la pertenencia a grupos protegidos, pero no pueden realizar otras modificaciones en cuentas y grupos protegidos.  
  
1.  Inicie sesión en un host administrativo seguro como la primera cuenta de administración.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Inicie **Active Directory usuarios y equipos** y busque el **grupo Admins**. del dominio.  
  
3.  Haga clic con el botón secundario en el grupo **Admins** . del dominio y haga clic en **propiedades**.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  En las **propiedades Admins**. del dominio, haga clic en la pestaña **miembros** y **haga clic en** agregar. Escriba el nombre de una cuenta a la que se concederán privilegios temporales de Admins. del dominio y haga clic en **Comprobar nombres**. Cuando el nombre de la cuenta esté subrayado, haga clic en **Aceptar** para volver a la pestaña **miembros** .  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  En la pestaña **miembros** del cuadro de diálogo **propiedades de Admins** . del dominio, haga clic en **aplicar**. Después de hacer clic en **aplicar**, la cuenta debe permanecer en un miembro del grupo de da y no recibirá ningún mensaje de error.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Haga clic en la pestaña **administrado por** del cuadro de diálogo **propiedades de Admins** . del dominio y compruebe que no puede escribir texto en ningún campo y que todos los botones estén atenuados.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  En el cuadro de diálogo **propiedades de Admins** . del dominio, haga clic en la pestaña **General** y compruebe que no puede modificar ninguna información de la ficha.  
  
    ![creación de cuentas de administración](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Repita estos pasos para los grupos protegidos adicionales según sea necesario. Cuando haya terminado, inicie sesión en un host administrativo seguro con una cuenta que sea miembro del grupo que ha creado para habilitar y deshabilitar las cuentas de administración. Después, restablezca la contraseña en la cuenta de administración que acaba de probar y deshabilite la cuenta. Ha completado la configuración de las cuentas de administración y el grupo que será responsable de habilitar y deshabilitar las cuentas.  
