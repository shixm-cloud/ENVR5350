##  Problem Set 2. Equations and Surface Fluxes

In this exercise we will examine the sensible and latent heat flux in a simulation using the Commumunity Earth System Model (CESM)

### Problem 1. Geographic Variation of Fluxes (5 points)

Please complete the code below and answer the questions below.
* _Over Australia, is the mean sensible flux stronger or the mean latent heat flux? On an annual mean and spatial basis, do you think the Bowen ratio over Australia is greater or smaller than 1?_
* _In northern midlatitude oceans, where do you see strongest sensible and latent heat flux? Do you think the fluxes in those regions are stronger in winter or summer?_

```
%% Surface Fluxes
%% Load data
LE = ncread('hfls_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'hfls');  % latent heat flux
SH = ncread('hfss_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'hfss');  % sensible heat flux
Ts = ncread('ts_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'ts');  % surface temperature
lat = ncread('ts_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'lat');
lon = ncread('ts_Amon_CESM2_ssp585_r1i1p1f1_gn_201501-210012.nc', 'lon');

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

### Problem 2. Trend in Fluxes (5 points)

In this part, please complete the code to calculate the monthly global-mean surface sensible and latent heat fluxes, and plot the time series to show how they are predicted to change under SSP5-8.5 (shared socio-economic pathway) scenario. Answer this question
* _According to this CESM simulation, do you think the globally averaged sensible heat flux and latent heat flux will increase or decrease due to global warming? Which one changes faster?_

```
%% Task 2: changes under global warming
%  Now let's plot the time series of globa mean monthly sensible and latent
%  heat fluxes.

% Compute time series, save to LEs and SHs
% remember to weight data with cos latitude
wgt = reshape(cosd(lat), 1, 192);
wgt = wgt / sum(wgt);
LEs = squeeze(mean(sum(LE.*wgt, 2), 1));
SHs = squeeze(mean(sum(SH.*wgt, 2), 1));
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
### Problem 3. Understanding the Change (5 points)
{% include mathjax.html %}

Please derive the thermodynamic equation using potential temperature (below) with the first law of thermodynamics.
\\[
c_p\frac{D\theta}{Dt} = \frac{\theta}{T}\dot{Q}
\\]


<!--
### Problem 3. Understanding the Change
{% include mathjax.html %}

In our class, we showed that over water surfaces, we may calculate latent heat flux with the following expression,

\\[
    LE \approx \rho L C_{\text{DE}} U_r\left(q^*(T_s)(1-\text{RH}) + \text{RH} B_e^{-1}\frac{c_p}{L}(T_s - T_a)  \right)
\\]

* _Please show that by assuming the exponential dependence on temperature of \\( q^* \\) dominate in this formula, it is **approximately** correct to state that_
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
Please complete the code below and answer the question below. The way to perform a simple linear regression is fairly straightforward, please read this page for details: [https://www.mathworks.com/help/matlab/data_analysis/linear-regression.html](https://www.mathworks.com/help/matlab/data_analysis/linear-regression.html)

* _What is the fractional sensitivity of latent heat on temperature based on your regression, that is, what is the approximate value of \\( \dfrac{1}{LE}\dfrac{\partial LE}{\partial T_s} \\)?_

* **Bonus Question:** _Can you try explaining why the sensitivity latent heat flux adopts the value from your regression?_  \[Hint: You should think about the energy balance of the climate system. Figures on Page 40 and 42 of Lecture 3 are helpful for you to answer this question.\]

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
-->

