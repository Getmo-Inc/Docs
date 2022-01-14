# Smartpush + React Native
Neste tutorial iremos demonstrar como integrar a biblioteca Smartpush ao seu projeto mobile desenvolvido em react native utilizando um módulo.

## Materiais de apoio
Apresentamos, a seguir, alguns links que foram utilizados como base na construção deste material e que podem ser uteis para o esclarecimentos de dúvidas.

### Criando módulos em RN
        - https://reactnative.dev/docs/native-modules-android
        - https://reactnative.dev/docs/native-modules-ios
        
### Documentação da biblioteca Smartpush (iOS/Android)  
        - https://github.com/Getmo-Inc/android-smartpush-lib
        - https://github.com/Getmo-Inc/SmartpushSDKiOS
        
### Projetos de apps nativas de exemplo integrando com a SDK Smartpush         
        - https://github.com/Getmo-Inc/android-smartpush-demo-java
        - https://github.com/Getmo-Inc/SmartpushSDKiOSDemo
        
# Configuração Módulo Android

__Etapa 1.__ Siga a documentação da biblioteca Smartpush Android disponível [aqui](https://github.com/Getmo-Inc/android-smartpush-lib) e configure a sdk Smartpush no seu projeto. 

> __Importante:__ Não esqueça de atualizar o seu projeto para baixar as dependências.

__Etapa 2.__ Agora, abra o Android Studio e crie 2 novos arquivos dentro da pasta /android

    - react-native-app/android/app/src/main/java/replace-with-your-package/Smartpush.java
        
    - react-native-app/android/app/src/main/java/replace-with-your-package/SmartpushAppPackage.java

Nestes arquivos precisamos adicionar o código nativo necessário para interagir com a SDK SmartPromo através do React Native.

A seguir apresentamos a implementação de cada arquivo-fonte. Você pode copiar e colar no seu projeto!!

### Smartpush.java
```java
import androidx.annotation.NonNull;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;
import br.com.smartpush.Smartpush;

public class Smartpush extends ReactContextBaseJavaModule {
    Smartpush(ReactApplicationContext context) {
        super(context);
    }

    @NonNull
    @Override
    public String getName() {
        return "Smartpush";
    }

    @ReactMethod
    public void subscribe( ) {
        // Register at Smartpush!
        Smartpush.subscribe( getApplicationContext() );
    }
                                                           
    @ReactMethod
    public void setTag( ) {
        // Register at Smartpush!
        Smartpush.subscribe( getApplicationContext() );
    }                                                           

    @ReactMethod
    public void delTag( ) {
        // Register at Smartpush!
        Smartpush.subscribe( getApplicationContext() );
    }
                                                           
    @ReactMethod
    public void geo( double lat, double lng ) {
        // Register at Smartpush!
        Smartpush.subscribe( getApplicationContext() );
    }                                                           


}
```

> __Importante:__ Essa é apenas uma implementação de referência. Você pode ajustar de acordo com a necessidade do seu projeto, e até mesmo adicionar outros métodos disponíveis na SDK Smartpush.

### SmartpushAppPackage.java

```java
import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class SmartpushAppPackage implements ReactPackage {
    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }

    @Override
    public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();
        modules.add(new Smartpush(reactContext));
        return modules;
    }
}
```

### MainApplication.java

__Etapa 3.__ Edite o arquivo *MainApplication.java* e adicione o módulo que acabamos de criar ao método *getPackages*. Veja como no exemplo abaixo.


```java
    @Override
    protected List<ReactPackage> getPackages() {
      @SuppressWarnings("UnnecessaryLocalVariable")
      List<ReactPackage> packages = new PackageList(this).getPackages();
      // Packages that cannot be autolinked yet can be added manually here, for example:
      packages.add(new SmartpushAppPackage());
      return packages;
    }
```

Pronto, módulo android concluido!

# Configuração do módulo iOS

__Etapa 1.__ Siga a documentação da biblioteca Smartpush iOS disponível [aqui](https://github.com/Getmo-Inc/smartpromo-android) e configure a sdk Smartpush no seu projeto. 


__Etapa 2.__ Agora, abra o XCode e crie 2 novos arquivos dentro da pasta /iOS

    - react-native-app/iOS/.../RCTSmartpushMod.h        
    - react-native-app/iOS/.../RCTSmartpushMod.m

> __Importante__: Ambos arquivos devem, obrigatoriamente, iniciar por __'RCT'__.

Nestes arquivos iremos adicionar o código nativo necessário para interagir com a SDK Smartpush através do React Native.

A seguir apresentamos a implementação para cada arquivo-fonte.


### RCTSmartpushMod.h

No arquivo header vamos definir que nosso módulo é do tipo RCTBridgeModule, desta forma o módulo poderá ser exportado para o React Native.

```
//  RCTSmartpushMod.h
#import <React/RCTBridgeModule.h>
@interface RCTSmartpushMod : NSObject <RCTBridgeModule>
@end
```

### RCTSmartpushMod.m

Neste arquivo vamos implementar os métodos da SDK Smartpromo que vamos expor para o React Native.

```
// RCTSmartpushMod.m
#import "RCTSmartpushMod.h"
#import "Smartpush/Smartpush.h"
#import <React/RCTUtils.h>
#import <UIKit/UIKit.h>
#import <dispatch/dispatch.h>

@implementation RCTSmartpushMod
RCT_EXPORT_MODULE(Smartpush);

RCT_EXPORT_METHOD(start:(NSString *)campaignID key:(NSString *)key secret:(NSString *)secret config:(NSDictionary *)config)
{
    SmartPromo *sp = [[SmartPromo alloc] init: campaignID];
    [sp setupAccessKey:key andSecretKey:secret];
    
    dispatch_async(dispatch_get_main_queue(), ^{
        UIViewController *vc = RCTPresentedViewController();
        [sp go:vc];
    });
}

@end
```

> __Importante:__ Essa é apenas uma implementação de referência. Você pode ajustar de acordo com a necessidade do seu projeto, e até mesmo adicionar outros métodos disponíveis na SDK Smartpush.

Nesta implementação definimos que o nome do módulo a ser exportado será __Smartpush__, da mesma forma como foi feito no módulo android. Esse será o nome que ficará disponível no __NativeModules__ para uso no React Native.

Pronto, módulo iOS concluido! Agora vamos ver como fazer uso destes módulos no seu projeto em React Native!

# Usando os módulos no código React Native

Depois de criar os módulos do Android e do iOS, agora basta importar o NativeModules no seu projeto.

    import React from 'react';
    import { NativeModules, Button } from 'react-native';

Com NativeModules, o uso da SDK Smartpush é bem simples, veja alguns exemplos de acionamento.

### Enviando uma atualização de localização

    // Acionamento
    NativeModules.Smartpush.geo( -30.1327123, -51.2318003 );

### Atualizando uma tag
 
    // Acionamento do registro de um valor
    NativeModules.Smartpush.setTag( 'USER_ID', 'AB12CD34EF56' );

    // Acionamento da exclusão do registro
    NativeModules.Smartpush.delTag( 'USER_ID' );

> __Importante:__ Não esqueça de criar a tag no painel da plataforma Smartpush antes de usa-la.
> 

### Acessando a caixa de entrada de notificações

    var campaign = '[SEU_ID_DA_CAMPANHA]';
    var key      = '[SUA_KEY]';
    var secret   = '[SUA_SECRET]';
       
    // Acionamento
    NativeModules.Smartpush.start(campaign, key, secret, config);


proguard-project

# Smartpush
-keep class br.com.smartpush.** { *; }

# SingleLink
-keep class br.com.singlelink.** { *; }