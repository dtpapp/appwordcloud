---
title: "Criando aplicativos via Shiny do Rstudio"
author: "Rafael Teixeira e Fellipe Gomes"
date: "9 de fevereiro de 2018"
output: 
  html_document:
    toc: true
    toc_float: true
---

# Aplicativo para construir Wordcloud

Crie sua própria wordcloud!

![](https://github.com/dtpapp/appwordcloud/blob/master/img/shiny4.png?raw=true)

Link do aplicativo:  http://dtpapp.shinyapps.io/appwordcloud

# Motivação para o aplicativo

Inicialmente uma função foi desenvolvida com finalidade de se criar nuvens de palavras em conjunto com técnicas de textmining de forma dinâmica, o nome da função é `wordcloud_sentiment()` e seu código e os parâmetros para seu uso podem ser conferidos no arquivo `wordcloud_sentiment.R`.

Com essa função foi possível que a criação de nuvens de palavras com técnicas de textmining se tornasse uma tarefa mais ágil, porém o conhecimento prévio de R para sua implementação seria recomendado caso necessite de alguma manutenção. Diante disto a ideia de uma abordagem mais interativa mostrou-se interessante, a seguir é possível conferir como foi sua implementação.

# Pacotes utilizados:

Os pacotes utilizados incluindo algumas referências:

  * `flexdashboard`: Pacote para gerar o aplicativo como dashboard ( [manual do pacote](https://rmarkdown.rstudio.com/flexdashboard/) )
  * `stringr`:       Pacote para manipulação de strings ([bom manual para manipular strings](http://material.curso-r.com/stringr/)  )   
  * `dplyr`:         Pacote para manipulação de dados ([Cheat Sheet para Data Wrangling](http://tidy.ws/29i5Kq))                       
  * `tm`:            Pacote de para text mining ([manual do CRAN](http://tidy.ws/29i5Lr))                       
  * `wordcloud`:     Pacote para nuvem de palavras ([bom manual para text mining, wordcloud e sentimentos](http://tidy.ws/29i5UO))       
  * `readxl`:        Pacote para leitura de dados Excel ([github tidyverse](https://github.com/tidyverse/readxl) )        
  * `tidytext`:      Manipulação de textos ([e-book tidytextmining](https://www.tidytextmining.com/))             
  * `reshape2`:      Manipulação de dados ( [github do hadley](https://github.com/hadley/reshape) )          
  * `lexiconPT`:     Importar palavras de sentimentos ( [CRAN](http://tidy.ws/29i5QU)     )                  
  * `memoise`:       Cache resultados de uma função ([github memoise](https://github.com/r-lib/memoise))            
  * `SnowballC`:     Para steamming ([manual do CRAN ](http://tidy.ws/29i61a))                       
  * `purrr`:         Ferramentas de programação funcional ([cheat Sheet para funcoes apply](http://tidy.ws/29i5WN )) 
  * `DT`:            Renderizar tabela ( [manual do pacote](https://rstudio.github.io/DT/)  )             
  * `ngram`:         Busca por sequencias de palavras ([guia para ngram no R](http://tidy.ws/29i607)   )                    

# Instalando os pacotes

A instalação de todos esses pacotes pode ser realizada em apenas 1 passo, para mais informações consultar o documento `install_packages.R` e seguir as instruções de instalação 

# Criando dashboards

Apesar de se tratar de um aplicativo shiny, seu layout foi todo configurado com o auxílio do pacote [`flexdashboard`](https://rmarkdown.rstudio.com/flexdashboard/) que possibilita uma boa apresentação. 

![[Imagem do site oficial](https://rmarkdown.rstudio.com/flexdashboard/) ](https://github.com/dtpapp/appwordcloud/blob/master/img/shiny1.png?raw=true)

No  [manual do pacote](https://rmarkdown.rstudio.com/flexdashboard/)  é possível encontrar varias configurações de layout para o dashboad bem como uma [sessão que fala sobre incluir aplicativos Shiny no flexdashbard](https://rmarkdown.rstudio.com/flexdashboard/shiny.html) 

## Alguns possíveis componentes 

HTML Widgets são frameworks que fornecem ligações R de alto nível para bibliotecas de visualização de dados JavaScript) via Shiny. Os gráficos baseados em htmlwidgets são ideais para uso com flexdashboard porque eles podem redimensionar dinamicamente, de modo que quase sempre se encaixam perfeitamente dentro dos limites de seus recipientes flexdashboard.

Os htmlwidgets disponíveis incluem:

  * [Leaflet](http://rstudio.github.io/leaflet/), uma biblioteca para criar mapas dinâmicos que suportam panorâmica e zoom, com várias anotações como marcadores, polígonos e pop-ups.

  * [dygraphs](http://rstudio.github.io/dygraphs), que fornece recursos ricos para traçar dados de séries temporais e inclui suporte para muitos recursos interativos, incluindo destaque / zoom de série / ponto, zoom e panning.

  * [Plotly](https://plot.ly/r/), que através da sua interface ggplotly permite que você possa facilmente traduzir seus gráficos ggplot2 para uma versão web interativa.

  * [rbokeh](http://hafen.github.io/rbokeh), uma interface para o Bokeh, uma poderosa estrutura declarativa do Bokeh para criar parcelas baseadas na web.

  * [Highcharter](http://jkunst.com/highcharter/), uma rica interface R para a popular biblioteca de gráficos JavaScript da Highcharts.

  * [visNetwork](http://dataknowledge.github.io/visNetwork), uma interface para os recursos de visualização de rede da biblioteca vis.js.

  * [DT](https://rstudio.github.io/DT/), os objetos de dados R (matrizes de dados ou dados) podem ser exibidos como tabelas em páginas HTML, e DataTables fornece filtragem, paginação, classificação e muitos outros recursos nas tabelas
  
Dentre muito outros htmlwidgets disponíveis no CRAN e no github dos desenvolvedores, o conteúdo desta sessão foi obtido [nesta sessão do manual do pacote flexdashboard](https://rmarkdown.rstudio.com/flexdashboard/using.html#html_widgets)

# Shiny

O projeto [Shiny](https://shiny.rstudio.com/) é um framework para aplicações web, criado pela equipe do [RStudio](https://www.rstudio.com/), e feito especificamente para a linguagem R. 

Ao incluir a opção `runtime: shiny ` no cabeçalho é possível [executar aplicativos shiny no dashboard](https://rmarkdown.rstudio.com/flexdashboard/shiny.html) de forma que se torne interativo.


<center>![](https://github.com/dtpapp/appwordcloud/blob/master/img/shiny2.png?raw=true)

Permite que o usuário do R crie apps web utilizando somente a própria linguagem R diminuindo a sobrecarga do usuário de forma que possa desenvolver e rodar suas aplicações localmente de formas muito simples como por exemplo os comandos`runAPP("myapp")`, `shinyApp(ui, server)` ou ainda `runApp(list(ui, server))`.

Para aplicativos definidos dessa maneira, o arquivo `server.R` deve retornar a função do servidor e o arquivo `ui.R` deve retornar o objeto UI. Em outras palavras, um é responsável por captar do usuário os parâmetros que serão aplicados no servidor e retornar para o usuário.


# Criando aplicativos a partir de exemplos

Na página oficial da [Shiny do Rstudio](https://shiny.rstudio.com/) é possível encontrar uma [galeria com muitos exemplos](https://shiny.rstudio.com/gallery/) de aplicativos e funcionalidades.

A implementação do aplicativo para wordcloud iniciou-se a partir do [exemplo básico de nuvem de palavras](https://shiny.rstudio.com/gallery/word-cloud.html) apresentado no site, veja:

![](https://github.com/dtpapp/appwordcloud/blob/master/img/shiny3.png?raw=true)

A partir deste exemplo em conjunto com outras funcionalidades disponíveis na [galeria](https://shiny.rstudio.com/gallery/) e na [Documentacao de funçoes em Shiny](https://shiny.rstudio.com/reference/shiny/1.0.5/) foram implementadas de forma que o usuário pudesse interagir e modificar a nuvem utilizando algumas funcionalidades de textmining.

O aplicativo é exibido da seguinte maneira:

![](https://github.com/dtpapp/appwordcloud/blob/master/img/shiny4.png?raw=true)

Para conferir o código do aplicativo comentado passo a passo, abrir o arquivo `appwordcloud.Rmd`.


# Fontes para aprender a criar apps Shiny

Além do [tutorial do Rstudio no Github oficial sobre shiny (em ingles)](http://rstudio.github.io/shiny/tutorial/), existem outras opções como referencias para se aprender a implementação de aplicativos html via Shiny, como o [curso na datacamp - Building Web Applications in R with Shiny - free course (em ingles)](https://www.datacamp.com/courses/building-web-applications-in-r-with-shiny) e a página no [curso-R](http://material.curso-r.com/shiny/) que explica seu framework.

Diversos links como o Shiny User Showcase, que contém um conjunto inspirador de aplicativos sofisticados desenvolvidos e contribuídos pelos usuários, podem ser encontrados na web para auxiliar no começo das aplicações, veja uma lista com alguns dos links encontrados:

  * [Galery Shiny User Showcase ](https://shiny.rstudio.com/gallery/)

  * [Adicionar widgets](http://shiny.rstudio.com/gallery/widget-gallery.html)

  * [Tutorial de shiny do Rstudio em vídeo](http://shiny.rstudio.com/tutorial/)

  * [Tutorial de shiny do Rstudio no github com exemplos](http://rstudio.github.io/shiny/tutorial/#hello-shiny)

  * [Curso-r Aula 10 Shiny](http://curso-r.github.io/posts/aula10.html)


# Disponibilizando o aplicativo

A página [https://www.shinyapps.io/](https://www.shinyapps.io/) possibilita implementar suas aplicações Shiny na Web em alguns minutos, não precisando de um servidor próprio ou saber como configurar um firewall para gerenciar seu aplicativo na nuvem

![[Imagem do site oficial](https://www.shinyapps.io/)](https://github.com/dtpapp/appwordcloud/blob/master/img/shiny5.png?raw=true)

Após criar uma conta, basta instalar o pacote, obter o seu token eno site e sincronizar o R com a [shinyapps.io](https://www.shinyapps.io/) com os seguintes comandos:

```{r}
# Instala o aplicativo
install.packages('rsconnect')
# Informa o nome do usuario da conta criada
rsconnect::setAccountInfo(name='Seu_Nome',
# O token obtido no site
                          token='CE597237EE030814F74C59DEF8F393E4',
# O segredo do aplicativo fornecido no site
                          secret='fisWfUngcgYWmwceMg3oeYtBFWbwbcvezrTYIHr6') 
```
Feito isto, podemos a aplicação pode ser implementada através dos comandos:

```{r}
library(rsconnect)
rsconnect::deployApp('local do aplicativo')
```

Por fim basta compartilhar o link que será criado em sua página e qualquer pessoa que tiver acesso a ele poderá utilizar o aplicativo.

A shinyapps é gratuito para a implementação de até 5 aplicativos, para mais recursos existem planos disponíveis que podem ser conferidos [no site oficial](https://www.shinyapps.io/)





