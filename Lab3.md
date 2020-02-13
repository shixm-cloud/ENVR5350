## 3. Surface Fluxes

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
LE15 = mean(LE(:,:,1:60), 3);
SH15 = mean(SH(:,:,1:60), 3);

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