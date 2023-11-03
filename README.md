# PanelTools

Collection of (hopefully) useful functions for dealing with panel datasets.

# Public functions

```
pcum(const series y)
```

Compute cumulated value for each panel unit. This is an alternative to gretl's built-in `cum()` function which also computes cumulated values for each cross-sectional unit but handles NAs differently (see below).

## Parameters

- `y`: series, target series

## Returns

Series with cumulated sum for each cross-sectional unit. NAs are replaced by zero before cumulation for each unit.

**Note:** In contrast, gretl's built-in `cum()` function stops computing cumulated values for a unit if the series has NAs between observations.



```
pmode(const series y)
```

Compute most frequent value for each cross-sectional unit.

## Parameters

- `y`: series, target series

## Returns

Series with most frequent values for each cross-sectional unit. `NA` values are ignored. If all values are `NA` for a unit, a series consisting only `NA` values is returned for that unit.


```
pfirst(const series y)
```

Compute first valid value for each panel unit.

## Parameters

- `y`: series, target series

## Returns

Series with the first valid value of `y` for each cross-sectional unit. If no valid values exists for a unit, `NA` is returned.


```
plast(const series y)
```

Compute last valid value for each panel unit.

## Parameters

- `y`: series, target series

## Returns

Series with the last valid value of `y` for each cross-sectional unit. If no valid values exists for a unit, `NA` is returned.


```
pquantile(const series y, const scalar value "p-th quantile")
```

Compute the p-th quantile for each unit.

## Parameters

- `y`: series, target series
- `value`: scalar, p-th quantile; must be in the range 0 < value < 1

## Returns

Series with the p-th quantile for each cross-sectional unit. `NA` values are ignored. If all values are `NA` for a unit, a series consisting only `NA` values is returned for that unit.


```
pxquantile(const series y, const scalar value "p-th quantile")
```

Compute the p-th quantile for each time period across all units.

## Parameters

- `y`: series, target series
- `value`: scalar, p-th quantile; must be in the range 0 < value < 1

## Returns

Series with the p-th quantile for each time period across-sectional units. `NA` values are ignored. If all values are `NA` for a unit, a series consisting only `NA` values is returned for that unit.


```
pxmean(const series y)
```

Compute the mean value of `y` across all units per time period. Missing values are ignored for computing the mean.

## Parameters

- `y`, series, target series

## Returns

Series with the mean value value across all units per observation. `NA` values are completely ignored and are not counted.


```
pxmax(const series y)
```

Compute the maximum value of `y` across all units per time period. Missing values in the cross-section for some unit are ignored for computing the maximum.

## Parameters

- `y`, series, target series

## Returns

Series with the maximum value across all units grouped by observation. `NA` values are completely ignored and are not counted.


```
pxmin(const series y)
```

Compute the minimum value of `y` across all units per time period. Missing values in the cross-section for some unit are ignored for computing the minimum.

## Parameters

- `y`, series, target series

## Returns

Series with the minimum value across all units grouped by observation. `NA` values are completely ignored and are not counted.


```
pxfraction(const series y, const scalar value "Value to search",
           const bool fraction[FALSE] "Fraction instead of total number")
```

Compute the total number of observations of "value" in `y` across all units per
time period. Missing values are completely ignored.

## Parameters

- `y`: series, target series
- `value`: scalar, Value to search in `y`
- `fraction`: bool, if `TRUE` (=1) compute the fraction of observations taking the value `value`, if `FALSE` (=0, default) compute the number of observations instead.

## Returns

Series with the respective value across all units per observation. `NA` values are completely ignored and are not counted.


```
ppolyfit(const series y, const int order[1::])
```

Fits a polynomial trend of order `order` to the input series `y` using OLS for each cross-sectional unit separately. This function mimics gretl's built-in function `polyfit()` which works only for time-series data correctly before version 2021c.

## Parameters

- `y`: series, target series
- `order`: integer, Order of the polynomial trend

## Returns

Series of fitted values for each unit. In case `y` has missing values for a specific unit, for this unit a time-series with NAs is returned.


```
panelinfo()
```

Compute basic information about the panel dimension.

## Returns

Nothing.


```
mpmean(const matrix m, const int N)
```

Compute vector of mean values of `m` for each cross-sectional unit. Matrix equivalent to gretl's built-in `pmean()` function. Missing values are ignored.

## Parameters

- `m`: matrix, Column vector of dimension TxN
- `N`: int, Number of cross-sectional units

## Returns

Matrix of stacked time-series of means of `m` for each unit. `NA` values are completely ignored.


```
fe_estimator (const matrix y, const matrix X, const int N[1::])
```

Matrix-based equivalent of gretl's built-in fixed-effect panel estimator (see `help panel`). Currently no inference is done.

## Parameters

- `y`: matrix, Column vector of stacked time-series
- `X`: matrix, Matrix of regressors of stacked time-series
- `N`: integer, Number of cross-sectional units

# Returns

Bundle object comprising both point estimates of coefficients (key: `coeff`) and estimated fixed-effects stored as stacked time-series (key: `ahat`).


```
pols_estimator (const matrix y, const matrix X, const int N[1::])
```

Matrix-based equivalent of gretl's built-in pooled `ols` command (see `help
panel`).

## Parameters

- `y`: matrix, Column vector of stacked time-series
- `X`: matrix, Matrix of regressors of stacked time-series
- `N`: integer, Number of cross-sectional units

## Returns

Bundle object comprising point estimates of coefficients (key: `coeff`).


```
pfcast (const matrix X, const bundle Model, const bool verbose[FALSE])
```

Compute static predictions for a given model. For the fixed-effects, the fixed-effects are added. This is the same treatment as gretl does for the panel fixed-effects model when applying the `fcast` command.

## Parameters

- `X`: matrix, Matrix of regressors of stacked time-series for test set.
- `Model`: bundle, Model object holding information on estimation results
- `verbose`: bool, print details (optional, default = `FALSE`)

## Returns

Matrix of stacked forecasts. N stacked time-series of length horizon. The horizon is internally determined by the input dimension of X and the number of cross-sectional units N as specified in the `Model` bundle.


```
mean_robust (const matrix X)
```

Works as gretl's built-in function `meanc()`. Compute mean values for each column even in case of missing values.

## Parameters

- `X`: matrix, Matrix with data and possibly missing values

## Returns

Row vector with mean values for each column.


```
sdc_robust (const matrix X)
```

Works as gretl's built-in function `sdc()`. Compute standard deviation for each column even in case of missing values.

## Parameters

- `X`: matrix, Matrix with data and possibly missing values

## Returns

Row vector with standard deviation values for each column.


```
panplot_quantile(const series y,
                 const bool do_pxquantile[FALSE],
                 bundle self[null])
```

Plot quantiles over time.

## Parameters

- `y`: series, target series
- `do_pxquantile`: bool, If `FALSE` (default) compute the quantiles across all observations. If `TRUE` compute the quantiles for each time period across all units.
- `self`: bundle, Optionally pass parameters for tweaking the plot.

Supported parameters for `self` are:

- `dateformat`: string (default "%Y-%m") Set date format of xtics. Requires that panel-time is set via e.g. `setobs 4 "2010:1" --panel-time`
- `fontsize`: scalar (default 10)
- `fontsize_tics`: scalar (default 8)
- `filename`: string (default "display", immediately show on screen)
- `key_position`: string (default "top left")
- `noenhanced`: string (default "noenhanced")
- `offset_xtics_x`: scalar (default = 0) Offset xtics along the x-axis
- `offset_xtics_y`: scalar (default = 0) Offset xtics along the y-axis
- `options_string`: string (default ""; gnuplot strings e.g. `set logscale y 2`)
- `rotate_xtics`: scalar (default = 0) Rotate xtics by some degrees
- `title`: string (default "")
- `xlabel`: string (default "")
- `ylabel`: string (default "")
- `yrange_min`: scalar (default `NA` (automatically set))
- `yrange_max`: scalar (default `NA` (automatically set))
- `quantiles`: matrix (default {0.05, 0.5, 0.95})
- `band`: series, Add “recession bars” to a plot. Series with values 0 and 1, where 1 indicates “on” and 0 “off” (drawn on left hand-side y-axis)
- `band_transparency`: int, Degree of transparency of the `band` series between 01 and 99 (default: 70)
- `band_width`: int, Width of vertical lines for `band` series (default: 10)
- `band_label`: string, Label the series of vertical bars (default: "band")
- `exogenous`: series, Series to add to plot and for which no quantiles are computed
- `exogenous_label`: string, Label for the exogenous series (default: "exog")
- `exogenous_dashtype`: int, Non-negative integer for controlling the dashtype (default: 1 (solid line)); see here: https://gnuplot.sourceforge.net/demo/dashtypes.html
- `exogenous_color`: string, Color of exogenous series (default: "black")



## Returns

FALSE if no error occurs, otherwise TRUE.


# Changelog

* **v0.7 (November 2023)**
    * Implement support for recession bars and plotting an 'exogenous' series for the `panplot_quantile()` function.
    * Add new functions `pxmin()` and `pxmax()` functions

* **v0.6 (April 2023)**
    * Print dates on x-axis for the `panplot_quantile()` function when the accessor `$obsdate` is available (panel time must be set)
    * Make use of Markdown format for help text

* **v0.5 (March 2023)**
    * `panplot_quantile()` now returns an error value
    * Add new optional parameter `noenhanced` for `panplot_quantile()` for deciding whether to consider underscores in labels and titles (default) or not
    * Bugfix: Parameter `options_string` did not work before for `panplot_quantile()`

* **v0.4 (October 2022)**
    * add new `pxquantile()` function
    * add new `panplot_quantile()` function

* **v0.3 (March 2022)**
    * Add new functions `mpmean()`, `meanc_robust()` and `sdc_robust()`
    * Add new function `fe_estimator()` for estimating a fixed-effects model using matrices as inputs (no inference supported, yet)
    * Add new function `pols_estimator()` for estimating a pooled-OLS model using matrices as inputs
    * Add new function `pfcast()` for computing predictions using some supported panel model using matrices.

* **v0.2 (July 2021)**
    * Add new `ppolyfit()` function

* **v0.1 (July 2021)**
    * Initial version
