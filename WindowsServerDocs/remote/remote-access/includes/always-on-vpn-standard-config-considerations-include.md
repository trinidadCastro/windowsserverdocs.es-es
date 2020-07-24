## <a name="standard-configuration-considerations"></a>Consideraciones sobre la configuración estándar

Always On VPN tiene muchas opciones de configuración. Sin embargo, si elige la configuración de VPN, incluya la siguiente información:

-   **Tipo de conexión.** La selección del Protocolo de conexión es importante y, en última instancia, va a mano con el tipo de autenticación que se va a usar. Para obtener más información acerca de los protocolos de túnel disponibles, consulte [tipos de conexión VPN](/windows/security/identity-protection/vpn/vpn-connection-type/).

-   **Automático.** En este contexto, las reglas de enrutamiento determinan si los usuarios pueden usar otras rutas de red mientras están conectadas a la VPN.

    -   La _tunelización dividida_ permite el acceso simultáneo a otras redes, como Internet.

    -   La _tunelización forzada_ requiere que todo el tráfico vaya exclusivamente a través de la VPN y no permita el acceso simultáneo a otras redes.

-   **Desencadenar.** La _activación_ determina cómo y cuándo se inicia una conexión VPN (por ejemplo, cuando se abre una aplicación, cuando el usuario activa el dispositivo, manualmente). Para ver las opciones de activación, consulte las [Opciones de Perfil de desencadenamiento automático de VPN](/windows/security/identity-protection/vpn/vpn-auto-trigger-profile/).

-   **Autenticación de dispositivo o de usuario.** Always On VPN usa certificados de dispositivo y una conexión iniciada por el dispositivo a través de una característica llamada [túnel de dispositivo](../vpn/vpn-device-tunnel-config.md). Esa conexión se puede iniciar automáticamente y es persistente, similar a una conexión de túnel de infraestructura de DirectAccess.

>[!TIP]
>Al migrar de DirectAccess a Always On VPN, considere la posibilidad de comenzar con las opciones de configuración que son comparables a las que tiene y, a continuación, expanda desde allí.

Mediante el uso de certificados de usuario, el cliente VPN de Always On se conecta automáticamente, pero lo hace en el nivel de usuario (después del inicio de sesión de usuario) en lugar de en el nivel de dispositivo (antes de que el usuario inicie sesión). La experiencia sigue siendo perfecta para el usuario, pero admite mecanismos de autenticación más avanzados, como Windows Hello para empresas.
