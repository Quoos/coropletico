## Mapa coroplético interativo de código aberto compatível com dispositivos móveis.
### Exemplo de aplicação na divulgação de dados da Covid-19.
![screenshot](https://docente.ifsc.edu.br/joao.quoos/scripts/images/GeoJSON-para_coropletico_leaflet_mapa.jpg "mapa interativo em diversas plataformas")
O presente conteúdo permitir exibir arquivo GeoJSON de polígonos e seus metadados oriundo de software SIG em formato coroplético usando a biblioteca JavaScript Leaflet e plugins. Dessa forma o usuário não necessita de adaptações ao código HTML e Javascript do site quando realiza atualização e ou substituição do arquivo GeoJSON.
Entre os principais recursos adaptados para essa aplicação estão as seguintes funcionalidades:
- Configuração de opções coropléticas por meio de plugin adicionados no script,
- Exibição de tabela com os dados do Geojson. isso permite ao usuário fazer a cópia tabulada dos dados referente aos polígonos do mapa,
- Adicionar marcador com a geolocalização do usuário por meio de botão,
- Botão para exibir em tela cheia (fullscreen),
- Web design responsivo por meio do uso de HTML e CSS para redimensionar automaticamente a página e assim dar uma boa aparência em todos os dispositivos (computadores, tablets e telefones).

(<a href="https://www.quoos.com.br/covid19/geo.mauricio.rizzatti/santamaria">Demo</a>) Demonstração do uso dos dados no site https://www.ufsm.br/coronavirus/observatorio/ .

![screenshot](https://docente.ifsc.edu.br/joao.quoos/scripts/images/mapa_santamaria_geografiaufsm.png "Mapa coroplético usando LeafLet")


## Uso:

- Para replicar o uso do mesmo script com outros dados GeoJSON segue abaixo lista de edições necessárias no arquivo `mapa.html`.
- Além disso quando seu arquivo geoJSON for gerado pelo software SIG o mesmo deve conter os seguintes atributos de dados:`bairro`, `confirmados` e `obitos` sem o uso de acentos. Em caso de dúvidas utilize o arquivo GeoJSON do exemplo como referência.

- (Importante) A função Geolocalização só funciona em servidor 'HTTPS' e o carregamento do arquivo GeoJSON só ocorre quando a consulta do arquivo `mapa.html`ocorrer em um serviço HTTP, não vai funciona se for file:///.

### Ajustes HTML

- Insira o título da página (Linha 7).

``` html
<title>Cidade de Santa Maria, RS, Brasil</title>
```
- Insira título da legenda que será exibido no canto superior esquerdo do mapa (Linha 123). Junto ao título está o botão que carrega os dados da tabela na tela.
``` html 
<div class="titulodalegenda">Casos Confirmados em 30/06/2020
```
### Ajuste no JavaScript

- .setView: Defina no código Javascript a localização central do seu mapa por meio de uma coordenada geográfica e o nível de zoom aplicado ao mapa ao carregar (Linha 135).
``` js
.setView([-29.7061, -53.7964], 12);
```

- L.tileLayer: Aqui você insere o código de URL para carregamento de mapa base. Na aplicação está indicada o uso do mapa base oferecido pela OpenStreetMaps (Linha 138). Em maxZoom vai indicar o máximo de zoom aplicado ao mapa quando o usuário realizar a ampliação. Em attribution será indicado os dados da fonte do mapa base e da origem do arquivo GeoJSON no formato HTML, tomando o cuidado para não deixar o texto muito extenso pois o mesmo é exibido no canto inferior direito do mapa.
``` js
L.tileLayer('	https://a.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  maxZoom: 18,
  attribution: '<a href="http://www.openstreetmap.org/copyright">OSM</a>|Dados <a href="https://www.ufsm.br/coronavirus/observatorio/">Observ. Covid19 UFSM</a>'
}).addTo(map)
```

- getJSON: Indique a localização do arquivo GeoJSON. No exemplo ele está em uma pasta denominada dados_qgis (Linha 146).
``` js
$.getJSON('dados_qgis/bairros.geojson', function (geojson) {
```

- choroplethLayer: Aqui são indicadas as informações para que o mapa em GeoJSON seja exibido de forma coroplética (150). Em     valueProperty indique qual propriedade do GeoJSON que você irá utilizar, no exemplo é o atributo Confirmado. Em scale é indicado a escala de cores aplicadas ao mapa e utilizadas na legenda gráfica do canto inferior direito. Não é obrigatório indicar uma cor para cada classe, mas sim a cor do menor valor até o maior valor. Este campo exige a cor no formato hexadecimal mas pode exibir em outros formatos e é baseado em uma aplicação de cores denominada Chroma.js (https://vis4.net/chromajs/) muito eficiente para criação de paletas incluindo adaptação coroplética para daltônicos (https://gka.github.io/palettes/). Em steps é informado o número de intervalos no exemplo foram utilizados 6. Por fim e também muito importante é necessário indicar o método de distribuição nos intervalos, sendo q para quantil, e para equidistante, k para K-means.
``` js
valueProperty: 'confirmados',
scale: ['#ffffff','#ffffd4','#fed98e','#fe9929','#d95f0e','#993404'],
steps: 6,
mode: 'e',
```
### Adaptações para GeoJSON com outros campos:
Caso você queira ajustar o arquivo para aceitar outro GeoJSON com nomes de atributos diferentes do utilizado no arquivo, as instruções seguem como comentários dentro do arquivo `mapa.html`.

#### Plugins utilizados:
- Leaflet: Biblioteca JavaScript para mapas. https://leafletjs.com
- Leaflet Choropleth: Plugin coroplético para Leaflet (escala de cores baseada em valor) https://github.com/timwis/leaflet-choropleth
- Leaflet.Locate: Um plugin útil para geolocalizar o usuário. https://github.com/domoritz/leaflet-locatecontrol
- Leaflet.fullscreen: Plugin para exibir em fullscreen mapas Leaflet. https://github.com/Leaflet/Leaflet.fullscreen

- Jquery: Biblioteca JavaScript rápida, pequena e rica em recursos, usado na aplicação para leitura do GeoJSON e dos dados. https://jquery.com/

 #### Observações
A Pandemia de Coronavírus (Covid-19) nas cidades abrangidas pela Universidade Federal de Santa Maria (UFSM) e no estado do Rio Grande do Sul é monitorada por um grupo multidisciplinar no site https://www.ufsm.br/coronavirus/observatorio/ onde estão outros mapas.

João Henrique Quoos - https://www.quoos.com.br

