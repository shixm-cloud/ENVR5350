## 2. Kinetic Energy

In this exercise, you will compute and compare the kinetic energy in the mean flow, stationary waves, and transient eddies. You will also look at the impact of global warming on the circulation.

The dataset provided are daily data of zonal wind (`ua`) and meridional wind (`va`) in the simulation of 1900 and 2100 (under SSP5) climate by the GFDL climate model. They are downloaded from CMIP6 website. For your reference, the change in global mean surface temperature is **3.8** K from 1900 to 2100.

### Problem 1. Zonal Mean, Time Mean Zonal Wind

Finish the code below and compare the plots of zonally averaged, time mean zonal wind field for 1900 and 2100. You should plot the mean zonal wind in a latitude-height(pressure) coordinate. Answer the following questions:
* At what pressure level do you see the _tropospheric_ jet streams?
* Compare the figure for 1900 and that for 2100, do you see any change in the tropospheric jet streams?

```
%% Compute the Kinetic Energy of the Atmosphere 
% The GFDL climate simulations of 1900 and 2100 (under SSP5) are compared.

%% Load data
ua19 = ncread('SSP585/ua_day_GFDL-CM4_historical_r1i1p1f1_gr2_19000101-19001231.nc', 'ua');
va19 = ncread('SSP585/va_day_GFDL-CM4_historical_r1i1p1f1_gr2_19000101-19001231.nc', 'va');
lat = ncread('SSP585/ua_day_GFDL-CM4_historical_r1i1p1f1_gr2_19000101-19001231.nc', 'lat');
lon = ncread('SSP585/ua_day_GFDL-CM4_historical_r1i1p1f1_gr2_19000101-19001231.nc', 'lon');
plev = ncread('SSP585/ua_day_GFDL-CM4_historical_r1i1p1f1_gr2_19000101-19001231.nc', 'plev');
% plev' = [100000 85000 70000 50000 25000 10000 5000 1000]
% we will look at 250hPa level later
ua21 = ncread('SSP585/ua_day_GFDL-CM4_ssp585_r1i1p1f1_gr2_21000101-21001231.nc', 'ua');
va21 = ncread('SSP585/va_day_GFDL-CM4_ssp585_r1i1p1f1_gr2_21000101-21001231.nc', 'va');

%% Task 1: Examine the zonal mean, time mean zonal wind (u) field.
% [REPLACE THE QUESTION MARKS WITH YOUR CODE; 
%  HINT: USE the function 'nanmean' instead of 'mean']
ubar19 = ?;
vbar19 = ?;   % time mean 
uavg19 = ?;
vavg19 = ?;   % zonal mean

ubar21 = ?;
vbar21 = ?;   % time mean 
uavg21 = ?;
vavg21 = ?;   % zonal mean

% plot uavg19 and uavg21
f1 = figure;
[Y, P] = meshgrid(lat, plev/100.0);
levels = -12:3:36;
contourf(Y, P, squeeze(uavg19)', levels, 'ShowText', 'on')
caxis([-12 36]);
set(gca, 'YScale', 'log')
set(gca,'YDir','reverse')
yticks([10 50 100 250 500 700 1000]);
yticklabels({'10', '50','100','250','500','700','1000'})
xlabel('latitude')
ylabel('pressure (hPa)')
title('Year 1900')
f2 = figure;
[Y, P] = meshgrid(lat, plev/100.0);
levels = -12:3:36;
contourf(Y, P, squeeze(uavg21)', levels, 'ShowText', 'on')
caxis([-12 36]);
set(gca, 'YScale', 'log')
set(gca,'YDir','reverse')
yticks([10 50 100 250 500 700 1000]);
yticklabels({'10', '50','100','250','500','700','1000'})
xlabel('latitude')
ylabel('pressure (hPa)')
title('Year 2100')
```

### Problem 2. Stationary Waves
{% include mathjax.html %}

Complet the code below so that you can obtain maps of stationary waves. You are supposed to plot the 250hPa stationary wave wind vectors (\\(\overline{u}^* \\)and \\(\overline{v}^* \\)) on a longitude-latitude map. The second map zooms into the North Pacific sector and compare the patterns of 1900 and 2100. You can find over East Asia high laitudes northerly wind dominates. This is actually responsible for the harsh winter over East Asian. Answer the question below:
* Based on the second map, do you think the winter in East Aisa will be harsher or milder (compared to other regions in the same latitudes) in the a warmed climate? 

### Problem 3. Kinetic Energy

In this part, you need to complete the code below to compute the kinetic energy contained in the (zonal and time) mean flow, stationary waves, and transient eddies. Plot the latitudinal distribution of kinetic energy for 250 hPa and 500 hPa. Answer these question:
* What component of the circulation (mean flow, stationary waves, or transient eddies) exhibit largest change? At what latitudes? 
* Based on the eddy kinetic energy at 500 hPa, do you think the future weather in mid- and high-latitudes will be more stormy or less?





