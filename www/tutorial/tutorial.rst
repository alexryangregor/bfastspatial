Time-series analysis
====================

Overview
--------

The Breaks for Additive Seasonal and Trend (BFAST) method enables you to analyze the dynamics of dense time-series satellite imagery and overcome the major challenge of distinguishing land-cover change from seasonal phenological variations.

`Verbesselt et al. (2010) <https://doi.org/10.1016/j.rse.2010.08.003>`__, `Dutrieux et al. (2015) <https://doi.org/10.1016/j.isprsjprs.2015.03.015>`__ and `DeVries et al. (2015) <https://doi.org/10.1016/j.rse.2015.08.020>`__ used this approach to not only demonstrate that time series can be divided into trend, seasonal, and remainder components, but also that the time and number of changes can be detected at high temporal resolution (e.g. 16 days), enabling detection of tree cover change and separation from the phenology signal.

The same authors developed the `bfastSpatial package <https://www.rdocumentation.org/packages/bfastSpatial/versions/0.6.2>`__, which provides utilities to perform change detection analysis on time series of spatial gridded data, such as the Landsat satellite imagery that covers our period of interest.

The package has been tested in the early versions of the cloud-based platform developed by the Food and Agriculture Organization of the United Nations (FAO) for parallel processing of remote sensing data (https://sepal.io). It has been recently adapted into a functional processing chain (https://github.com/yfinegold/runBFAST), which is wrapped in the current Shiny application.

.. note:: 
    Since the processing of time-series using BFAST is computed in SEPAL, selecting a powerful instance is recommended. The computation is generated using several Central Processing Units (CPUs), so an instance with a large number of CPUs is recommended as well. Select the terminal button and choose a new instance. Instance ”m16” with 16 CPUs, for example, may be sufficient for your processing needs.

Introduction
------------

In the introduction section, the welcome, intro to BFAST and parameterization tabs provide additional information about the mechanics of the algorithm. It is highly recommended to read the text in these tabs.

.. note:: 
    
    The application is only available in English at the moment. French and Spanish translations will be updated shortly. If you are interested in translating the application into your own language, contact remi.dannunzio@fao.org or yelena.finegold@fao.org. 

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/introduction.png
    :group: bfastspatial
 
Download the test dataset
-------------------------

If this is your first time using the Time-series analysis tool, the SEPAL team recommends trying first with the example data set. This will help you to understand the logic and parameters of the analysis with previously-prepared, sound data; afterwards, you can apply it to your own data. The example data set can be downloaded in the ‘Download test data’ tab.

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/test_download.png
    :group: bfastspatial
 
Select :guilabel:`Download test dataset`. The data is downloaded into the :code:`bfast_data_test` folder in your root directory. The file location and information about the download will appear in the lower-right corner. 

Run time-series analysis
------------------------

Select the Process tab in the left column.

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/process_tab.png
    :group: bfastspatial
 
Select the Time series folder button to view your downloaded data (either downloaded from the SEPAL search option or the test data set).

Select the Time series folder in your Download folder. If your Area of Interest (AOI) has different features, the application will ask you to select which one you want to process (you can select one, some, or all of them). In the example below, you can see how it appears when you have 140 distinct features.

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/select_ts.png
    :group: bfastspatial

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/select_ts_tile.png
    :group: bfastspatial
 
There is an option to apply a mask and run BFAST only on areas inside the mask. You can select a file with 0 and 1 values (0 values will be excluded and 1 included in the computation).

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/select_mask.png
    :group: bfastspatial
 
If you would like to use a mask, select **FNF mask** and then choose the raster file by selecting the **forest/non-forest** mask button and choosing the mask file. 

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/fnf_mask.png
    :group: bfastspatial

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/browse_mask.png
    :group: bfastspatial
 
 
Next, change the parameters for your study area. At this stage, the BFAST explorer described in section 2 can be very useful. You can use it to understand the seasonal and interannual patterns of the land cover that you are analyzing in your study area. You can do this over several pixels to have a better idea. 

.. note:: 

    Remember that this module will define a historical period and a monitoring period,  corresponding to the option, “bfastmonitor” in the BFAST explorer module.

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/parameters.png
    :group: bfastspatial
 
The parameters include:

-   **History beginning year** – The year that marks the start of the historical period. The actual start date will depend on the historical parameter chosen.
-   **Monitoring start and end years** – The monitoring start year is the year that marks the end of the historical period and the start of the monitoring period. The monitoring end year marks the end of the monitoring period.
-   **History parameter** – Specifies the start of a stable historical period. The options are:
    
    -   Reverse ordered CUSUM (ROC), which navigates backwards in time, using a stepwise approach, to identify a stable historical period;
    -   Bai and Perron breakpoint estimation (BP), which identifies a stable historical period and can be used to identify disturbances in the historical period;
    -   All, which uses all available observations; and
    -   Numeric, which allows the start date to be specified, using the year (e.g. 2011).

-   **Elements of the formula** – The formula describes the type of regression model applied. The options are: 

    -   Trend + harmon, a linear trend and a harmonic season component; 
    -   Harmon, a harmonic season component; and
    -   Trend, a linear trend.

-   **Order parameter** – Specifies the order of the harmonic term, defaulting to 3.
-   **Type parameter** – Specifies the type of monitoring process. To learn more, go to the `strucchange package documentation <https://cran.r-project.org/web/packages/strucchange/index.html>`__. The options are:

    -   Moving sums of residuals (MOSUM), where residuals are calculated as the difference between expected values and actual observations in a monitoring period based on OLS residuals;
    -   Cumulative sum (CUSUM), cumulative sums of standardized residuals (MOSUM uses a moving sum, while CUSUM uses a cumulative sum of the same residuals);
    -   Moving estimates (ME), where the moving estimates process is returned; and
    -   Fluctuation, which returns the recursive estimates process.

-   **Raster band outputs** – Result layers to be returned. Can be any combination of :code:`breakpoint`, :code:`magnitude`, :code:`error`, :code:`history`, :code:`r.squared`, :code:`adj.r.squared`, :code:`coefficients`. The default, :code:`breakpoint`, :code:`magnitude` and :code:`error` are returned by the function. It is important to know which layers have been requested and in which order they will be exported because the layer names are not specified. Note that if "coefficients" is included, the output will include the following: "(Intercept)" and any trend and/or harmonic coefficients depending on the values of formula and order.
-   **Computation mode** – Choose between running the calculation for the entire monitoring period (Overall) or each year of the monitoring period (Sequential):

    -   Overall – Runs BFAST one time for the monitoring period and provides a maximum of one breakpoint for the entire monitoring period.
    -   Sequential – Runs BFAST for each year of the monitoring period. The output will be per year of the monitoring period and will provide a maximum of one breakpoint per year in the monitoring period. This option does not create the thresholded output and will not display the output within the application. To view the results, use the visualizer in SEPAL or download the results to your local computer. 

Once you have decided on your parameters, run BFAST by selecting the Launch BFAST calculation button in the Results box. 

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/launch.png
    :group: bfastspatial

Depending on the size of your area and the size of your instance, BFAST can take a long time to run. It is not necessary to keep this application open for the results to be created; it is only necessary to make sure that the instance is running. 

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/log.png
    :group: bfastspatial
 
If your AOI has multiple polygons and contains many numeric folders (e.g. 1, 2, 3, etc.), it will run the BFAST calculation for each of the folders recursively. 

If you are running a large area or have a weak internet connection, causing the application to disconnect, you can go to User resources in SEPAL and set the amount of time your session should stay open (see image below). This way you can shut down SEPAL and the calculation will continue.

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/instance.png
    :group: bfastspatial

.. tip:: 

    If the interface turns gray and you see the message, Disconnected from the server, rest assured that the process is still running. You can follow the previous step to make sure your session remains active.

If you are feeling patient or have a small study area, you can wait for the algorithm to finish running and view one of the outputs: the thresholded magnitude. 

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/finished.png
    :group: bfastspatial

When the calculation finishes running, you will see the text :code:`Done processing!!! Click on DISPLAY THE RESULTS`. You can now select the :guilabel:`Display BFAST results from this session` button to display the thresholded magnitude.
 
The output from BFAST by default includes 3 bands, the breakpoint, the magnitude and error. An additional output is calculated in this application, which is the thresholded magnitude. The thresholded magnitude is calculated using the magnitude output, calculating the mean magnitude value over the AOI and applying thresholds up to +/- 4 standard deviations from the mean. This layer indicates the positive or negative intensity of change for each pixel. For anything above 2 standard deviations, you can interpret that a change has certainly occurred compared to the historical period modeled.  

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/preview.png
    :group: bfastspatial

.. note:: 
    
    If you are not using the instance anymore to process more time series, please shutdown the instance by selecting the Trash bin button.

You can also download your results to your hard drive (e.g. with FileZilla using ArcGIS). Here you can see an example of how the layers can be displayed: 

BFAST was computed over the following area in Indonesia during the years 2013-2019. The years 2013-2016 were used for the historical period; 2016-2019 were used as the monitoring period.

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/result_rgb.png
    :group: bfastspatial

**Band_1** shows the date when the breakpoint was detected. The output is stored as a decimal date. 

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/result_band_1.png
    :group: bfastspatial
  
**Band_2** shows the BFAST magnitude of change. In this case, if the mean of the cumulative increase or decrease of the Normalized Difference Moisture Index (NDMI) since the monitoring period started, it would indicate pixels where vegetation has become wetter or drier. The values can be interpreted as relative changes, where the units are related to the average deviation from the trend of the NDMI. 

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/result_band_2.png
    :group: bfastspatial
  
**Band_3** shows the errors. These are pixels where the algorithm did not find enough data to compute the trends.

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/result_band_3.png
    :group: bfastspatial
 
Finally, you will find an additional layer called “threshold”. The thresholded magnitude is calculated using the magnitude output, calculating the mean magnitude value over the AOI and applying thresholds up to +/- 4 standard deviations from the mean. The layer is a thematic classification map that has values ranging from 0-10, which correspond to the legend, as seen below. You can also see how to name them in the figure.

.. thumbnail:: https://raw.githubusercontent.com/12rambau/bfastspatial/master/www/tutorial/img/result_sigma.png
    :group: bfastspatial
