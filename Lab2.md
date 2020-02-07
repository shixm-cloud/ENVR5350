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

```
%% Task2: Examine stationary waves
% stationary wave
% [REPLACE THE QUESTION MARKS WITH YOUR CODE]
ustar19 = ?;
vstar19 = ?;
ustar21 = ?;
vstar21 = ?;

% plot the stationary waves
[X, Y] = meshgrid(lon(1:2:end), lat(1:2:end));
f3 = figure;
axesm ('eqdcylin','Grid', 'on', 'Origin', [0 180]);
geoshow('landareas.shp')
q1 = quiverm(Y, X, (vstar19(1:2:end,1:2:end,5))', (ustar19(1:2:end,1:2:end,5))');
set(q1, 'ColorMode', 'manual', 'Color', 'b');
title('Stationary Eddies')

f4 = figure;  % overlay two stationary eddy fields
axesm ('eqdcylin','Grid', 'on', 'Origin', [0 180], 'MapLatLimit', [0 70], 'MapLonLimit', [80 270]);
geoshow('landareas.shp')
q1 = quiverm(Y, X, (vstar19(1:2:end,1:2:end,5))', (ustar19(1:2:end,1:2:end,5))');
set(q1, 'ColorMode', 'manual', 'Color', 'b');
hold on 
q2 = quiverm(Y, X, (vstar21(1:2:end,1:2:end,5))', (ustar21(1:2:end,1:2:end,5))');
set(q2, 'ColorMode', 'manual', 'Color', 'r');
title('Stationary Eddies for 1900 (blue) and 2100 (red)')
hold off
```

### Problem 3. Kinetic Energy

In this part, you need to complete the code below to compute the kinetic energy contained in the (zonal and time) mean flow, stationary waves, and transient eddies. Plot the latitudinal distribution of kinetic energy for 250 hPa and 500 hPa. Answer these question:
* What component of the circulation (mean flow, stationary waves, or transient eddies) exhibit largest change? At what latitudes? 
* Based on the eddy kinetic energy at 500 hPa, do you think the future weather in mid- and high-latitudes will be more stormy or less?

```
%% Task 3: Compute kinetic energy of mean flow, stationary waves, and transient eddies
% compute transient eddy component
% [REPLACE THE QUESTION MARKS WITH YOUR CODE]
uprm19 = ?;
vprm19 = ?;
uprm21 = ?;
vprm21 = ?;

% compute kinetic energy
MKE19 = 0.5 * (uavg19.^2 + vavg19.^2);   % mean flow kinetic energy
MKE21 = 0.5 * (uavg21.^2 + vavg21.^2);

SKE19 = nanmean(0.5 * (ustar19.^2 + vstar19.^2), 1);  % stationary wave energy
SKE21 = nanmean(0.5 * (ustar21.^2 + vstar21.^2), 1);

EKE19 = nanmean(0.5 * (uprm19.^2 + uprm19.^2), [1, 4]);  % eddy kinetic energy
EKE21 = nanmean(0.5 * (uprm21.^2 + uprm21.^2), [1, 4]);  % 

% let's plot the 250hPa level energy distribution
f5 = figure;
plot(lat, MKE19(1,:,5), 'k-', lat, SKE19(1,:,5), 'b-', lat, EKE19(1,:,5), 'r-', ...
     lat, MKE21(1,:,5), 'k--', lat, SKE21(1,:,5), 'b--', lat, EKE21(1,:,5), 'r--')
xlim([-90 90])
title('Kinetic Energy at 250hPa')
legend('mean flow (1900)', 'stationary wave (1900)', 'transient eddy (1900)', ...
       'mean flow (2100)', 'stationary wave (2100)', 'transient eddy (2100)')
xlabel('latitude')
ylabel('kinetic energy (m^2s^{-2})')

% 500hPa level
f6 = figure;
plot(lat, MKE19(1,:,4), 'k-', lat, SKE19(1,:,4), 'b-', lat, EKE19(1,:,4), 'r-', ...
     lat, MKE21(1,:,4), 'k--', lat, SKE21(1,:,4), 'b--', lat, EKE21(1,:,4), 'r--')
xlim([-90 90])
title('Kinetic Energy at 500hPa')
legend('mean flow (1900)', 'stationary wave (1900)', 'transient eddy (1900)', ...
       'mean flow (2100)', 'stationary wave (2100)', 'transient eddy (2100)')
ylabel('kinetic energy (m^2s^{-2})')
```


