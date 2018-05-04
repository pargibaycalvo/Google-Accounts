# Google-Accounts
Use of gmail accounts in app Android 

Vamos a ver como hacer un login mediante nuestras cuentas añadidas al teléfono móvil, en este caso usaremos las de gmail Google

https://developers.google.com/android/reference/com/google/android/gms/common/AccountPicker

En el AndroidManifest.xml tenemos que darle permisos a la app para que pueda ver las cuentas:

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission
        android:name="android.permission.MANAGE_ACCOUNTS"
        tools:ignore="ProtectedPermissions" />
    <uses-permission
        android:name="android.permission.AUTHENTICATE_ACCOUNTS"
        tools:ignore="ProtectedPermissions" />
    <uses-permission
        android:name="android.permission.GET_ACCOUNTS"
        tools:ignore="ProtectedPermissions" />
    <uses-permission
        android:name="android.permission.ACCOUNT_MANAGER"
        tools:ignore="ProtectedPermissions" />

Pasamos a la MainActivity y declaramos:

    private static final int PICK_ACCOUNT_REQUEST = 0;
    
Al declarar este PICK lo que nos va a obligar es a escoger entre una de las cuentas que nos mostrará en la pantalla, de esta forma automáticamente pasará a una función llamada onActivityResult.
El onActivityResult será la encargada de realizar las demás funciones que queramos para nuestra app, lo mismo que haríamos si tenemos una ventana de login y pasamos a ver datos pero con la diferencia de hacerlo mediante la cuenta de gmail.

Yo en este caso utilizo un botón que recoge todas las cuentas "com.google" que yo tengo sincronizadas en el móvil, cabe decir que nos podemos encontrar más funciones para la reocogida de cuentas desde la app:

        // Button to execute the login from gmail account
        btngmail.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                try{
                    Intent googlePicker = AccountManager.newChooseAccountIntent(null, null,
                            new String[]{"com.google"}, true, null, null, null, null);
                    startActivityForResult(googlePicker, PICK_ACCOUNT_REQUEST);
                } catch (ActivityNotFoundException ex) {
                    Toast.makeText(MainActivity.this, "Error de sincronización en las cuentas: " + ex.getMessage(), Toast.LENGTH_LONG);
                }
            }
        });
        
Al ejecutarlo obtendríamos las cuentas con extensión "com.google" o no, pero falta saber si realmente esta cuenta nos está funcionando bien, para ello necestiamos la función comentada anteriormente la onActivityResult:

    // Function that complements the registry by gmail
    @SuppressLint("WrongConstant")
    @Override
    protected void onActivityResult(final int requestCode, final int resultCode, final Intent data) {
        if (requestCode == PICK_ACCOUNT_REQUEST && resultCode == RESULT_OK) {
            useremail = data.getStringExtra(AccountManager.KEY_ACCOUNT_NAME);
            Toast.makeText(this,"Cuenta sincronizada con éxito",3000).show();
        }else{
            Toast.makeText(this,"No hay cuentas sincronizadas",3000).show();

        }
    }

Con todo esto ya obtendríamos las cuentas vínculadas en nuestro teléfono y podríamos hacer login desde nuestra app.

##Se puede probar a cambiar la extensión del método "com.google" por otro, para ver si este indica cuentas ajenas a Google.
