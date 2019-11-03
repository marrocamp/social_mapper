# Social Mapper (Solução de problemas) 

Um guia rápido para solução de problemas do Social Mapper, para ajudá-lo a corrigir problemas que possam surgir 
devido a alterações no site.

Envie solicitações pull e volte atrás se você corrigir algo que um site de mídia social mudou!

## Por que o Social Mapper está quebrado?

O Social Mapper pode parar de funcionar em determinados sites de mídia social que alteram os nomes dos HTML classes  que Social
O Mapper usa para extrair informações das páginas da web.

## Como faço para corrigir isso?

Para corrigir um site danificado, basta abrir o Python module no [modules](modules) pasta e atualize os nomes das classes
que o BeautifulSoup está procurando ou alterando o código de análise.

Na imagem abaixo, você pode ver várias das coisas nas quais o Social Mapper se baseia:

* O URL que ele usa para procurar os destinos.
* O nome da classe da div que contém cada uma das pessoas retornadas da pesquisa.
* O código usado para manipular os dados extraídos para analisar o link para o perfil e a foto do perfil.

Por exemplo, no [facebookfinder.py](modules/facebookfinder.py) module, está encontrando perfis de usuário com base em
 \_401d div class. Se o Facebook mudasse isso, linha 77 no facebookfinder.py precisaria ser mudado adequadamente.

![Fixing Social Mapper](facebook-html-classes.png?raw=true "Fixing Social Mapper")