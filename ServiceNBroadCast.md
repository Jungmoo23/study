Service:오래 실행되는 작업을 위한 것으로 화면(앞단)에서 실행되는 것이 아니라 화면 뒤(뒷단)에서 실행됩니다.
화면이 없으니 동작하는 방식도 액티비티와 다름.

서비스란 백그라운드에서 실행되는 앱의 구성 요소를 말합니다.
서비스는 시스템에서 관리하므로 매니페스트에 넣어 앱 설치 시에 시스템이 알 수 있게 해야 합니다.

서비스의 실행원리와 역할
서비스를 실행하려면 메인 액티비티에 startService 메서드를 호출하면 됩니다.
서비스 중요 역할 중 하나는 단말이 항상 실행 되어 있는 상태로 다른 단말과 데이터를 주고 받거나 필요한 기능을 백그라운드에서 
실행 하는 것입니다. 그래서 서비스는 실행 상태를 계속 유지하기 위해 서비스가 비정상적으로 종료되더라도 시스템이 자동으로 
재 실행 합니다.

서비스를 시작시키기 위해 startService 메서드를 호출할 때는 인텐트 객체를 파라미터로 전달합니다.
인텐트 객체는 어떤 서비스를 실행할 것인지에 대한 정보를 담고 있으며 시스템은 서비스를 시작시킨 후
인텐트 객체를 서비스에 전달합니다.

서비스가 실행중이면 실행 이후에 startService 메서드를 여러 번 호출해도 서비스는 이미 메모리에 만들어진 상태로 유지됩니다.
startService 메서드는 서비스를 시작하는 목적 이외에 인텐트를 전달하는 목적으로도 자주 사용됩니다. 예를 들어, 액티비티에서 서비스로 데이터를 전달하려면 인텐트 객체를 만들고 부가 데이터(Extra data)를 넣은 후 startService 메서드를 호출하면서 전달 하면 됩니다. 앞에서 가정하고 있는 상태는 서비스가 startService 메서드에 의하여 메모리에  만들어져 있는 상태입니다. 이런 경우에는 시스템이 onCreate 메서드가 아니라 onStartCommand메서드를 실행 한다. onStartCommand 메서드는 서비스로 전달된 인텐트 객체를 처리하는 메서드 이다.

서비스가 서버 역할을 하면서 액티비티와 연결될 수 있도록 만드는 것을 바인딩이라고 한다.
서비스를 만들며누 생성자 메서드와 onBind 메서드만 있음

Class MyService extends Service{
	MyService(){}
	
	public void onCreate(){
	
]
	private void process(Intent intent){
	Intent showIntent =new Intent(getApplicationContext(), MainActivity.class)
	showIntent.addflag(intent.FLAG_ACVITY_NEW_TASK |
					   INTENT.FALG_ACTIVITY_SINGLE_TOP | 
					  INTENT.FLAG_ACTIVITY_CLEAR_TOP)

	showIntent.putExtra();
        showIntent.putExtra();
	startActivity(showIntent)
}

서비스에서 startActivity 메서드를 호출할 때는 새로운 태스크를 생성하도록 Flag_Activity_new_Task 플래그를 인텐트에 추가해야 합니다. 서비스는 화면이 없기 때문에 화면이 없는 서비스에서 화면이 있는 액티비티를 띄우려면 새로운 태스크를 만들어야 함.

MainActivity 객체가 이미 메모리에 만들어져 있을 때 재사용하도록 
FLAG_ACTIVITY_SINGLE_TOP과 FLAG_ACTIVITY_CLEAR_TOP 사용

onCreate(): MainActivit가 메모리가 만들어져 있지 않은 상태에서 처음 만들어진다면
Oncrreate 메서드 안에서 getIntent 메서드를 호출하여 인텐트 객체를 참조합니다.


onNewIntent(Intent intent)
MainActivity 가 이미 메모리에 올라가 있으면 onCreate가 호출되자 않고 onNewIntent가 호출이 된다.

IntentServcie: 인텐트 서비스는 지금까지 살펴본 서비스와 달리 필요한 함수가 수행되고 나면 종료가 됨.
즉, 백그라운드에서 실행되는 것은 같지만 길게 지속되는 서비스라기보다는 한 번 실행되고 끝나는 작업을 수행할 때 사용합니다.

06-2 브로드캐스트 수신자 이해하기

브로드캐스팅: 메시지를 여러 객체에 전달하는 것을 말함.
예시) 다른 사라믕로부터 문자를 받았을 때 이 문자를 sms 수신 앱에 알려줘야 한다면 브로드캐스팅으로 전달하면 됩니다. 
이런 메시지 전달 방식은 단말 전체에 적용될 수 있겠죠? 그래서 이런 메시지 전달 방식을 글로버르 이벤트라고 부릅니다.
글로벌 이벤트의 예시로는 ”전화가 왔습니다”, “문자 메시지가 도착했습니다.”

브로드캐스트 수신자는 매니페스트 등록 방식이 아닌 소스 코드에서 regisiterReceiver 메서드를 사용해 등록할 수 있음.

명시적 브로드캐스트
	     - Receiver 를 고정해서 등록해 놓고 원하는 방송만 수신하는 receiver 다.
- 사용자가 직접 AndroidManifest.xml 파일에 receiver 를 등록하는 방식이다.
- 한번 등록하면 해제할 수 없다.
- 해당 앱이 설치될 때 자동으로 등록된다.
암시적 브로드캐스트: 안드로이드 API 26이상에서는 암시적 브로드캐스트 사용 불가

브로드캐스트 수신자 등록하고 사용하기
브로드캐스트 수신자에는 onReceive 메서드를 정의해야 합니다.

Receiver 태그 안에 Intent Filter 를 넣어 어떤 인텐트를 받을 것인지 지정한다.
Intent Filter을 넣어 액션을 추가.


Code

public class MainActivity extends AppCompatActivity {

    private BroadcastReceiver mReceiveer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mReceiveer = new MyReceiver();
    }

    @Override
    protected void onResume() {
        super.onResume();
        IntentFilter filter = new IntentFilter();
        filter.addAction(Intent.ACTION_POWER_CONNECTED);
        //로컬로 등록
        filter.addAction(MyReceiver.MyAction);
        registerReceiver(mReceiveer, filter);
    }

    @Override
    protected void onPause() {
        super.onPause();
        unregisterReceiver(mReceiveer);
    }

    public void sendMyBroadCast(View view) {
        //액션을 담아 리시버로 인텐트를 발송
        Intent intent = new Intent(MyReceiver.MyAction);
        sendBroadcast(intent);
    }
}

public class MyReceiver extends BroadcastReceiver {

    public final static String MyAction = "hello.world.broadcastreceiverexam.ACTION_MY_BROADCAST";

    @Override
    public void onReceive(Context context, Intent intent) {
        // TODO: This method is called when the BroadcastReceiver is receiving
        // an Intent broadcast.
        //각 방송 정보는 intent로 전달됨
        if (Intent.ACTION_POWER_CONNECTED.equals(intent.getAction())) {
            Toast.makeText(context, "전원 연결", Toast.LENGTH_SHORT).show();
        }
        else if(MyAction.equals(intent.getAction())) {
            Toast.makeText(context, "방송", Toast.LENGTH_SHORT).show();
        }
    }
}


Receiver로 브로드캐스트 전달
브로드캐스트를 전달할 때는 두가지 방법이 있습니다.
* sendBroadcast(Intent): 리시버들에게 순서 없이 브로드캐스트를 전달합니다. 리시버들에게 거의 동시에 전달되기 때문에 전달 속도가 빠릅니다.
* sendOrderedBroadcast(Intent, String): 한번에 하나의 리시버에게 브로드캐스트를 전달합니다. IntentFilter의 android:priority의 값을 참고하여 순서를 정하고 이 순서대로 전달합니다. 이전에 보낸 리시버가 모든 처리를 끝내야 다음 리시버로 이벤트를 전달하기 때문에 전달 속도가 느립니다. 이전에 처리한 리시버의 결과를 읽을 수 있으며, 다음 리시버로 전달이 안되도록 막을 수 있습니다.
단순이 이벤트를 전달하는 것이 목적이라면 sendBroadcast()를 사용하시면 됩니다. 이 메소드들은 Activity(Context)에 구현되어있습니다.


암시적인 방법으로 브로드캐스트 전달
암시적인 방법이란 의미는 Framework이 인텐트와 관련된 모든 리시버를 찾고 그 리시버들에게 모두 전달해주는 것을 의미합니다.
아래와 같이 전달할 인텐트를 생성하고 sendBroadcast()의 인자로 전달하면 이 인텐트를 수신하는 모든 리시버에게 전달됩니다.
val intent = Intent("com.codechacha.action.test")
sendBroadcast(intent)

명시적인 방법으로 브로드캐스트 전달
명시적인 방법이란 의미는 브로드캐스트를 전달하는 앱에서 지정한 앱의 리시버에게만 이벤트를 전달하는 것을 의미합니다.
다음과 같이 Intent에 보내고 싶은 앱의 package name을 설정하면 그 앱의 리시버에게만 전달됩니다.
val intent = Intent("com.codechacha.action.test")
intent.setPackage("com.codechacha.broadcastreceiver")
sendBroadcast(intent)
