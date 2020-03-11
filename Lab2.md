## 2. Surface Fluxes

In this exercise we will examine the sensible and latent heat flux in a simulation using the Commumunity Earth System Model (CESM)

### Problem 1. Geographic Variation of Fluxes

Please complete the code below and answer the questions below.
* _Over Australia, is the mean sensible flux stronger or the mean latent heat flux? On an annual mean and spatial basis, do you think the Bowen ratio over Australia is greater or smaller than 1?_
* _In northern midlatitude oceans, where do you see strongest sensible and latent heat flux? Do you think the fluxes in those regions are stronger in winter or summar?_

```
%% Surface Fluxes
%% Load data
LE = ncread('CESM/hfls_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'hfls');  % latent heat flux
SH = ncread('CESM/hfss_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'hfss');  % sensible heat flux
Ts = ncread('CESM/ts_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'ts');  % surface temperature
lat = ncread('CESM/ts_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'lat');
lon = ncread('CESM/ts_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'lon');

%% Task 1: Global Distribution of the latent and sensible heat fluxes
%  We have 86 year data in total, but let's exmine the global distribution 
%  between 2015/01 to 2019/12 first.

% Compute the time mean LE and SH, save to LE15 and SH15
LE15 = [FILL YOUR CODE HERE];
SH15 = [FILL YOUR CODE HERE];

%% Show the sensible heat on a map
[LON, LAT] = meshgrid(lon, lat);
f1 = figure;
worldmap([-90, 90], [0, 360]);
setm(gca,'mapprojection','eqdcylin', 'Grid', 'on','MlabelParallel', 'south')
levels = linspace(-40, 120, 17);
contourfm(LAT, LON, SH15', levels, 'LineStyle', 'none');
colormap jet
caxis([-40 120]);
c = colorbar;
c.Position = [0.9 0.325 0.025 0.4];
title('Sensible Heat Flux (W m^{-2})')
hold on
load coastlines
plotm(coastlat,coastlon)
hold off

%% Show the latent heat on a map
f2 = figure;
worldmap([-90, 90], [0, 360]);
setm(gca,'mapprojection','eqdcylin', 'Grid', 'on','MlabelParallel', 'south')
levels = linspace(0, 250, 26);
contourfm(LAT, LON, LE15', levels, 'LineStyle', 'none');
colormap jet
caxis([0 250]);
c = colorbar;
c.Position = [0.9 0.325 0.025 0.4];
title('Latent Heat Flux (W m^{-2})')
hold on
load coastlines
plotm(coastlat,coastlon)
```

### Problem 2. Trend in Fluxes

In this part, please complete the code to calculate the global mean, monthly mean surface sensible and latent heat fluxes, and plot the time series to what they are predited to change under SSP5-8.5 (shared socio-economic pathway). Answer this question
* _According to this CESM simulation, do you think the globally averaged sensible heat flux and latent heat flux will increase or decrease due to global warming?_

```
%% Task 2: changes under global warming
%  Now let's plot the time series of globa mean monthly sensible and latent
%  heat fluxes.

% Compute time series, save to LEs and SHs
% remember to weight data with cos latitude
[FILL YOUR CODE HERE]
LEs = [FILL YOUR CODE HERE];
SHs = [FILL YOUR CODE HERE];
time = linspace(2015, 2100+11/12, numel(LEs));

% Plot the the two time series
f3 = figure;
plot(time, SHs, 'r-', time, LEs, 'b-')
ylim([0 105]);
xlim([2015, 2100])
xlabel('year')
ylabel('flux (W m^{-2})')
legend('sensible', 'latent', 'Location', 'east')
```

### Problem 3. Understanding the Change
{% include mathjax.html %}

In our class, we showed that over water surfaces, we may calculate latent heat flux with the following expression,

\\[
    LE \approx \rho L C_{\text{DE}} U_r\left(q^*(T_s)(1-\text{RH}) + \text{RH} B_e^{-1}\frac{c_p}{L}(T_s - T_a)  \right)
\\]

* _Please show that by assuming the exponential dependence on temperature of\\( q^* \\) dominate in this formula, it is approximately correct to state that_
\\[
LE \propto q^*
\\]
_So one can expect latent heat flux will increase by 7%/K under global warming, just like moisture will do._


Now complete the code below to make a scatter plot of the annual mean, global mean latent heat flux and temperature. 

* _Based on this figure, do you think the dependence of latent heat on temperature is linear or exponential?_

```
%% Task 3: understanding the change in latent heat flux
% compute annual mean values of globally averaged LE and temperature, save to LEa and Tsa
[FILL YOUR CODE HERE]

f4 = figure;
scatter(Tsa, LEa, 25, Tsa, 'filled');
xlabel('surface temperature ({\circ}C)')
ylabel('latent heat flux (W m^{-2})')
```

Lastly, based on your guess, perform a simple regression for `LEa` and `Tsa`. If you think they have exponential relationship, the regression equation should be
\\[
\log LE = \beta_0 + \beta_1 T_s
\\]
If you think they have a linear relationship, the equation is
\\[
LE = \beta_0 + \beta_1 T_s
\\]
Please complete the code below and answer the question below. The way to perform a simple linear regression is fairly straigtforward, please read this page for details: [https://www.mathworks.com/help/matlab/data_analysis/linear-regression.html](https://www.mathworks.com/help/matlab/data_analysis/linear-regression.html)

* _What is the fractional sensitivity of latent heat on temperature based on your regression, that is, what is the approximate value of \\( \dfrac{1}{LE}\dfrac{\partial LE}{\partial T_s} \\)?_

* **Bonus Question:** _Can you try to explain why the sensitivity latent heat flux adopts the value from your regression?_  \[Hint: You should think about the energy balance of the climte system. Figures on Page 26 of Lecture 3 are helpful for you to answer thsi question.\]

```
% linear regression
X = [ones(86,1) Tsa];
b = [FILL YOUR CODE HERE];

LEaCalc = X*b;
hold on
plot(Tsa, LEaCalc, 'k-')
hold off

fprintf('Regression slope: %5.2f W/m^2/K \n\n', b(2))
```



