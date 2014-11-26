Os-m-todos-setDefaultPushCallback-e-trackAppOpened-descontinuados
=================================================================

Os métodos setDefaultPushCallback e trackAppOpened foram descontinuados! E agora? 

O método `ParseAnalytics.trackAppOpened(getIntent())`, geralmente utilizado no OnCrete foi substituído pelo método `ParseAnalytics.trackAppOpenedInBackground(getIntent())`. Já o método `PushService.setDefaultPushCallback`, utilizado dentro da classe de Aplicaçao (extends Application) não teve um método para substitui-lo, mas sim uma implementação da Classe `ParsePushBroadcastReceiver`, como segue no exemplo a seguir:

    public class Receiver extends ParsePushBroadcastReceiver {
       @Override
       public void onPushOpen(Context context, Intent intent){
          Log.e("Push", "Aberto");
          Intent i = new Intent(context, MainActivity.class);
          i.putExtras(intent.getExtras());
          i.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
          context.startActivity(i);
       }
    }
Crie esta classe, lembrando de substituir MainActivity.class pela classe que deseja chamar ao abrir a notificação.

Agora, por último, deve-se alterar o ArdroidManifest:

Onde era:

    <receiver
            android:name="com.parse.ParsePushBroadcastReceiver"
            android:exported="false" >
            <intent-filter>
                <action android:name="com.parse.push.intent.RECEIVE" />
                <action android:name="com.parse.push.intent.DELETE" />
                <action android:name="com.parse.push.intent.OPEN" />
            </intent-filter>
    </receiver>
Agora fica:

    <receiver
            android:name="pacote.Receiver"
            android:exported="false" >
            <intent-filter>
                <action android:name="com.parse.push.intent.RECEIVE" />
                <action android:name="com.parse.push.intent.DELETE" />
                <action android:name="com.parse.push.intent.OPEN" />
            </intent-filter>
    </receiver>
Está é a substituição dos Métodos deprecated.
