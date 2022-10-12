위험 권한 부여하기
매니페스트에 넣어준 권한은 앱을 설치할 때 사용자가 허용하면 한꺼번에 권한이 부여되는데 마시멜로(API 23) 부터는 
중요한 권한들을 분류하여 설치 시점이 아니라 앱을 실행 했을 때 사용자로부터 권한을 부여받도록 변경되었습니다.

일반 권한과 위험 권한의 차이점 알아보기

마시멜로 버전부터는 권한과 위험 권한으로 나누었습니다.
앱을 설치하는 시점에 사용자에게 물어보는 기존의 방식은 사용자가 아무런 생각 없이 앱을 설치하는 경우가 많았으며 이에 따라
설치된 앱들이 단말의 주요 기능을 마음대로 사용할 수 있었습니다. 이 때문에 위험 권한으로 분류된 권한들에 대해서는 앱을 설치할 때가 아니라 앱을 실행할 때 권한을 부여하도록 변경된 것이다.

예을들어 인터넷을 사용할 때 부여하는 인터넷 권한은 일반 권한입니다. 따라서 앱을 설치할 때 사용자에게 부여되어야 함을 알려주고 설치할 것인지 물어 봅니다. 그러나 위험 권한으로 분류되는 RECEIVE_SMS 의 경우에는 설치 시에 부여한 권한은 의미가 없으며 실행 시 사용자에게 권한을 부여할 것인지 물어보게 됩니다. 만약 사용자 권한을 부여 하지 않으면 해당 기능은 동작하지 않습니다.
즉 앱을 설치했다고 하더라도 권한에 따라 생성할 수 있는 기능에 제약이 생기는 것입니다.

위험 권한으로 분류된 주요 권한들을 보면 대부분 개인정보가 담겨있는 정보에 접근하거나 개인정보를 만들어 낼 수 있는 단말의 주요 장치에 접근할 때 부여된다는 것을 알 수 있습니다. 위치, 카메라, 마이크, 연락처, 전화, 문자, 일정, 센서로 대표되는 위험 권한은 다음과 같은 세부 권한으로 나뉩니다.

매니페스트에 권한 부여
기본 권한을 부여할 때는 <user-permission>

코드로 권한 부여

MainActivity.java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        // 위험 권한을 부여할 권한 지정
        String[] permissions = {
                Manifest.permission.READ_EXTERNAL_STORAGE,
                Manifest.permission.WRITE_EXTERNAL_STORAGE
        };
        
        checkPermissions(permissions);
    }
    
    public void checkPermissions(String[] permissions) {
        ArrayList<String> targetList = new ArrayList<String>();
        
        for (int i = 0; i < permissions.length; i++) {
            String curPermission = permissions[i];
            int permissionCheck = ContextCompat.checkSelfPermission(this, curPermission);
            if (permissionCheck == PackageManager.PERMISSION_GRANTED) {
                Toast.makeText(this, curPermission + " 권한 있음", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(this, curPermission + " 권한 없음", Toast.LENGTH_SHORT).show();
                if (ActivityCompat.shouldShowRequestPermissionRationale(this, curPermission)) {
                    Toast.makeText(this, curPermission + " 권한 설명 필요함.", Toast.LENGTH_SHORT).show();
                } else {
                    targetList.add(curPermission);
                }
            }
        }
        
        String[] targets = new String[targetList.size()];
        targetList.toArray(targets);
        
        // 위험 권한 부여 요청
        ActivityCompat.requestPermissions(this, targets, 101);
    }
}

ContextCompat: ContextCompat은 Resource에서 값을 가져오거나 퍼미션을 확인할 때 사용할 때 SDK버전을 고려하지 않아도 되도록 (내부적으로 SDK버전을 처리해둔) 클래스입니다. 

    public static int getColor(@NonNull Context context, @ColorRes int id) {
        if (Build.VERSION.SDK_INT >= 23) {
            return context.getColor(id);
        } else {
            return context.getResources().getColor(id);
        }
    }

ActivityCompat 과 ContextCompat 차이
￼

특정 시점에 위험 권한을 부여하는 것도 가능하다.
- 위험 권한을 부여하는 것은 앱이 실행 중이라면 언제든 가능합니다. 예을 들어 액티비티가 메모리에 만들어지는 시점에 부여되도록 하려면 onCreate 메서드 안에서 권한 부여 요청을 하지만 버튼이 눌렀을 때 권한이 부여되게 할 수도 있습니다.

위험 권한 자동 부여 방법

private void requestPermission() {
  AndPermission.with(this)
      .runtime()
      .permission(Permission.Group.STORAGE)
      .onDenied(new Action<List<String>>() {
        @Override
        public void onAction(List<String> list) {
          finish();
        }
      })
      .onGranted(new Action<List<String>>() {
        @Override
        public void onAction(List<String> list) {
          App.get().initialize();
          tryLogin();
        }
      })
      .start();
}

Code Review: 매니페스트 파일 안에 넣은 권한 중에서 위험 권한을 자동으로 체크한 후 권한 부여를 요청 하는 방식.
onGranted 소괄호 안에 전달된 객체의 onAction 메서드로 전달 받습니다. 권한 부여 결과는 승인 또는 거부로 나뉘는데 이 때문에 onGranted 외에도 onDenied 메서드의 onAction 메서드가 호출 될수 있습니다.
권한이 여러개 일 경우 onGranted, onDenied로 나뉘어 호출됩니다.

리소스와 매니페스트 이해하기

안드로이드 앱은 크게 자바 코드와 리소스로 구성됩니다. 자바 코드에서는 앱의 흐름과 기능을 정의하고 리소스에서는 레이아웃이나 이미지 처럼 사용자에게 보여주기 위해 사용하는 파일이나 데이터를 관리합니다.
지금까지 XML 레이아웃이나 이미지를 보여줄 때 사용해 보았으므로 res 폴더 안에 들어 있는 리소스가 낯설지는 않을 것입니다.

매니페스트: 앱의 구성 요소가 어떤 것인지, 그리고 어떤 권한이 부여 되었는지 시스템에 알려주기 때문에 매우 중요.
모든 안드로이드 앱은 가장 상위 폴더에 매니페스트 파일이 있어야 하며, 이 정보는 앱이 실행되기 전에 시스템이 알아야 할 내용들을 정의하고 있습니다. 

매니페스트의 주요 역할
- [ ] 앱의 패키지 이름 지정
- [ ] 앱 구성 요서에 대한 정보 등록(액티비티, 서비스, 브로드캐스트 수신자, 내용 제공자)
- [ ] 각 구성 요소를 구현하는 클래스 이름 지정
- [ ] 앱이 가져야 하는 권한에 대한 정보 등록
- [ ] 다른 앱이 접근하기 위해필요한 권한에 대한 정보 등록\
- [ ] 앱 개발 과정에서 프로파일링으르 위해 필요한 instrumen-tation 클래스 등록
- [ ] 앱에 필요한 안드로이드 API의 레벨 정보 등록
- [ ] 앱에서 사용하는 라이브러리 리스트

매니페스트에 <Application> 태그는 매니페스트 안에 반드시 하나만 있어야 합니다.

메인 액티비티는 항상 다음과 같은 형태로 추가되어야 합니다. 즉, 인텐트 필터에 들어가는 정보는<action>
태그의 경우 Main이 되어야 하고 <categoty> 태그의  경우 LAUNCHER가 되어야  합니다.

 리소스의 사용
/app/res:  빌드되어 설치 파일에 추가되지만 애셋은 빌드되지 않습니다.
/app/Asset: 동영상이나 웹페이지와 같이 용량이 큰 데이터를 의미함


리소스는 /app/res/ 폴더 밑에 있는 여러 가지 폴더에 나누어 저장되며 리소스 유형별로 서로 다른 폴더에 저장합니다. 프로젝트를 처음 만들면 몇 개의 폴더만 들어 있ㅈ는데 실제 만들 수 있는 폴더는 훨씬 많아서 필요한 경우에 만들어 사용해야 합니다. 리소스가 갱신되면 그때마다 리소스의 정보가 R.java 파일에 자동으로 기록되며 그 정보는 리소스에 대한 내부적인 포인터 정보가 됩니다.

리소스의 유형마다 다른 폴더에 넣어주어야 합니다.
Why: 리소호스는 그 유형에 따라 정해진 폴더 안에 넣어야 합니다. 이렇게 리소스가 유형별로 서로 다른 폴더에서 관리되면 리소스별로 구분하기 쉽고 유지관리가 편리하다는 장점이 생깁니다.

/app/res/values 폴더에는 문자열이나 기타 기본 데이터 타입에 해당하는 정보들이 저장됩니다. 
예을들어 string.xml 파일에는 문자열을 저장합니다. 만약 다른 이름의 파일을 만들어 저장하는 경우 그 안에는 xml 포맷에 맞는 데이터가 들어가 있어야 합니다.

/app/res/drawble 폴더에는 이지미를 저장합니다. 이 폴더는 단말의 해상도에 따라 이미지를 보여줄 수 있도록 
/app/res/drawble-xhdpi, /app/res/drawble-hdpi, /app/res/drawble-mdpi 등으로 나누어 저장할 수 있습니다. 그러면 각 단말에서 해상도에 맞는 폴더를 찾아 그 안에 들어 있는 이미지를 참조하게 됩니다. 이렇게 저장되어 있는 리소스 정보를 코드에서 사용할 때에는 리소스 객체를 참조하여 리소스를 읽어 들여야 합니다.

리소스 객체는 context.getResources()메서드를 이용해 액티비티 안에서 언제든지 참조할 수 있습니다.  이 객체는 리소스의 유형에 따라 읽어 들일 수 있는 메서드가 정의 되어 있어 필요에 따라 사용할 수 있습니다. 

스타일과 테마
스타일과 테마는 여러 가지 속성들을 한꺼번에 모아서 정의한 것으로 가장 대표적인 예로 대화상자를 들 수 있습니다.
대화상자의 경우에는 액티비티와 달리 타이틀 부분이나 모서리 부분의 형태가 약간 다르게 뵈는데 이런 속성들을 다이얼로그 테마로 정의하여 액티비티에 적용하면 대화상자 모양으로 보이게 됩니다. 만약 스타일을 직접 정의하여 사용하고 싶다면 /app/res/valyes/themes.xml 파일에 추가해야 합니다.

그래들 이해하기

앱을 실행하거나 앱 스토어에 올릴 때 소스 파일이나 리소스 파일을 빌드하거나 배포하는 작업이 필요합니다. 이떄 사용되는 것이 그래들 입니다. 다시 말해, 그래들은 안드로이드 스튜디오에서 사용하는 빌드 및 배포 도구인 것입니다.

applicationId는 app의 id 값입니다. 여러분이 만든 앱은 id 값으로 구분되기 때문에 id 값은 전 세계에서 유일한 값으로 설정되어야 합니다.
CompileSdkVersion: 빌드를 진행할 때 어떤 버전의 SDK를 사용할 것인지를 지정합니다. 최신 버전의 SDK 버전을 지정하게 됩니다.
minSdkVersion은 이 앱이 어떤 하위 버전까지 지원하도록 할 것인지를 지정합니다. 모든 단말을 지원하면 좋겠지만 보통 앱에서 사용하는 최신 기능을 하위 단말에서 지원하지 못하는 경우에는 앱에서 사용하는 기능을 지원하기 시작한 버전을 minSdkVersion으로 지정하게 됩니다.
targerSdkVersion: 이 앱이 검증된 버전이 어떤 SDK 버전인지를 지정합니다. 만약 새로운 SDK가 출시되었다고 하더라도 해당SDK에서 검증되지 않은 앱은 이 버전을 이전 버전으로 지정할 수 있음.
Implementation으로 시작하는 한 줄은 여러분이 직접 추가한 외부 라이브러리입니다.
Local.properties: 파일 안에는 현재 사용하고 있는 pc에 설치된 sdk의 위치가 기록되어 있으며 gradle.properties 파일 안에는 메모리 설정이 들어있습니다. 
Gradle-wrapper.properties파일에는 그래들 버전 정보 등이 들어 있는데, 이런 정보들은 안드로이드 스튜디오에서 자동 설정하는 경우가 많아 외우지 않아도 됨.
