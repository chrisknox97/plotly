# Plotly & Belly Button Biodiversity

## Overview

### To create a filterable HTML-based database of belly button bacteria, by which users can filter by volunteer number and be presented with a bar chart showcasing the top 10 bacterial cultures found in said volunteer's navel, a gauage counter of the approximate times the volunteer washed their belly button each week, and a bubble chart that displays the porptional rate of each bacterial culture present in the volunteer's navel. 


## Creating Plotly Charts

### Deliverable 1: Bar Chart

The first chart we created for this website was a Bar Chart that showed the visitor the Top 10 Bacterial Cultures found in any specified volunteer. To do this we first created a ``buildCharts`` function and used ``d3.json`` to load and retrieve the ``samples.json`` file where our data is stored. 

    function buildCharts(sample) {
    
      d3.json("samples.json").then((data) => {

From here we created variables to hold our sample array, to filter based on specified sample number, to hold the first sample in our array, and variables to hold the ``otu_ids``, ``otu_labels``, and ``sample_values`` (which also had to specify to display only the top 10 bacterial cultures found).

      var samples = data.samples;
      
      var resultArray = samples.filter(sampleObj => sampleObj.id == sample);
      
      var result = resultArray[0];
      
      var barIds = result.otu_ids.slice(0,10).reverse()
      var barLabels = result.otu_labels.slice(0,10).reverse()
      var barValues = result.sample_values.slice(0,10).reverse()
      
After establishing our variables, we must then establish our ``yticks`` for our Bar Chart. 

      var yticks = barIds.map(sampleObj => "OTU " + sampleObj).slice(0.10).reverse()
      console.log(yticks)

We then create the foundations of our Bar Chart by dictating its data values, orientation, and type in its ``trace``; as well as displaying its titles in its ``layout``. 

      var barData = [
      {
        x: barValues,
        y: yticks,
        type: "bar",
        orientation: "h",
        text: barLabels 
        }
      ];
      
      var barLayout = {
        title: "Top 10 Bacteria Cultures Found",
        xaxis: { title: "Sample Values" },
        yaxis: { title: "OTU IDs" }
    }; 

With everything else in place, all that's left is to use ``Plotly`` to plot the data. 

    Plotly.newPlot("bar", barData, barLayout);
    
![Deliverable1](https://github.com/chrisknox97/plotly/blob/main/PNG/barchart.png)

### Deliverable 2: Bubble Chart

Our second task was to create a Bubble Chart displaying the proportional represenation of bacterial cultures in any given volunteer. To do this we first had to reastablish our ``otu_ids``, ``otu_labels``, and ``sample_values`` variables for all relevant data rather than just the top 10 most prominent. 

    var ids = result.otu_ids
    var labels = result.otu_labels
    var values = result.sample_values
    
Then, with the groundwork already laid with Deliverable 1, all we needed to do was create another trace specifying x and y values, mode and marker as well as a layout with appropriate titles and margins. 

    var bubbleData = [
    {
          x: ids,
          y: values,
          text: labels,
          mode: 'markers',
          marker: {
            size: values,
            color: ids,
            colorscale: 'ice'
          }
        }
      ];
      
      var bubbleLayout = {
  
        title: 'Bacteria Cultures Per Sample',
        xaxis: { title: "OTU ID "},  
        yaxis: { title: "Sample Values"},  
        automargin: true,
        hovermode: 'closest',   
        showlegend: false
      };  
      
This data is once again plotted and displayed using ``Plotly`` functionality. 

       Plotly.newPlot("bubble", bubbleData, bubbleLayout);  
       
![Deliverable2](https://github.com/chrisknox97/plotly/blob/main/PNG/bubblechart.png)  

### Deliverable 3: Gauge Counter

The third deliverable required the creation of a gauge counter that would display the approximate times a volunteer washed their belly button each week. To do this we first had to create variables that would filter based on specified sample data numbers within the metadata array, one that would hold the first sample within that array, and a variable for washing frequency. 

        var metadata = data.metadata;
        var gaugeArray = metadata.filter(data => data.id == sample);
        
        var firstGauge = gaugeArray[0];
        
        var washfreq = firstGauge.wfreq
        console.log(washfreq)
        
Once again, after establishing these variables, we need only use our trace and lyout to specify our data and titles, and Plotly to plot and display our new chart. 

        var gaugeData = [
        {
            value: washfreq,
            title: { text: '<b> Bully Button Washing Frequency <b> <br><br> Scrubs Per Week'},
            type: 'indicator',
            mode: 'gauge+number',
            gauge: {
            axis: { range: [null, 10], tickwidth: 1, tickcolor: "black" },
            bar: {color: 'black'},
            steps: [
                {range: [0, 2], color: 'red'},
                {range: [2, 4], color: 'orange'},
                {range: [4, 6], color: 'yellow'},
                {range: [6, 8], color: 'lightgreen'},
                {range: [8, 10], color: 'green'},
            ]}}
        ];
        
            var gaugeLayout = { 
                automargin: true,  
            };

        Plotly.newPlot("gauge", gaugeData, gaugeLayout);
        });
    }
    
![Deliverable3](https://github.com/chrisknox97/plotly/blob/main/PNG/gauge.png)  

### Deluvarble 4: Customization

While completing these three charts presents potential visitors with a user-friendly interface; the interface itself remained plain and ordinary. As a result, using our knowledge of HTML and CSS, we endeavored to customize the website in the following ways:

* Bacterial Image Added to Jumbotron & Change in Font Color

      <div class="col-xs-12 col-sm-12 col-md-12 jumbotron text-center" style="background-image: url('https://jooinn.com/images/bacteria-15.jpg');
      background-size: cover; background-position: center; color: rgb(187, 180, 227)">
      
* Project Overview Added

        <div class="container-fluid">
            <div class="row">
                <div class="col-md-4">
                    <h3>Cultivating Cultures</h3>
                  </div>
                  <div class="col-md-8">
                    <p>
                    Using Scientific Ingenuity, Improbable Beef has undertaken the task of cultivating proteins for synthetic beef from volunteers' navels. 
                    If Improbable Beef succeeds in this endeavor, this website will allow the company to locate those volunteers whose navels  
                    featured the proteins that will need to be synthesized and manufactured. 
                    <p>
             </div>
          </div>
   
  * Overviews of Each Graph Added

        <div class="col-md-5 jumbotron text-center" style="background-color: rgb(95, 102, 232);">
            <div id="bar"></div>
                <p>
                This Bar Chart illustrates the top 10 bacterial Cultures featured in the specified volunteer. 
                <p>
            </div>
        <div class="col-md-5 jumbotron text-center" style="background-color: rgb(95, 102, 232);">
            <div id="gauge"></div>
                <p>
                This Gauge displays the approximate weekly belly button washing frequency of a specified volunteer.
                <p>
            </div>
        </div>
        <div class="row">
            <div class="col-md-12 jumbotron text-center" style="background-color: rgb(95, 102, 232);"></div>
                <div id="bubble"></div>
                <p>
                This Bubble Chart proportionally depicts the bacteria count of each culture based on a specified volunteer.
                <p>
            </div>
        </div>

* Additional Colors Added Via CSS

        body {
            color: #000000;
            background-color: #578ee1;
        }

        p {
            color: rgb(209, 220, 236);
        }


## Analysis

Ultimately ``Plotly`` presents a rather user-intuitive method to create Bar Charts, Bubble Charts, and/or Gauge counters for a HTML-based website; representing a viable alternative to ``Matplotlib``. 

