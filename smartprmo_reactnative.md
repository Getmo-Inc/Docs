# Smartpromo + React Native
Neste tutorial iremos apresentar como integrar a biblioteca Smartpromo a um projeto mobile desenvolvido em react native.

## Materiais de apoio
        - https://reactnative.dev/docs/native-modules-android
        - https://reactnative.dev/docs/native-modules-ios
        - https://github.com/Getmo-Inc/smartpromo-android
        - https://github.com/Getmo-Inc/SmartPromoiOS
        
# Instalação sdk Android

A seguir apresentamos os passos necessários para integrar a SDK Android da solução Smartpromo ao seu projeto de aplicação.

__Passo 1.__ É necessário configurar a dependência da sdk Smartpromo no seu projeto, para isso siga a documentação da biblioteca Smartpromo Android disponível [aqui](https://github.com/Getmo-Inc/smartpromo-android).

> __Importante:__ Não esqueça de atualizar o seu projeto para baixar essa dependência.

__Passo 2.__ No projeto React Native será necessário criar 2 novos arquivos

    - react-native-app/android/app/src/main/java/com/replace-with-your-app/SmartPromoStarter.java
    
e
    
    - react-native-app/android/app/src/main/java/com/replace-with-your-app/MyAppPackage.java

Esses arquivos irão conter o código nativo para interagir com a SDK SmartPromo e deixa-la disponível no React Native.

A seguir uma sugestão de implementação de cada arquivo-fonte.

```java
import androidx.annotation.NonNull;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;
import br.com.getmo.smartpromo.SmartPromo;
import br.com.getmo.smartpromo.SmartPromo;
import br.com.getmo.smartpromo.models.FSPAddress;
import br.com.getmo.smartpromo.models.FSPConsumer;
import br.com.getmo.smartpromo.models.FSPGenre;
import java.text.SimpleDateFormat;
import android.graphics.Color;

public class SmartPromoStarter extends ReactContextBaseJavaModule {
    SmartPromoStarter(ReactApplicationContext context) {
        super(context);
    }

    @NonNull
    @Override
    public String getName() {
        return "SmartPromo";
    }

    @ReactMethod
    public void startCampaign(String campaignID, String accessKey, String secretKey, ReadableMap config) {
        SmartPromo smartPromo = new SmartPromo();
        smartPromo.setupAccessKeyAndSecretKey(accessKey, secretKey);

        smartPromo = parseCampaignColor(smartPromo, config);
        smartPromo = parseCampaignConsumer(smartPromo, config);
        smartPromo.setIsHomolog(config.getBoolean("isHomolog"));
        smartPromo.setMetadata(config.getString("metadata"));

        smartPromo.go(campaignID, getCurrentActivity());
    }

    @ReactMethod
    public void startMultiCampaigns(String headnote, String title, String message, String accessKey, String secretKey, ReadableMap config) {
        SmartPromo smartPromo = new SmartPromo();
        smartPromo.setupAccessKeyAndSecretKey(accessKey, secretKey);

        smartPromo = parseCampaignColor(smartPromo, config);
        smartPromo = parseCampaignConsumer(smartPromo, config);
        smartPromo.setIsHomolog(config.getBoolean("isHomolog"));
        smartPromo.setMetadata(config.getString("metadata"));

        smartPromo.goMulti(headnote, title, message, getCurrentActivity());
    }

    @ReactMethod
    public void startScanner(String campaignID, String accessKey, String secretKey, String consumerID, ReadableMap config) {
        SmartPromo smartPromo = new SmartPromo();
        smartPromo.setupAccessKeyAndSecretKey(accessKey, secretKey);

        smartPromo = parseCampaignColor(smartPromo, config);
        smartPromo.setIsHomolog(config.getBoolean("isHomolog"));
        smartPromo.setMetadata(config.getString("metadata"));
        smartPromo.scan(campaignID, consumerID, getCurrentActivity());
    }
                                                                   
    private SmartPromo parseCampaignColor( SmartPromo smartPromo, ReadableMap config ) {
        if (config.hasKey( "color")) {
            String colorHex = config.getString("color");
            smartPromo.setColor(Color.parseColor(colorHex));
        }
        return smartPromo;
    }

    private SmartPromo parseCampaignConsumer(SmartPromo smartPromo, ReadableMap config) {
        FSPConsumer consumer = null;

        if ( config.hasKey( "cpf" ) ) {
            consumer = ( consumer == null ) ? new FSPConsumer() : consumer;
            consumer.setCpf( config.getString( "cpf" ) );
        }

        if ( config.hasKey( "nome" ) ) {
            consumer = ( consumer == null ) ? new FSPConsumer() : consumer;
            consumer.setName( config.getString( "nome" ) );
        }

        if ( config.hasKey( "email" ) ) {
            consumer = ( consumer == null ) ? new FSPConsumer() : consumer;
            consumer.setEmail( config.getString( "email" ) );
        }

        if ( config.hasKey( "genero" ) ) {
            consumer = ( consumer == null ) ? new FSPConsumer() : consumer;

            FSPGenre genre;
            switch ( config.getString( "genero" ) ) {
                case "M":
                    consumer.setGenre( FSPGenre.MALE );
                    break;
                case "F":
                    consumer.setGenre( FSPGenre.FEMALE );
                    break;
                case "NB":
                    consumer.setGenre( FSPGenre.NOT_BINARY );
                    break;
                default:
                    consumer.setGenre( FSPGenre.NOT_INFORMED );
            }
        }

        if ( config.hasKey( "telefone" ) ) {
            consumer = ( consumer == null ) ? new FSPConsumer() : consumer;
            consumer.setPhone( config.getString( "telefone" ) );
        }

        if ( config.hasKey( "aniversario" ) ) {
            consumer = ( consumer == null ) ? new FSPConsumer() : consumer;

            try {
                String pattern = "yyyy-MM-dd";
                consumer.setBdate(
                   new SimpleDateFormat(pattern).parse(config.getString("aniversario"))
                );
            } catch ( Exception e ) {
                Log.e( "ERR", e.getMessage(), e );
            }
        }

        if ( config.hasKey( "endereco" ) ) {
            consumer = ( consumer == null ) ? new FSPConsumer() : consumer;

            String[] tokens =
                    config.getString( "endereco" ).split(";");

            if ( tokens.length > 6 ) {
                FSPAddress address = new FSPAddress();

                address.setStreetName( tokens[0] );
                address.setStreetNumber( tokens[1] );
                address.setComplement( tokens[2] );
                address.setNeighborhood( tokens[3] );
                address.setCity( tokens[4] );
                address.setState( tokens[5] );
                address.setZipCode( tokens[6] );

                consumer.setAddress( address );
            }
        }

        if ( consumer != null ) {
            smartPromo.setConsumer( consumer );
        }

        return smartPromo;
    }
}
```

> __Importante:__ Essa é apenas uma sugestão de implementação. 
> 
> É necessário revisar e ajustar para adequar o exemplo ao seu projeto. 
>
>Por exemplo, os códigos de __gênero__, o formato de __data__, estrutura de __endereço__, podem divergir entre o que sua aplicação usa e a implementação de referência, sendo importante revisar, validar e, eventualmente, ajustar. 

```java
import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MyAppPackage implements ReactPackage {
    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }

    @Override
    public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();
        modules.add(new SmartPromoStarter(reactContext));
        return modules;
    }
}
```

__Passo 3.__ Por fim edite o arquivo MainApplication.java. No método getPackages adicione o módulo que acabamos de criar.
Isso é feito na linha: *packages.add(new MyAppPackage());*

```java
    @Override
    protected List<ReactPackage> getPackages() {
      @SuppressWarnings("UnnecessaryLocalVariable")
      List<ReactPackage> packages = new PackageList(this).getPackages();
      // Packages that cannot be autolinked yet can be added manually here, for example:
      packages.add(new MyAppPackage());
      return packages;
    }
```

# Instalação sdk iOS

A seguir apresentamos os passos necessários para integrar a SDK iOS da solução Smartpromo ao seu projeto de aplicação.

__Passo 1.__ É necessário configurar a dependência da sdk Smartpromo no seu projeto, para isso altere o arquivo *Podfile* e insira a linha a seguir. 

```
pod 'SmartPromo', '2.2.0'
```

A seguir um arquivo Podfile de exemplo
```
require_relative '../node_modules/react-native/scripts/react_native_pods'
require_relative '../node_modules/@react-native-community/cli-platform-ios/native_modules'

platform :ios, '10.0'

target 'SmartPromoModule' do
  config = use_native_modules!

  # pod que nos interessa
  pod 'SmartPromo'

  use_react_native!(
    :path => config[:reactNativePath],
    # to enable hermes on iOS, change `false` to `true` and then install pods
    :hermes_enabled => false
  )

  target 'SmartPromoModuleTests' do
    inherit! :complete
    # Pods for testing
  end

  # Enables Flipper.
  #
  # Note that if you have use_frameworks! enabled, Flipper will not work and
  # you should disable the next line.
  use_flipper!()

  post_install do |installer|
    react_native_post_install(installer)
  end
end
```

Feito isso é preciso atualizar o seu projeto para baixar essa dependência. Para isso execute o comando abaixo na pasta ios do project React Native.

```
pod install
```

> __Importante:__ a documentação da biblioteca Smartpromo iOS está disponível [aqui](https://reactnative.dev/docs/native-modules-ios). 

__Passo 2.__ Abra o XCode e crie 2 novos arquivos

        - RCTSmartPromoMod.h
e
        - RCTSmartPromoMod.m

O arquivo .h é o header com a definição do módulo e o arquivo .m contém a implementação do módulo.

> __Importante__: Ambos arquivos devem iniciar por RCT - o restante do nome basta ser coerente em ambos.

Veja um exemplo de arquivo header que define nosso módulo como sendo do tipo RCTBridgeModule. Desta forma o módulo pode ser exportado para o React Native.

```
//  RCTSmartPromoMod.h
#import <React/RCTBridgeModule.h>
@interface RCTSmartPromoMod : NSObject <RCTBridgeModule>
@end
```

Veja um exemplo do arquivo de implementação que expoem o acionamento da SDK Smartpromo para o React Native.

```
// RCTSmartPromoMod.m
#import "RCTSmartPromoMod.h"
#import "SmartPromo/SmartPromo.h"
#import <React/RCTUtils.h>
#import <UIKit/UIKit.h>
#import <dispatch/dispatch.h>

@implementation RCTSmartPromoMod
RCT_EXPORT_MODULE(SmartPromo);

RCT_EXPORT_METHOD(startCampaign:(NSString *)campaignID key:(NSString *)key secret:(NSString *)secret config:(NSDictionary *)config)
{
    SmartPromo *sp = [SmartPromo new];
    [sp setupAccessKey:key andSecretKey:secret];
    [sp setIsHomolog:[[config valueForKey:@"isHomolog"] boolValue]];
    [sp setMetadata: [[config valueForKey:@"metadata"] stringValue]];
    
    [self parseCampaignColor:sp config:config];
    [self parseCampaignConsumer:sp config:config];
    
    dispatch_async(dispatch_get_main_queue(), ^{
        UIViewController *vc = RCTPresentedViewController();
        [sp go: campaignID above: vc];
    });
}

RCT_EXPORT_METHOD(startMultiCampaigns:(NSString *)headnote title:(NSString *)title message:(NSString *)message key:(NSString *)key secret:(NSString *)secret config:(NSDictionary *)config)
{
    SmartPromo *sp = [SmartPromo new];
    [sp setupAccessKey:key andSecretKey:secret];
    [sp setIsHomolog:[[config valueForKey:@"isHomolog"] boolValue]];
    [sp setMetadata: [[config valueForKey:@"metadata"] stringValue]];
    
    [self parseCampaignColor:sp config:config];
    [self parseCampaignConsumer:sp config:config];
    
    dispatch_async(dispatch_get_main_queue(), ^{
        UIViewController *vc = RCTPresentedViewController();
        [vc pushViewController:[sp goMultiWithHeadnote: headnote title: title message: message] animated:true];
    });
}

RCT_EXPORT_METHOD(startScanner:(NSString *)campaignID key:(NSString *)key secret:(NSString *)secret consumerID:(NSString *)consumerID config:(NSDictionary *)config)
{
    SmartPromo *sp = [SmartPromo new];
    [sp setupAccessKey:key andSecretKey:secret];
    [sp setIsHomolog:[[config valueForKey:@"isHomolog"] boolValue]];
    [sp setMetadata: [[config valueForKey:@"metadata"] stringValue]];
    
    [self parseCampaignColor:sp config:config];
    
    dispatch_async(dispatch_get_main_queue(), ^{
        UIViewController *vc = RCTPresentedViewController();
        [sp scan: campaignID consumerID:consumerID above:vc];
    });
}

- (void) parseCampaignColor: (SmartPromo*) smartPromo config:(NSDictionary*) config {
    NSString* stringColor = [config valueForKey:@"color"];
    if (stringColor) {
        [smartPromo setColor: [self colorFromHexString:stringColor]];
    }
}

- (void) parseCampaignConsumer: (SmartPromo*) smartPromo config:(NSDictionary*) config {
    FSPConsumer* consumer;
    
    NSString* cpf = [config valueForKey:@"cpf"];
    if (cpf) {
        consumer = consumer ? consumer : [FSPConsumer new];
        [consumer setCpf: cpf];
    }
    
    NSString* nome = [config valueForKey:@"nome"];
    if (nome) {
        consumer = consumer ? consumer : [FSPConsumer new];
        [consumer setName: nome];
    }
    
    NSString* email = [config valueForKey:@"email"];
    if (email) {
        consumer = consumer ? consumer : [FSPConsumer new];
        [consumer setEmail: email];
    }
    
    NSString* genero = [config valueForKey:@"genero"];
    if (genero) {
        consumer = consumer ? consumer : [FSPConsumer new];
        
        if ([genero isEqual: @"M"]) {
            [consumer setGender: FSPGenderTypeMale];
            
        } else if ([genero isEqual: @"F"]) {
            [consumer setGender: FSPGenderTypeFamale];
            
        } else if ([genero isEqual: @"NB"]) {
            [consumer setGender: FSPGenderTypeNotBinary];
            
        } else {
            [consumer setGender: FSPGenderTypeNotInformed];
        }
    }
    
    NSString* telefone = [config valueForKey:@"telefone"];
    if (telefone) {
        consumer = consumer ? consumer : [FSPConsumer new];
        [consumer setPhone: telefone];
    }
    
    NSString* aniversario = [config valueForKey:@"aniversario"];
    if (aniversario) {
        consumer = consumer ? consumer : [FSPConsumer new];
        
        NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
        [dateFormatter setDateFormat:@"yyyy-MM-dd"];
        NSDate *date  = [dateFormatter dateFromString:aniversario];
        [consumer setBirthday: date];
    }
    
    NSString* endereco = [config valueForKey:@"endereco"];
    if (endereco) {
        consumer = consumer ? consumer : [FSPConsumer new];
        
        NSArray* tokens = [endereco componentsSeparatedByString:@";"];
        if (tokens.count > 6) {
            FSPAddress* address = [FSPAddress new];
            
            [address setStreetName: tokens[0]];
            [address setStreetNumber: tokens[1]];
            [address setComplement: tokens[2]];
            [address setNeighborhood: tokens[3]];
            [address setCity: tokens[4]];
            [address setState: tokens[5]];
            [address setZipCode: tokens[6]];

            [consumer setAddress: address];
        }
    }
    
    if (consumer) {
        [smartPromo setConsumer: consumer];
    }
}

- (UIColor *)colorFromHexString:(NSString *)hexString {
    unsigned rgbValue = 0;
    NSScanner *scanner = [NSScanner scannerWithString:hexString];
    [scanner setScanLocation:1];
    [scanner scanHexInt:&rgbValue];
    
    return [UIColor colorWithRed:((rgbValue & 0xFF0000) >> 16)/255.0
                           green:((rgbValue & 0xFF00) >> 8)/255.0
                            blue:(rgbValue & 0xFF)/255.0 alpha:1.0];
}
@end
```
O arquivo da implementação define o nome do módulo a ser exportado como __SmartPromo__ da mesma forma como foi feito no módulo android. Esse será o nome que ficará disponível em NativeModules para uso no React Native.

Além disso, o arquivo exporta um método chamado 'start' que recebe 4 parâmetros: id, chave, segredo, e configurações da campanha


# Uso dos módulos no código React Native

Depois de seguir os passos do Android e do iOS, basta importar o NativeModules.

    import React from 'react';
    import { NativeModules, Button } from 'react-native';

Com NativeModules, basta usar da seguinte maneira.

### Iniciando a SDK no modo campanha:

     
    var campaign = '[SEU_ID_DA_CAMPANHA]';
    var key      = '[SUA_KEY]';
    var secret   = '[SUA_SECRET]';
    
    // Objeto Config é opcional!
    // Você pode configurar um objeto config com os dados do consumidor
    // que já possuir do cliente, e também para definir a cor default da campanha. 
    // Se o objeto for omitido, ou se 
    // faltarem informações, a SDK mobile irá usar a cor default e solicitar os dados 
    // do consumidor no optin.
    
    var config = {};
    config.color = '#0000CC'
    config.cpf = '90134602080';
    config.nome = 'John Doe';  
    config.email = 'john.doe@gmail.com';
    config.genero = 'M';
    config.telefone = '555000552';
    config.aniversario = '1973-01-04';
    config.endereco = 'Rua sem nome;100;apto 101;ipanema;Porto Alegre;RS;90500000';
    config.metadata = 'Qualquer coisa como String'; // Opcional

    // Caso queira utilizar o ambiente de homologação
    config.isHomolog = true;
       
    // Acionamento
    NativeModules.SmartPromo.startCampaign(campaign, key, secret, config);

#### Campanha 

> __campaignID__, __accessKey__, e __secretKey__ serão fornecidos pelo time da __Getmo__ para serem configurados no seu projeto.
> 

### Iniciando a SDK no modo Scanner de notas:
    var campaign   = '[SEU_ID_DA_CAMPANHA]';
    var key        = '[SUA_KEY]';
    var secret     = '[SUA_SECRET]';
    var consumerID = '[IDENTIFICADOR_DO_CONSUMIDOR]';
    
    var config = {};
    config.color = '#0000CC'
    config.metadata = 'Qualquer coisa como String'; // Opcional
    
    // Caso queira utilizar o ambiente de homologação
    config.isHomolog = true;
    
    // Acionamento
    NativeModules.SmartPromo.startScanner(campaign, key, secret, consumerID, config);

> __campaignID__, __accessKey__, e __secretKey__ serão fornecidos pelo time da __Getmo__ para serem configurados no seu projeto.
> 
