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
        SmartPromo smartPromo = initializeSmartPromo(call);
        String campaignID = call.getString("campaignID");
        smartPromo.go(campaignID, this.getContext());
    }

    public void startMultiCampaigns(PluginCall call) {
        SmartPromo smartPromo = initializeSmartPromo(call);
        String headnote = call.getString("headnote");
        String title = call.getString("title");
        String message = call.getString("message");
        smartPromo.goMulti(headnote, title, message, this.getContext());
    }

    public void startScanner(PluginCall call) {
        SmartPromo smartPromo = initializeSmartPromo(call);
        String campaignID = call.getString("campaignID");
        String consumerID = call.getString("consumerID");
        smartPromo.scan(campaignID, consumerID, this.getContext());
    }

    private SmartPromo initializeSmartPromo(PluginCall call) {
        String accessKey = call.getString("accessKey");
        String secretKey = call.getString("secretKey");

        SmartPromo smartPromo = new SmartPromo();
        smartPromo.setupAccessKeyAndSecretKey(accessKey, secretKey);

        boolean isHomolog = call.getBoolean("isHomolog", false);
        smartPromo.setIsHomolog(isHomolog);
        smartPromo.setMetadata(call.getString("metadata", ""));

        smartPromo = parseCampaignColor(smartPromo, call);
        smartPromo = parseCampaignConsumer(smartPromo, call);

        return smartPromo;
    }

    private SmartPromo parseCampaignConsumer(SmartPromo smartPromo, PluginCall call) {
        FSPConsumer consumer = null;

        if (call.hasOption("cpf")) {
            consumer = (consumer == null) ? new FSPConsumer() : consumer;
            consumer.setCpf(call.getString("cpf"));
        }

        if (call.hasOption("name")) {
            consumer = (consumer == null) ? new FSPConsumer() : consumer;
            consumer.setName(call.getString("name"));
        }

        if (call.hasOption("email")) {
            consumer = (consumer == null) ? new FSPConsumer() : consumer;
            consumer.setEmail(call.getString("email"));
        }

        if (call.hasOption("genre")) {
            consumer = (consumer == null) ? new FSPConsumer() : consumer;
            String genre = call.getString("genre");

            switch (genre) {
                case "M":
                    consumer.setGenre(FSPGenre.MALE);
                    break;
                case "F":
                    consumer.setGenre(FSPGenre.FEMALE);
                    break;
                case "NB":
                    consumer.setGenre(FSPGenre.NOT_BINARY);
                    break;
                default:
                    consumer.setGenre(FSPGenre.NOT_INFORMED);
            }
        }

        if (call.hasOption("phone")) {
            consumer = (consumer == null) ? new FSPConsumer() : consumer;
            consumer.setPhone(call.getString("phone"));
        }

        if (call.hasOption("birthday")) {
            consumer = (consumer == null) ? new FSPConsumer() : consumer;

            try {
                String pattern = "yyyy-MM-dd";
                consumer.setBdate(new SimpleDateFormat(pattern).parse(call.getString("birthday")));
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        if (call.hasOption("address")) {
            consumer = (consumer == null) ? new FSPConsumer() : consumer;

            String[] tokens = call.getString("address").split(";");
            if (tokens.length > 6) {
                FSPAddress address = new FSPAddress();

                address.setStreetName(tokens[0]);
                address.setStreetNumber(tokens[1]);
                address.setComplement(tokens[2]);
                address.setNeighborhood(tokens[3]);
                address.setCity(tokens[4]);
                address.setState(tokens[5]);
                address.setZipCode(tokens[6]);

                consumer.setAddress(address);
            }
        }

        if (consumer != null) {
            smartPromo.setConsumer(consumer);
        }

        return smartPromo;
    }

    private SmartPromo parseCampaignColor(SmartPromo smartPromo, PluginCall call) {
        if (call.hasOption("color")) {
            String colorHex = call.getString("color");
            try {
                smartPromo.setColor(Color.parseColor(colorHex));
            } catch (IllegalArgumentException e) {
                e.printStackTrace();
            }
        }
        return smartPromo;
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

    @PluginMethod
    public void startMultiCampaigns(PluginCall call) {
        SmartPromoStarter smartPromoStarter = new SmartPromoStarter();
        smartPromoStarter.startMultiCampaigns(call);
        call.success();
    }

    @PluginMethod
    public void startScanner(PluginCall call) {
        SmartPromoStarter smartPromoStarter = new SmartPromoStarter();
        smartPromoStarter.startScanner(call);
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
        let smartPromo = initializeSmartPromo(call)
        let campaignID = call.getString("campaignID") ?? ""

        DispatchQueue.main.async {
            smartPromo.go(campaignID, above: self.bridge.viewController)
        }

        call.success()
    }

    @objc func startMultiCampaigns(_ call: CAPPluginCall) {
        let smartPromo = initializeSmartPromo(call)
        let headnote = call.getString("headnote") ?? ""
        let title = call.getString("title") ?? ""
        let message = call.getString("message") ?? ""

        DispatchQueue.main.async {
            smartPromo.goMulti(headnote, title: title, message: message, above: self.bridge.viewController)
        }

        call.success()
    }

    @objc func startScanner(_ call: CAPPluginCall) {
        let smartPromo = initializeSmartPromo(call)
        let campaignID = call.getString("campaignID") ?? ""
        let consumerID = call.getString("consumerID") ?? ""

        DispatchQueue.main.async {
            smartPromo.scan(campaignID, consumerID: consumerID, above: self.bridge.viewController)
        }

        call.success()
    }

    private func initializeSmartPromo(_ call: CAPPluginCall) -> SmartPromo {
        let accessKey = call.getString("accessKey") ?? ""
        let secretKey = call.getString("secretKey") ?? ""

        let smartPromo = SmartPromo()
        smartPromo.setupAccessKey(accessKey, andSecretKey: secretKey)
        smartPromo.setIsHomolog(call.getBool("isHomolog") ?? false)
        smartPromo.setMetadata(call.getString("metadata") ?? "")

        smartPromo = parseCampaignColor(smartPromo, call)
        smartPromo = parseCampaignConsumer(smartPromo, call)

        return smartPromo
    }

    func parseCampaignConsumer(_ smartPromo: SmartPromo, _ call: CAPPluginCall) -> SmartPromo {
        var consumer: FSPConsumer? = nil

        if let cpf = call.getString("cpf") {
            consumer = (consumer == nil) ? FSPConsumer() : consumer
            consumer?.cpf = cpf
        }

        if let name = call.getString("name") {
            consumer = (consumer == nil) ? FSPConsumer() : consumer
            consumer?.name = name
        }

        if let email = call.getString("email") {
            consumer = (consumer == nil) ? FSPConsumer() : consumer
            consumer?.email = email
        }

        if let genre = call.getString("genre") {
            consumer = (consumer == nil) ? FSPConsumer() : consumer
            switch genre {
            case "M":
                consumer?.genre = .male
            case "F":
                consumer?.genre = .female
            case "NB":
                consumer?.genre = .notBinary
            default:
                consumer?.genre = .notInformed
            }
        }

        if let phone = call.getString("phone") {
            consumer = (consumer == nil) ? FSPConsumer() : consumer
            consumer?.phone = phone
        }

        if let birthday = call.getString("birthday") {
            consumer = (consumer == nil) ? FSPConsumer() : consumer
            let dateFormatter = DateFormatter()
            dateFormatter.dateFormat = "yyyy-MM-dd"
            if let date = dateFormatter.date(from: birthday) {
                consumer?.birthday = date
            }
        }

        if let address = call.getString("address") {
            consumer = (consumer == nil) ? FSPConsumer() : consumer
            let tokens = address.split(separator: ";")
            if tokens.count > 6 {
                let address = FSPAddress()
                address.streetName = String(tokens[0])
                address.streetNumber = String(tokens[1])
                address.complement = String(tokens[2])
                address.neighborhood = String(tokens[3])
                address.city = String(tokens[4])
                address.state = String(tokens[5])
                address.zipCode = String(tokens[6])

                consumer?.address = address
            }
        }

        if let consumer = consumer {
            smartPromo.consumer = consumer
        }

        return smartPromo
    }

    func parseCampaignColor(_ smartPromo: SmartPromo, _ call: CAPPluginCall) -> SmartPromo {
        if let colorHex = call.getString("color") {
            var cString: String = colorHex.trimmingCharacters(in: .whitespacesAndNewlines).uppercased()

            if cString.hasPrefix("#") {
                cString.remove(at: cString.startIndex)
            }

            if cString.count == 6 {
                var rgbValue: UInt64 = 0
                Scanner(string: cString).scanHexInt64(&rgbValue)

                let color = UIColor(
                    red: CGFloat((rgbValue & 0xFF0000) >> 16) / 255.0,
                    green: CGFloat((rgbValue & 0x00FF00) >> 8) / 255.0,
                    blue: CGFloat(rgbValue & 0x0000FF) / 255.0,
                    alpha: 1.0
                )

                smartPromo.setColor(color)
            } else {
                smartPromo.setColor(UIColor.gray)
            }
        }
        return smartPromo
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

    @objc func startMultiCampaigns(_ call: CAPPluginCall) {
        let starter = SmartPromoStarter()
        starter.startMultiCampaigns(call)
    }

    @objc func startScanner(_ call: CAPPluginCall) {
        let starter = SmartPromoStarter()
        starter.startScanner(call)
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
```

### Iniciando a SDK no modo campanha:
SmartPromoPlugin.startCampaign({
  campaignID: 'SEU_ID_DA_CAMPANHA',
  accessKey: 'SUA_KEY',
  secretKey: 'SUA_SECRET',
  cpf: '12345678900',
  name: 'John Doe',
  email: 'johndoe@gmail.com',
  genre: 'M',
  phone: '123456789',
  birthday: '1980-01-01',
  address: 'Rua sem nome;100;apto 101;ipanema;Porto Alegre;RS;90500000',
  isHomolog: true, // Opcional
  metadata: 'Informação adicional' // Opcional
}); 

### Iniciando a SDK no modo multi campanhas:
SmartPromoPlugin.startMultiCampaigns({
  headnote: 'Campanha especial',
  title: 'Promoção de Natal',
  message: 'Participe e ganhe prêmios!',
  accessKey: 'SUA_KEY',
  secretKey: 'SUA_SECRET',
  cpf: '12345678900',
  name: 'John Doe',
  email: 'johndoe@gmail.com',
  genre: 'M',
  phone: '123456789',
  birthday: '1980-01-01',
  address: 'Rua sem nome;100;apto 101;ipanema;Porto Alegre;RS;90500000',
  isHomolog: true, // Opcional
  metadata: 'Informação adicional' // Opcional
});

### Iniciando a SDK no modo Scanner de notas:
SmartPromoPlugin.startScanner({
  campaignID: 'SEU_ID_DA_CAMPANHA',
  accessKey: 'SUA_KEY',
  secretKey: 'SUA_SECRET',
  consumerID: 'ID_DO_CONSUMIDOR',
  cpf: '12345678900',
  name: 'John Doe',
  email: 'johndoe@gmail.com',
  genre: 'M',
  phone: '123456789',
  birthday: '1980-01-01',
  address: 'Rua sem nome;100;apto 101;ipanema;Porto Alegre;RS;90500000',
  isHomolog: true, // Opcional
  metadata: 'Informação adicional' // Opcional
});
```

> **Importante:** `campaignID`, `accessKey` e `secretKey` serão fornecidos pelo time da **Getmo** para serem configurados no seu projeto.
