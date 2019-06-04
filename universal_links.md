# Universal Links #

É um único link que você pode usar em todos os lugares: em cada plataforma, cada dispositivo e cada loja de aplicativos para direcionar os usuários para a sua aplicação. É um único link, totalmente configurado para conectar-se a sua aplicação vindo de todos os canais (Facebook, Twitter, Gmail, etc) e ser listado em cada portal de busca (Google App Indexing, Bing, Apple Spotlight, etc).

# O que as Universal Links podem fazer? #

"Toda estratégia de marketing em app consiste basicamente em adquirir e reter usuários. Muito investimento é feito em marketing digital com este objetivo, mas você sabe qual canal digital que trás mais resultados, e qual trás menos? Isso é importante para decidir onde intensificar seu investimento e esforços, e quais abandonar. A escolha do canal digital mais apropriado para a sua audiência fará toda a diferença nos seus resultados de aquisição e retenção de usuários. Facebook, Google fornecem informações que auxiliam neste sentido, mas são soluções descentralizadas, e que deveriam ser auditadas, mas e os outros canais como emails, sms, push, in-app message? Como montar uma estratégia de marketing sem informações centralizadas e atualizadas?"


# Guia de Integração Universal Link #
Aqui estão as etapas de alto nível para ter o Universal Links funcionando no seu aplicativo:

1. Configure seu aplicativo para registrar domínios aprovados
1.1 Registre seu aplicativo no developers.apple.com

1.2 Ative ‘Associated Domains’ no seu identificador de aplicativo

1.3 Ative ‘Associated Domain’ no seu projeto Xcode

1.4 Adicione o domain entitlement correto

1.5 Tenha certeza de que o arquivo de entitlements está incluso na compilação


# Configure o seu app entitlements #
Para registrar seu projeto Xcode para Universal Links, você precisa criar uma App ID no portal de desenvolvedores da Apple e ativar os entitlements adequados. Isto é muito semelhante à configuração exigida para compras no aplicativo.

Você não pode usar um identificador de aplicativo curinga para Universal Links

Registre o seu aplicativo em developer.apple.com:
Primeiro, vá até developers.apple.com e faça a autenticacão. Em seguida, clique em “Certificate, Identifiers & Profiles” e depois clique em “Identifiers”.
![picture](http://cdn.getmo.com.br/images/universal_links/developer_portal.png)




