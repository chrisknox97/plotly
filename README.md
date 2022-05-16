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
      
This data is once again plotted and displayed using Plotly functionality. 

       Plotly.newPlot("bubble", bubbleData, bubbleLayout);  

### Deliverable 3: Gauge Counter

### Deluvarble 4: Customization


## Analysis
