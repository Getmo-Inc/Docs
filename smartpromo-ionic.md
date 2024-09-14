# Smartpromo + Ionic
Neste tutorial iremos apresentar como integrar a biblioteca Smartpromo a um projeto mobile desenvolvido em Ionic com Capacitor.

## Materiais de Apoio
- [Ionic Native Modules](https://ionicframework.com/docs/native)
- [Capacitor Plugin Development](https://capacitorjs.com/docs/plugins)
- [Smartpromo Android SDK](https://github.com/Getmo-Inc/smartpromo-android)
- [Smartpromo iOS SDK](https://github.com/Getmo-Inc/SmartPromoiOS)

---

## Instalação SDK Android

Aqui estão os passos necessários para integrar a SDK Android da solução Smartpromo ao seu projeto Ionic.

### Passo 1: Configurar a Dependência da SDK Smartpromo

Siga a documentação da biblioteca Smartpromo Android disponível [aqui](https://github.com/Getmo-Inc/smartpromo-android) para configurar a dependência da SDK no seu projeto Android.

> **Importante:** Não esqueça de atualizar o projeto para baixar essa dependência.

### Passo 2: Criar Arquivos Nativos Android

No projeto Ionic, será necessário criar 2 novos arquivos dentro do diretório Android do projeto Capacitor:

- `android/app/src/main/java/com/replace-with-your-app/SmartPromoStarter.java`
- `android/app/src/main/java/com/replace-with-your-app/MyAppPackage.java`

Esses arquivos conterão o código nativo que interage com a SDK SmartPromo e a torna disponível para o Ionic.

Aqui está um exemplo de implementação para o arquivo `SmartPromoStarter.java`:

```java
import androidx.annotation.NonNull;
import com.getcapacitor.PluginCall;
import br.com.getmo.smartpromo.SmartPromo;
import br.com.getmo.smartpromo.models.FSPConsumer;
import br.com.getmo.smartpromo.models.FSPAddress;
import br.com.getmo.smartpromo.models.FSPGenre;
import android.graphics.Color;

public class SmartPromoStarter {

    public void startCampaign(PluginCall call) {
        String campaignID = call.getString("campaignID");
        String accessKey = call.getString("accessKey");
        String secretKey = call.getString("secretKey");

        SmartPromo smartPromo = new SmartPromo();
        smartPromo.setupAccessKeyAndSecretKey(accessKey, secretKey);

        boolean isHomolog = call.getBoolean("isHomolog", false);
        smartPromo.setIsHomolog(isHomolog);
        smartPromo.setMetadata(call.getString("metadata", ""));

        smartPromo.go(campaignID, this.getContext());
    }
}
```

O arquivo `MyAppPackage.java` registra o módulo que criamos:

```java
import com.getcapacitor.Plugin;
import com.getcapacitor.PluginCall;
import com.getcapacitor.PluginMethod;
import org.json.JSONObject;

public class MyAppPackage implements Plugin {
    @PluginMethod
    public void startCampaign(PluginCall call) {
        SmartPromoStarter smartPromoStarter = new SmartPromoStarter();
        smartPromoStarter.startCampaign(call);
        call.success();
    }
}
```

### Passo 3: Editar o Arquivo `MainActivity.java`

Por fim, edite o arquivo `MainActivity.java` para incluir o módulo:

```java
import com.getcapacitor.BridgeActivity;
import com.getcapacitor.Plugin;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends BridgeActivity {
    @Override
    public void onStart() {
        super.onStart();

        List<Class<? extends Plugin>> additionalPlugins = new ArrayList<>();
        additionalPlugins.add(MyAppPackage.class);
        this.init(additionalPlugins);
    }
}
```

---

## Instalação SDK iOS

Aqui estão os passos necessários para integrar a SDK iOS da solução Smartpromo ao seu projeto Ionic.

### Passo 1: Configurar a Dependência da SDK Smartpromo no iOS

No arquivo `ios/App/Podfile`, adicione a linha para a SDK Smartpromo:

```ruby
pod 'SmartPromo', '2.6.1'
```

A seguir, um exemplo de `Podfile` atualizado:

```ruby
platform :ios, '10.0'

target 'App' do
  pod 'SmartPromo', '2.6.1'
  # Outros pods necessários
end
```

Depois, execute o comando `pod install` no diretório `ios/` do seu projeto:

```bash
cd ios
pod install
```

### Passo 2: Criar Arquivos Nativos iOS

No projeto iOS, crie os arquivos `SmartPromoStarter.swift` e `SmartPromoPlugin.swift` no diretório `ios/App`.

Exemplo do arquivo `SmartPromoStarter.swift`:

```swift
import SmartPromo

@objc(SmartPromoStarter)
public class SmartPromoStarter: CAPPlugin {
    @objc func startCampaign(_ call: CAPPluginCall) {
        let campaignID = call.getString("campaignID") ?? ""
        let accessKey = call.getString("accessKey") ?? ""
        let secretKey = call.getString("secretKey") ?? ""

        let smartPromo = SmartPromo()
        smartPromo.setupAccessKey(accessKey, andSecretKey: secretKey)
        smartPromo.setIsHomolog(call.getBool("isHomolog") ?? false)
        smartPromo.setMetadata(call.getString("metadata") ?? "")
        
        DispatchQueue.main.async {
            smartPromo.go(campaignID, above: self.bridge.viewController)
        }
        
        call.success()
    }
}
```

No arquivo `SmartPromoPlugin.swift`, registre o plugin:

```swift
import Capacitor

@objc(SmartPromoPlugin)
public class SmartPromoPlugin: CAPPlugin {
    @objc func startCampaign(_ call: CAPPluginCall) {
        let starter = SmartPromoStarter()
        starter.startCampaign(call)
    }
}
```

### Passo 3: Atualizar o `AppDelegate.swift`

No arquivo `AppDelegate.swift`, importe o módulo SmartPromo:

```swift
import SmartPromo
```

---

## Uso dos Módulos no Código Ionic

Após a configuração dos módulos nativos no Android e no iOS, você pode usar os métodos nativos diretamente no código Ionic com Capacitor.

```typescript
import { Plugins } from '@capacitor/core';
const { SmartPromoPlugin } = Plugins;

SmartPromoPlugin.startCampaign({
  campaignID: 'SEU_ID_DA_CAMPANHA',
  accessKey: 'SUA_KEY',
  secretKey: 'SUA_SECRET',
  isHomolog: true, // Opcional
  metadata: 'Qualquer informação adicional', // Opcional
  color: '#FF0000', // Opcional
});
```

> **Importante:** `campaignID`, `accessKey` e `secretKey` serão fornecidos pelo time da **Getmo** para serem configurados no seu projeto.
