# readrba 0.1.1
* refresh internal data, update vignette and README

# readrba 0.1.1
* utils::download.file() used to attempt to address corporate network problems with curl
* 'www' added to URLs, to fix error encountered on some systems

# readrba 0.1.0
* Speed and stability improvements
* More non-standard tables can be tidied

# readrba 0.0.4
* `read_cashrate()` convenience function added
* Speed improvements, particularly when using series IDs

# readrba 0.0.3
* Historical and current RBA forecasts now available via `rba_forecasts()`
* Bug fixes and speed improvements

# readrba 0.0.2
* Can now use `series_id` argument to `read_rba()` to fetch based on series ID(s)
* Examine available RBA data using `browse_rba_series()` and `browse_rba_tables()`
* `cur_hist = "all"` no longer allowed
* Some non-standard tables now able to be tidied
* Added a `NEWS.md` file to track changes to the package.
