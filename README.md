
# react-native-seoul
<p align="left">
  <a href="https://npmjs.org/package/react-native-seoul"><img alt="npm version" src="http://img.shields.io/npm/v/react-native-seoul.svg?style=flat-square"></a>
  <a href="https://npmjs.org/package/react-native-seoul"><img alt="npm version" src="http://img.shields.io/npm/dm/react-native-seoul.svg?style=flat-square"></a>
</p>
React Native 카카오 로그인 라이브러리 입니다.
세부 예제는 KakaoLoginExample 폴더 안의 예제 프로젝트를 확인해주시면 감사하겠습니다.

## Getting started

`$ npm install react-native-kakao-logins --save`

### Mostly automatic installation

`$ react-native link react-native-kakao-logins`

### Manual installation

#### iOS

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-kakao-logins` and add `RNKakaoLogins.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libRNKakaoLogins.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Run your project (`Cmd+R`)<

#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java`
  - Add `import com.dooboolab.kakaologins.RNKakaoLoginsPackage;` to the imports at the top of the file
  - Add `new RNKakaoLoginsPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-kakao-logins'
  	project(':react-native-kakao-logins').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-kakao-logins/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      compile project(':react-native-kakao-logins')
  	```


### Post installation

#### iOS

#### Android

1. 안드로이드 카카오 SDK 설치 관련해서는 [여기](https://developers.kakao.com/docs/android/getting-started)를 참고해주세요. 성공적으로 build가 되는 것을 확인하시면 아래를 진행하시면 됩니다.
2. `react-native-kakao-logins`에서 `string.xml`을 열어 `kakao_app_key`를 본인의 application key로 바꿔주세요.
3. `MainApplication.java`에서 `MainApplication` 클래스를 다음과 같이 만들어주세요. `com.dooboolab.kakaologins.GlobalApplication`를 `extend` 받아야 합니다.
   ```
	 public class MainApplication extends GlobalApplication implements ReactApplication {
	 ```

#### Methods
| Func  | Param  | Return | Description |
| :------------ |:---------------:| :---------------:| :-----|
| login |  | `callback (err, result: string)` | 로그인. |
| getProfile |  | `callback (err, result: JSONObject)` | 프로필 불러오기. |
| logout |  | `callback (err, result: string)` | 로그아웃. |

#### params in result when `getProfile`
|    | iOS | Android | Comment |
|----|-----|---------|------|
|`id`| ✓ | ✓ | 카카오 고유 아이디 |
|`nickname`| ✓ | ✓ | 별칭 |
|`email`| ✓ | ✓ | 이메일 주소 |
|`display_id`| ✓ | ✓ | 별칭 id |
|`phone_number`| ✓ | ✓ | 휴대폰 번호 |
|`email_verified`| ✓ | ✓ | 이메일 인증 여부 |
|`kakaotalk_user`| ✓ | ✓ | 카카오톡 유저 여부 |
|`profile_image_path`| ✓ | ✓ | 프로필 이미지 |
|`thumb_image_path`| ✓ | ✓ | 썸네일 이미지 |
|`has_signed_up`| ✓ | ✓ | 가입 여부 |


## Usage
아래 예제는 `KakaoLoginExample` 프로젝트의 `App.js`파일과 동일합니다. 로그인 후 result에 들어오는 결과값음 `accessToken`입니다.
```javascript
export default class App extends Component<{}> {
  constructor(props) {
    super(props);
    this.state = {
      isKakaoLogging: false,
      token: 'token has not fetched',
    };
  }

  // 카카오 로그인 시작.
  kakaoLogin() {
    console.log('   kakaoLogin   ');
    RNKakaoLogins.login((err, result) => {
      if (err){
        console.log(err);
        return;
      }
      Alert.alert('result', result);
    });
  }

  kakaoLogout() {
    console.log('   kakaoLogout   ');
    RNKakaoLogins.logout((err, result) => {
      if (err){
        console.log(err);
        return;
      }
      Alert.alert('result', result);
    });
  }

  // 로그인 후 내 프로필 가져오기.
  getProfile() {
    console.log('getKakaoProfile');
    RNKakaoLogins.getProfile((err, result) => {
      if (err){
        console.log(err);
        return;
      }
      Alert.alert('result', result);
    });
  }

  render() {
    return (
      <View style={ styles.container }>
        <View style={ styles.header }>
          <Text>LOGIN</Text>
        </View>
        <View style={ styles.content }>
          <NativeButton
            isLoading={this.state.isNaverLoggingin}
            onPress={() => this.kakaoLogin()}
            activeOpacity={0.5}
            style={styles.btnKakaoLogin}
            textStyle={styles.txtNaverLogin}
          >LOGIN</NativeButton>
          <Text>{this.state.token}</Text>
          <NativeButton
            onPress={() => this.kakaoLogout()}
            activeOpacity={0.5}
            style={styles.btnKakaoLogin}
            textStyle={styles.txtNaverLogin}
          >Logout</NativeButton>
          <NativeButton
            isLoading={this.state.isKakaoLogging}
            onPress={() => this.getProfile()}
            activeOpacity={0.5}
            style={styles.btnKakaoLogin}
            textStyle={styles.txtNaverLogin}
          >getProfile</NativeButton>
        </View>
      </View>
    );
  }
}
```
  