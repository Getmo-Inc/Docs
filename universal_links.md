# Universal Links #

É um único link que pode ser usado em todos os lugares: em cada plataforma, cada dispositivo e cada loja de aplicativos para direcionar os usuários para a sua aplicação. É um único link, totalmente configurado para conectar-se a sua aplicação vindo de todos os canais (Facebook, Twitter, Gmail, etc) e ser listado em cada portal de busca (Google App Indexing, Bing, Apple Spotlight, etc).

# O que os Universal Links podem fazer? #

Toda estratégia de marketing em app consiste basicamente em adquirir e reter usuários. Muito investimento é feito em marketing digital com este objetivo, mas você sabe qual canal digital que trás mais resultados, e qual trás menos? Isso é importante para decidir onde intensificar seu investimento e esforços, e quais abandonar. A escolha do canal digital mais apropriado para a sua audiência fará toda a diferença nos seus resultados de aquisição e retenção de usuários. Facebook, Google fornecem informações que auxiliam neste sentido, mas são soluções descentralizadas, e que deveriam ser auditadas, mas e os outros canais como emails, sms, push, in-app message? Como montar uma estratégia de marketing sem informações centralizadas e atualizadas?


# Guia de Integração Universal Link #
Aqui estão as etapas para ter o Universal Links funcionando no seu aplicativo:

1. Configure seu aplicativo para registrar domínios aprovados
  1. Registre seu aplicativo no developers.apple.com
  1. Ative ‘Associated Domains’ no seu identificador de aplicativo
  1. Ative ‘Associated Domain’ no seu projeto Xcode
  1. Adicione o domain entitlement correto
  1 Tenha certeza de que o arquivo de entitlements está incluso na compilação
  
1. Configure o seu website para hospedar o arquivo ‘apple-app-site-association’
  1. Compre um nome de domínio ou escolha a partir de um já existente
  1. Adquira uma certificação SSL para o nome de domínio
  1. Crie o arquivo JSON estruturado ‘apple-app-site-association’
  1. Assine o arquivo JSON com a certificação SSL

# Configure o seu app entitlements #
Para registrar seu projeto Xcode para Universal Links, você precisa criar uma App ID no portal de desenvolvedores da Apple e ativar os entitlements. Isto é muito semelhante à configuração exigida para push notifications.

Você não pode usar um identificador de aplicativo geral para Universal Links

**Registre o seu aplicativo em developer.apple.com:**

1. Primeiro, vá até developers.apple.com e faça a autenticacão. Em seguida, clique em “Certificate, Identifiers & Profiles” e depois clique em “Identifiers”.
<img src="http://cdn.getmo.com.br/images/universal_links/developer_portal.png" width="500">

Se você não tem um App Identifier já registado, você vai precisar criar um clicando no sinal de +. Se você tem um, pule para a próxima seção.

Você precisa preencher 2 campos aqui: nome e bundle ID. Para o nome, você, basicamente, coloca o que quiser. Para bundle ID, você vai preencher com o valor do bundle

Essa parte é bastante intuitiva, principalmente depois das últimas atualizações do portal.

2. Ativar ‘Associated Domains’ no seu identifier de aplicativo em developers.apple.com.

3. Ativar ‘Associated Domains’ no seu projeto Xcode.

4. Adicione o domain entitlement.
<img src="http://cdn.getmo.com.br/images/universal_links/associated_domains.png" width="500">



