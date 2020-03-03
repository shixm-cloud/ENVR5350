## 4. Available Potential Energy

In this exercise, we will look at how avaiable potential energy (APE) resonds to global warming by examining simulations using the Commumunity Earth System Model (CESM). 

### Problem 1. Mean Lapse Rate

Two Southern Hemisphere temperature datasets are provided. One is `historical` simulation for 1897 to 1899, and the other is the simulation of 2097-2099 under the SSP5-8.5 scenario. Please complete the code below and answer the following question,
* _Do you think the mean lapse rate for the warmed climate and the current climate are significantly different? How large error (in terms of percentage) may be introduced by using the mean lapse rate profile for 1897-1899 in estimating the APE of 2097-2099?_
\[_Hint: you can certainly do detailed calculation to find out the error. However, here I wish you to estimate the relative error by comparing the two lapse rate profile and using the definition of APE (Equation 18 on class slides)_\]
```
%% Southern Hemisphere Available Potential Energy
%% Task 1: Compute mean lapse rate in historical and future climate
% Read data
ta18 = ncread('ta_day_CESM2_historical_r11i1p1f1_gn_18970101-18991231_SH.nc', 'ta');
ta20 = ncread('ta_day_CESM2_ssp585_r1i1p1f1_gn_20970101-209912301_SH.nc', 'ta');

% Calculate mean lapse rate. Lapse rate in fact is not a function of pressure only, but here let's assume it is.
lat = ncread('ta_day_CESM2_historical_r11i1p1f1_gn_18970101-18991231_SH.nc', 'lat');
clat = reshape(cosd(lat), 1, 96);  % weight data by cos(lat)
clat = repmat(clat, 1, 1, 8);  % make clat the same shape as ta18m and ta20m below

% Average over longitude and time
[FILL YOUR CODE HERE, SAVE TO 'ta18m' AND 'ta20m']

% Average over latitude
clat(isnan(ta18m)) = NaN;   % set weight for points underground as NaN
[FILL YOUR CODE HERE, SAVE TO 'ta18m' AND 'ta20m']

% Find out height of each layer 
plev = ncread('ta_day_CESM2_historical_r11i1p1f1_gn_18970101-18991231_SH.nc', 'plev');
% assuming the scale height is 7.6km, find the geopotential height of each
% level
zlev = 7600.0 * log(100000.0 ./ plev);
zlev = reshape(zlev, size(ta18m));  % reshape it for later convenience in broadcasting

% Calculate mean lapse rate now, using the difference between adjacent levels to approximate
[FILL YOUR CODE HERE]

zhalf = 0.5 * (zlev(1:7) + zlev(2:8));  % height at half levels

% Plot the lapse rate
f1 = figure;
plot(squeeze(Gamma18(:))*1000.0, squeeze(zhalf)/1000.0, 'b-o', ...
     squeeze(Gamma20(:))*1000.0, squeeze(zhalf)/1000.0, 'r-o')
xlabel('lapse rate (K/km)')
ylabel('height (km)')
legend('1897-1899', '2097-2099')
% We can use the lowest 4 (half) levels only, because the 5th is already
% above troupopause.

% Estimate overestimation/underestimation that may be introduce by using the mean lapse
% rate of one climate for both.
[FILL YOUR CODE HERE, SAVE RELATIVE ERROR ESTIMATION TO 'err']

fprintf('If we use the historical climate profile, error in APE is: %5.1f %%\n', err)
```
