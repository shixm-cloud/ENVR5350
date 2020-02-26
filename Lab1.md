## 1. Carbon Cycle

In this assgnment, we will explore simulation data (distributed in class) from the C4MIP. You are requited to look at the following paper to understand the background of the data.

* **Reference**: Jones, C. D., Arora, V., Friedlingstein, P., Bopp, L., Brovkin, V., Dunne, J., Graven, H., Hoffman, F., Ilyina, T., John, J. G., Jung, M., Kawamiya, M., Koven, C., Pongratz, J., Raddatz, T., Randerson, J. T., and Zaehle, S.: C4MIP – The Coupled Climate–Carbon Cycle Model Intercomparison Project: experimental protocol for CMIP6, Geosci. Model Dev., 9, 2853-2880, doi:10.5194/gmd-9-2853-2016, 2016.


### Problem 1.  CO<sub>2</sub> Variability and Temperature 

First, let's explore the spatial and temporal variability of the data of CO<sub>2</sub> concentration. You need to answer the following questions after your analysis.
* _In each of four seasons (DJF, MAM, JJA, SON), what latitudes exhibit lowest concentration of CO<sub>2</sub>?_
* _The data are from one of the Tier 1 experiments mentioned in Jones et al. 2016, can tell which one we are analyzing, `1pctCO2-gbc` or `esm-ssp585`?_

```
%% Task 1: CO2 variability and temperature

% Import CO2 data
co2 = ncread('C4MIP/co2_Amon_GFDL-ESM4_r1i1p1f1_gr1_000101-010012.nc', 'co2');
% Import pressure level information; You'll find the 6th level is 500hPa
plev = ncread('C4MIP/co2_Amon_GFDL-ESM4_r1i1p1f1_gr1_000101-010012.nc', 'plev');
% Because 1000-hPa data is missing for some places, let's use 500hPa data
co2m = co2(:,:,6,:);
% In case the co2 file is too big for your computer, load the co2m directly
% by uncomment the one of the two options below.

% Option 1:
% load C4MIP/co2at500hPa.mat

% Option 2:
% co2m = ncread('C4MIP/co2m_Amon_GFDL-ESM4_r1i1p1f1_gr1_000101-010012.nc', 'co2m');
% co2m = reshape(co2m, 288, 180, 1, 1200);   % to make it compatible with code below

%% Q1 of Task 1
% Now let's work on the first question of Problem 1. We need to get four
% lines, each of which should show the distribution of CO2 concentration in 
% one season as a function of latitude.

% Average in the east-west direction first. The size of co2m can be checked
% by the command 'size(co2m)'. Its dimensions are (lon,lat,plev,time).
co2me = mean(co2m, 1);

% Winter (DJF) mean
co2winters = (co2me(1, :, 1, 12:12:end) + ...    % December
              co2me(1, :, 1,  1:12:end) + ...    % January
              co2me(1, :, 1,  2:12:end)) / 3.0;  % Feburary
co2win = squeeze(mean(co2winters, 4));   % average in time

% Spring (MAM) mean
[FILL YOUR OWN CODE HERE, SAVE AS 'co2spr']

% Summer (JJA) mean
[FILL YOUR OWN CODE HERE, SAVE AS 'co2sum']
         
% Fall (SON) mean
[FILL YOUR OWN CODE HERE, SAVE AS 'co2fal']

% Import latitude data
lat = ncread('C4MIP/co2_Amon_GFDL-ESM4_r1i1p1f1_gr1_000101-010012.nc', 'lat');

% Plot mean CO2 distribution for the four seasons
plot(lat, co2win*1.0e6, lat, co2spr*1.0e6, lat, co2sum*1.0e6, lat, co2fal*1.0e6)
legend('DJF', 'MAM', 'JJA', 'SON')
xlim([-90, 90])
xlabel('latitude')
ylabel('CO2 concentration (ppm)')
saveas(gcf, 'seasonalMeanCO2.pdf')
% save the figure for submission

%% Q2 of Task 1
% To answer this question, we need to plot global mean CO2 and temperature 
% as a function of time, so that we can verify which experiment this one
% is.

% Creat area weights first because the grid area decreases with latitude
wgt = cosd(lat);
wgt = reshape(wgt/sum(wgt), 1, 180, 1, 1);   
% normalize wgt so that its sum is 1 
% reshape wgt so that it can be multiplied to co2m directly

% Calculate the global areal mean
co2gAve = squeeze(sum(mean(wgt.*co2m, 1), 2));

% Plot the time series
time = (1:1200)/12.0;   % years
plot(time, co2gAve*1.0e6)
xlabel('time (year)')
ylabel('CO2 concentration (ppm)')
saveas(gcf, 'globalMeanCO2.pdf')
% save the figure for submission

% Import surface temperature data
tas = ncread('C4MIP/tas_Amon_GFDL-ESM4_r1i1p1f1_gr1_000101-010012.nc', 'tas');
% Compute its global area average
wgt = reshape(wgt, 1, 180, 1);   % wgt needs reshaping because tas is a 3D array 
% Calculate the global areal mean temperature
[FILL YOUR OWN CODE HERE, SAVE AS 'tasgAve'; PLOT THE TIME SERIES]

% This plot is kind of too noisy, let's smooth the time series by taking
% the one-year moving average of the data
tasgAveS = movmean(tasgAve, 12);
plot(time, tasgAveS)
xlabel('time (year)')
ylabel('near-surface air temperature (K)')
saveas(gcf, 'globalMeanTAS.pdf')
% save the figure for submission
```
Now with these plots, you should be able to answer the questions above.

### Problem 2.  Net Primary Productivity 

In this task, you will explore the relationship between CO<sub>2</sub> concentration and Net Primary Prouductivity (NPP). After the analysis, you should answer the question below:
* _Does NPP exhibit a linear inreasing trend as CO<sub>2</sub> concentration increases? Is its growth faster or slower than a linear function predicts?_

```
%% Task 2: Net Primary Productivity

% import NPP data
npp = ncread('C4MIP/npp_Lmon_GFDL-ESM4_r1i1p1f1_gr1_000101-010012.nc', 'npp');

% calculate the global-areal-mean time series of NPP and plot it
[FILL YOUR OWN CODE HERE, SAVE THE TIME SERIES AS 'nppgAve']

xlabel('time (year)')
ylabel('net primary productivity (kg m^{-2} s^{-1})')
% Again, you will find the data is very noisy, so let's smooth it by taking the
% one-year moving mean
nppgAveS = movmean(nppgAve, 12);
% Plot NPP and CO2 in the same figure
yyaxis left
plot(time, co2gAve*1.0e6);
ylabel('CO2 concentration (ppm)')
yyaxis right
plot(time, nppgAveS)
xlabel('time (year)')
ylabel('net primary productivity (kg m^{-2} s^{-1})')
saveas(gcf, 'NPP_n_CO2_time_series.pdf')

% A better way to investigate their relationship would be using a scatter
% plot
close   % close the previous figure
scatter(co2gAve*1.0e6, nppgAveS, '.')
grid on
% Since when CO2 concentration is 0, NPP should be 0, maybe we can fit the
% data with a simple equation NPP = b * CO2. Let's try.
b = co2gAve*1.0e6 \ nppgAveS;    % the MATLAB way to do linear regression
hold on    % so that the line can be overlaid on the scatter plot
plot(co2gAve*1.0e6, b*co2gAve*1.0e6)
xlabel('CO2 concentration (ppm)')
ylabel('net primary productivity (kg m^{-2} s^{-1})')
saveas(gcf, 'NPP_n_CO2_scatter.pdf')
```
Now with the plot, you should be able to answer this part.

### Problem 3.  Carbon Storage

In this final task, you are asked to analyze the storage of carbon in vegetation. Follow the instructions in the code below to finish the analysis, and answer the question:
* _How many times of vegetation mass do we need to plant (instead of relying on natural growth) by the end of this simulation in order to have the same fraction of carbon stored vegetation as at the begining?_

```
%% Task 3: Carbon Storage

% Let's calculate the mass of carbon in the atmosphere first.
% Assuming the global mean surface pressure is 1000 hPa, the number of
% moles of molecules in a column is
nAtmos = 100000 / 9.8   ...  % kg / m^2
         * 1000 / 29.0;      % 29 g/mol is the molar mass of air
% The unit of nAtmos is mol / m^2

% Hint: the molar mass of dry air is 29 g/mol. And
% the unit of nAtmos is mol / m^2

% So the number of moles of CO2 per m^2 is 
% [Hint: You should use 'co2gAve' here, which has the unit mol/mol]
[FILL YOUR CODE HERE, SAVE THE RESULT AS 'nco2']
% The unit of nco2 is mol / m^2 of CO2

% So the mass of carbon per m^2 in the atmosphere is 
% [Hint: molar mass of 1 carbon atom is 12 g/mol (0.012 kg/mol)]
[FILL YOUR CODE HERE, SAVE THE RESULT AS 'mC']
% The unit of mC is kg C / m^2

% Import the data of carbon mass in vegetation 
cVeg = ncread('C4MIP/cVeg_Lmon_GFDL-ESM4_r1i1p1f1_gr1_000101-010012.nc', 'cVeg');
% Calculate its global areal mean
cVgAve = squeeze(sum(mean(wgt.*cVeg, 1), 2));
   
% Plot
yyaxis left
plot(time, cVgAve);
ylim([0, 3.5]);
ylabel('carbon mass in vegetation (kg m^{-2})')
yyaxis right
plot(time, mC)
ylim([0, 3.5]);
xlabel('time (year)')
ylabel('carbon mass in atmosphere (kg m^{-2})')
saveas(gcf, 'cVeg_time_series.pdf')
```
