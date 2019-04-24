# Modelos de Notificações

A seguir são apresentados os modelos de notificações fornecidos de forma nativa em conjunto a SDK mobile da plataforma SMARTPUSH e como configura-las via push. O envio da configuração pode ser feito através do painel do SMARTPUSH ou via chamada a API REST.

O uso destas notificações elimina a necessidade do desenvolvedor de aplicativos de criar notificações sofisticadas para engajar sua audiência. Todavia o uso não é obrigatório podendo o desenvolvedor criar suas próprias notificações.

## Notificações iOS

### Formatos de mídia

| Midia  | Recomendações  |
|---|---|
| **Imagem** | Respeite a proporção 2:1. Recomendamos arquivos png com tamanho de 800x450. |
| **Audio** | Crie trilhas em formato mp3. Preferencialmente com no máximo 10seg de duração. |
| **Video** | Recomendamos videos com no máximo 30seg de duração. O video pode ser de alta qualidade. |
| **GIF** | Respeite a proporção 2:1. recomendamos arquivos com tamanho de 800x450. |

**Obs**: Sempre teste as notificações com as mídias criadas. Isso é importante para garantir o melhor ajuste no momento da apresentação da notificação.

### 1. Notificação Simples

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| aps.alert.title  | Primeira linha da notificação. É apresentada em **negrito**  |
| aps.alert.body  | Segunda linha da notificação  |
| mutable-content  | Parâmetro obrigatório. Deve receber sempre o valor = 1. É necessário para o acionamento da contabilização dos eventos de impressão da notificação e abertura da app.  |
| custom  | abaixo do **"mutable-content"** adicione outros atributos que você precise enviar para o aplicativo quando a notificação for acionada pelo usuário.  |

**Exemplo**

```json
{	
    "notifications": [{
		"params": {
			"aps": {
				"alert": {
					"title": "Os Simpsons na FOX",
					"body": "Assine agora a FOX e não perca nenhum episódio da nova temporada de Os Simpsons"
				},
				"mutable-content": 1,
                "comment":"Adicione outros parâmetros aqui para usar na sua app"
			}
		}, "extra":[]
	}]
}
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/ios_simples.jpg)

### 2. Notificação com aplicação de banner

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| aps.alert.title  | Primeira linha da notificação. É apresentada em **negrito**  |
| aps.alert.body  | Segunda linha da notificação  |
| mutable-content  | Parâmetro obrigatório. Deve receber sempre o valor = 1. É necessário para o acionamento da contabilização dos eventos de impressão da notificação e abertura da app.  |
| aps.category  | Parâmetro obrigatório. Deve receber sempre o valor = **BANNER**  |
| mediaURL  | Parâmetro obrigatório. Deve receber o link para a imagem que será usada como banner.  |
| custom  | abaixo do **"mutable-content"** adicione outros atributos que você precise enviar para o aplicativo quando a notificação for acionada pelo usuário.  |

**Exemplo**


```json
{
	"notifications": [{
		"params": {
			"aps": {
				"alert": {
					"body": "Assine agora a FOX e não perca nenhum episódio da nova temporada de Os Simpsons",
					"title": "Os Simpsons na FOX"
				},
				"mutable-content": 1,
				"category": "BANNER"
			},
			"mediaURL": "https://s.aolcdn.com/hss/storage/midas/b386937631a1f03665c1d57289070898/203417456/simpsons.jpg"
		}, "extra":[]
	}]
}
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/ios_banner.jpg)

### 3. Notificação com aplicação de GIF (animação)

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| aps.alert.title  | Primeira linha da notificação. É apresentada em **negrito**  |
| aps.alert.body  | Segunda linha da notificação  |
| mutable-content  | Parâmetro obrigatório. Deve receber sempre o valor = 1. É necessário para o acionamento da contabilização dos eventos de impressão da notificação e abertura da app.  |
| aps.category  | Parâmetro obrigatório. Deve receber sempre o valor = **GIF**  |
| mediaURL  | Parâmetro obrigatório. Deve receber o link para o gif que será usada na notificação.  |
| custom  | abaixo do **"mediaURL"** adicione outros atributos que você precise enviar para o aplicativo quando a notificação for acionada pelo usuário.  |

**Exemplo**


```json
{
	"notifications": [{
		"params": {
			"aps": {
				"alert": {
					"body": "Assine agora a FOX e não perca nenhum episódio da nova temporada de Os Simpsons",
					"title": "Os Simpsons na FOX"
				},
				"mutable-content": 1,
				"category": "GIF"
			},
			"mediaURL": "http://media3.giphy.com/media/kEKcOWl8RMLde/giphy.gif"
		}, "extra":[]
	}]
}
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/ios_gif.gif)

### 4. Notificação com aplicação de Audio

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| aps.alert.title  | Primeira linha da notificação. É apresentada em **negrito**  |
| aps.alert.body  | Segunda linha da notificação  |
| mutable-content  | Parâmetro obrigatório. Deve receber sempre o valor = 1. É necessário para o acionamento da contabilização dos eventos de impressão da notificação e abertura da app.  |
| aps.category  | Parâmetro obrigatório. Deve receber sempre o valor = **AUDIO**  |
| mediaURL  | Parâmetro obrigatório. Deve receber o link para o audio que será usado na notificação.  |
| custom  | abaixo do **"mediaURL"** adicione outros atributos que você precise enviar para o aplicativo quando a notificação for acionada pelo usuário.  |

**Exemplo**


```json
{
	"notifications": [{
		"params": {
			"aps": {
				"alert": {
					"body": "Assine agora a FOX e não perca nenhum episódio da nova temporada de Os Simpsons",
					"title": "Os Simpsons na FOX"
				},
				"mutable-content": 1,
				"category": "AUDIO"
			},
			"mediaURL": "http://www.audiocheck.net/Audio/audiocheck.net_welcome.mp3"
		}, "extra":[]
	}]
}
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/ios_audio.gif)

### 5. Notificação com aplicação de Video

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| aps.alert.title  | Primeira linha da notificação. É apresentada em **negrito**  |
| aps.alert.body  | Segunda linha da notificação  |
| mutable-content  | Parâmetro obrigatório. Deve receber sempre o valor = 1. É necessário para o acionamento da contabilização dos eventos de impressão da notificação e abertura da app.  |
| aps.category  | Parâmetro obrigatório. Deve receber sempre o valor = **VIDEO**  |
| mediaURL  | Parâmetro obrigatório. Deve receber o código do video (no youtube) que será usado na notificação.  |
| custom  | abaixo do **"mediaURL"** adicione outros atributos que você precise enviar para o aplicativo quando a notificação for acionada pelo usuário.  |

**Exemplo**


```json
{
	"notifications": [{
		"params": {
			"aps": {
				"alert": {
					"body": "Assine agora a FOX e não perca nenhum episódio da nova temporada de Os Simpsons",
					"title": "Os Simpsons na FOX"
				},
				"mutable-content": 1,
				"category": "VIDEO"
			},
			"mediaURL": "lW4pUQdRo3g"
		}, "extra":[]
	}]
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/ios_video.gif)

### 6. Notificação com aplicação de Carrossel

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| aps.alert.title  | Primeira linha da notificação. É apresentada em **negrito**  |
| aps.alert.body  | Segunda linha da notificação  |
| mutable-content  | Parâmetro obrigatório. Deve receber sempre o valor = 1. É necessário para o acionamento da contabilização dos eventos de impressão da notificação e abertura da app.  |
| aps.category  | Parâmetro obrigatório. Deve receber sempre o valor = **CARROUSSEL**  |
| banners  | Parâmetro obrigatório. É um array de imagens e links para a abertura da app  |
| carouselType  | Parâmetro obrigatório. Definie a aparência das transições do carrossel. O conjunto de configurações possíveis são: **COVERFLOW**, **ROTARY**, **LINEAR**, **CYLINDER**, **CYLINDER_INVERTED**, **TIME_MACHINE**, **TIME_MACHINE_INVERTED** |
| custom  | abaixo do **"carouselType"** adicione outros atributos que você precise enviar para o aplicativo quando a notificação for acionada pelo usuário.  |

**Exemplo**


```json
{
	"notifications": [{
		"params": {
			"aps": {
					"body": "Quer saber como funciona?",
					"title": "Promo Destino Inesquecível ✈️⚽️🍀"
				"mutable-content": 1,
				"category": "CARROUSSEL"
			},
			"banners": [{
				"mediaURL": "http://cdn.getmo.com.br/images/card1.png",
				"uri": "bourboncard://promo"
			}, {
				"mediaURL": "http://cdn.getmo.com.br/images/card2.png",
				"uri": "bourboncard://promo"
			}, {
				"mediaURL": "http://cdn.getmo.com.br/images/card3.png",
				"uri": "bourboncard://promo"
			}, {
				"mediaURL": "http://cdn.getmo.com.br/images/card4.png",
				"uri": "bourboncard://promo"
			}],
			"carouselType": "COVERFLOW"
		}, "extra":[]
	}]
}
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/ios_carrossel.gif)

### 7. Cancelamento de Notificação

Abaixo segue a lista de parâmetros para a configuração da notificação de cancelamento. Esse push é um push especial, ele remove a notificação no dispositivo do usuário.

| Parâmetros  | Obs  |
|---|---|
| aps.alert  | Parâmetro obrigatório.  |
| mutable-content  | Parâmetro obrigatório. Deve receber sempre o valor = 1. É necessário para o acionamento da contabilização dos eventos de impressão da notificação e abertura da app.  |
| REMOVE_OLD  | Parâmetro obrigatório. Deve receber sempre o valor = 1. |

**Exemplo**

```json
{
	"notifications": [{
		"params": {
			"aps": {
				"alert": "Cancelamento",
				"mutable-content": 1
			},
			"REMOVE_OLD": 1
		}, "extra":[]
	}]
}
```

**Print de Exemplo**

Não se aplica.

## Notificações Android

### Formatos de mídia

| Midia  | Recomendações  |
|---|---|
| **Imagem** | Respeite a proporção 2:1. Recomendamos arquivos png com tamanho de 800x450. |
| **Video** | Recomendamos videos com no máximo 30seg de duração. O video pode ser de alta qualidade. |

**Obs**: Sempre teste as notificações com as mídias criadas. Isso é importante para garantir o melhor ajuste no momento da apresentação da notificação.


### 1. Notificação Simples

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| type  | Parâmetro obrigatório. Representa o tipo da notificação. Usar o valor PUSH |
| provider  | Parâmetro obrigatório. Garante que a notificação será tratada pela SDK Smartpush   |
| title  | Parâmetro obrigatório. Primeira linha da notificação.  |
| detail  | Parâmetro obrigatório. Segunda linha da notificação.  |
| url  | Parâmetro obrigatório. Link da ação que deve ser executada quando a notificação for clicada.  |
| video  | Parâmetro não-obrigatório. Deve receber o código do video (no youtube) que será usado na notificação.  |


**Exemplo**

```json
{
  "notifications": [
    {
      "params": {
        "type": "PUSH",
        "provider": "smartpush",
        "title": "Go Getmo",
        "detail": "Notificações que engajam!",
        "url": "getmo://home",
        "video": "lW4pUQdRo3g"
      }, "extra":[]
    }
  ]
}
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/android_simples.jpg)

### 2. Notificação com aplicação de banner

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| type  | Parâmetro obrigatório. Representa o tipo da notificação. Usar o valor PUSH |
| provider  | Parâmetro obrigatório. Garante que a notificação será tratada pela SDK Smartpush   |
| title  | Parâmetro obrigatório. Primeira linha da notificação.  |
| detail  | Parâmetro obrigatório. Segunda linha da notificação.  |
| banner  | Parâmetro obrigatório. Deve receber o link para o banner que será usado na notificação. |
| url  | Parâmetro obrigatório. Link da ação que deve ser executada quando a notificação for clicada.  |
| video  | Parâmetro não-obrigatório. Deve receber o código do video (no youtube) que será usado na notificação.  |


**Exemplo**

```json
{
  "notifications": [
    {
      "params": {
        "type": "PUSH",
        "provider": "smartpush",
        "title": "Go Getmo",
        "detail": "Notificações que engajam!",
        "banner": "",
        "url": "getmo://home",
        "video": "lW4pUQdRo3g"
      }, "extra":[]
    }
  ]
}
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/android_banner.jpg)

### 3. Notificação com aplicação de GIF (animação)

    ** EM BREVE **

### 4. Notificação com aplicação de Audio

    ** EM BREVE **

### 5. Notificação com aplicação de Video

    ** VER CONFIGURAÇÃO DAS DEMAIS NOTIFICAÇÕES **

### 6. Notificação com aplicação de Carrossel

Abaixo segue a lista de parâmetros para a configuração da notificação.

| Parâmetros  | Obs  |
|---|---|
| type  | Parâmetro obrigatório. Representa o tipo da notificação. Usar o valor PUSH |
| provider  | Parâmetro obrigatório. Garante que a notificação será tratada pela SDK Smartpush   |
| title  | Parâmetro obrigatório. Primeira linha da notificação.  |
| detail  | Parâmetro obrigatório. Segunda linha da notificação.  |
| banner  | Parâmetro obrigatório. Deve receber o link para o banner que será usado na notificação. Fallback para dispositivos que não suportam a notificação carrossel. |
| url  | Parâmetro obrigatório. Link da ação que deve ser executada quando a notificação for clicada. Fallback para dispositivos que não suportam a notificação carrossel. |
|---|---|
| frame:??:banner  | **??** é substituido por um número de 1-5. Define a URL do banner que deve ser carregado no item do carrossel. |
| frame:??:url  | **??** é substituido por um número de 1-5. Define a URL que será carregada quando o item do carrossel for clicado. |

**Exemplo**

```json
{
	"notifications": [{
		"params": {
			"provider": "smartpush",
			"type": "CARROUSSEL",
			"title": "Buscape",
			"detail": "Escolhemos ofertas especiais para voce!",
			"banner": "https://movietvtechgeeks.com/wp-content/uploads/2017/06/xbox-one-vs-ps4-long-battle-images.jpg",
			"url": "buscape://search?productId=27062&site_origem=23708552"
		},
		"extra": {
			"frame:1:banner": "https://movietvtechgeeks.com/wp-content/uploads/2017/06/xbox-one-vs-ps4-long-battle-images.jpg",
			"frame:1:url": "buscape://search?productId=27062&site_origem=23708552",
			"frame:2:banner": "https://i.pinimg.com/originals/fe/63/26/fe6326895705f9f34f250fe274ca9bf3.png",
			"frame:2:url": "buscape://search?productId=606585&utm_source=alertadepreco&utm_medium=push&utm_campaign=606585",
			"frame:3:banner": "https://www.digiseller.ru/preview/115936/p1_2179893_ef9d38d0.jpg",
			"frame:3:url": "buscape://search?productId=623321&utm_source=alertadepreco&utm_medium=push&utm_campaign=623321",
			"frame:4:banner": "https://www.digiseller.ru/preview/115936/p1_2179893_ef9d38d0.jpg",
			"frame:4:url": "buscape://search?productId=623321&utm_source=alertadepreco&utm_medium=push&utm_campaign=623321",
			"frame:5:banner": "https://www.digiseller.ru/preview/115936/p1_2179893_ef9d38d0.jpg",
			"frame:5:url": "buscape://search?productId=623321&utm_source=alertadepreco&utm_medium=push&utm_campaign=623321"
		}
	}]
}
```

**Obs 1**.: Você pode copiar e colar essa configuração no painel do SMARTPUSH para testar.

**Obs 2**.: Você pode facilmente adicionar emojis as suas notificações. Para isso basta usar um teclado emoji no momento da configuração do push.

![](img/android_carrossel.gif)

### 7. Cancelamento de Notificação

    ** EM BREVE **

### 8. Notificações Locais

A SDK permite a utilização de notificações locais originadas por APIs de terceiros.

#### Exemplos

**Método no padrão builder montar a notificação:**
```
    private func requestPushNotification(_ notification: SmartpushNotification) {
        let builder  = SmartpushNotificationBuilder()
        builder.title = notification.title
        builder.body = notification.body
        builder.category = notification.category
        builder.carouselType = notification.carouselType
        builder.mediaUrl = notification.mediaUrl
        builder.banners = notification.banners
        builder.build()
    }
```

**Enum para as categorias**
```
enum Categorys:String{
    case PushNormal = "PUSH",
    PushEmoji = "EMOJI",
    PushImage = "BANNER",
    PushGif = "GIF",
    PushAudio = "AUDIO",
    PushVideo = "VIDEO",
    PushCarouselCoverflow = "COVERFLOW",
    PushCarouselRotary = "ROTARY",
    PushCarouselCylinder = "CYLINDER",
    PushCarouselCylinderInverted = "INVERTED" ,
    PushCarouselLinear = "LINEAR",
    PushCarouselStack = "TIME_MACHINE",
    PushCarouselStackInverted = "TIME_MACHINE_INVERTED"
}
```

**Push normal:**
```
	let notification  = SmartpushNotification()
	notification.title = "Oferta"
	notification.body = "Temos algumas ofertas especiais para você"
	notification.category = "PUSH"
	requestPushNotification(notification)
```

**Push emoji:**
```
    let notification  = SmartpushNotification()
    notification.title = "Oferta"
    notification.body = "Temos algumas ofertas especiais para você 😃"
    notification.category = Categorys.PushEmoji.rawValue // enum
    requestPushNotification(notification)
```

**Imagem:**
```
    let notification  = SmartpushNotification()
    notification.title = "Oferta"
    notification.body = "Temos algumas ofertas especiais para você"
    notification.mediaUrl = "http://cdn.getmo.com.br/images/1512note.png"
    notification.category = Categorys.PushImage.rawValue
    requestPushNotification(notification)
```

**Gif:**
```
    let notification  = SmartpushNotification()
    notification.title = "Oferta"
    notification.body = "Temos algumas ofertas especiais para você"
    notification.mediaUrl = "https://media.giphy.com/media/qi29MoLjWNPUI/giphy.gif"
    notification.category = Categorys.PushGif.rawValue
    requestPushNotification(notification)
```

**Audio:**
```
    let notification  = SmartpushNotification()
    notification.title = "Oferta"
    notification.body = "Temos algumas ofertas especiais para você"
    notification.mediaUrl = "http://www.audiocheck.net/Audio/audiocheck.net_welcome.mp3"
    notification.category = Categorys.PushAudio.rawValue
    requestPushNotification(notification)
```

**Vídeo:**
```
    let notification  = SmartpushNotification()
    notification.title = "Oferta"
    notification.body = "Temos algumas ofertas especiais para você"
    notification.mediaUrl = "0S6pHex-KCo" // id do video do YouTube
    notification.category = Categorys.PushVideo.rawValue
    requestPushNotification(notification)
```

**Carroussel:**
```
    let banner1 = "http://cdn.getmo.com.br/images/1512note.png"
    let banner2 = "http://cdn.getmo.com.br/images/1312phone.png"
    let banner3 = "http://cdn.getmo.com.br/images/2607fog.png"
    let banner4 = "http://cdn.getmo.com.br/images/1001cafeteira.png"
    let banner5 = "http://cdn.getmo.com.br/images/0110galax.png"
    let banner6 = "http://cdn.getmo.com.br/images/2507Tablet.png"
    let banners = [banner1, banner2, banner3, banner4, banner5, banner6];

    let notification  = SmartpushNotification()
    notification.title = "Oferta"
    notification.body = "Temos algumas ofertas especiais para você"
    notification.banners = banners
    notification.category = "CARROUSSEL"
    notification.carouselType = Categorys.PushCarouselCoverflow.rawValue
    requestPushNotification(notification)
```