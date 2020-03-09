    library(shiny)
    library(maps)
    library(mapproj)
    library(ggplot2)
    library(ggmap)

    ## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.

    ## Please cite ggmap if you use it! See citation("ggmap") for details.

    source("helpers.R")

    counties <- readRDS("data/counties.rds")
    head(counties)

    ##              name total.pop white black hispanic asian
    ## 1 alabama,autauga     54571  77.2  19.3      2.4   0.9
    ## 2 alabama,baldwin    182265  83.5  10.9      4.4   0.7
    ## 3 alabama,barbour     27457  46.8  47.8      5.1   0.4
    ## 4    alabama,bibb     22915  75.0  22.9      1.8   0.1
    ## 5  alabama,blount     57322  88.9   2.5      8.1   0.2
    ## 6 alabama,bullock     10914  21.9  71.0      7.1   0.2

    ui <- fluidPage(
      titlePanel("censusVis"),
      
      sidebarLayout(
        sidebarPanel(
          helpText("Create demographic maps with 
            information from the 2010 US Census."),
          
          selectInput("var", 
                      label = "Choose a variable to display",
                      choices = c("Percent White", "Percent Black",
                                  "Percent Hispanic", "Percent Asian"),
                      selected = "Percent White"),
          
          sliderInput("range", 
                      label = "Range of interest:",
                      min = 0, max = 100, value = c(0, 100))
        ),
        
        mainPanel(plotOutput("map"))
      )
    )

    # Server logic ----
    server <- function(input, output) {
      output$map <- renderPlot({
        data <- switch(input$var, 
                       "Percent White" = counties$white,
                       "Percent Black" = counties$black,
                       "Percent Hispanic" = counties$hispanic,
                       "Percent Asian" = counties$asian)
        percent_map(counties$white, "darkgreen", "% White")
      })
    }

    # Run app ----
    shinyApp(ui, server)

    ## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.

<!--html_preserve-->
Shiny applications not supported in static R Markdown documents

<!--/html_preserve-->
