# Social Mapper

A Social Media Mapping Ferramenta que correlaciona perfis via reconhecimento facial (https://github.com/marrocamp)).

O Social Mapper é uma ferramenta de inteligência de código aberto que usa o reconhecimento facial para correlacionar 
perfis de mídia social em diferentes sites em grande escala. É necessária uma abordagem automatizada para pesquisar 
nos sites populares de mídia social os nomes e as imagens dos alvos para detectar e agrupar com precisão a presença 
de uma pessoa, apresentando os resultados em um relatório que um operador humano pode revisar rapidamente.

O Social Mapper possui diversos usos no setor de segurança, por exemplo, a coleta automatizada de grandes 
quantidades de perfis de mídia social para uso em sites direcionados campangas phishing. 
O reconhecimento facial ajuda esse processo removendo falsos positivos nos resultados da pesquisa, para que a 
análise desses dados seja mais rápida para um operador humano.

Social Mapper suporta as seguintes plataformas de mídia social:

* LinkedIn
* Facebook
* Twitter
* Google Plus
* Instagram
* VKontakte
* Weibo
* Douban

O Social Mapper usa uma variedade de tipos de entrada, como:

* O nome de uma organização, pesquisando via LinkedIn
* Uma pasta cheia de imagens nomeadas
* Um arquivo CSV com nomes e URLs para imagens online

## Casos de uso (por que você deseja executar isso)

Social Mapper é voltado principalmente para Testadores de penetração e Red Teamers, que o usarão para expandir e
segmentar listas e encontrar em perfis de mídia social. A partir daqui, o que você faz é limitado apenas pela sua 
imaginação, mas aqui estão algumas idéias para começar:

(Nota: O Social Mapper não realiza esses ataques, ele reúne os dados necessários para executá-los em uma escala em massa.)

* Crie perfis de mídia social falsos para 'um amigo' dos alvos e envie links ou malware. Estatísticas recentes
mostram que os usuários de mídia social têm duas vezes mais chances de clicar em links e abrir documentos em comparação 
com aqueles entregue por e-mail.
* Engane os usuários a divulgarem seus e-mails e números de telefone com comprovantes e ofertas para fazer o pivô
para dentro phishing, vishing ou smishing.
* Crie campanhas de phishing personalizadas para cada site de mídia social, sabendo que o destino tem uma conta. Faça estes mais
realista, incluindo a foto do perfil no e-mail. Capture a senha para reutilização de senha.
* Veja fotos de destino procurando emblemas do cartão de acesso dos funcionários e familiarize-se com building interiores.

## Começando

Estas instruções mostrarão os requisitos e como usar Social Mapper.

### Pré-requisitos

Como essa é uma ferramenta baseada em Python3, ela deve rodar teoricamente no Linux, 
ChromeOS ([Modo de desenvolvedor](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/generic)) 
e macOS. Os principais requisitos são o Firefox, Selenium e Geckodriver. Para instalar a ferramenta e configurá-la
siga estas 4 etapas:

1) Instale a versão mais recente do Mozilla Firefox para macOS aqui:

```
https://www.mozilla.org/en-GB/firefox/new/
```

Ou para Debian/Kali (mas não é necessário para o Ubuntu) obtenha a versão  non-ESR do Firefox com:
```
sudo add-apt-repository ppa:mozillateam/firefox-next && sudo apt update && sudo apt upgrade
```

Verifique se a nova versão do Firefox está no caminho. Caso contrário, adicione-o manualmente.

2) Instale o Geckodriver para o seu sistema operacional e verifique se ele está no seu caminho. No Mac, você pode colocá-lo
no `/usr/local/bin`, no ChromeOS, você pode colocá-lo`/usr/local/bin`, e no Linux você pode colocá-lo `/usr/bin`.

Download da versão mais recente do Geckodriver aqui:

```
https://github.com/mozilla/geckodriver/releases
```

3) Instale as bibliotecas necessárias:

No Linux, instale os seguintes pré-requisitos:
```
sudo apt-get install build-essential cmake
sudo apt-get install libgtk-3-dev
sudo apt-get install libboost-all-dev
```

No Linux e macOS, conclua a instalação com:

```
git clone https://github.com/marrocamp/social_mapper
cd social_mapper/setup
python3 -m pip3 install --no-cache-dir -r requirements.txt ou pip3 install -r requirements.txt
```

No Mac, observe as[setup/setup-mac.txt](setup/setup-mac.txt) para visualizar alguns xcode adicionais,
instruções de instalação do brew e xquartz.

4) Forneça credenciais ao Social Mapper para fazer logon nos serviços de mídia social:

```
Abra social_mapper.py e insira credenciais de mídia social em variáveis globais na parte superior do arquivo
```

5) No Facebook, verifique se o idioma da conta para a qual você forneceu credenciais está definido como 'English (US)' 
pela duração da corrida. Além disso, verifique se todas as suas contas estão funcionando e se podem fazer login
sem exigir autenticação de 2 fatores. 

## Usando o Social Mapper

O Social Mapper é executado na linha de comando usando uma mistura de parâmetros obrigatórios e opcionais. Você pode especificar opções
como tipo de entrada e quais sites verificar junto com vários outros parâmetros que afetam a velocidade e a precisão.

### Parâmetros necessários

Para iniciar a ferramenta, devem ser fornecidos 4 parâmetros, um formato de entrada, o arquivo ou pasta de entrada e o
modo de execução:

```
-f, --format	: Specify if the -i, --input is a 'name', 'csv', 'imagefolder' or 'socialmapper' resume file
-i, --input	: The company name, a CSV file, imagefolder or Social Mapper HTML file to feed into Social Mapper
-m, --mode	: 'fast' or 'accurate' allows you to choose to skip potential targets after a first likely match is found, in some cases potentially speeding up the program x20
```

Além disso, pelo menos um site de mídia social a ser verificado deve ser selecionado incluindo um ou mais dos 
seguintes itens:

```
-a, --all		: Selects all para todas as opções abaixo e verifica todos os sites nos quais o Social Mapper possui credenciais
-fb, --facebook		: Check Facebook
-tw, --twitter		: Check Twitter
-ig, --instagram	: Check Instagram
-li, --linkedin		: Check LinkedIn
-gp, --googleplus	: Check Google Plus
-vk, --vkontakte	: Check VKontakte
-wb, --weibo		: Check Weibo
-db, --douban		: Check Douban
```

### Parâmetros opcionais

Parâmetros opcionais e adicionais também podem ser configurados para adicionar personalização adicional à maneira como 
o Social Mapper é executado:

```
-t, --threshold		: Personaliza o limite de reconhecimento facial para correspondências, isso pode ser visto como a precisão da correspondência. O padrão é 'standard', mas pode ser definido como 'loose', 'standard', 'strict' ou 'superstrict'. 
Po exemplo 'loose' encontrará mais correspondências, mas algumas podem estar incorretas Enquanto 'strict' pode encontrar menos correspondências, mas também conter menos falsos positivos no relatório final. -cid, --companyid	: Additional parameter to add in a LinkedIn Company ID for if name searches are not picking the correct company.
-s, --showbrowser	: Torna o navegador Firefox visível para que você possa ver as pesquisas realizadas. Útil para debugging.
-v, --version		: Exibir versão atual.
-e, --email		: Forneça um formato de email fuzzy como "<f><last>@domain.com" para gerar Arquivos adicionais CSV para cada site
com nome, sobrenome, nome completo, email, profileURL, photoURL. Eles podem ser inseridos em estruturas de phishing, como
Gophish ou Lucy.
```

### Execuções exemplo

Aqui estão alguns exemplos de execuções para começar em diferentes casos de uso:

```
Uma rápida vereficação no Facebook e no Twitter em alguns destinos que você tem em uma pasta de imagens que planeja revisar manualmente
e não se importe com alguns falsos positivos:
python3 social_mapper.py -f imagefolder -i ./mytargets -m fast -fb -tw

Uma execução exaustiva em uma grande empresa em que os falsos positivos devem ser reduzidos ao mínimo:
python3 social_mapper.py -f company -i "SpiderLabs" -m accurate -a -t strict

Uma execução grande que precisa ser dividida em várias sessões devido ao tempo, a primeira executando no LinkedIn e no Facebook,
com a segunda retomando e preenchendo o Twitter, Google Plus e Instagram:
python3 social_mapper.py -f company -i "SpiderLabs" -m accurate -li -fb
python3 social_mapper.py -f socialmapper -i ./SpiderLabs-social-mapper-linkedin-facebook.html -m accurate -tw -gp -ig

Uma vereficação rápida (~5min) sem reconhecimento facial para gerar um CSV cheio de nomes, endereços de e-mail, perfis e
links de fotos de até 1000 pessoas retiradas de uma empresa do LinkedIn, onde o formato de e-mail é conhecido por "firstname.lastname":
python3 social_mapper.py -f company -i "SpiderLabs" -m accurate -li -e "<first>.<last>@spiderlabs.com"
```

### Solução de problemas

Os sites de mídia social geralmente alteram seus formatos de página e nomes de classe, se o Social Mapper não estiver funcionando para você em um 
site específico, confira o[docs](docs/TroubleShooting_Social_Mapper) seção para obter conselhos sobre solução de problemas sobre como 
para fixar isso. Sinta-se à vontade para enviar um pull request com suas correções.

### Maltego

um guia para carregar seus resultados do Social Mapper no Maltego, confira o [docs](docs/Maltego_Import_Guide) 
seção.


